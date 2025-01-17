# Ticket Booking Example

![Ticket Booking Process](booking-service-java/src/main/resources/ticket-booking.png)

A ticket booking example using 
* Camunda Platform 8, 
* RabbitMQ,
* Java Spring Boot App
* NodeJS App

![Architecture Overview](architecture.png)

# How To Run

<a href="http://www.youtube.com/watch?feature=player_embedded&v=m3MYuRKLZa8" target="_blank"><img src="http://img.youtube.com/vi/m3MYuRKLZa8/0.jpg" alt="Walkthrough" width="240" height="180" border="10" /></a>

## Aanmaken instanties EC2 Linux 2

### Installation Server Queue Server

* sudo yum update
* sudo amazon-linux-extras enable docker=latest
* sudo yum clean metadata
* sudo yum install docker

* Toevoegen security rechten om poort 15672 en 5672 open te zetten

```
Starten:
sudo systemctl start docker
sudo docker run --name some-rabbit -p 15672:15672 -p 5672:5672 rabbitmq:3-management

Als eenmaal een keer gestart, middels sudo docker stop some-rabbit stoppen en sudo docker start some-rabbit starten.
```
* http://'ec2_host':15672/#/queues/
* User: guest
* Password: guest

### Installation Server Ticket Booking App
* sudo yum update
* sudo yum install -y amazon-linux-extras
* sudo amazon-linux-extras enable java-openjdk11
* sudo yum clean metadata
* sudo yum install java-11-openjdk
* sudo update-alternatives --config javac --> selecteer optie java11
* sudo yum install maven

* in application.properties juiste ip adres opvoeren van Queue server
* toevoegen security rechten om poort 8080 open te zetten

## Create Camunda Platform 8 SaaS Cluster

* Login to https://camunda.io/
* Create a new cluster
* When the new cluster appears in the console, create a new set of API client credentials.
* Copy the client credentials into
  * Java App  `booking-service-java/src/main/resources/application.properties`
  * Node App `fake-services-nodejs/.env`


## Run NodeJs Fake Services

Eerst de drie fake services starten voordat je deze applicatie opstart.

## Run Java Ticket Booking Service

If you want to understand the code, please have a look into this documentation: https://github.com/camunda/camunda-platform-get-started/tree/main/spring

```
mvn package exec:java -f booking-service-java/pom.xml
```

## Test

```
 curl -i -X PUT http://localhost:8080/ticket
```

Simulate failures by:

```
curl -i -X PUT http://localhost:8080/ticket?simulateBookingFailure=seats
curl -i -X PUT http://localhost:8080/ticket?simulateBookingFailure=ticket
```
