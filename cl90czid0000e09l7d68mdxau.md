# Docker: RPM Builder Container

I spent the last months playing around [Docker](https://www.docker.com). Even if I've some doubt about using it in production environment and the way you need to use it on Windows and OSx (with a Linux VirtualBox VM), it is really impressive what you can do and how you can simplify your way to work.

But I don't want to talk about all the pros and cons about docker, I just want to show you how I "fix" a recent problem I had using it!

**Problem:** My production server runs EL7 (CentOS7) and I need the latest *git* package installed. No way to find an existent RPM to install and I don't have any other ready server to use to build the RPM.

**Solution (quick and dirty):** Download the sources of git directly on the production server; install all required packages to build it; *make && make install*.
In case of problem... you know... it's the production server!!

**Docker Solution:** Configure a CentOS7 docker container and use it to build the RPM (and it's the thing I did ;) [docker-gitrpm-centos7](https://github.com/mmornati/docker-gitrpm-centos7)).

After this I try to create a sort of *dynamic* docker container I can reuse to build any sort of RPMs for any sort of RedHat based platform.
The result of this work is the [docker-mock-rpmbuilder](https://github.com/mmornati/docker-mock-rpmbuilder).

You just need sources+spec file or directly the SourceRPM package, run the docker selecting your target platform, and wait for the RPM.

The build process is based on [Mock](http://fedoraproject.org/wiki/Projects/Mock) and my current implementation allows to build packages for which all dependencies are available on the on-line repositories (official + EPEL)... I will try anything complex in the future ;)

Here you are a little doc to use my project (you can find it on the github Readme file too).

---
First off all you need to build the docker container:

<pre class="language-bash command-line" data-user="root" data-host="server"><code class="language-bash">git clone https://github.com/mmornati/docker-mock-rpmbuilder.git
cd docker-mock-rpmbuilder
docker build -t mmornati/mockrpmbuilder .</code></pre>

##### Create working directory

To allow the import/export of created RPMs you need to create a docker volume and allow the read/write rights (or add owner) to the user builder(uid:1000).

<pre class="language-bash command-line" data-user="root" data-host="server"><code class="language-bash">mkdir /tmp/rpmbuild
chown -R 1000:1000 /tmp/rpmbuild</code></pre>
In this folder you can put the src.rpms to rebuild.

##### Execute the container to build RPMs

To execute the docker container and rebuild RPMs four SRPMs you can run it in this way:

<pre class="language-bash command-line" data-user="root" data-host="server"><code class="language-bash">docker run -d -e MOCK_CONFIG=epel-6-i386 -e SOURCE_RPM=git-2.3.0-1.el7.centos.src.rpm -v /tmp/rpmbuild:/rpmbuild --privileged=true mmornati/mockrpmbuilder</code></pre>

If you don't have the source RPMs yet, but you get spec file + sources, to build RPMs you need to start the docker container in this way:

<pre class="language-bash command-line" data-user="root" data-host="server"><code class="language-bash">docker run -d -e MOCK_CONFIG=epel-6-i386 -e SOURCES=SOURCES/git-2.3.0.tar.gz -e SPEC_FILE=SPECS/git.spec -v /tmp/rpmbuild:/rpmbuild --privileged=true mmornati/mockrpmbuilder</code></pre>

It is important to know:

* With spec file the build process could be longer. The reason is mock it is invoked 2 times: the first to build SRPM the second to build all other RPMS.
* The folders specified for SPEC_FILE, SOURCES and SOURCE_RPM env variables are relative to your mount point. This means if files are at the root of mount point you need to specify only the file name, otherwise the subfolder should be added too. (SOURCES in my example)



> NB: It's important to run the container with privileged rights because mock needs the "unshare" system call to create a
> new mountpoint inside the process.
> Without this you will get this error:
>
>  ERROR: Namespace unshare failed.
>
> A different solution (which didn't worked for me right now) should be to change the lxc-configuration to allow docker the right admin just for this operation.
> With this command: setcap cap_sys_admin+ep
> But I didn't find the right way to execute it (any hint is welcome) :)

##### Allowed configurations

<pre class="language-bash command-line" data-user="root" data-host="server"><code class="language-bash">default        epel-7-x86_64     fedora-19-x86_64  fedora-20-x86_64   fedora-21-s390x         fedora-rawhide-s390
epel-5-i386    fedora-19-armhfp  fedora-20-armhfp  fedora-21-aarch64  fedora-21-x86_64        fedora-rawhide-s390x
epel-5-ppc     fedora-19-i386    fedora-20-i386    fedora-21-armhfp   fedora-rawhide-aarch64  fedora-rawhide-sparc
epel-5-x86_64  fedora-19-ppc64   fedora-20-ppc64   fedora-21-i386     fedora-rawhide-armhfp   fedora-rawhide-x86_64
epel-6-i386    fedora-19-ppc     fedora-20-ppc     fedora-21-ppc64    fedora-rawhide-i386     
epel-6-ppc64   fedora-19-s390    fedora-20-s390    fedora-21-ppc64le  fedora-rawhide-ppc64    
epel-6-x86_64  fedora-19-s390x   fedora-20-s390x   fedora-21-s390     fedora-rawhide-ppc64le</code>
</pre>

##### Check build state

To check the rpmbuild progress (and/or errors) you can simply check docker logs.

<pre class="language-bash command-line" data-user="root" data-host="server" data-output="2-3,5-28"><code class="language-bash">docker ps
CONTAINER ID        IMAGE                            COMMAND             CREATED             STATUS              PORTS               NAMES
f8d161e72832        mmornati/mockrpmbuilder:latest   "/build-rpm.sh"     2 seconds ago       Up 1 seconds                            modest_bardeen
docker logs -f f8d161e72832
=> Building parameters:
      MOCK_CONFIG:    epel-6-i386
      SOURCE_RPM:     git-2.3.0-1.el7.centos.src.rpm
INFO: mock.py version 1.2.6 starting (python version = 2.7.5)...
Start: init plugins
INFO: selinux disabled
Finish: init plugins
Start: run
INFO: Start(/rpmbuild/git-2.3.0-1.el7.centos.src.rpm)  Config(epel-6-i386)
Start: clean chroot
Finish: clean chroot
Start: chroot init
INFO: calling preinit hooks
INFO: enabled root cache
INFO: enabled yum cache
Start: cleaning yum metadata
Finish: cleaning yum metadata
INFO: enabled ccache
Mock Version: 1.2.6
INFO: Mock Version: 1.2.6
Start: yum install
[....]
</code></pre>

And use Mock log files, that are created in the output dir:

<pre class="language-bash command-line" data-user="root" data-host="server" data-output="2-5"><code class="language-bash">ll /tmp/rpmbuild/output/
totale 188
-rw-rw-r--. 1 1000 1000  40795 21 feb 10:37 build.log
-rw-rw-r--. 1 1000 1000 144994 21 feb 10:34 root.log
-rw-rw-r--. 1 1000 1000    962 21 feb 10:34 state.log</code></pre>

##### Output

If all worked well, you should have all the RPMs (source + binaries) availables in the configured output folder:

<pre class="language-bash command-line" data-user="root" data-host="server" data-output="2-17"><code class="language-bash">ll /tmp/rpmbuild/output/
totale 28076
-rw-rw-r--. 1 1000 1000   117010 21 feb 10:40 build.log
-rw-rw-r--. 1 1000 mock  7941092 21 feb 10:39 git-2.3.0-1.el6.i686.rpm
-rw-rw-r--. 1 1000 mock  5193722 21 feb 10:33 git-2.3.0-1.el6.src.rpm
-rw-rw-r--. 1 1000 mock     5472 21 feb 10:39 git-all-2.3.0-1.el6.i686.rpm
-rw-rw-r--. 1 1000 mock    24540 21 feb 10:39 git-arch-2.3.0-1.el6.i686.rpm
-rw-rw-r--. 1 1000 mock    90668 21 feb 10:39 git-cvs-2.3.0-1.el6.i686.rpm
-rw-rw-r--. 1 1000 mock 14123468 21 feb 10:40 git-debuginfo-2.3.0-1.el6.i686.rpm
-rw-rw-r--. 1 1000 mock    37600 21 feb 10:39 git-email-2.3.0-1.el6.i686.rpm
-rw-rw-r--. 1 1000 mock   240400 21 feb 10:39 git-gui-2.3.0-1.el6.i686.rpm
-rw-rw-r--. 1 1000 mock   148940 21 feb 10:39 gitk-2.3.0-1.el6.i686.rpm
-rw-rw-r--. 1 1000 mock   437148 21 feb 10:39 git-svn-2.3.0-1.el6.i686.rpm
-rw-rw-r--. 1 1000 mock   145996 21 feb 10:39 gitweb-2.3.0-1.el6.i686.rpm
-rw-rw-r--. 1 1000 mock    67256 21 feb 10:39 perl-Git-2.3.0-1.el6.i686.rpm
-rw-rw-r--. 1 1000 1000   147267 21 feb 10:40 root.log
-rw-rw-r--. 1 1000 1000     1248 21 feb 10:40 state.log</code></pre>