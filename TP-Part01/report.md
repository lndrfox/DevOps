# TP PART 01

## Database

### Adminer container

First run the following command to launch the adminer container

`   docker run \
    -p "8090:8080" \
    --net=app-network \
    --name=adminer \
    -d --rm\
    adminer
    `


### PSQL container

In TP-Part01/Database/psql build the docker image using

`docker build -t database .`

Then run it 

`docker run --name psqldb -v psql-vol:/var/lib/postgresql/data -d -e POSTGRES_PASSWORD="pwd" --rm -p 5432:8888 --net=app-network database`


### Access Adminer

Acces adminer at `http://localhost:8090/`


## Backend

In TP-Part01/Backend , build the docker image using

`docker build -t backend .`

Then run the container using

`docker run -d --name backend -p 8080:8080 --net=app-network backend`

### Why do we need a multistage build 

What we are trying to do is compile the java sources and then run them using only docker rather than compiling the sourcers locally and the compiles bytecode in our container. However, compiling and running java code does not require the same ressources. 

For the build we need the jdk-17 as well as maven, however for running the compiled code we only need the java jre. To avoid bloating the final docker image we would need to remove manually maven and everything else that we don't need if we did everything in only one stage. However multistage build allow us to copy only what we want from the first stage of the build here the jar) thus keeping the final docker size as small as possible. 


### Dockerfile explained

```Dockerfile 
# Build 
FROM maven:3.8.6-amazoncorretto-17 AS myapp-build 
ENV MYAPP_HOME /opt/myapp
WORKDIR $MYAPP_HOME
COPY simpleapi/pom.xml .
COPY simpleapi/src ./src
RUN mvn package -DskipTests

# Run
FROM amazoncorretto:17
ENV MYAPP_HOME /opt/myapp
WORKDIR $MYAPP_HOME
COPY --from=myapp-build $MYAPP_HOME/target/*.jar $MYAPP_HOME/myapp.jar

ENTRYPOINT java -jar myapp.jar

```

#### Build stage

- In the build stage, we use the version  `3.8.6-amazoncorretto-17` of the *maven* docker image as a base, an with `AS myapp-build` we name this build stage `myapp-build`.

- We define the environent variable `MYAPP_HOME` then we declare it as the working directory (from which `COPY` and `RUN` commands will be executed).

- We copy our `pom.xml` and our java source (`src/`) in our working directory.

- Finally we build our application by running `mvn package` (`-DskipTests` skips the unit tests phase of the maven lifecyle).

#### Run stage 

- In the run stage, we use the version `17` of the *amazoncorretto* docker image as a base.

- We define the environment variable `MYAPP_HOME` then we declare it as the working directory.

- We copy the built jar from the previous phase (`--from=myapp-build`) inside of our working directory and we rename it as `myapp.jar`

- We define launching the jar as out entrypoint. 


## Http Server

In the HttpServer directory, build the docker image by running

` docker build -t my-apache2 .`

Then run the container with the command 

`docker run -dit --name my-running-app -p 8181:80 --net=app-network my-apache2`

The application will be available at `http://localhost:8181/`