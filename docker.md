# Spring boot deployment with docker
## spring boot application config

-	add a file in root of your project with name <b>DockerFile</b>.

```
FROM java:8
ADD /target/{application_name}-1.0.0.war {application_name}.war
ENTRYPOINT ["java", "-jar", "/{application_name}.war"]
```

## Docker commands
1.	build image - 

```
docker image build -t {dockerId}/{repository}:{tag} .
```

2. docker run - 

```
 docker run -itd -p 8082:8081 --name {{app_name}} -d {dockerId}/{repository}:{tag}
```

-	8082 - where docker is running
- 	8081 - where spring application is running inside docker
-  to access the docker - localhost:8082
- -d - flag for running the docker in background.

3. list all docker containers

```
docker ps -a
```

4. stop a container

```
docker container stop {container_id}
```

5. delete all images / names

```
docker rmi -f $(docker images -a -q)   
docker rm -vf $(docker ps -a -q)  
```

6. push to docker hub

```
docker login

docker push {dockerId}/{repository}:{tag} 
```

## deploy spring boot application to ec2 instance with docker image

- add a security group to the created ec2 instance with ssh, http, https inbounds to anywhere

-	get a key-pair from ec2 and download and keep it in your local (ex application.pem)
- connect to ec2 via ssh (first go to the path where key-pair is located)

```
ssh -i "application" ec2-user@ec2-23-233-233-23.ap-south-1.compute.amazonaws.com 
```
- sudo yum update -y
- install docker - sudo yum install docker -y
- pull the image from docker hub

```
docker pull {dockerId}/{repository}:{tag}
```
- run docker - (ec2 provide port 80 for http)

```
docker run -itd -p 80:8081 --name {{app_name}} -d {dockerId}/{repository}:{tag}
```
- acces the docker like via public ipv4 address.