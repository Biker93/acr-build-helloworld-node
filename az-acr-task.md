# az acr task --help

Group
    az acr task : Manage a collection of steps for building, testing and OS & Framework patching
    container images using Azure Container Registries.

Subgroups:
    credential : Manage credentials for a task. Please see https://aka.ms/acr/tasks/cross-registry-
                 authentication for more information.
    identity   : Managed Identities for Task. Please see https://aka.ms/acr/tasks/task-create-
                 managed-identity for more information.
    timer      : Manage timer triggers for a task.

Commands:
    cancel-run : Cancel a specified run of an Azure Container Registry.
    create     : Create a series of steps for building, testing and OS & Framework patching
                 containers. Tasks support triggers from git commits and base image updates.
    delete     : Delete a task from an Azure Container Registry.
    list       : List the tasks for an Azure Container Registry.
    list-runs  : List all of the executed runs for an Azure Container Registry, with the ability to
                 filter by a specific Task.
    logs       : Show logs for a particular run. If no run-id is supplied, show logs for the last
                 created run.
    run        : Manually trigger a task that might otherwise be waiting for git commits or base
                 image update triggers.
    show       : Get the properties of a named task for an Azure Container Registry.
    show-run   : Get the properties of a specified run of an Azure Container Registry Task.
    update     : Update a task for an Azure Container Registry.
    update-run : Patch the run properties of an Azure Container Registry Task.

For more specific examples, use: az find "az acr task"





az acr task list --registry $ACR_NAME
az acr task list-runs --registry $ACR_NAME


