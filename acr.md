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
# Create ACR Registry
ACR_NAME=<registry-name>
ACR_NAME=amtacr
RES_GROUP=$ACR_NAME-rg # Resource Group name
az group create --resource-group $RES_GROUP --location eastus
az acr create --resource-group $RES_GROUP --name $ACR_NAME --sku Standard --location eastu
done ...

ACR Tasks
Build, Run, Push and Patch containers in Azure with ACR Tasks. Tasks supports Windows, Linux and ARM with QEMU.

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Docker was not running ...
az acr build --registry $ACR_NAME --image helloacrtasks:v1 .
The resource with name 'amtacr' and type 'Microsoft.ContainerRegistry/registries' could not be found in subscription 'Visual Studio Enterprise (cf71d4cd-095a-47ec-bca0-060c571abedf)'.

az acr login -n amtacr
You may want to use 'az acr login -n amtacr --expose-token' to get an access token, which does not require Docker to be installed.
An error occurred: DOCKER_COMMAND_ERROR
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

🔥 acr-build-helloworld-node (master) 💍 az acr login -n amtacr --expose-token
You can perform manual login using the provided access token below, for example: 'docker login loginServer -u 00000000-0000-0000-0000-000000000000 -p accessToken'
{
  "accessToken": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6IkVERFE6SFVYWDpMQzQ3OlpCUk06T0k0WTpPUjY2OkFVWko6Qlk3RTo3N0pWOjU0UlI6UU1BSzpOTDI1In0.eyJqdGkiOiJhMzZiNGRlZi0wYzY5LTRiOGMtOGZmMi04MWFhODIwNGMwY2YiLCJzdWIiOiJsaXZlLmNvbSNrdGhvbXNvbkBhaXIub3JnIiwibmJmIjoxNjI3MDM3MjAzLCJleHAiOjE2MjcwNDg5MDMsImlhdCI6MTYyNzAzNzIwMywiaXNzIjoiQXp1cmUgQ29udGFpbmVyIFJlZ2lzdHJ5IiwiYXVkIjoiYW10YWNyLmF6dXJlY3IuaW8iLCJ2ZXJzaW9uIjoiMS4wIiwicmlkIjoiOGJmODg5ZDRmNTlhNDQwNTg2MWZjZmViNjI2N2MwZDQiLCJncmFudF90eXBlIjoicmVmcmVzaF90b2tlbiIsImFwcGlkIjoiMDRiMDc3OTUtOGRkYi00NjFhLWJiZWUtMDJmOWUxYmY3YjQ2IiwidGVuYW50IjoiZTUzZTQ2ODktMmZmNC00NmIzLWE2Y2ItYWY3OGE4ZGZiMzhhIiwicGVybWlzc2lvbnMiOnsiYWN0aW9ucyI6WyJyZWFkIiwid3JpdGUiLCJkZWxldGUiXSwibm90QWN0aW9ucyI6bnVsbH0sInJvbGVzIjpbXX0.Bm4Ob09oP5sNA-ryjwaD7zSrMUHXd_PfMZgL5LgbwV8VMhhrh0Oqpzv8PZfBY2oreRlE3AL59uTIMwa-_cTc3QYuhhnT2UkpT2npWt_JYrAkWWDkduq-P8SqOkf2vg7Hug9exqpN9k6DL0iHZbjYagCMaNGtvgX2DaOUYcCwTRjac-hHO7Hxuf1aut12A4jfa_fIa7B1tiwrhJ8Ne0GclCXoIux7NRt8qlMP0E_aU82c0YnUkMH93RA2MWRElufsNmVDZUIkQpDsjeuimhxRqWWVi6vD-qsk-ITiUqcFnskepCUnIAAnxLgVmi91CpeOs4h6tYuv_UiTjLCHA76Wlw",
  "loginServer": "amtacr.azurecr.io"
}

Docker Desktop/Engine not running ... started up ....

=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# 
az acr login -n amtacr
Login Succeeded

🔥 acr-build-helloworld-node (master) 💍 

az acr build --registry $ACR_NAME --image helloacrtasks:v1 .

Packing source code into tar to upload...
Excluding '.git' based on default ignore rules
Uploading archived source code from '/var/folders/jk/mh2r3tn51kq05cbf65y8n3zw0000gp/T/build_archive_351c5094615242788369e20062dd1f0b.tar.gz'...
Sending context (7.115 KiB) to registry: amtacr...
Queued a build with ID: ca1
Waiting for an agent...
2021/07/24 00:55:42 Downloading source code...
2021/07/24 00:55:43 Finished downloading source code
2021/07/24 00:55:44 Using acb_vol_9a3ce203-ecf6-4dd0-8030-4c7a31b8994c as the home volume
2021/07/24 00:55:44 Setting up Docker configuration...
2021/07/24 00:55:45 Successfully set up Docker configuration

