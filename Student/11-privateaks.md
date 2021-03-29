# Challenge 11: Private AKS

[< Previous Challenge](./10-networking.md) - **[Home](../README.md)** 

## Introduction

So far, we have used AKS deployments with public endpoints, but what about AKS deployments where we only want Private Endpoints which are available from an Azure Virtual Network.  We not only want to move the user accessible endpoints, but we also want to protect the AKS Control Plane from public access, by moving it out of the user managed Azure Subscription.

Within a Private AKS Cluster, the control plane or API server is in an Azure Kubernetes Service (AKS)-managed Azure subscription. A customer's cluster or node pool is in the customer's subscription. The server and the cluster or node pool can communicate with each other through the Azure Private Link service in the API server virtual network and a private endpoint that's exposed in the subnet of the customer's AKS cluster.

## Part 1: Create the Private AKS Cluster, Networking components and a Virtual Machine on the network.

In Student/Resources/Challenge 11/ARM is a directory that contains an Azure Resource Manager (ARM) Template and scripts for deploying this template to Azure.  One of the scripts is for PowerShell, and one is for Bash.  Using the script of your choice, edit it to specify your resource group, cluster name, and Azure region.  Then run the scipt. (Do this before reading the explanation documentation, as it takes a while to complete).  When the script completes, it will display credentials for the Azure Service Principal controlling the cluster.  Make sure you copy these down, as there is no way to regenerate them later.

While the script is running, take a moment and look at the full documentation for creating a [Private AKS Deployment](https://docs.microsoft.com/en-us/azure/aks/private-clusters)

Also take a look at the template.  ARM templates are the standard way of deploying resources as Infrastructre-As-Code into Azure.  If you are unfamiliar ARM Tempalte documetnation can be found here: https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/overview

Microsoft is also creating a new language to manipulate and deploy ARM templates.  It is called Bicep.  It is not being used in this hack, but you will hear more about it in the coming months.  Documentation on Bicep can be found here: https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/bicep-overview

## Part 2:  Connect to the VM with Azure Bastion

Now that you have the resources created, you need to connect to the VM in order to access them.  We will do this with the Azure Bastion Service.  Note that the virtual machine has been configured to allow outgoing requests to the internet, so that we can run this hack in a less difficult way, but to not allow incoming request, which is why we need bastion.  In a production system, you could also limit connections in both directions, as your Vnet could be connected to an Azure VPN or ExpressRoute, making connection from your organization simple.

To create the Azure Bastion Service, we are going to follow the instructions here: https://docs.microsoft.com/en-us/azure/bastion/tutorial-create-host-portal

## Part 3: Connect to AKS and run the application

From the Azure Portal, connect to the virtual machine in Azure using the bastion service.  The user name is 'azureuser'.  Use the private SSH key from a local file, which you will find was created during the ARM template deployment (the passphrase requested is for the private key).

Once you are connected to the machine, we need to do a small bit of maintenance: update the version of the Azure CLI which is on the machine.  This can be done simply by typing "sudo az upgrade".

Then, login to the Azure CLI

Then, let's get the credentials to the new AKS cluster.  Remember that the VM is on the same Vnet as the cluster, so once we are logged in, we can connect to the AKS cluster just as we did from a non-protected instance.

az aks get-credentials --resource-group privateaks --name aksCluster1 (modify the resource group and cluster name to however you built the new cluster)

Then, let's download this repo to that machine: git clone https://github.com/snapfisher/akshack.git akshack

In Student/Resources/Challenge 11/ you will find 4 yaml files.  We are going to go all the way back to challenge 4 and deploy the same application.  First you need to modify the deployment files to match your ACR and V1 container version names.  Then deploy using kubectl just as you did in challenge 4.

It will fail.  Use 'Kubectl Describe' to determine why.

Delete the deployments.  The reason for failure was that if you recall, way back in the early challenges, when we created the AKS instance, we assigned the ACR to the cluster for permissions and authorization.  Since we already have the cluster created, we need to add this functionality.  Information on how to do this can be found here: https://docs.microsoft.com/en-us/azure/aks/cluster-container-registry-integration

Once you have worked out the ACR permissions, create the deployments again.  You should get a public IP address, and since this virtual network does not block specifically created output public IP addresses you should be able to access the running application from the internet.  However, if this was a real internal application, you might not want outside users to be able to access your application.  You want the Web Service to supply a private IP address instead.  Delete the web service, modify the service yaml file, and then redeploy.  Information on the modifications you need to make can be found here: https://docs.microsoft.com/en-us/azure/aks/internal-lb

After a minute or so, you can look at the kubectl output and see that now, instead of a public IP, the web application has an internal IP that should be 10.x.x.x.  However, we did not create any connected desktop machines, so we cannot run the app, however, if you created a windows Virtual Machine on the Vnet, you would be able to access the application via this private IP address.


## Success Criteria

1. You can create a Private AKS deployment
2. You can connect to the AKS cluster from the virtual machine on the Vnet
3. You can run the application with either a public or private IP address, using kubectl and standard tools, as you did for public available clusters

Full credit to Larry Claman: https://github.com/onemtc/privateaks-cicd.  I highly recommend taking a look at this if you want to see an example of how to do CI/CD into your Private AKS Cluster using GitHub.
