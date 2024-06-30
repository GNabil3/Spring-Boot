# Spring_Boot

## Docker File:
This project utilizes a multi-stage Docker build process to create a lightweight container image for a Java application. The build process uses Gradle for building the application and OpenJDK for running it.
Stage 1: Build the Application
1- Base Image: gradle:7.5.1-jdk11
2- Working Directory: /app
3- Copy Files: COPY . /app
4- Print Gradle Version: RUN gradle --version
5- Build the Application: RUN gradle build --no-daemon --stacktrace --info

Stage 2: Run the Application
1- Base Image: openjdk:11-jre-slim
2- Working Directory: /app
3- Copy Jar File: COPY --from=builder /app/build/libs/*.jar app.jar
4- Expose Port: EXPOSE 8080
5- Entry Point: ENTRYPOINT ["java", "-jar", "app.jar"]

### This repository contains an Azure Pipeline YAML configuration for building, pushing, pulling, and deploying a Spring Boot project Docker image to a Kubernetes cluster. The pipeline is structured into multiple stages for a streamlined CI/CD process.
Pipeline Stages
The pipeline consists of the following stages:

BuildImage: Builds the Docker image of the Spring Boot project.
PushImage: Pushes the Docker image to the specified container registry.
PullImage: Pulls the Docker image from the container registry.
DeployToK8s: Deploys the Docker image to an AWS Kubernetes cluster.
CreateIngress: Creates an ingress resource in the Kubernetes cluster

YAML Configuration: The configuration for this file exist in Azure pipeline.yaml file


