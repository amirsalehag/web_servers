when configuring http inside `/etc/nginx/nginx.conf`, 
-
`include` for example `include       /etc/nginx/mime.types;` means that go and read the content of this configuration file  
and process them as well and then continue the rest of the conf file.

---
`default_type  application/octet-stream;` means it only applies to file extensions that have not been defined in the mime.types file.

---
* `server {}` A server block is a subset of Nginx's configuration that defines a virtual server used to  
handle requests of a defined type. Administrators often configure multiple server blocks and decide  
which block should handle which connection based on the requested domain name, port, and IP address.  
* `listen (port number);` defines which port to listen to and also if the request ,
met this condition,(we can have multiple server blocks) it will process this server block.
* `server_name (domain name);` Server name is what it will listen for , from that specified port.  
* location / {  
gzip on or off;  
root (example: /var/www/html);  
index index.html;  
etc...  
}  
in `location {}` we specify which kind and wich files to show when the client,puts `/` after the url  
(for example: `example.com/` or when `location /images {}` then `example.com/images`) and what to do with that files.  

---
`sendfile     on or off;` specifies if the file could be shared of not.

---
