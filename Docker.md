# Docker

## Points to discuss

![699BA00A-0E48-493E-8B10-A5A8121550C4.jpeg](Docker%2025b875eaa928464cb8df28c2899fa188/699BA00A-0E48-493E-8B10-A5A8121550C4.jpeg)

## What are Containers? Why we need them?

![3A1561D7-E7FF-4DAC-803C-CFC7DDC40292.png](Docker%2025b875eaa928464cb8df28c2899fa188/3A1561D7-E7FF-4DAC-803C-CFC7DDC40292.png)

![527A99D5-2F8F-4692-A88D-E6D5E27912FE.jpeg](Docker%2025b875eaa928464cb8df28c2899fa188/527A99D5-2F8F-4692-A88D-E6D5E27912FE.jpeg)

## What is image?

![C5FB4CED-0A32-4EEC-93DD-98E7D2B27BD6.jpeg](Docker%2025b875eaa928464cb8df28c2899fa188/C5FB4CED-0A32-4EEC-93DD-98E7D2B27BD6.jpeg)

## Why we need layers in image?

![BE477E76-C243-4535-B64F-CD4369EF08DC.jpeg](Docker%2025b875eaa928464cb8df28c2899fa188/BE477E76-C243-4535-B64F-CD4369EF08DC.jpeg)

## Docker vs Virtual Machine

![8038AACD-CC45-4213-8CB6-43ACB59FA6AA.jpeg](Docker%2025b875eaa928464cb8df28c2899fa188/8038AACD-CC45-4213-8CB6-43ACB59FA6AA.jpeg)

## What is inside a container? What is Port Binding?

![49FE17F2-BF7A-4DA2-B591-CC64FB5807CA.jpeg](Docker%2025b875eaa928464cb8df28c2899fa188/49FE17F2-BF7A-4DA2-B591-CC64FB5807CA.jpeg)

## Using docker for your application

![9770B4C6-68A9-45B2-9C78-EF8B29F0402D.jpeg](Docker%2025b875eaa928464cb8df28c2899fa188/9770B4C6-68A9-45B2-9C78-EF8B29F0402D.jpeg)

## What is docker network?

![F2DD228F-C640-4161-BDAF-C7AD6A4AFF6E.jpeg](Docker%2025b875eaa928464cb8df28c2899fa188/F2DD228F-C640-4161-BDAF-C7AD6A4AFF6E.jpeg)

## Docker commands

- **docker version** → to get version
- **docker images** → to get all images installed on your local machine
- **docker ps** → to get all running containers
- **docker ps -a** → to get all containers including the ones that are stopped
- **docker pull (image)** → fetches image from dockerhub
- **docker run (image)** → looks for image locally and runs it in a container. If not found pulls from dockerhub and then runs it. Every time you write docker run a new container starts.
- **docker run -p3000:6379 redis** → binds redis container port 6379 to local host port 3000. Now can access on local machine at that port. Port binding done to remove conflicts between containers with same ports.
- **docker stop (container id)** → stop container
- **docker start (container id)** → to resume stopped container
- **docker logs (container id)** → to get information about container
- **docker run -d —name my-container mongo** → runs image mongo in my-container in detached mode
- **docker exec -it (container id) /bin/(bash or sh)** → opens virtual file system of container. Write exit to get out of the terminal.
- **docker network create (network name)** → creates an isolated network where containers can communicate with just names
- **docker network ls** → list all networks
- **docker run mongo —net mongo-network**→ run mongo container in network called mongo-network
- **docker rmi (image id)** → deletes image
- **docker rm (container id)** → delete a container

## Docker compose

Docker compose is used to run multiple containers which need to interact with each other. Better than writing command for each container every single time. Also creates a default network for all the services mentioned.

Example➖

```yaml
version:'3'
services:
	mongodb:
		image:mongo
		ports:
			-27017:27017
		environment:
			-MONGO..._USERNAME=admin
	mongo-express:
		image:mongo-express
		ports:
			-8080:8080
		environment:
			-ME_CONFIG_MONGODB_A...
```

- **docker-compose -f (config.yaml) up** → runs all containers inside the file and creates a default network for them automatically
- **docker-compose -f (config.yaml) down** → stops all running containers mentioned inside the file

> Logs of all containers might get mixed since starting at the same time
> 

## Dockerfile

It’s a blueprint for building images. Is used to create an image out of your application that you have made. 

Example➖

```yaml
FROM node

ENV MONGO_DB_USERNAME=admin
		MONGO_DB_PWD=password

RUN mkdir -p/home/app

COPY . /home/app

CMD ["node", "server.js"]
```

- FROM node means installing node. This will behave as the base image for your nodejs app.
- You can set ENV as well but it will become difficult to change later so better is to do so with docker compose.
- RUN command runs a linux command. This will create a directory inside the container i.e all changes made by it don’t affect the local host machine.
- COPY will run on the local host and fetch files.
- CMD is the entry point command to run the app. It’s just used one time unlike RUN.
- **docker build -t (image name):(version/tag) (path of root folder where Dockerfile present)** → this command will create an image with the specified tag using Dockerfile
- Whenever changes made to Dockerfile you need to rebuild image.

## Private Docker repository

Pushing your image to some private repository like AWS ECR.

- First you need to authenticate yourself to push image. After creating a repository using AWS ECR UI you can then follow steps to do successful login on your local machine. For this you need to install AWS cli and configure your credentials.
- After that you have to change the image name to private-repository-name/image-name since that is how it will recognize that you are trying to push image to a private repository and not dockerhub.
- Finally run **docker push (new image name with domain registry)** to store your image. In AWS ECR each repo can only store 1 image and its many versions.

## Deploying on server

Now we can just pull all images needed to deploy the app on server. For pulling our app image from private repo we need to login on the server as well. And for the images to be pulled form dockerhub we can do that freely. We will create a docker compose file and start all services. The app image, mongo and mongo-express together form a docker network this time. 

So the node app can access mongo without port no.

```yaml
version:'3'
services:
	my-app:
		image:private-repo-name/image-name:tag
		ports:
			-3000:3000
	mongodb:
		image:mongo
		ports:
			-27017:27017
		environment:
			-MONGO..._USERNAME=admin
	mongo-express:
		image:mongo-express
		ports:
			-8080:8080
		environment:
			-ME_CONFIG_MONGODB_A...
```

So building an image and pushing to repository is something that a CI tool like Jenkins will do. 

## Docker Volumes

Used for data persistence. We can replicate data stored in container inside our local machine so the next time we restart container the data is not lost. Has 3 types➖

1. Host volumes → We can provide host path where data will be stored like this; **host-path:container-path** 
2. Anonymous volumes → No host path, given anonymously provides path on host for data; **container-path**
3. 🌟 Named volumes → can use names for the data volume to be stored on local machine, it will provide a path depending on OS automatically; **name:container-path**

```yaml
version:'3'
services:
	my-app:
		....
	mongodb:
		image:mongo
		ports:
			-27017:27017
		environment:
			-MONGO..._USERNAME=admin
		volumes:
			- mongo-data:/data/db
	mongo-express:
		....
volumes:
	mongo-data:
		driver:local
```
