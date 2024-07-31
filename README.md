# Jenkins-playground
I've just setup jenkins to Docker installed on my Synology NAS. I use this repository for testing functionality and configuration of jenkins.

## Setup Docker Cloud in Jenkins
To setup Docker as cloud in jenkins, I had to provide the URI to it. Since jenkins runs inside the docker itself it wasn't able to access to it. To resolve that problem I looked for a way to forward traffic to docker from jenkins and I found a project called socat. 

'''docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock'''

you can get the IP address using this command (replace <container_id> with the actual container id of socat. 
'''docker inspect <container_id> | grep IPAddress'''

It listens to port 2375. So if the command above returns 127.0.0.3, you should use 
'''tcp://127.0.0.3:2375'''
as the docker URI
