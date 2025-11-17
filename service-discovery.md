In microservices architecture, we tend to create lot of services which are loosely coupled from each other. Inherently this distributed system design has more inter-service communication and hence each service running in cloud should be able to locate other dependent services easily.

Service discovery in the past has been done using service locators or configured within the application itself. It is challenging to configure all the service dependencies in a microservice architecture and hence there is a need for more dynamic and automatic service discovery pattern.

Spring Cloud Eureka allows the clients to register themselves to the Eureka Server and also helps the clients to discover other services running in the cloud.Typically there are multiple instances of Eureka Server running in different availability zones to provide high availability.

Below diagram shows the sample MyOnlineBank having Eureka Server which allows the Account Service to discover one of it’s dependent service to fetch transaction history records.

service-discovery

The source code for the sample application MyOnlineBank can be found here on GitHub.

Few important highlights-

AccountTransactionHistoryService

AccountTransactionHistoryService is a simple Spring Boot Application that has a dependency on “spring-cloud-starter-eureka-server” which allows it to register itself with the Eureka Server.

AccountTransactionHistoryServiceApplication.java is annotated with @EnableDiscoveryClient which wraps the functionality to register the client with Eureka Server.

Account Service

Account Service has a dependency on “spring-cloud-starter-eureka-server” which allows it to register itself with the service registry and discover other services running in the cloud.

Account Service fetches the list of Service Instances using the application name of the service configured in bootstrap.yml. It then iterates over this list of service instances using java streams to fetch the transaction history service URI.

List serviceIntanceList = discoveryClient.getInstances(“transactionhistory”);

String txnHistoryURI = serviceIntanceList.stream().findFirst().get().getUri().toString();

MyOnlineBankServiceRegistry

MyOnlineBankServiceRegistry is also a Spring Boot application with a dependency on “spring-cloud-starter-eureka-server”. It is annotated with @EnableEurekaServer which wraps the Eukera Server implementation.

Services will register with Eureka Server as soon as they are started and will keep on sending the heartbeats periodically to let Eureka Server know that they are still active and available to service the client.

Below is a screenshot of the Eukera Server endpoint http://localhost:8812/

eureka-dashboard
