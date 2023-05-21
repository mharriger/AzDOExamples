An example of dynamically setting parameter values in an Azure DevOps pipeline.

Because Azure DevOps requires parameter values to be set very early in pipeline
execution, before any tasks run, it is not possible to set the value of a
parameter based on the output of a task. It is not possible to run a single
pipeline without human intervention, using values that are not known until the
time of the pipeline run. There is a workaround: use one pipeline to determine
the parameter value(s) and trigger a second pipeline, which consumes the
dynamically generated parameter values.

In this example, the first pipeline uses a PowerShell task to find all of the
files in a folder. It then passes a list of those filenames as a parameter to a
second pipeline, which loops over the filenames in that list and runs a
PowerShell task for each of those files, printing out the contents of the file.
The looping takes places within the pipeline defintion, not inside the
PowerShell task.

To try this example yourself, create two pipelines in Azure DevOps. The first
pipeline should use the azure-pipelines-1.yml file. The second pipeline must
be named "second-pipeline" and must use the azure-pipelines-2.yml file. You
must also add the
[Trigger Build Task](https://marketplace.visualstudio.com/items?itemName=benjhuser.tfs-extensions-build-tasks)
extension to your Azure DevOps organization.

In order for the first pipeline to trigger the second, you need to grant the
"Queue builds" permission on second-pipeline to your project's build service.
This can be done from the Manage security option in the context menu for
second-pipeline. See the screenshots below for details on where to set this
pernmission.

![Screenshot of the Azure DevOps build pipelines screen, with the context menu
open for the pipeline named second-pipeline. The Manage security item is
highlighted](documentation/screenshots/pipeline-security-1.PNG)
![Screenshot of the Permissions for second-pipeline window in Azure DevOps,
The build service user is selected on the left-hand side of the window, and
the Queue builds permission drop-down is set to Allow.](documentation/screenshots/pipeline-security-2.PNG)

To demonstrate that the pipeline really does pick up the list of files
dynamically, you can fork this repo and set up the pipelines against your fork.
Then, add some additional files to the Files folder and re-run the pipeline.
The contents of those additional files will be printed when second-pipeline
runs.