2021/07/24 00:55:45 Logging in to registry: amtacr.azurecr.io
2021/07/24 00:55:46 Successfully logged into amtacr.azurecr.io
2021/07/24 00:55:46 Executing step ID: build. Timeout(sec): 28800, Working directory: '', Network: ''
2021/07/24 00:55:46 Scanning for dependencies...
2021/07/24 00:55:46 Successfully scanned dependencies
2021/07/24 00:55:46 Launching container with name: build
Sending build context to Docker daemon  27.14kB

Step 1/5 : FROM node:15-alpine
15-alpine: Pulling from library/node
ddad3d7c1e96: Already exists
0e18143e8d4d: Pulling fs layer
377ad682a98b: Pulling fs layer
99b3e0ba5237: Pulling fs layer
99b3e0ba5237: Verifying Checksum
99b3e0ba5237: Download complete
377ad682a98b: Download complete
0e18143e8d4d: Verifying Checksum
0e18143e8d4d: Download complete
0e18143e8d4d: Pull complete
377ad682a98b: Pull complete
99b3e0ba5237: Pull complete
Digest: sha256:6edd37368174c15d4cc59395ca2643be8e2a1c9846714bc92c5f5c5a92fb8929
Status: Downloaded newer image for node:15-alpine
 ---> 75631da67663
Step 2/5 : COPY . /src
 ---> 30199872b732
Step 3/5 : RUN cd /src && npm install
 ---> Running in cf815768e576

up to date, audited 1 package in 496ms

found 0 vulnerabilities

npm notice
npm notice New minor version of npm available! 7.7.6 -> 7.20.1
npm notice Changelog: <https://github.com/npm/cli/releases/tag/v7.20.1>
npm notice Run `npm install -g npm@7.20.1` to update!
npm notice
Removing intermediate container cf815768e576
 ---> c3c2ece75a39
Step 4/5 : EXPOSE 80
 ---> Running in 1e1ac39e5f4f
Removing intermediate container 1e1ac39e5f4f
 ---> 55e6d1a0f0af
Step 5/5 : CMD ["node", "/src/server.js"]
 ---> Running in 68a2da24ed73
Removing intermediate container 68a2da24ed73
 ---> 795cbac7ce92
Successfully built 795cbac7ce92
Successfully tagged amtacr.azurecr.io/helloacrtasks:v1
2021/07/24 00:55:55 Successfully executed container: build
2021/07/24 00:55:55 Executing step ID: push. Timeout(sec): 3600, Working directory: '', Network: ''
2021/07/24 00:55:55 Pushing image: amtacr.azurecr.io/helloacrtasks:v1, attempt 1
The push refers to repository [amtacr.azurecr.io/helloacrtasks]
20a3d5e672eb: Preparing
31cbdcdfd046: Preparing
92db18d3c4c1: Preparing
cfd365fa5138: Preparing
af7c3dff418f: Preparing
9a5d14f9f550: Preparing
9a5d14f9f550: Waiting
20a3d5e672eb: Pushed
31cbdcdfd046: Pushed
92db18d3c4c1: Pushed
cfd365fa5138: Pushed
9a5d14f9f550: Pushed
af7c3dff418f: Pushed
v1: digest: sha256:420b01dc9867c836315b26add7af3b8c21c79af1c6ae2c38627aa2428df804a2 size: 1576
2021/07/24 00:56:10 Successfully pushed image: amtacr.azurecr.io/helloacrtasks:v1

2021/07/24 00:56:10 Step ID: build marked as successful (elapsed time in seconds: 9.232443)
2021/07/24 00:56:10 Populating digests for step ID: build...
2021/07/24 00:56:12 Successfully populated digests for step ID: build
2021/07/24 00:56:12 Step ID: push marked as successful (elapsed time in seconds: 15.097792)

2021/07/24 00:56:12 The following dependencies were found:
2021/07/24 00:56:12
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
🥃 acr-build-helloworld-node (master) 💍

Near the end of the output, ACR Tasks displays the dependencies it's discovered for your image. This enables ACR Tasks to automate image builds on base image updates, such as when a base image is updated with OS or framework patches. You learn about ACR Tasks support for base image updates later in this tutorial series.


