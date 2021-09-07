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
docker image build -t {dockerId}/{repository}:{tag}
```

2. docker run - 

```
 docker run -itd -p 8082:8081 --name cobidv29 -d {dockerId}/{repository}:{tag}
```

-	8082 - where docker is running
- 	8081 - where spring application is running inside docker
-  to access the docker - localhost:8082
- -d - flag for running the docker in background.