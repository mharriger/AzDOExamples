An example of dynamically setting parameter values in an Azure DevOps pipeline.

Because Azure DevOps requires parameter values to be set very early in pipeline
execution, before any tasks run, it is not possile to set the value of a
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
be named "second-pipeline" and must use the azure-pipelines-2.yml file.