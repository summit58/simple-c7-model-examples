# Simple Camunda 7 Process Model Examples (for Trnsact)

This repository contains a small set of simple, example process models for use with Camunda 7. Each model illustrates a specific concept, and the repository contains a Postman collection (`Trnsact C7.postman_collection.json`) with individual requests that allow you to deploy each model, start instances and perform any other operations that may be necessary given each process model's implementation.

## Included Models

Currently, this repository contains the following example process models:

1. `simple-user-task-example.bpmn`: This model illustrates the use of User Tasks in Camunda 7. Note that it is designed for use with Camunda 7 Tasklist.
2. `simple-messaging-example.bpmn`: This illustrates the use of a Message Intermediate Catch Event in Camunda 7. Note that Receive Tasks will work in exactly the same manner, with the only exception being that it's possible to attach a Boundary Event to a Receive Task.
3. `simple-call-activity-example-main.bpmn`: In conjunction with the `simple-call-activity-example-sub.bpmn` model, this model illustrates the use of a Call Activity in a Camunda 7 model to call a subprocess model.
4. `simple-call-activity-example-sub.bpmn`: Please see the description for `simple-call-activity-example-main.bpmn` above.

## Running the Models

To run an instance of any model (or all models) in this project, please follow the instructions below:

1. Check out the code using your preferred Git client.
2. Import the included Postman collection into your local Postman installation. (For example, in version `9.29.0`, choose `File... Import` and then drag & drop your collection into the resulting screen.)
3. Ensure that your target instance of Camunda 7 is running. You can use the default download of Camunda 7 (which uses Tomcat), Camunda Run or a custom Spring Boot project; the included Postman requests will work with any standard Camunda 7 configuration. You will need to change the hostname and port in your requests, however, if the hostname and port aren't - respectively - `localhost` and `8080`.
4. Deploy your chosen model (or models) using the appropriate `Create Deployment...` Postman request. For instance, to deploy the `simple-messaging-example.bpmn` model, you would use `Create Deployment (simple-messaging-example)`. You will need to specify the location of the process model(s) before running the request.
5. Open the associated `Start Process Instance...` Postman request for your chosen model. At this point, the directions diverge and will be provided separately below.

### simple-user-task-example

6. When you open the `Start Process Instance (simple-user-task-example)` request, you'll see that it calls for a `variables/firstName/value` literal, which is defaulted to "John". Replace that value if desired and click `Send`.
7. (Optional) If you open Camunda 7 Cockpit to look at the running instance, you'll see that it's sitting at the `Provide a Surname` User Task activity and is waiting for a human to complete that task. We'll complete that User Task in the next step.
8. Open Camunda 7 Tasklist and navigate to your User Task. Typically, you would have an "All Tasks" list with all User Tasks in the system.Once you get to the User Task, claim it, provide a value for the surname (or accept the default of "Doe") and click the `Complete` button.
9. This will complete the instance, and it will no longer be visible in Cockpit in Camunda 7 Community Edition.

### simple-messaging-example

6. When you open the `Start Process Instance (simple-messaging-example)` request, you'll see that it calls for a `businessKey`. While this is a string literal that can have any value, that literal will be used to uniquely correlate a message you're going to send to the process instance in just a moment. Please be sure that it will be unique across the set of instances of `simple-messaging-example`. Once you've set the `businessKey`, click `Send`.
7. (Optional) If you open Camunda 7 Cockpit to look at the running instance, you'll see that it's sitting at the `Listen` Message Intermediate Catch Event step and is awaiting message "correlation", the Camunda term for sending a message to a specific process instance. We'll correlate that message in the next step.
8. Open the `Correlate Message (simple-messaging-example)` request, and change the `businessKey` (in the `Body` tab) to align with the `businessKey` you provided in step 6 above. Once you've done that, click `Send`.
9. This will complete the instance, and it will no longer be visible in Cockpit in Camunda 7 Community Edition.

### simple-call-activity-example (including both simple-call-activity-example-main and simple-call-activity-example-sub)

6. When you open the `Start Process Instance (simple-user-task-example)` request, you'll see that it doesn't call for any additional input. When you're ready, click `Send`.
7. Unlike the other example models, there are no intermediate steps, and - as a result - these process instances will be completed right away. You won't be able to see the information about the completed instances in Cockpit in Camunda 7 Community Edition, but you'll know they've completed successfully if you see the following two messages in your Camunda 7 log:

```Text
This subprocess has been called (initiated).
The subprocess has been called (initiated).
```

**NOTE**: To see the details for your completed process instances, you will need to connect to the relational database that Camunda 7 is using and use SQL statements to view the data.