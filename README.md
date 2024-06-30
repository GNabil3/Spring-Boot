# Spring-Boot
## Docker File
The provided Dockerfile effectively utilizes a multi-stage build to create a smaller and more optimized image for running a Java application built with Gradle. Here's a breakdown of the code and its advantages:

Stage 1: Building the Application (builder stage)

Base Image: gradle:7.5.1-jdk11: This image includes the Gradle build tool and Java Development Kit (JDK) 11, providing the necessary environment to build your application.
Working Directory: WORKDIR /app: Sets the working directory within the container to /app.
Copying Code: COPY . /app: Copies your application's source code from the local directory (."") to the /app directory within the container.
Printing Gradle Version: RUN gradle --version: Runs the gradle --version command to print the Gradle version installed in the container (optional for debugging or verification purposes).
Building the Application: RUN gradle build --no-daemon --stacktrace --info: Runs the Gradle build command using the following options:
--no-daemon: Disables the Gradle daemon for this build, preventing it from persisting across builds.
--stacktrace: Includes stack traces in error messages for better troubleshooting.
--info: Provides more detailed information during the build process.
Stage 2: Running the Application (runtime stage)

Base Image: openjdk:11-jre-slim: This image contains only the OpenJDK 11 runtime environment, which is sufficient to run your compiled Java application, resulting in a smaller image size compared to the builder stage.
Working Directory: WORKDIR /app: Sets the working directory within the container to /app, aligning with the builder stage.
Copying Application Jar: COPY --from=builder /app/build/libs/*.jar app.jar: This crucial step copies the application JAR file (typically named *.jar) generated during the build process in the builder stage. The --from=builder syntax indicates copying from the previous stage (builder). The *.jar wildcard will match any JAR file found in the /app/build/libs directory, ensuring the latest build is copied.
Exposing Port: EXPOSE 8080: Exposes port 8080 within the container. This allows external services to access your application running on this port.
Entrypoint: ENTRYPOINT ["java", "-jar", "app.jar"]: Defines the command to be executed when the container starts. This launches the Java runtime (java) with the -jar flag and specifies the application JAR file (app.jar) as the argument.
