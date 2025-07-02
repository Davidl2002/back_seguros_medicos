# 1. Etapa de build: compilar el .jar con Maven Wrapper
FROM eclipse-temurin:17-jdk-alpine AS build

WORKDIR /app

# Copia solo lo esencial para aprovechar la cache de Docker
COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .

# Descarga dependencias sin compilar aún
RUN chmod +x mvnw && \
    ./mvnw dependency:go-offline -B

# Copia el resto del código
COPY src src

# Compila el JAR
RUN ./mvnw clean package -DskipTests -Dmaven.test.skip=true -B

# 2. Etapa final: ejecutar el JAR
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

# Copia el JAR generado en la fase anterior
COPY --from=build /app/target/*.jar app.jar

# Exponer el puerto de Spring Boot
EXPOSE 8080

# Comando de arranque
ENTRYPOINT ["java","-jar","app.jar"]
