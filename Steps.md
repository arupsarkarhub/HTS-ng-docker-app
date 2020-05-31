# Steps
##### setup ubuntu linux 

##### setup nodejs, angular cli

install node.js: 
```
sudo apt-get install -y nodejs npm
```
verify: 
```
node -v, npm -v
```
install angular cli: 
```
sudo apt-get install -g @angular/cli
```
verify: 
```
ng --version
```


##### create the angular project

```
ng new hts-ng-docker-app
```
verify: 
```
cd hts-ng-docker-app
```
```
ng serve
```
4. setup docker
```
sudo apt-get install docker.io
```
verify: 
```
docker -v
```

##### Add Dockerfile to the project

##### Build and run application using docker locally

build: 
```
sudo docker build -t hts-ng-docker-app:dev .
```
run: 
```
sudo docker run --publish 4201:4200 --detach --name hts hts-ng-docker-app:dev
```
verify: 
```
http://localhost:4201
```

##### Setup docker-compose

install: 
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
give docker-compose executable permission: 
```
sudo chmod +x docker-compose
```
verify: 
```
docker-compose --version
```

##### run docker using docker compose

create docker-compose file
Build the image and fire up the container: 
```
sudo docker-compose up -d --build
```
verify: 
```
http://localhost:4202/
```
stop and remove: 
```
sudo docker-compose down --remove-orphans
```

##### create production docker build
create Dockerfile-prod. First, we take advantage of the multistage build pattern to create a temporary image used for building the artifact – the production-ready Angular static files – that is then copied over to the production image. The temporary build image is discarded along with the original files, folders, and dependencies associated with the image. This produces a lean, production-ready image. Next, the unit and e2e tests are run in the build process, so the build will fail if the tests do not succeed.

##### run production docker using docker compose

create prod docker compose yml
run: 
```
sudo docker-compose -f docker-compose-prod.yml up -d --build
```
verify: 
```
http://localhost:80/
```
stop and remove containers: 
```
sudo docker-compose down --remove-orphans
```

##### setup dockerhub and created a repo to host the docker image

##### pushed prod docker image to dockerhub
updated docker-compose-prod.yml to push images to dockerhub account repo
```
sudo docker-compose -f docker-compose-prod.yml build
```
```
sudo docker login docker.io
```
```
sudo docker push arupsarkardocker/hts-ng-docker-app
```

##### verify prod docker image
pull image: 
```
sudo docker pull arupsarkardocker/hts-ng-docker-app
```
run: 
```
sudo docker run --publish 80:80 --detach --name hts-prod arupsarkardocker/hts-ng-docker-app:latest
```
verify: 
```
http://localhost:80/
```

##### make a smalll development change to test out CICD

##### build and create new image
```
sudo docker-compose -f docker-compose-prod.yml build
```

##### update docker image with this new image in dockerhub

##### verify the new prod image from dockerhub

```
sudo docker pull arupsarkardocker/hts-ng-docker-app
```
```
sudo docker run --publish 80:80 --detach --name hts-prod arupsarkardocker/hts-ng-docker-app:latest
```

##### add project to Git
```
git remote add origin git@github.com:gitswarup/hts-ng-docker-app.git
```
```
git push -u origin master
```

##### link github with dockerhub
linking github with dockerhub auto build runs on dockerhub infrastructure. The same can be achieved vice-versa. Using github packaging. Via github packaging, git build will create push the docker image to dockerhub. But that needs more config setup. But it might be more cheaper as github infra will be much cheaper than dockerhub infra. For now, both are same and free.

