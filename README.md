### Distributed Tracing in Java with OpenTelemetry and Atatus

## Requirements

* Java >= 11
* Mysql 8
* serve

## Install dependencies run below command

* For installation of `serve` see: [https://www.npmjs.com/package/serve](https://www.npmjs.com/package/serve)

```bash
npm install -g serve
```

### Configuring MySQL Connection in Application Properties

To initialize the database connection, navigate to the `src/main/resources/application.properties` files of the `user-service`, `payment-service`, and `order-service` modules. Set up the MySQL connection by configuring the following properties:

```mysql
spring.datasource.url=jdbc:mysql://localhost:3306/db_name
spring.datasource.username=db_username
spring.datasource.password=db_password
```
Ensure to replace db_name, db_username, and db_password with your actual database name, username, and password respectively.

### Running the code

* This application require Eureka service registry

```bash
cd discovery-server
mvn clean install -Dmaven.test.skip
docker build -t discovery-service:1.0.1 .
docker run -d --name discovery-service -p 8761:8761 discovery-service:1.0.1
```

## Access

```bash
http://localhost:8761
```

### Start individual microservice using below commands

1) UserService


```bash
cd user-service
mvn clean install -Dmaven.test.skip
sh ./script/start.sh

```

2) OrderService


```bash
cd order-service
mvn clean install -Dmaven.test.skip
sh ./script/start.sh

```

3) PaymentService


```bash
cd payment-service
mvn clean install -Dmaven.test.skip
sh ./script/start.sh

```

4) Demo UI

* To capture traces from above microservice run the [sample-ui](ui) application using below commands

```bash
serve -l 9090 ui
```

5) Access the application using below urls

```bash
http://localhost:9090/
```

## OTel Setup 

Install Otelcol-contribute [using this link](https://github.com/open-telemetry/opentelemetry-collector-releases/releases)


## Atatus collector Configuration

* you can use collector configuration file `atatus-collector.yaml` for send OTel data to Atatus.


## Run otel-contrib

```bash
./otelcol-contrib --config=<Your-Local-path>/atatus-collector.yaml
```