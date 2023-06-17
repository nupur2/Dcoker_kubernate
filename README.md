# Dcoker_kubernate
Docker and Kubernative notes 

https://hub.docker.com/_/alpine/
https://alpinelinux.org/


Docker help to run ur application 

orcestration of service is done by - kubernate 


what is docker ?
-------------------------------------------------
Docker is tool to run application in isolated ENV
Similar to virtual mashines
App run in same env
just works
standared for software deployment

benifits
-----------------------------------------------
Run containers in seconds instead of minutes
less resource result less disk space
usses less memory
does not need full Operating system 
deployment
testing 

----------------------------------------------------
~ docker ps ----- to check docker demon is wokring or not

-------------------------------------
Docker image 



Image is template for creating an env of ur choice
from iamge you run the contaner 
snapshot 
has everything you want to run (os, app to run )

registry is a place where you download images
--------------------------------------------------------
Pull public images ---
~ docker pull nginx   It will download nginx
-----------------------------------------

How to see list of images ---
~ docker images

repository tag     image id  craeted        size
NGINX      latest  someid    3 days ago     109 mb

---------------------------------------------------

How to run image

~ docker run nginx:latest  

~ docker conatiner ls - you can see image up a dn running 

~ docker run -d nginx:latest  (it will give id and nothing is hanging in detached mode)

------------------------------------
How to map local host 
~docker run -d -p 8080:80 nginx:latest (mapp local host 8080 to 80 inside continer )

in broswer  type -> localhost:8080 

~ docker stop pid 
---------------------------------------

How to map multiple port 
~docker run -d -p 8080:80 -p 8080:80 nginx:latest


------------------------------------------
delete container 
~ docker rm either name of container or ID
------------------------------------------

delete all comatiner 
~ docker ps -aq
u can see list of all doc with id 

~ docker rm $(docker ps -aq) (sometime it will not work it will sat first remove running conatner)

~ docker rm -f $(docker ps -aq)

------------------------------------------------
Docker with name 
~ docker run --name website_two -d -p 8080:80 nginx:latest

------------------------------------------------------------------------------
format the docker 
~ docker ps --format="ID" (long text)

~ export FORMAT="ID" (long text)

~docker ps format= $FORAMT

-----------------------------------------------------------------
Dcoker vloumes

1) Volumes allow to share the data it can be file and folder 
2) allow to share data between host and conatiner 
3) also with in contaner

you can create volume inside continer that will appear in host 

n the context of Docker, a volume is a persistent storage location that exists outside of the container. 
Volumes are useful for storing data that needs to persist even if the container is stopped or removed. 
In a Dockerfile, the VOLUME instruction is used to specify a mount point for a volume within the container.


Hosting some simple static content in "https://hub.docker.com/_/nginx"

$ docker run --name some-nginx -v /some/content:/usr/share/nginx/html:ro -d nginx

---------------------------------------------------------------------

How to override default home page of nginx ?


using volume
-------------------------------------------------------

share data between comatiners
 
-> website docker run --name webiste-copy --volumes-from website -d -p 8081:80 nginx:latest

------------------------------------------------------------------------

How to create dockerfile, build ur own image


inside the root folder -> create Dockerfile

inside dockerfile  ---

FROM nginx:latest              (from)
ADD . /user/share/nginx/html (destination)

Now run 
-> website docker build --tag website:latest (give tage to ur image)
-> website docker image ls (you can see image details)

---------------------------------------------------------
How to add node JS 
Create dockerFile inside node project

FROM node:latest
WORKDIR /app
ADD . .
RUN npm install
CMD node index.js

Create .dockerignore (here you can add all the file that you dont want to include as part of image)
node_modules
DockerFiles
.git


------------------------------------------------------------

Caching in docker


1) dont want to rerun npm install 

How ?
1) you can set work dire - WORKDIR /app
2) then Add pakage*.json ./   (here pakage .json not changed evertime. here we set work dir and data will took from cache)

FROM node:latest
WORKDIR /app
ADD pakage*.json ./  (it will run from canche)
RUN npm install      (it will run from cache) 
Add . .
CMD node index.js   (here source code change so that is in Add ..)

when you run it you can see  ---->using cache
-----------------------------------------

Image SIze 

for exmaple you haive 100 iamges - then docker will be overloaded and it will take to much time to download


yo can alpine images 
https://alpinelinux.org/
----------------------------------------------------
https://hub.docker.com/_/node/ serach alpine 

docker pull node:lts-alpine

previous node was 900 mb
and alpine node is 75 mb

Same for nginx

docker pull nginx:latest-alpine (alpine not found)

docker pull nginx:alpine

nginx original -126 MB
alpine - 21 MB
-----------------------------------------------------------------------
Tags , versioning and tagging

1)Allows you to control the image
2)Avoid breking changes
3)Its safe
---------------------------------
whenever u update any image and used the same name then u  can see <none>
------------------------------------------------------------------------

How to give specific version ?

---------------------------------------------------------

Docker registry

1)Highly scalable server side application that stores and lets you distribute docker image
2)used in your CD/CI pipeline
3)run your application

private /public image registrey provider
1)docker hub
2)query.io
3)Amazon ECR

-------------------------------------------------
crete docker hub repo

got to docker hub -> repository -> create repository -> you can decide visibilty private/public


pushing image to dockerhub repo 


~ docker push userName/reponame 
