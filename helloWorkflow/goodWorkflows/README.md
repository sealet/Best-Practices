# Training Hello Workflow README

## Workflow Overview
These workflows are designed to show different ways that variables can be set, changed, passed down to subflows, and returned to parent workflows. They also show some of the different ways to print out values of dataDefs to different places (Reach Engine Logs, Catalina Logs, and execution label expressions). The end result will be transform a string to equal 'Hello World'. If 'Hello World' is not the resulting string of the workflow, the workflow will fail.

## Import Order (The workflows that start with 'bad' are just bad formatting. They will accomplish the same goal, but they do not adhere to best practices)
1. helloWorldWorkflow
2. helloWorkflow


### Descriptions
#### User Input
* User adds a string to "Hello Workflow Input:" field on the user input form and selects either yes (true) or no (false) from "Allow Subflow Processing:". "Hello Workflow Input" has a default value built into the workflow, but will be overwritten by anything the user adds in. "Allow Subflow Processing" has a default value of true.

#### Result
* These workflows process the inputs given by the user input form to equal "Hello World", as long as allowSubflowProcessing == true

#### Sysconfig
* Here are the necessary system configurations for the workflows:
    * workflow.defaultUserHello=Hello Workflow
    * workflow.groovyHello=Groovy Hello

### helloWorkflow (parent workflow)
#### Initial step:
* checks the value of userHello
    * If userHello == "Hello Workflow", go to 'set hellowWorkflow' step
    * Else, go to 'groovy hello' step

* 'set helloWorkflow'
    * sets 'helloWorkflow' variable to the value of 'userHello'
      * go to 'test hello'
* 'groovy hello'
    * sets 'helloWorkflow' variable to the value of 'groovyHello'
      * go to 'test hello'

* 'test hello' is hit by both of the above steps and prints out the value of 'userHello' and 'helloWorkflow' to the Reach Engine Logs
    * go to 'hello workflow to hello world'

* 'hello workflow to hello world' sends the value of 'helloWorkflow' and 'allowSubflowProcessing' to a subflow, "helloWorldWorkflow"
    * returns the outcome of the subflow to the data def 'helloWorld'
      * go to 'set success or failure'

* 'set success or failure' sets the value of a boolean-type variable, 'success', based on whether the value of 'helloWorld' is equal to 'Hello World'
    * If 'success' == true  
        * go to 'we won' and then end
    * Else, go to 'we lost' which is a failWorkflowStep


### helloWorldWorkflow
#### Initial Step:
* checks value of allowSubflowProcessing
    * If allowSubflowProcessing == true, go to 'set helloWorld'
    * Else, go to end

* uses the value of the string, 'helloWorkflow'(which will either be 'Groovy Hello' or 'Hello Workflow'), to parse the string to 'Hello World' within a groovy step

* When 'helloWorldWorkflow' completes, it will return the value of helloWorld to the parent workflow, 'helloWorkflow'.


### Flow:
1) 'helloWorkflow' -->
2) 'helloWorldWorkflow' --> passes the resultDataDef back to 'helloWorkflow'
