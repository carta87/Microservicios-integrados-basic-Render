FROM openjdk:17-jdk-slim

WORKDIR /app

COPY pom.xml .
COPY .mvn .mvn
COPY mvnw .

RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

RUN ./mvnw dependency:go-offline

COPY src ./src

RUN ./mvnw clean package -DskipTests

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "/app/target/microservice-gateway-0.0.1-SNAPSHOT.jar"]