In my previous post on Cloud Native Applications, we discussed the benefits of 12 factor application and the characteristics that makes the application cloud ready. Once of the characteristics we discussed was to store the configuration outside the build artifact.

Now let’s see how we can leverage Spring Cloud Configuration server to store and inject the configuration that is maintained outside the cloud native application.

Spring cloud configuration provides a centralized configuration for distributed applications deployed on the cloud. Any configuration change in the environment can be version controlled to keep the track of changes and for audit requirements.

One of the alternatives to spring cloud configuration server could be to store the configuration in the environment variables, however with large number of configuration variables it might not be easy to maintain the application and also the configuration cannot be changed on the fly since it requires the web server restart to refresh the configuration.

Below diagram shows a sample Online Bank Config Server which pulls the configuration from Git by polling it periodically. As soon as the changes are committed and pushed to Git repository, config server will pull the most recent configuration for the application. The changes won’t be available to all the instances of the application running in the cloud until the configuration is refreshed by sending a refresh POST request to the cloud bus. There could be multiple instances of application running in the cloud and it would be difficult for someone to call the refresh endpoint for each of the instances. If the application is running on Cloud Foundry, we won’t have access to the endpoints for each of the instances since all the traffic is routed via the Cloud Foundry Router.

Here we are using cloud bus (messaging queue) to send the refresh message to all the application instances after the refresh request is received by one of the instances.

config-server The source code for the sample application MyOnlineBank can be found here on GitHub. Since the sample application is running locally and not on cloud foundry, there is a single instance of the Account Service refreshed directly without the Cloud Bus implementation. If the application is running on the cloud foundry with multiple instances, we can create a RabbitMQ service and bind it to each of the Account Service instances.Account Service will also require a dependency on Cloud Bus AMQP to send and receive messages on the Cloud Bus.

Few important highlights-

AccountService

AccountService is a Spring Boot application created with a dependency on Spring Cloud Starter Config. It has a REST endpoint (http://localhost:8811/accountservice/accounts) to fetch the account details with account description being fetched from external configuration. ExternalConfigProperties class holds the external configuration and is annotated with  @ConfigurationProperties which would refresh the bean with the most recent configuration from the config server when the POST request is sent to the refresh endpoint.

bootstrap.yml file provides the application name and the location of the config server.

The endpoint for the account service is http://localhost:8811/accountservice/accounts

MyOnlineBankConfigServer

MyOnlineBankConfigServer is a Spring Boot application created with a dependency on Spring Cloud Config Server. MyOnlineBankConfigServerApplication class is annotated with @EnableConfigServer which wraps the config server implementation.

application.yml file provides the location of the Git repository to the config server.

MyOnlineBankConfiguration

MyOnlineBankConfiguration is the Git folder that stores the configuration for the application.

accountservice-dev.yml stores the configuration which would be pulled by the config server. Since the repository could store the configuration for multiple environments like dev, QA, production etc., the name of the yml file should be the application name (defined in bootstrap.yml) and followed by the profile name.
