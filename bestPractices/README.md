# Best Practices

### Descriptions/Readme:
  * Make sure descriptions and readme are, you guessed it, descriptive.
      * Give an overview of the whole workflow process, which may entail multiple workflows
      * **IMPORTANT** Import order is very important for workflows. If a workflow calls a subflow, that subflow _must_ be in Reach Engine before importing the parent. It will fail to import otherwise
      * Descriptions:
          * User Inputs to the workflows
          * Are there any necessary system configurations that need to be added for the workflow to function?
          * Describe the workflows (typically starting with the parent and going down the chain to all of the workflows involved in the process)
      * The Flow of work, from once workflow to the next.

### Comments:
  * Just like any other traditional programming language, comments help anyone else who is looking at your code
  * \<!-- we are looking at this comment that is showing how to comment out a comment with the hopes that this comment accurately depicts a comment -->
  * very helpful when looping within the workflow \<!-- here is the start of a loop and it's conditions for when it will loop or exit --> _THELOOP_THATISLOOPING_ \<!-- here is the end of the loop and the conditions to loop or to exit -->

### Step Names:
    * Make Step names descriptive:
        * If a convertVideoStep is transcoding a proxy video file, name the step accordingly:
            * name="create proxy" or "generate proxy"

### Transitions:  
    * Transitioning using a noopStep vs transitions within a step:
        * A noopStep is its own step that moves the workflow to different steps based on a logical evaluation:
            * Example of a transition using a noopStep, see example 1 in workflowExamples
        * Transitioning within a step is _essentially_ like having a noopStep within the step that you are using
            * Example of a transition within a step, see example 2 in workflowExamples

### Sysconfig vs Hardcoding defaultDataDefs
##### Sysconfig:
    * Sysconfigs are much easier to maintain.
    * They can be set dynamically in the dynamic.reach-engine.properties file via API/cURL
    * Can be set in local.reach-engine.properties, but necessitates a restart of Reach Engine
    * Allows for easy visibility to what all of the defaultDataDefs are set to
        * Naming convention:
            * workflow.theParentWorkflowName/Process.specificAttribute=THE_VALUE
##### Hardcoding:
    * Hard to change.
    * Requires that the workflow be exported, changed, and then reimported.
    * DataDefs are not easily visible and changeable

### Dynamic vs Local Props files
    * Dynamic can be, surprise-surprise, dynamically updated when the Reach Engine Platform API is used
        * URL for Reach Engine API docs: http://docs.reachengineapi.apiary.io/#  (Go to Platform and look for Dynamic Properties, Get and Update are the options)
        * Can have _most_ values set dynamically (cron jobs, filesystem stuff won't be read until after a restart. Best practice is to put those items in local props)
    * Local Props is a good place to put things that _shouldn't_ change often, such as filesystem roots, cron workflow jobs, streaming urls, the system password, db config, etc.
    * Since it necessitates a restart for Reach Engine to read and apply changed values, it is a more stable and 'solid' place to put system configurations.

    * In workflow, the best place to put our sysconfigs is in dynamic props >>> see examples 3 and 4 in workflow examples
        * See updateDynamicPropsWithHeader for an example of how to format an API call to Update Dynamic Props in Postman
        * The body should be (in this case) a JSON that looks like:
          * {"workflow.myWorkflowName.mySysconfig":"THE_STRING_TO_PASS_TO_WORKFLOW"}

### Versioning (GitHub)
    * Any questions?
    * Here's our best practice (Start at fork for new repo or new clone)
      * NEW FORK
      * Fork (github.com/repo or github desktop GUI) -- go to github, find the repo, and click 'Fork' in the top right --> select your user --> copy and paste url
      * Clone (in command line/terminal) -- git clone <yourForkedUrl>
      * Add Upstream -- git remote add upstream <parentRepoUrl>
          * After merging with master, and if no changes to push, it is good practice to Fetch the most up-to-date master branch from the parentRepo
              * Fetch -- git fetch upstream
      * NEW BRANCH
      * Create a Branch --> git branch <nameForYourBranch>
      * Switch to that Branch --> git checkout <nameForYourBranch>
      * PUSH TO GITHUB
      * Add All Changes to 'Queue'--> git add . (or git add -A)
      * Add One Changed File to 'Queue' --> git add /path/to/file
      * Check Status --> git status
      * Commit Changes to Github --> git commit -m "A descriptive message about what is being pushed to github"
      * Push the Changes to Your Repo and Branch --> git push origin <nameForYourBranch> (git push origin master    will push to the master branch)
      * Make A Pull Request --> Back in github, create a pull request, check for merge conflicts, and then, if you have write permissions to the repo, confirm the pull request or ask someone who does.

### Logging
    * Test steps --> Prints to the Reach-Engine.logs (relog or tailre to get to the logs)
    * Execution Labels --> Shows up in the UI when looking at the Status page in Access
    * println --> Prints to the Catalina.logs (tailcat to get to the logs or /reachengine/tomcat/logs)
    * log4j (same as test steps, log4j config properties can be found in /reachengine/tomcat/lib/log4j.properties, and you can turn logging up or down there)


## Questions?????

### Resources:
* http://docs.reachengineapi.apiary.io/#
* Workflow Function Guide, Workflow For Programmers, Workflow Step Reference
* 
