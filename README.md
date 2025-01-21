# Using SSH with Azure AI Foundry's Compute Instances

[Azure AI Foundry](https://learn.microsoft.com/en-us/azure/ai-studio/) is a cloud-based platform that simplifies the development, deployment and management of artificial intelligence (AI) solutions. It provides a variety of tools and services to help you build, train and deploy AI models. One of the key components of Azure AI Foundry is compute instances. These are virtual machines that you can use to train and run your AI models.

This document provides instructions on how to use SSH to connect to Azure AI Foundry's compute instances. There are two methods for connecting: through a public network or through a private network.

## Table of Contents:
* Part 1: [Configuring SSH environment](#configuring-ssh-environment)
* Part 2: [Connecting through public network](#connecting-through-public-network)
* Part 3: [Connecting through private network](#connecting-through-private-network)

## Configuring SSH environment
1. Generate and store SSH keys in Azure:
``` bash
az sshkey create --name "AIFoundrySSHKey" --resource-group <Resource_Group_Name>
```
> [!NOTE]
> Replace `<Resource_Group_Name>` with the name of your Azure resource group.
2. During Compute Instance creation, select "Enable SSH Access", then "Use existing public key stored in Azure" and select the public key stored in Azure.

## Connecting through public network
1.  From your local machine, execute the following Az CLI command:
``` bash
az ml compute connect-ssh --name <Compute_Instance_Name> --resource-group <Resource_Group_Name> --workspace-name <AI_Foundry_Name> --private-key-file-path <Path_to_Private_Key>
```
> [!NOTE]
> Replace the following placeholders with the appropriate values:
>    *   `<Compute_Instance_Name>`: The name of your Azure AI Foundry compute instance.
>    *   `<Resource_Group_Name>`: The name of the resource group that contains your compute instance.
>    *   `<AI_Foundry_Name>`: The name of your Azure AI Foundry workspace.
>    *   `<Path_to_Private_Key>`: The path to your private SSH key file on your local machine.

## Connecting through private network
1. Enable Private Endpoint for Azure AI Foundry to a custom VNet.
> The specific steps for enabling a Private Endpoint are beyond the scope of this document. Please refer to the [Azure AI Foundry's documentation](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/configure-private-link) for detailed instructions.
2.  Provision Azure VM in a custom VNet and install Az CLI. For Ubuntu-based VM, the installation of Az CLI may require execution of these commands:
``` bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash 
az extension add -n ml -y
```
3.  From the Azure VM, execute the following Az CLI command:
``` bash
az ml compute connect-ssh --name <Compute_Instance_Name> --resource-group <Resource_Group_Name> --workspace-name <AI_Foundry_Name> --private-key-file-path <Path_to_Private_Key>
```
> [!NOTE]
> Replace the following placeholders with the appropriate values:
>    *   `<Compute_Instance_Name>`: The name of your Azure AI Foundry compute instance.
>    *   `<Resource_Group_Name>`: The name of the resource group that contains your compute instance.
>    *   `<AI_Foundry_Name>`: The name of your Azure AI Foundry workspace.
>    *   `<Path_to_Private_Key>`: The path to your private SSH key file on your Azure VM.

By following these steps, you should be able to use SSH to connect to your Azure AI Foundry's compute instances from either a public network or a private network.
