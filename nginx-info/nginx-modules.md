# Adding modules
For adding modules to nginx, we need to download nginx from source and compile it ourselves.(click on this [link](https://www.youtube.com/watch?v=R4hPcNAW33c) for installing from source.)
* loading dynamic modules, you can see click on this [link](https://dzone.com/articles/introducing-dynamic-modules-in-nginx-1911-nginx).
---
# Adding vts module
When we downloaded nginx from source, we change directory to the extracted directory and we use the `./configure` binary, and as its  
arguments that defines various aspects of the system, including the methods nginx is allowed to use for connection processing.  
At the end it creates a `Makefile`.
* Check this [link](http://nginx.org/en/docs/configure.html) to see its arguments.
* we clone the modules repository on our host:
```
sudo git clone https://github.com/vozlt/nginx-module-vts.git
```
* And then we specify its path in `./configure` as `--add-module=` argument:
```
sudo ./configure --sbin-path=/usr/bin/nginx --conf-path=/etc/nginx/nginx.conf --prefix=/etc/nginx/  \  
--error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid  \  
--with-http_ssl_module --with-pcre --add-module=/home/devops/nginx-rebuild/nginx-module-vts
```
(these are some basic configurations for nginx, you can the link above to add more configurations.)  
* After that,we need to configure the `nginx.conf` file to be able to use this module:
```
http {
    vhost_traffic_status_zone;

    ...

    server {

        ...

        location /status {
            vhost_traffic_status_display;
            vhost_traffic_status_display_format html;
        }
    }
}
```
we specify the module on the http server that we want it to be monitored, and then we configure another server  
to specify its ports and directive names needed.  
(we also need to set a `listen` port other than 80 or whatever the main server is listening on to be able to connect to it).  
(we also dont necessarily need to set the directive to `/status`, we can use anything else we want).  
and then we reload its service.  
* the metrics pages can be access to as follows:
`/status`
`/status/format/json`
`/status/format/html`
`/status/format/prometheus`
`/status/format/jsonp`
* We can see more configurations and details on its own repository.  
