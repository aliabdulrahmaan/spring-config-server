### What is the difference between application.yml && bootstrap.yml :

#### application.yml or application.properties

application.yml/application.properties file is specific to Spring Boot applications.
Unless you change the location of external properties of an application , spring boot
will always load application.yml from the following location:
```
/src/main/resources/application.yml
```

You can store all the external properties for your application in this file.
Common properties that are available in any Spring Boot project can be found at:
https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html
You can customize these properties as per your application needs. Sample file is shown below:

```
spring:
    application:
        name: foobar
    datasource:
        driverClassName: com.mysql.jdbc.Driver
        url: jdbc:mysql://localhost/test
server:
    port: 9000
```




#### bootstrap.yml or bootstrap.properties
``` 
   It's only used/needed if you're using Spring Cloud and your application's 
   configuration is stored on a remote configuration server 
   (e.g. Spring Cloud Config Server).
   
   A Spring Cloud application operates by creating a "bootstrap" context,
   which is a parent context for the main application. Out of the box it 
   is responsible for loading configuration properties from the external sources,
   and also decrypting properties in the local external configuration files.
```
#### Another Answer :

```
bootstrap.yml on the other hand is specific to spring-cloud-config
and is loaded before the application.yml

bootstrap.yml is only needed if you are using Spring Cloud 
and your microservice configuration is stored on a 
remote Spring Cloud Config Server.
```

#### Important points about bootstrap.yml

1. When used with Spring Cloud Config server, you shall specify the application-name and config git
location using below properties.

```
spring.application.name: "application-name"
spring.cloud.config.server.git.uri: "git-uri-config"
```

2. When used with microservices (other than cloud config server), we need to specify the application name 
and location of config server using below properties.
```
spring.application.name: 
spring.cloud.config.uri: 
```

3. This properties file can contain other configuration relevant to Spring Cloud environment 
for e.g. eureka server location,encryption/decryption related properties.

Upon startup, Spring Cloud makes an HTTP(S) call to the Spring Cloud Config Server with the name 
of the application and retrieves back that application’s configuration.

application.yml contains the default configuration for the microservice and any configuration retrieved (from cloud config server) during the
bootstrap process will override configuration defined in application.yml


#### How to add spring cloud config client to microservice:
1. Add spring cloud config client dependency to POM.xml
```
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-config</artifactId>
		</dependency>
```

2. Add the below to application.yml:
```
spring:
  application:
    name: profile-service
  config:
    import-check:
      enabled: false
    import: optional:configserver:http://localhost:8888
```

#### spring.application.name 
is the name of yaml file in config server