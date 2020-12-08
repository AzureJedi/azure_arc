---
type: docs
title: "Onboard an existing Windows server with Azure Arc"
linkTitle: "Onboard an existing Windows server with Azure Arc"
weight: 2
description: >
---

## Onboard an existing Windows server with Azure Arc

The following README will guide you on how to connect an Windows machine to Azure Arc using a simple PowerShell script.

## Prerequisites

* [Install or update Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest). **Azure CLI should be running version 2.7** or later. Use ```az --version``` to check your current installed version.

* Create Azure service principal (SP)

    To connect a server to Azure Arc, an Azure service principal assigned with the "Contributor" role is required. To create it, login to your Azure account run the below command (this can also be done in [Azure Cloud Shell](https://shell.azure.com/)).

    ```console
    az login
    az ad sp create-for-rbac -n "<Unique SP Name>" --role contributor
    ```

    For example:

    ```console
    az ad sp create-for-rbac -n "http://AzureArcServers" --role contributor
    ```

    Output should look like this:

    ```json
    {
    "appId": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    "displayName": "AzureArcServers",
    "name": "http://AzureArcServers",
    "password": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    "tenant": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    }
    ```

    > **Note: It is optional but highly recommended to scope the SP to a specific [Azure subscription and resource group](https://docs.microsoft.com/en-us/cli/azure/ad/sp?view=azure-cli-latest)**

* Create a new Azure resource group where you want your machine(s) to show up.

    ![Screenshot showing Azure Portal wtih empty resource group](./01.png)

* Download the [az_connect_win](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/scripts/az_connect_win.ps1) PowerShell script.

* Change the environment variables according to your environment and copy the script to the designated machine.

    ![Screenshot showing PowerShell script](./02.png)

## Deployment

On the designated machine, Open PowerShell ISE **as Administrator** and run the script. Note the script is using *$env:ProgramFiles* as the agent installation path so make sure **you are not using PowerShell ISE (x86)**.

![Screenshot showing PowerShell script](./03.png)

![Screenshot showing PowerShell script](./04.png)

Upon completion, you will have your Windows server, connected as a new Azure Arc resource inside your resource group.

![Screenshot showing PowerShell script being run](./05.png)

![Screenshot showing Azure Portal with Azure Arc enabled server resource](./06.png)

![Screenshot showing Azure Portal with Azure Arc enabled server resource detail](./07.png)

## Delete the deployment

The most straightforward way is to delete the server via the Azure Portal, just select server and delete it.

![Screenshot showing delete resource function in Azure Portal](./08.png)

If you want to delete the entire environment, just delete the Azure resource group.

![Screenshot showing delete resource group function in Azure Portal](./09.png)