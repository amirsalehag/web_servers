# what is traefik proxy
* Traefik is an edge router that receives requests on behalf of your system and that it intercepts and routes every incoming request.  
it knows all the logic and every rule that determine which services handle which requests (based on `path`, the `host`, `headers`...).  
* What sets Traefik apart, is that it automatically discovers the right configuration for your services.The magic happens when Traefik  
inspects your infrastructure, where it finds relevant information and discovers which service serves which request.  
With Traefik, there is no need to maintain and synchronize a separate configuration file: everything happens automatically, in real time (no restarts, no connection interruptions).  
* Traditionally edge routers (or reverse proxies) need a configuration file that contains every possible route to your services, Traefik gets them from the services themselves.  
Deploying your services, you attach information that tells Traefik the characteristics of the requests the services can handle.It means that when a service is deployed, Traefik detects it immediately and updates the routing rules in real time. Similarly, when a service is removed from the infrastructure, the corresponding route is deleted accordingly.  

---
