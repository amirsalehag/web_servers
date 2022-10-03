# *nginx logs inside container*
When we run nginx as a container, the log files (`var/log/nginx/{access.log,error.log}`) are softlinked to `dev/{stdout,stderr}`.
For mounting its log directory to a `volume`, we want it to be not a softlink file, therefore we either write its command in  
Dockerfile or `rm` and `touch` the log files instead and restart the container.
