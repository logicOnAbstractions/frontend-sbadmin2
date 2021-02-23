# this dockerfile builds a npm/gulp environment for front-end dev, specially for the sb-admin-2 setup
# as described in the readme.
FROM node:buster

RUN mkdir frontend
WORKDIR frontend
COPY ./frontend . 

RUN npm install


# apparently browser-sync, which is what Gulp uses to broadcast live changes, runs on 3000 by default
EXPOSE 3000	

CMD ["npm","start"]

# then run all that on docker
# first build that image, if not already done, & push it to dockerhub
# docker build -t franckit/myfrontendenv .
# docker push franckit/myfrontendenv

# then run it with the bind mount. -it is nice because on the host, you'll have to user the container's IP (172.0.1.2:3000 or similar), because browsersync runs localhost on the container, not the host.
# cd to /frontend
# docker run -it -v "$(pwd):/frontend franckit/myfrontendenv"
# magic happens:
# to run gulp tasks:docker run -it -v "$(pwd)/":/frontend franckit/myfrontendev node_modules/.bin/gulp [ css | js | vendor | watch ]
# 

# issues
# 1 - node_module is created at container build. but in bind mounts, the host's mapping has priority. So that destroys the node_modules install in the container. To remedy this, you can run the same command but override the CMD with "bash", and from there run 'npm install'. Takes a little while, but it creates the node_modules folder with the bind mount on, which means it'll be on the host. So next time you run the container, the modules will be there (since they will be mounted from the host directory)
# 2- must use quotes (-v "$(..../frontend" otherwise the command fails. Perhaps the : or / confuses docker and it interprets the images to run wrong.)