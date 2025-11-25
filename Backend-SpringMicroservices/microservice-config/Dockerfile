# ---------- BUILD STAGE ----------
FROM eclipse-temurin:17-jdk-alpine AS build

WORKDIR /app

COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .

# 🔥 IMPORTANTE: dar permisos de ejecución al wrapper de Maven
RUN chmod +x mvnw

# Descarga dependencias
RUN ./mvnw dependency:go-offline -B

# Copia el código fuente
COPY src src

# Compila el proyecto
RUN ./mvnw clean package -DskipTests


# ---------- RUNTIME STAGE ----------
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

COPY --from=build /app/target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "/app/app.jar"]