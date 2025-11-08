FROM maven:3.9.11-eclipse-temurin-17-alpine@sha256:5d774e0953f89cf5c07bbb22924aede9b0a351c24faccd6a759de4c3f209cac7 AS build

WORKDIR /app

COPY ./pom.xml .

RUN mvn dependency:go-offline -B

COPY ./src ./src

RUN mvn clean package -DskipTests




FROM eclipse-temurin:17-jre-alpine@sha256:e7ed585b34913e0a780e0282330183a0ea14ad6b929362d02aea1156b43262bf AS runtime

WORKDIR /app

COPY --from=build /app/target/*jar /app/app.jar

EXPOSE 8080

ENTRYPOINT [ "java", "-jar", "app.jar" ]


