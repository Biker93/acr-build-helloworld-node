=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Azure Container Registry tutorials

https://docs.microsoft.com/en-us/azure/container-registry/container-registry-intro
https://docs.microsoft.com/en-us/azure/security-center/container-security
https://docs.microsoft.com/en-us/azure/aks/operator-best-practices-container-image-management#automatically-build-new-images-on-base-image-update


Tutorial
https://docs.microsoft.com/en-us/azure/container-registry/container-registry-tutorial-quick-task

Fork:  https://github.com/Azure-Samples/acr-build-helloworld-node
Clone: https://github.com/Biker93/acr-build-helloworld-node

Add extensions to basic Azure CLI:
https://docs.microsoft.com/en-us/cli/azure/azure-cli-extensions-overview
https://docs.microsoft.com/en-us/cli/azure/azure-cli-extensions-list

az extension --help
Group
    az extension : Manage and update CLI extensions.
Commands:
    add            : Add an extension.
    list           : List the installed extensions.
    list-available : List publicly available extensions.
    list-versions  : List available versions for an extension.
    remove         : Remove an extension.
    show           : Show an extension.
    update         : Update an extension.
For more specific examples, use: az find "az extension"

az extension list
[]

az version ... to find the version and dependent libraries that are installed. 
az upgrade ... To upgrade to the latest version, run 
  You already have the latest azure-cli version: 2.26.1

https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-azure-cli
https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-portal

https://docs.microsoft.com/en-us/azure/container-registry/container-registry-intro
https://docs.microsoft.com/en-us/azure/container-registry/container-registry-skus
Basic $5/mo Standard $20/mo Premium $50/mo

https://azure.microsoft.com/en-us/services/devops/
https://azure.microsoft.com/en-us/pricing/details/devops/azure-devops-services/
Under the MSDN account User unique identifier 10033FFF82D0F231 ... this might be connected to AIR
https://portal.office.com/account/?ref=MeControl
Kip Thomson Senior Cloud Architect, Information Technology
https://www.office.com/?auth=2&home=1

Azure DevOps
https://dev.azure.com/American-Institutes-for-Research/
https://kipthomson.visualstudio.com/
https://dev.azure.com/American-Institutes-for-Research/_settings/billing
MS hosted: 1800 min, self host unlimited, 5 Basic users, 2GB Arifacts
https://docs.microsoft.com/en-us/azure/devops/organizations/billing/billing-faq?view=azure-devops&tabs=new-nav&viewFallbackFrom=vsts

https://docs.microsoft.com/en-us/azure/container-registry/container-registry-tutorial-build-task

https://docs.microsoft.com/en-us/azure/container-registry/container-registry-tutorial-base-image-update


(I first used portal for this ... and kept it Basic)

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Tutorial: Build and deploy container images in the cloud with Azure Container Registry Tasks

1. https://docs.microsoft.com/en-us/azure/container-registry/container-registry-tutorial-quick-task 

Fork this repository: https://github.com/Azure-Samples/acr-build-helloworld-node
clone git clone https://github.com/<your-github-username>/acr-build-helloworld-node
cd acr-build-helloworld-node
🔥 acr-build-helloworld-node (main) 💍 

*** Docker must be running ***

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Create ACR Registry

ACR_NAME=<registry-name>
ACR_NAME=amtacr
RES_GROUP=$ACR_NAME-rg # Resource Group name

az group create --resource-group $RES_GROUP --location eastus
az acr create --resource-group $RES_GROUP --name $ACR_NAME --sku Standard --location eastu

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# ACR Tasks
Build, Run, Push and Patch containers in Azure with ACR Tasks. Tasks supports Windows, Linux and ARM with QEMU.


=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Build in Azure with ACR Tasks

az acr login -n amtacr
Login Succeeded

az acr build --registry $ACR_NAME --image helloacrtasks:v1 .
...

2021/07/24 00:56:10 Successfully pushed image: amtacr.azurecr.io/helloacrtasks:v1
...
2021/07/24 00:56:12 The following dependencies were found:
- image:
    registry: amtacr.azurecr.io
    repository: helloacrtasks
    tag: v1
    digest: sha256:420b01dc9867c836315b26add7af3b8c21c79af1c6ae2c38627aa2428df804a2
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 15-alpine
    digest: sha256:6edd37368174c15d4cc59395ca2643be8e2a1c9846714bc92c5f5c5a92fb8929
  git: {}
Run ID: ca1 was successful after 31s


Near the end of the output, ACR Tasks displays the dependencies it's discovered for your image. This enables ACR Tasks to automate image builds on base image updates, such as when a base image is updated with OS or framework patches. You learn about ACR Tasks support for base image updates later in this tutorial series.


=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Deploy to Azure Container Instances

