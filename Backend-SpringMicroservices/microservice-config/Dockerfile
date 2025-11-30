# ---------- BUILD STAGE ----------
FROM eclipse-temurin:17-jdk-alpine AS build

WORKDIR /app

# Copiar archivos necesarios para que Maven pueda resolver dependencias
COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .

# Dar permisos al wrapper
RUN chmod +x mvnw

# Descargar dependencias (esto mejora el cache del build)
RUN ./mvnw dependency:go-offline -B

# Copiar el código fuente
COPY src src

# Construir el JAR
RUN ./mvnw clean package -DskipTests

# ---------- RUNTIME STAGE ----------
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

# Copiar el JAR construido
COPY --from=build /app/target/*jar app.jar

# Importante: NO uses un puerto fijo
EXPOSE $PORT

ENTRYPOINT ["sh", "-c", "java -jar /app/app.jar"]
