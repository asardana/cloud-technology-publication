My previous post on Microservices discussed the benefits of decomposing monolithic applications into smaller services which could be deployed independently. We also discussed that microservices is an architectural style which promotes the applications to have a clear separation of concern a.k.a. bounded contexts and use the most appropriate technology stack. Once we embark on the microservices journey, we would want these services to be deployed on a cloud platform like Heroku, Cloud Foundry, Amazon Elastic Beanstalk etc.

Cloud Native Applications built using microservices and running on a cloud offers following benefits.

Speed – Traditionally any new application deployed in test, QA and production requires provisioning of the infrastructure which could be in days and sometimes can even take months. This severely limits the time to market for the product or feature being shipped. With cloud native applications running on PaaS (Platform as a Service), it takes few seconds to provision infrastructure and make the application running on the cloud environment.
Safety – Cloud native application architecture helps in isolating the fault in the distributed system. Metrics, alerts, log streams, monitoring etc. provides better visibility into the application state and automatic recovery.
Scale – Cloud Native applications running on PaaS can be easily scaled out horizontally during increased loads without any manual intervention.
We’ll now discuss the characteristics of cloud native applications that provides the benefits discussed above.

Twelve Factor Applications
Twelve factor app is a collection of 12 patterns documented by engineers at Heroku based on their experience of running the cloud native applications. Application adhering  to these 12 factors can be considered a cloud native application and could be deployed to any PaaS like cloud foundry, Amazon beanstalk etc.

Factor #1 – Single Code Base

One codebase tracked in revision control, many deploys

Application should have a single code base in a revision control system. The same code is deployed in multiple environments like dev, QA and production.

Factor #2 – Dependencies

Explicitly declare and isolate dependencies

Application should explicitly declare and isolate dependencies via. dependency management tools like Maven, Gradle etc.

Factor #3 – Config

Store configuration outside the build artifact

Application code should not contain any configuration that could differ based on the environment. Applications could get the configuration via. environment variables or config server.

Factor #4 – Backing Services

Treat backing services as attached resources

Backing services like databases, message brokers, cache etc. are treated as attached resources with connection properties injected into the application by the cloud platform.

Factor #5 – Build, Release, Run

Strictly separate build and run stages

Application should be build separately to generate the deployable artifact. The platform then takes this artifact and creates a release based on the environment configuration. Finally the platform runs the application processes.

Factor #6 – Processes

Execute the app as one or more stateless processes

Any data that needs to persist must be stored in a stateful backing service, typically a database.

Factor #7 – Port Binding

The twelve-factor app is completely self-contained

Platform automatically maps the actual host and port at which the application runs.

Factor #8 – Concurrency

Scale out via the process model

Concurrency can be accomplished by scaling out the processes horizontally.

Factor #9 – Disposability

Maximize robustness with fast startup and graceful shutdown

Applications running on cloud platform are considered ephemeral and hence it’s important for the processes to quickly start and recover from crashes gracefully.

Factor #10 – Dev/Prod Parity

Keep development, staging, and production as similar as possible

Cloud platform provides consistent dev/test and production environments enabling continuous deployments and speed to market.

Factor #11 – Logs

Treat logs as event streams

Cloud native application should not manage logs, instead should write to the standard output stream or error stream. Logs are streams of aggregated, time-ordered events which should be aggregated by the platform and shipped to a central repository for analysis.

Factor #12 – Admin Processes

Run admin/management tasks as one-off processes

Administrative tasks such as data migration scripts should be version controlled and executed as a process in the same environment in which the application is running.

The above 12 patterns or characteristics ensures that the cloud native application can be deployed with speed and scaled out easily as the applications are stateless. The cloud platform provides consistent environment where version controlled applications could be deployed safely and reliably.

View a summary on Cloud Native Applications and Microservices by clicking on following link.
