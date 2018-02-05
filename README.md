# Little web server in docker container

## Built docker image
```sh
$ sudo docker build -t webserver .
```

## Run
```sh
 $ sudo docker run -p 80:80 webserver
 ```

## Dokerfile

For build docker container we need to create Dokerfile.

[More information about it](https://gist.github.com/remarkablemark/aacf14c29b3f01d6900d13137b21db3a)

```dockerfile
FROM debian:9.3-slim

RUN rm /bin/sh && ln -s /bin/bash /bin/sh

RUN apt-get update \
    && apt-get install -y curl

ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 8.9.4

# https://github.com/creationix/nvm#install-script
RUN curl --silent -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.2/install.sh | bash

RUN source ~/.bashrc && nvm install $NODE_VERSION && nvm alias default $NODE_VERSION && nvm use default

ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

RUN node -v
RUN npm -v

RUN npm install express

COPY . .

EXPOSE 80

CMD node server.js
```

Instructions for dokerfile

 ### FROM - base image for your image
```dockerfile
FROM ubuntu:16.04
```
Use image of Ubuntu version 16.04 as base

### MAINTAINER - author of image

### RUN - run command

### CMD - final command for initialization

### ENV - Environment variables with name

```dockerfile
ENV USER_DIR \home\user

RUN cd $USER_DIR
```

### COPY <source> <file system of container> - copy folder or files from **source** in your container

```dockerfile
COPY . .
```
Copy files from current folder to current (default root) folder
