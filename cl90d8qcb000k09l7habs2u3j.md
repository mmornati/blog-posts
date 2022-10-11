# DockerHub: automate your docker images build and push

The [DockerHub](https://hub.docker.com/) website it is created by Docker to allow developers to automate the Docker images build and push the image into the Docker repository.
In this way you don't need to use your CPU time to build the image and not even your bandwidth to upload the image into the Docker repository to allow others to pull it.

The procedure to configure DockerHub is really simple.

**1)** Create a repository pointing to your source repository (GitHub repo):
![RepositoryDefinition](https://res.cloudinary.com/blog-mornati-net/image/upload/v1472992771/ompu7km5llsymm9fyos8.png)
and set is as an automate build. It is not necessar but in this way anytime you are pushing changes to your repository, DockerHub will build the Docker image automatically.
![AutomatedBuildDefinition](https://res.cloudinary.com/blog-mornati-net/image/upload/v1472992876/edfosetsfbifisc1xmfu.png)

**2)** Configure then for your repository the branch and/or tag you want to auto-check for builds.
![TriggerBuildDefinition](https://res.cloudinary.com/blog-mornati-net/image/upload/v1472992978/b1oqzuxesturkuozdybc.png)
In the example, which is mine ghostblog configuration, there is a "listener" on the **master** branch building the **latest** version of the Docker and a second lister checking tag.
If any tag is pushed matching the RegExp into the name:
`/^v.[0-9.]+$/`
an image is built using the same version than the one used into the tagname.
Examples:

* **v.0.10.1** tag triggers a build of an image tagged as v.0.10.1
* **update_dockerfile** tag is ignored by dockerhub

**3)** Change your code on the associated GitHub repo and push code (and tags if needed).
You can see if any build is started on the *Build Details* page. 
![BuildDetailsPage](https://res.cloudinary.com/blog-mornati-net/image/upload/v1472992947/qf7hzy0d3numjssgplwk.png)
At the end of the build procedure (depending on your Docker could take only few seconds or hours) you will have the status of the Docker image:
![DockerBuildSuccess](https://res.cloudinary.com/blog-mornati-net/image/upload/v1472993403/wa8sp3ijlaqkue9us2xv.png)
Or, in case of errors:
![DockerBuildError](https://res.cloudinary.com/blog-mornati-net/image/upload/v1472993438/mvy4fsbd4yerjzedyyf2.png)
You can then click on the failing build to access to the details. Here you can find the build problem and check the build log.
![DockerBuildErrorDetails](https://res.cloudinary.com/blog-mornati-net/image/upload/v1472993514/xbn59rtguhw3kb5ycwjv.png)
![DockerBuildLog](https://res.cloudinary.com/blog-mornati-net/image/upload/v1472993858/f9hjfiyxdbexlyjk2eag.png)
**4)** Check on the *Tags* page to see the available images of your docker.
Here you are also able to manage them removing, for example, some old or wrong image.
![DockerImages](https://res.cloudinary.com/blog-mornati-net/image/upload/v1472993638/djz6sg5idluo2aovoqcb.png)

**5)** As I said at the begging, all the images built using this method, are available on the Docker repository.
This means you can simply *pull* the desired image/tag.

<pre class="language-bash command-line" data-user="marco" data-host="server"><code class="language-bash">sudo docker pull mmornati/docker-ghostblog:v0.10.0</code></pre>

 
> ==**INFO**==: all the images are built as "fresh" image. This means that all the intermediate steps are removed for any new build.
In my example **it is important to have this** cause, one of the step into the Dockerfile is downloading a file from internet (ghost latest version). If you keep all the build steps you can't be sure to have the Ghost's latest version into your Docker.

<pre class="language-docker"># Install Ghost
RUN \
  cd /tmp && \
  wget https://ghost.org/zip/ghost-latest.zip && \
  unzip ghost-latest.zip -d /ghost && \
  rm -f ghost-latest.zip<code class="language-docker"></code></pre>


It is easy and quick, isn't it? :)