# Configure registry authentication
AKV_NAME=$ACR_NAME-vault
az keyvault create --resource-group $RES_GROUP --name $AKV_NAME
{
...
  "type": "Microsoft.KeyVault/vaults"
}



=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Create a service principal and store credentials
Use the az ad sp create-for-rbac command to create the service principal, and az keyvault secret set to store the service principal's password in the vault. 
https://docs.microsoft.com/en-us/cli/azure/ad/sp?view=azure-cli-latest#az_ad_sp_create_for_rbac

# Create service principal, store its password in AKV (the registry *password*)

az keyvault secret set \
  --vault-name $AKV_NAME \
  --name $ACR_NAME-pull-pwd \
  --value $(az ad sp create-for-rbac \
                --name $ACR_NAME-pull \
                --scopes $(az acr show --name $ACR_NAME --query id --output tsv) \
                --role acrpull \
                --query password \
                --output tsv)

showing each step results:

1. ----------------------
az acr show --name $ACR_NAME --query id --output tsv
/subscriptions/cf71d4cd-095a-47ec-bca0-060c571abedf/resourceGroups/amtacr-rg/providers/Microsoft.ContainerRegistry/registries/amtacr

2. ----------------------
az ad sp create-for-rbac \
                --name $ACR_NAME-pull \
                --scopes $(az acr show --name $ACR_NAME --query id --output tsv) \
                --role acrpull \
                --query password \
                --output tsv
Found an existing application instance of "2a2955f3-4248-43af-812b-1c3563d3b578". We will patch it
Creating 'acrpull' role assignment under scope '/subscriptions/cf71d4cd-095a-47ec-bca0-060c571abedf/resourceGroups/amtacr-rg/providers/Microsoft.ContainerRegistry/registries/amtacr'
  Role assignment already exists.

The output includes credentials that you must protect. Be sure that you do not include these credentials in your code or check the credentials into your source control. For more information, see https://aka.ms/azadsp-cli
'name' property in the output is deprecated and will be removed in the future. Use 'appId' instead.
1A.BS1gnAR5a2Vd3G_i4hcWJfG5XPn6Pc5

3. ---------------------------
az keyvault secret set \
  --vault-name $AKV_NAME \
  --name $ACR_NAME-pull-pwd \
  --value 1A.BS1gnAR5a2Vd3G_i4hcWJfG5XPn6Pc5
{
  "attributes": {
    "created": "2021-07-24T01:50:35+00:00",
    "enabled": true,
    "expires": null,
    "notBefore": null,
    "recoveryLevel": "Recoverable+Purgeable",
    "updated": "2021-07-24T01:50:35+00:00"
  },
  "contentType": null,
  "id": "https://amtacr-vault.vault.azure.net/secrets/amtacr-pull-pwd/1f4b7fefdaa44cca9c56a37cb5c52aea",
  "kid": null,
  "managed": null,
  "name": "amtacr-pull-pwd",
  "tags": {
    "file-encoding": "utf-8"
  },
  "value": "1A.BS1gnAR5a2Vd3G_i4hcWJfG5XPn6Pc5"
}

check on the portal:

amtacr-vault / Secret / amtacr-pull-pwd
CURRENT VERSION 1f4b7fefdaa44cca9c56a37cb5c52aea

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Store service principal ID in AKV (the registry *username*)

az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp list --display-name $ACR_NAME-pull --query [].appId --output tsv)

1. az ad sp list --display-name $ACR_NAME-pull
...
    "appDisplayName": "amtacr-pull",
    "appId": "2a2955f3-4248-43af-812b-1c3563d3b578",
    "appOwnerTenantId": "e53e4689-2ff4-46b3-a6cb-af78a8dfb38a",
...

2. az ad sp list --display-name $ACR_NAME-pull --query [].appId --output tsv
zsh: no matches found: [].appId
try this in bash

bash
The default interactive shell is now zsh.
To update your account to use zsh, please run `chsh -s /bin/zsh`.
For more details, please visit https://support.apple.com/kb/HT208050.
MacBook-Pro:Sites kthomson$

ACR_NAME=amtacr
RES_GROUP=$ACR_NAME-rg
AKV_NAME=$ACR_NAME-vault
echo $ACR_NAME $RES_GROUP $AKV_NAME

az ad sp list --display-name $ACR_NAME-pull --query [].appId --output tsv
2a2955f3-4248-43af-812b-1c3563d3b578

*** so zsh does not work ... must be the --query ***
    --query                  : JMESPath query string. See http://jmespath.org/ for more information
                               and examples.

3. 
az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp list --display-name $ACR_NAME-pull --query [].appId --output tsv)


=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Deploy a container with Azure CLI
Now that the service principal credentials are stored as Azure Key Vault secrets, your applications and services can use them to access your private registry.

