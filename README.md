# HTS-Ng-Docker-App

To run this app from docker image;
1. pull the image from dockerhub 
```docker pull arupsarkardocker/hts-ng-docker-app```
2. run the image
```docker run --publish 80:80 --detach --name hts-prod arupsarkardocker/hts-ng-docker-app:latest```


# Development steps
Please look at the [a relative link](Steps.md) to understand the process of how this application is built, dockerized to follow simple CICD pipeline.


# Contributing
1. make sure you have installed docker, docker-compose (if you want to manage the app using docker-compose. this project supports docker-compose as well), node.js and angular-cli
2. build this app using docker 
```docker build -t hts-ng-docker-app:dev .```
and run it inside a container
```docker run --publish 4201:4200 --detach --name hts hts-ng-docker-app:dev```
OR
build and run using docker-compose
```docker-compose up -d --build```
3. app should be running locally at port 4201 (if you running user docker command) or 4202 (if you are running using docker-compose)
4. make changes and send us a PR. once merged into master, code will be automatically build and the new image will be published into dockerhub with your changes in it.
