# what is traefik proxy
* Traefik is an edge router that receives requests on behalf of your system and that it intercepts and routes every incoming request.  
it knows all the logic and every rule that determine which services handle which requests (based on `path`, the `host`, `headers`...).  
* What sets Traefik apart, is that it automatically discovers the right configuration for your services.The magic happens when Traefik  
inspects your infrastructure, where it finds relevant information and discovers which service serves which request.  
With Traefik, there is no need to maintain and synchronize a separate configuration file: everything happens automatically, in real time (no restarts, no connection interruptions).  
* Traditionally edge routers (or reverse proxies) need a configuration file that contains every possible route to your services, Traefik gets them from the services themselves.  
Deploying your services, you attach information that tells Traefik the characteristics of the requests the services can handle.It means that when a service is deployed, Traefik detects it immediately and updates the routing rules in real time. Similarly, when a service is removed from the infrastructure, the corresponding route is deleted accordingly.  

---
# configuration file
Traefik has two different configuration file:  
* Static configuration which there are three ways to define the static configuration options: In a configuration file, In the command-line arguments, As environment variables which we can only use one at a time. If no value was provided for a given option, a default value applies.  
* Dynamic donfiguration which contains everything that defines how the requests are handled by your system. This configuration can change and is seamlessly hot-reloaded, without any request interruption or connection loss.  

---
# trouble-shooting return codes
* When trying to request to a url, and we get an error code, you can we the cuases of them in this [documentation](https://doc.traefik.io/traefik/getting-started/faq/#why-is-traefik-answering-xxx-http-response-status-code).

---
# docker vs dynamic file(personal preference)
* I personally recommend using dynmic configuration in most cases because using docker labels for example, when needed to change  
a containers label, we need to also restart and recreate the docker containers as well, but the traefiks dynamic configuration file,  
because its traefik does a HOT-RELOAD , theres no need for restarting even the traefik container itself(except for its static configuration).

---
# providers
* Configuration discovery in Traefik is achieved through Providers. The providers are infrastructure components, whether orchestrators, container engines, cloud providers, or key-value stores.  
* You can check the list of providers from this [link](https://doc.traefik.io/traefik/providers/overview/#supported-providers).  
### docker
* for using docker as its provider, first we define the `endpoint` where it should listen for docker container informations:  
```
providers:
  docker:
    endpoint: "unix:///var/run/docker.sock"
```
Then because traefik uses the labels attached to the containers to work, we then configure the containers using labels(for example):  
```
version: "3"
services:
  my-container:
    # ...
    labels:
      - traefik.http.routers.my-container.rule=Host(`example.com`)
```
You can read more about the labels configurations in its [document](https://doc.traefik.io/traefik/routing/providers/docker/).

---
### dynamic configuration
* dynamic configuration is a yaml file which we specify it in the static file:  
```
providers:
  file:
    filename: /path/to/config.yml
    or
    directory: /path/to/config-directory
```
* entryPoints:  
EntryPoints are the network entry points into Traefik. They define the port which will receive the packets, and whether to listen for TCP or UDP. for example:  
```
entryPoints:
  website:
    address: ":80"
  secureweb:
    address: ":443"
```
* routers:  
A router is in charge of connecting incoming requests to the services that can handle them. In the process, routers may use pieces of middleware to update the request, or act before forwarding the request to the service.  
* rule:  
Rules are a set of matchers configured with values, that determine if a particular request matches specific criteria. If the rule is verified, the router becomes active, calls middlewares, and then forwards the request to the service.for example:  
```
rule = "Host(`example.com`)" && "PathPrefix(`/prefix`)"
```
* middlewares:  
Attached to the routers, pieces of middleware are a means of tweaking the requests before they are sent to your service (or before the answer from the services are sent to the clients).  
There are several available middleware in Traefik, some can modify the request, the headers, some are in charge of redirections, some add authentication, and so on.  
* service:  
The Services are responsible for configuring how to reach the actual services that will eventually handle the incoming requests. Heres a simple example:  
```
http:
  services:
    my-service:
      loadBalancer:
        servers:
        - url: "http://<private-ip-server-1>:<private-port-server-1>/"
        - url: "http://<private-ip-server-2>:<private-port-server-2>/"
```
* We can configure the services 