az container create \
    --resource-group $RES_GROUP \
    --name acr-tasks \
    --image $ACR_NAME.azurecr.io/helloacrtasks:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --registry-username $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-pwd --query value -o tsv) \
    --dns-name-label acr-tasks-$ACR_NAME \
    --query "{FQDN:ipAddress.fqdn}" \
    --output table

FQDN
-----------------------------------------
acr-tasks-amtacr.eastus.azurecontainer.io





=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Verify the deployment

az container attach --resource-group $RES_GROUP --name acr-tasks


=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Clean up resources

az container delete --resource-group $RES_GROUP --name acr-tasks
Are you sure you want to perform this operation? (y/n): y
{
  "containers": [
...
   "dnsNameLabel": "acr-tasks-amtacr",
    "fqdn": "acr-tasks-amtacr.eastus.azurecontainer.io",
    "ip": "52.191.95.182",
    "ports": [

}

To remove all resources you've created in this tutorial, including the container registry, key vault, and service principal, issue the following commands. These resources are used in the next tutorial in the series, however, so you might want to keep them if you move on directly to the next tutorial.

az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull


=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Automate container image builds in the cloud when you commit source code
https://docs.microsoft.com/en-us/azure/container-registry/container-registry-tutorial-build-task

Currently, ACR Tasks doesn't support commit or pull request triggers in GitHub Enterprise repos.

https://github.com/settings/tokens/new
Personal access tokens function like ordinary OAuth access tokens. They can be used instead of a password for Git over HTTPS, or can be used to authenticate to the API over Basic Authentication.
  Azure az acr trigger tutorial
  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

ACR_NAME=<registry-name>        # The name of your Azure container registry
GIT_USER=<github-username>      # Your GitHub user account name
GIT_PAT=<personal-access-token> # The PAT you generated in the previous section

ACR_NAME=amtacr
RES_GROUP=$ACR_NAME-rg
AKV_NAME=$ACR_NAME-vault
GIT_USER=biker93
GIT_PAT=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
echo $ACR_NAME $RES_GROUP $AKV_NAME $GIT_USER $GIT_PAT


az acr task create \
    --registry $ACR_NAME \
    --name taskhelloworld \
    --image helloworld:{{.Run.ID}} \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git#main \
    --file Dockerfile \
    --git-access-token $GIT_PAT

az acr task run --registry $ACR_NAME --name taskhelloworld

cd /Users/kthomson/Classes/ACR/acr-build-helloworld-node
echo "Hello World! " > hello.txt
git add hello.txt
git commit -m "Testing ACR Tasks"
git push origin master

az acr task logs --registry $ACR_NAME
Could not find the last created run.
*** no ***

az acr task list-runs --registry $ACR_NAME --output table
RUN ID    TASK            PLATFORM    STATUS     TRIGGER    STARTED               DURATION
--------  --------------  ----------  ---------  ---------  --------------------  ----------
ca3       taskhelloworld  linux       Succeeded  Manual     2021-07-24T09:22:26Z  00:00:18
ca2       taskhelloworld  linux       Succeeded  Manual     2021-07-24T09:11:04Z  00:00:28
ca1                       linux       Succeeded  Manual     2021-07-24T00:55:41Z  00:00:31

az acr task logs --registry $ACR_NAME --run-id ca3
*** works ***

git push  origin main

az acr task list-runs --registry $ACR_NAME --output table
RUN ID    TASK            PLATFORM    STATUS     TRIGGER    STARTED               DURATION
--------  --------------  ----------  ---------  ---------  --------------------  ----------
ca5       taskhelloworld  linux       Running    Commit     2021-07-24T09:31:19Z
ca4       taskhelloworld  linux       Succeeded  Manual     2021-07-24T09:29:35Z  00:00:24
ca3       taskhelloworld  linux       Succeeded  Manual     2021-07-24T09:22:26Z  00:00:18
ca2       taskhelloworld  linux       Succeeded  Manual     2021-07-24T09:11:04Z  00:00:28
ca1                       linux       Succeeded  Manual     2021-07-24T00:55:41Z  00:00:31
MacBook-Pro:acr-build-helloworld-node kthomson$



=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Automate container image builds when a base image is updated 
https://docs.microsoft.com/en-us/azure/container-registry/container-registry-tutorial-base-image-update

az acr build --registry $ACR_NAME --image baseimages/node:15-alpine --file Dockerfile-base .


new task to trigger on base image change
az acr task create \
    --registry $ACR_NAME \
    --name baseexample1 \
    --image helloworld:{{.Run.ID}} \
    --arg REGISTRY_NAME=$ACR_NAME.azurecr.io \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git#main \
    --file Dockerfile-app \
    --git-access-token $GIT_PAT

Operation returned an invalid status 'Conflict'



edit Dockerfile-base and add "a"
ENV NODE_VERSION 15.2.1a

then rebuild base image
az acr build --registry $ACR_NAME --image baseimages/node:15-alpine --file Dockerfile-base .

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# 

ƒ
