# ---------- BUILD STAGE ----------
FROM eclipse-temurin:17-jdk-alpine AS build

# Crea un directorio de trabajo
WORKDIR /app

# Copia los archivos de Maven/Gradle primero (para cache)
COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .

# Descarga dependencias sin compilar aún (mejora tiempos de build)
RUN ./mvnw dependency:go-offline -B

# Copia el código fuente completo
COPY src src

# Compila el proyecto
RUN ./mvnw clean package -DskipTests

# ---------- RUNTIME STAGE ----------
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

# Copia el JAR generado desde la etapa anterior
COPY --from=build /app/target/*.jar app.jar

# Exponer el puerto (Render lo detectará igual)
EXPOSE 8080

# Comando de ejecución
ENTRYPOINT ["java", "-jar", "/app/app.jar"]