# Stage 1: Build the Spring Boot application
# This stage compiles your Java code and produces a JAR file.
# We are using 'maven:3.9.6-jdk-23' because you are using Java 23.
# I've also updated Maven to its latest stable version (3.9.6) for better support with newer JDKs.
FROM maven:3.9.6-jdk-23 AS build 

# Set the /app directory inside the container as the working directory.
WORKDIR /app

# Copy all your project code from the host machine to the /app directory in the container.
COPY . .

# Build the project using Maven.
# 'clean package' creates the JAR file, and '-DskipTests' skips tests for faster builds.
RUN mvn clean package -DskipTests

# Stage 2: Create the final lightweight image
# This stage creates the final, much smaller image containing only the Java Runtime Environment (JRE)
# necessary to run your application. This significantly reduces the image size.
# We are using 'openjdk:23-jdk-slim' because you are using Java 23.
FROM openjdk:23-jdk-slim 

# This line informs Docker that the container will listen on port 8080.
# Platforms like Render typically map this EXPOSEd port to their assigned port (e.g., 10000).
EXPOSE 8080

# Copy the generated JAR file from the 'build' stage (which is in the 'target' folder)
# to this final image, naming it 'app.jar'.
# You might need to replace '*.jar' with the exact name of your JAR file 
# (e.g., 'your-application-0.0.1-SNAPSHOT.jar') if you have multiple JARs or want to be specific.
COPY --from=build /app/target/*.jar app.jar

# This is the command that will be executed when the Docker container starts.
# It runs your 'app.jar' file using the Java Virtual Machine.
ENTRYPOINT ["java","-jar","/app.jar"]