=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# 
az keyvault create --resource-group $RES_GROUP --name $AKV_NAME
{
  "id": "/subscriptions/cf71d4cd-095a-47ec-bca0-060c571abedf/resourceGroups/amtacr-rg/providers/Microsoft.KeyVault/vaults/amtacr-vault",
  "location": "eastus",
  "name": "amtacr-vault",
  "properties": {
    "accessPolicies": [
      {
        "applicationId": null,
        "objectId": "7ecd05dd-21d3-4b95-a1f0-7f7fb91dd4fe",
...
    "provisioningState": "Succeeded",
    "sku": {
      "family": "A",
      "name": "standard"
    },
    "softDeleteRetentionInDays": 90,
    "tenantId": "e53e4689-2ff4-46b3-a6cb-af78a8dfb38a",
    "vaultUri": "https://amtacr-vault.vault.azure.net/"
  },
  "resourceGroup": "amtacr-rg",
  "systemData": {
    "createdAt": "1970-01-19T19:58:08.506000+00:00",
    "createdBy": "kthomson@air.org",
    "createdByType": "User",
    "lastModifiedAt": "1970-01-19T19:58:08.506000+00:00",
    "lastModifiedBy": "kthomson@air.org",
    "lastModifiedByType": "User"
  },
  "tags": {},
  "type": "Microsoft.KeyVault/vaults"
}



=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# 
az keyvault secret set \
  --vault-name $AKV_NAME \
  --name $ACR_NAME-pull-pwd \
  --value $(az ad sp create-for-rbac \
                --name $ACR_NAME-pull \
                --scopes $(az acr show --name $ACR_NAME --query id --output tsv) \
                --role acrpull \
                --query password \
                --output tsv)

WARNING: Creating 'acrpull' role assignment under scope '/subscriptions/cf71d4cd-095a-47ec-bca0-060c571abedf/resourceGroups/amtacr-rg/providers/Microsoft.ContainerRegistry/registries/amtacr'

WARNING: The output includes credentials that you must protect. Be sure that you do not include these credentials in your code or check the credentials into your source control. For more information, see https://aka.ms/azadsp-cli

WARNING: 'name' property in the output is deprecated and will be removed in the future. Use 'appId' instead.
{
  "attributes": {
    "created": "2021-07-24T01:04:41+00:00",
    "enabled": true,
    "expires": null,
    "notBefore": null,
    "recoveryLevel": "Recoverable+Purgeable",
    "updated": "2021-07-24T01:04:41+00:00"
  },
  "contentType": null,
  "id": "https://amtacr-vault.vault.azure.net/secrets/amtacr-pull-pwd/333a2924145b4cbc8d9e148b520a30a4",
  "kid": null,
  "managed": null,
  "name": "amtacr-pull-pwd",
  "tags": {
    "file-encoding": "utf-8"
  },
  "value": "Sr7TlnG_uViC80L5t2QhYSqXb3BoaIZDv2"
}


=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Store service principal ID in AKV (the registry *username*)

https://docs.microsoft.com/en-us/cli/azure/ad/sp?view=azure-cli-latest#az_ad_sp_create_for_rbac

az ad sp create-for-rbac [--cert]
                         [--create-cert]
                         [--keyvault]
                         [--name]
                         [--role]
                         [--scopes]
                         [--sdk-auth {false, true}]
                         [--skip-assignment {false, true}]
                         [--years]


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

*** so zsh does not work ... must be the --query

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
    {
      "command": null,
      "environmentVariables": [],
      "image": "amtacr.azurecr.io/helloacrtasks:v1",
      "instanceView": {
        "currentState": {
          "detailStatus": "",
          "exitCode": null,
          "finishTime": null,
          "startTime": "2021-07-24T02:19:12.905000+00:00",
          "state": "Running"
        },
        "events": null,
        "previousState": null,
        "restartCount": 0
      },
      "livenessProbe": null,
      "name": "acr-tasks",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ],
      "readinessProbe": null,
      "resources": {
        "limits": null,
        "requests": {
          "cpu": 1.0,
          "gpu": null,
          "memoryInGb": 1.5
        }
      },
      "volumeMounts": null
    }
  ],
  "diagnostics": null,
  "dnsConfig": null,
  "id": "/subscriptions/cf71d4cd-095a-47ec-bca0-060c571abedf/resourceGroups/amtacr-rg/providers/Microsoft.ContainerInstance/containerGroups/acr-tasks",
  "identity": null,
  "imageRegistryCredentials": [
    {
      "password": null,
      "server": "amtacr.azurecr.io",
      "username": "2a2955f3-4248-43af-812b-1c3563d3b578"
    }
  ],
  "instanceView": {
    "events": [],
    "state": "Running"
  },
  "ipAddress": {
    "dnsNameLabel": "acr-tasks-amtacr",
    "fqdn": "acr-tasks-amtacr.eastus.azurecontainer.io",
    "ip": "52.191.95.182",
    "ports": [
      {
        "port": 80,
        "protocol": "TCP"
      }
    ],
    "type": "Public"
  },
  "location": "eastus",
  "name": "acr-tasks",
  "networkProfile": null,
  "osType": "Linux",
  "provisioningState": "Succeeded",
  "resourceGroup": "amtacr-rg",
  "restartPolicy": "Always",
  "tags": {},
  "type": "Microsoft.ContainerInstance/containerGroups",
  "volumes": null
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
GIT_USER=biker93
GIT_PAT=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
echo $ACR_NAME $GIT_USER $GIT_PAT

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



=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# 


=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# 


