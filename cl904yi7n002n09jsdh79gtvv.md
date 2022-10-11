# OVH VPS SSH Broken Pipe (Timeout): How to keep alive

I recently noticed that on my OVH VPS Server, SSH sessions remain blocked (appended with <em>Write failed: Broken pipe</em> message after a while) when I leave them unused for a while (about 30 seconds). This means you will need to reconnect once again, and all ssh/bash processes from the previous connection will be alive (waste of memory!).

I think there is a proxy (new proxy? knowing I didn't have this problem before) between me and my server that kill idle connections.

To solve this problem you can change settings on the client, adding <em>ServerAliveInterval</em> property in you sshd config file.
If you are on Linux or Mac client, you need to edit <em>sshd_config</em>:
<pre><code> <code>sudo vi /etc/ssh_config</code></pre>
Append to the end of file
<pre><code> <code>ServerAliveInterval 30</code></pre>
which means your client sends an <em>handshake</em> message any 30 seconds to the server.

If you have a windows client you can set the KeepAlive like in the following screenshot

<a href="https://res.cloudinary.com/blog-mornati-net/image/upload/v1391640946/keepAlive_wrk4p3.jpg"><img class="aligncenter size-thumbnail wp-image-990" alt="keepAlive" src="https://res.cloudinary.com/blog-mornati-net/image/upload/c_crop,h_448,w_448,x_9,y_0/h_150,w_150/v1391640946/keepAlive_wrk4p3.jpg" width="150" height="150" /></a>

Now all your ssh connection should be kept active even if you leave your computer for a coffe ;)