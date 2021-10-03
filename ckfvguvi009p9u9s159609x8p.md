## HTTP2 Some Basic tests to check if it is working on your website

# HTTP2 Some Basic tests to check if it is working on your website


I didn't know before, but today I discovered that all my dockers were already HTTP2 enabled.

If you want to test your server/website can simply use this [online service](https://tools.keycdn.com/http2-test). In the answer, you will know if your website is supporting http2 or not.

![Image for post](https://miro.medium.com/max/60/0*Q3aVt-pKzNG0Zn_b.png?q=20)

<noscript><img alt="Image for post" class="t u v hc aj" src="https://miro.medium.com/max/1488/0*Q3aVt-pKzNG0Zn_b.png" width="744" height="359" srcSet="https://miro.medium.com/max/552/0*Q3aVt-pKzNG0Zn_b.png 276w, https://miro.medium.com/max/1104/0*Q3aVt-pKzNG0Zn_b.png 552w, https://miro.medium.com/max/1280/0*Q3aVt-pKzNG0Zn_b.png 640w, https://miro.medium.com/max/1400/0*Q3aVt-pKzNG0Zn_b.png 700w" sizes="700px"/></noscript>

I then try to investigate a little bit more trying to have better information about why it was working (I know, a normal person in the same situation would have said ‘Cool, nothing to do’, but… you know… **I’m an engineer** :)).

# cUrl

Using curl (you need a brand new version http2 enabled. If you are on OSx you can simply install it using _brew_) you are able to force a request using http2 protocol and check the verbose output


```
curl -vso /dev/null https://blog.mornati.net 
* Rebuilt URL to: https://blog.mornati.net/ 
* Trying 51.254.141.153... 
* TCP_NODELAY set 
* Connected to blog.mornati.net (51.254.141.153) port 443 (#0) 
* ALPN, offering h2 
* ALPN, offering http/1.1 
* Cipher selection: ALL:!EXPORT:!EXPORT40:!EXPORT56:!aNULL:!LOW:!RC4:@STRENGTH 
* successfully set certificate verify locations: 
* CAfile: /usr/local/etc/openssl/cert.pem CApath: none 
* TLSv1.2 (OUT), TLS header, Certificate Status (22): } [5 bytes data] 
* TLSv1.2 (OUT), TLS handshake, Client hello (1): } [512 bytes data] 
* TLSv1.2 (IN), TLS handshake, Server hello (2): { [114 bytes data] 
* NPN, negotiated HTTP2 (h2) { [5 bytes data] 
* TLSv1.2 (IN), TLS handshake, Certificate (11): { [2731 bytes data] 
* TLSv1.2 (IN), TLS handshake, Server key exchange (12): { [589 bytes data] 
* TLSv1.2 (IN), TLS handshake, Server finished (14): { [4 bytes data] 
* TLSv1.2 (OUT), TLS handshake, Client key exchange (16): } [70 bytes data] 
* TLSv1.2 (OUT), TLS change cipher, Client hello (1): } [1 bytes data] 
* TLSv1.2 (OUT), TLS handshake, Unknown (67): } [36 bytes data] 
* TLSv1.2 (OUT), TLS handshake, Finished (20): } [16 bytes data] 
* TLSv1.2 (IN), TLS change cipher, Client hello (1): { [1 bytes data] 
* TLSv1.2 (IN), TLS handshake, Finished (20): { [16 bytes data] 
* SSL connection using TLSv1.2 / ECDHE-RSA-AES128-GCM-SHA256 
* ALPN, server did not agree to a protocol 
* Server certificate: 
* subject: CN=blog.mornati.net 
* start date: Sep 30 19:51:00 2016 GMT 
* expire date: Dec 29 19:51:00 2016 GMT 
* subjectAltName: host "blog.mornati.net" matched cert's "blog.mornati.net" 
* issuer: C=US; O=Let's Encrypt; CN=Let's Encrypt Authority X3 
* SSL certificate verify ok. 
* Using HTTP2, server supports multi-use 
* Connection state changed (HTTP/2 confirmed) 
* Copying HTTP/2 data in stream buffer to connection buffer after upgrade: len=0 } [5 bytes data] 
* Using Stream ID: 1 (easy handle 0x7fc0bc805200) } [5 bytes data] > GET / HTTP/1.1 > Host: blog.mornati.net > User-Agent: curl/7.50.3 > Accept: */* > { [5 bytes data] 
* Connection state changed (MAX_CONCURRENT_STREAMS updated)! } [5 bytes data] < HTTP/2 200 < server: nginx/1.11.3 < date: Mon, 10 Oct 2016 20:00:10 GMT < content-type: text/html; charset=utf-8 < content-length: 18269 < x-powered-by: Express < cache-control: public, max-age=0 < etag: W/"475d-WYO6IWvGpMGFIyCdpUCIeQ" < vary: Accept-Encoding < strict-transport-security: max-age=31536000 < { [3839 bytes data] * Curl_http_done: called premature == 0 
* Connection #0 to host blog.mornati.net left intact</span>
```


All seems good. The only thing I need to investigate better is **ALPN, server did not agree to a protocol**. I think it is anything related to the TLS ciphers used.

# Check your webserver for HTTP2

The almost only thing you need for the http2 protocol is to enable it on your webserver.
To check on the NGINx you can simply execute the nginx command with the**-V** parameter.


```
docker exec -it xxxxxxxxx nginx -V nginx version: nginx/1.11.3 
built by gcc 4.9.2 (Debian 4.9.2-10) built with OpenSSL 1.0.1t 3 May 2016 TLS SNI support enabled configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-http_ssl_module --with-http_realip_module --with-http_addition_module --with-http_sub_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_random_index_module --with-http_secure_link_module --with-http_stub_status_module --with-http_auth_request_module --with-http_xslt_module=dynamic --with-http_image_filter_module=dynamic --with-http_geoip_module=dynamic --with-http_perl_module=dynamic --add-dynamic-module=debian/extra/njs-0.1.0/nginx --with-threads --with-stream --with-stream_ssl_module --with-stream_geoip_module=dynamic --with-http_slice_module --with-mail --with-mail_ssl_module --with-file-aio --with-ipv6 --with-http_v2_module --with-cc-opt='-g -O2 -fstack-protector-strong -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2' --with-ld-opt='-Wl,-z,relro -Wl,--as-needed'</span>
```


And check if in the output you have **— with-http_v2_module**. Easy!

Check then if the http2 is enabled on the nginx server configuration file.

![Image for post](https://miro.medium.com/max/60/0*CcA3Nrk4SoHNMPZJ.png?q=20)

<noscript><img alt="Image for post" class="t u v hc aj" src="https://miro.medium.com/max/906/0*CcA3Nrk4SoHNMPZJ.png" width="453" height="392" srcSet="https://miro.medium.com/max/552/0*CcA3Nrk4SoHNMPZJ.png 276w, https://miro.medium.com/max/906/0*CcA3Nrk4SoHNMPZJ.png 453w" sizes="453px"/></noscript>

# Tools

There are already plenty of tools online to check/use/configure the http2 protocol.
The only thing we have to know is that we need to code the website in a completely different way to allow a better performance in http2\. For example: before we used the javascript/css joiner and minifier. If we will keep plenty of little (minified files) without merging them we will have a better performance on our websites.
There are some good documentations online about http2:
[https://github.com/bagder/http2-explained](https://github.com/bagder/http2-explained)
[https://www.smashingmagazine.com/2016/02/getting-ready-for-http2/](https://www.smashingmagazine.com/2016/02/getting-ready-for-http2/)

I made a load/benchmark tests using an http2 tool (which should only use the http2 protocol).

![Image for post](https://miro.medium.com/max/60/0*LQsO_ewxwUBKfuej.png?q=20)

<noscript><img alt="Image for post" class="t u v hc aj" src="https://miro.medium.com/max/1682/0*LQsO_ewxwUBKfuej.png" width="841" height="464" srcSet="https://miro.medium.com/max/552/0*LQsO_ewxwUBKfuej.png 276w, https://miro.medium.com/max/1104/0*LQsO_ewxwUBKfuej.png 552w, https://miro.medium.com/max/1280/0*LQsO_ewxwUBKfuej.png 640w, https://miro.medium.com/max/1400/0*LQsO_ewxwUBKfuej.png 700w" sizes="700px"/></noscript>

And the same test using the ApacheBenchmark (ab)

![Image for post](https://miro.medium.com/max/54/0*-Y55O5AX31J-jY3e.png?q=20)

<noscript><img alt="Image for post" class="t u v hc aj" src="https://miro.medium.com/max/1114/0*-Y55O5AX31J-jY3e.png" width="557" height="623" srcSet="https://miro.medium.com/max/552/0*-Y55O5AX31J-jY3e.png 276w, https://miro.medium.com/max/1104/0*-Y55O5AX31J-jY3e.png 552w, https://miro.medium.com/max/1114/0*-Y55O5AX31J-jY3e.png 557w" sizes="557px"/></noscript>

I’m not sure the data are really comparable… test in http2 took less with better performance. But, for what I know, it can only be the tools used for the test.

Anyway. Let’s start using HTTP2 and prepare your servers and applications for it.



_Originally published at_ [_https://blog.mornati.net_](https://blog.mornati.net/http2-some-basic-tests-to-check-if-it-is-working-on-your-website/) _on October 10, 2016._