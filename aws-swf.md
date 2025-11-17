AWS Simple Workflow Service (SWF) is a web service that helps in coordinating the execution of automated tasks and human tasks within a workflow. The processing steps can be execution of scripts and code, web service calls, human action etc.

SWF Workers and Deciders
Workers are programs that interact with SWF service to get the tasks, process the tasks and finally return the result back to the SWF service.

Deciders are programs that controls the coordination of tasks i.e. whether the tasks will run in parallel or sequential, ordering of the tasks and deciding the path in the workflow based on a certain logic.

AWS SWF orchestrates the interaction between the deciders and workers. It ensures that the task is executed only once and is not duplicated. Workers and deciders don’t have to keep the track of execution state since it’s maintained and managed by the SWF itself. Tasks can run and scale independently to meet the increased load on the system which might impact only a subset of tasks within a workflow.

SWF Domians
SWF resources should be grouped and scoped within a domain. There can be multiple workflows within a domain that can interact with each other, however workflows outside a domain cannot interact with each other. Domain helps in isolating tasks and execution process from unrelated workflows created in a different domain.

When registering a domain using the RegisterDomain action, there is a provision to define the workflow history retention period which determines the length of time SWF will retain the information about workflow execution.

Maximum workflow execution time is 1 year and at the most 100,000 open workflow executions are allowed within a domain.
