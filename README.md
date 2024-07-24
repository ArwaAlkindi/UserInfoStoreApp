# Spring Boot MVC Application with MySQL

This repository contains a Spring Boot MVC application that stores user information in a MySQL database. 

This guide will help you fork the repository, and provide you steps to manually run this application.

These manual steps can be used to write a Dockerfile for this application along with a docker-compose.yaml, and run both application and database containers locally by just using one command.

## Prerequisites

Before you begin, ensure you have the following installed on your machine:

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Git](https://git-scm.com/)

## Getting Started

### 1. Fork the Repository

1. Go to the original repository: [Link of Repo](https://github.com/CodelineAtyab/UserInfoStoreApp)
2. Click the `Fork` button on the top right to create your own copy of the repository.

### 2. Clone Your Forked Repository

1. Clone your forked repository. You can simply do it by clicking on `Code` and copying the HTTPS URL.
2. Now simply clone the repo on your PC. Command will be like `git clone [URL]` where URL is the one, you got from step 1.

### 3. Create a New Branch
Before implementing anything, make sure you are on a new branch.

You can create a new branch and switch to it by running:
```bash
git checkout -b your-name-containerize-app
```
Please replace `your-name` with your actual name in lower case and without any spaces.

### 4. Create Dockerfile for Spring Boot Application
You have to create Dockerfile for the application at the same location where `pom.xml` is located.

Here are the manual steps of building and running this application:

1. Install `OpenJDK 17`
2. Install `maven`
3. In the directory where `pom.xml` is located, Run `mvn clean package -DskipTests`
4. A jar file by the name `rihal-0.0.1-SNAPSHOT.jar` will be generated in `./target` directory. This target directory will be created as a result of command run in step 3.
5. Now navigate to the target directory by `cd target`
6. Before running the application, make sure that MySQL server is running with the right configuration. Here is an example of `docker run` command to run a MySQL container with the right configuration: 

```bash
docker run --name mysql-cont -p 3307:3307 -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=usersystem -e MYSQL_PASSWORD=root -e MYSQL_TCP_PORT=3307 mysql:9.0.0
```

7. Verify that the DB server is accessible on port `3307` by connecting it using DBVisualizer or any other DB Client Application.

8. Run the application using the following command: `java -jar app.jar`

9. Test the application in the browser by navigating to http://localhost:8080/

10. Make sure the data is save in the DB by creating some users, updating any specific user and deleting one user. If we restart the application, the data should remain as is.

#### Some important things to note:

- If you want, you can skip the installation of OpenJDK 17 and maven by simply using this Docker base image in your Dockerfile: `FROM maven:3.8.1-openjdk-17 AS build`

- The above image will give you OpenJDK and Maven, and now any `java` and `mvn` commands can be run.

- You can simply write the Dockerfile by looking at the commands mentioned in steps 3 and 8. This will let you run the application within a container without the need to install java and maven on your PC.

### 5. Create docker-compose.yml
Create a docker-compose.yml file in the root directory.

Make sure to specify a volume for MySQL Database, so the data can be stored on your local machine.

This is useful for backing up the data in case the container is deleted.

This is the container's path `/var/lib/mysql` that should be mounted to a volume.

### 6. Build and Run the Containers
Run the following command to build and start the containers:
```bash
docker-compose up --build
```
This command will:

1. Build the Docker image for the Spring Boot application.
2. Start the MySQL container and initialize the database.
3. Start the Spring Boot application container.

### 7. Access the Application
Once the containers are up and running, you can access the application at `http://localhost:8080`

### 8. Stopping the Containers
```bash
docker-compose down
```