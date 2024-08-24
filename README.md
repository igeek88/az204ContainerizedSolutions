# az204ContainerizedSolutions
Terraform repository based on https://learn.microsoft.com/en-us/training/paths/az-204-implement-iaas-solutions/

## Introduction

This repository will have, among other things, some terraform code that will be used to deploy an Azure Container Application, along with some budget alarms.

### Documentation used to create the code

#### Terraform
* [Azure - Consumption Budget](
https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/consumption_budget_resource_group)
* [Azure - Features Block](https://registry.terraform.io/providers/hashicorp/Azurerm/latest/docs/guides/features-block)

#### Azure Portal
* [Quickstart Center](https://portal.azure.com/#view/Microsoft_Azure_Resources/QuickstartCenterBlade)
* [AZ-204: Implement containerized solutions
](https://learn.microsoft.com/en-us/training/paths/az-204-implement-iaas-solutions/)
    * [Implement Azure Container Apps](https://learn.microsoft.com/en-us/training/modules/implement-azure-container-apps/)
        1. `az extension add --name containerapp --upgrade`

        1. Register the Microsoft.App namespace:
        
            `az provider register --namespace Microsoft.App`

        1. Register the Microsoft.OperationalInsights provider for the Azure Monitor Log Analytics workspace if you haven't used it before:
        
            `az provider register --namespace Microsoft.OperationalInsights`

        1. Set environment variables:
        
            `myRG=az204-containerapp-rg        
        myLocation=eastus 
        myAppContEnv=az204-env-$RANDOM`

        1. Create a resource group:
        
            `az group create \
    --name $myRG \
    --location $myLocation`

        1. Create a containerapp environment:
        
            `
        az containerapp env create \
    --name $myAppContEnv \
    --resource-group $myRG \
    --location $myLocation`
        1. Create a container app:
        
            `az containerapp create \
    --name my-container-app \
    --resource-group $myRG \
    --environment $myAppContEnv \
    --image mcr.microsoft.com/azuredocs/containerapps-helloworld:latest \
    --target-port 80 \
    --ingress 'external' \
    --query properties.configuration.ingress.fqdn`