

1. install storage and ingress class which used in k8s cluster before apply any mainfest file

```bash

kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/v0.0.26/deploy/local-path-storage.yaml

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

2. assign mysql or postgres database

* add dependency section in `pom.xml` fil

```xml
<!-- mysql -->

<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <scope>runtime</scope>
</dependency>

<!-- postgres -->

<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <scope>runtime</scope>
</dependency>
```

* in `src/main/resources/application.properties` 

```
# --- PostgreSQL Connection (ACTIVE) ---
#spring.datasource.url=jdbc:postgresql://${POSTGRES_HOST}:${POSTGRES_PORT}/${POSTGRES_DB}
#spring.datasource.username=${POSTGRES_USER}
#spring.datasource.password=${POSTGRES_USER_PASSWORD}

# --- MongoDB Connection ---
#spring.data.mongodb.uri=mongodb://${MONGO_USER}:${MONGO_PASSWORD}@${MONGO_HOST}:${MONGO_PORT}/${MONGO_DB_NAME}

# --- MySQL Connection ---
app.second-datasource.url=jdbc:mysql://${MYSQL_HOST}:${MYSQL_PORT}/${MYSQL_DB}
app.second-datasource.username=${MYSQL_USER}
app.second-datasource.password=${MYSQL_USER_PASSWORD}

```

3. add prometheus 

* in `pom.xml` fil add dependency section

```xml
<!-- prometheus -->

<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```
* in `src/main/resources/application.properties` add 

```
# Actuator
management.endpoints.web.exposure.include=*

# management.endpoints.web.exposure.include=health,prometheus
```

* `.env` file for deploy the application using docker and docker compose

```
MYSQL_HOST=mysqldb-service
MYSQL_USER=blogging
MYSQL_DATABASE=mysqldb
MYSQL_PORT=3306
MYSQL_ROOT_USER=root
MYSQL_ROOT_PASSWORD= blogging-app
MYSQL_PASSWORD= blogging-app

#POSTGRES_HOST=postgresdb-service
#POSTGRES_USER=blogging
#POSTGRES_DB=postgresdb
#POSTGRES_PORT=5432
#POSTGRES_ROOT_USER=root
#POSTGRES_ROOT_PASSWORD=YmxvZ2dpbmctYXBw # blogging-app (base64 encoded)
#POSTGRES_USER_PASSWORD=YmxvZ2dpbmctYXBw # blogging-app (base64 encoded)
```

4. jacoco plugin for sonarqube code quality check

```xml
 <!-- JaCoCo Maven Plugin -->
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>${jacoco.version}</version>
    <executions>
        <execution>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <id>report</id>
            <phase>test</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```