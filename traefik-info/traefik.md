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
