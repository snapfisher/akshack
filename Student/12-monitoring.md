# Challenge 11: Monitoring - Container Insights

[< Previous Challenge](./11-privateaks.md) - **[Home](../README.md)** 

## Introduction

The one piece of the running AKS in production that we have not really addressed is monitoring.  Monitoring is important for a production system, but with the complexity behind kubernetes, it can also be important for dev/test systems as well.  kubectl describe is an excellent command, and it can detail what occurs on a pod, but you need to know which pod has failed first.

Container insights is a feature designed to monitor the performance of container workloads deployed to:

- Managed Kubernetes clusters hosted on AKS
- Self-managed Kubernetes clusters hosted on Azure using AKS Engine
- Azure Container Instances
- Self-managed Kubernetes clusters hosted on Azure Stack or on-premises
- Azure Red Hat OpenShift
- Azure Arc enabled Kubernetes (currently in preview)

Container insights supports clusters running the Linux and Windows Server 2019 operating system. The container runtimes it supports are Docker, Moby, and any CRI compatible runtime such as CRI-O and ContainerD.

This challenge is more about exploration than doing, so take time to click on other items in the Azure Portal to become familiar with the available capabilities

## Part 1: Enable Container Insights

We are going to go back to the original cluster we used for this hack, not the private cluster that we used in Challenge 11.  In the Azure Portal, go to the Azure Monitor page and enable Cluster Insights for your cluster.  You can also enable it from the Cluster page in the portal.

## Part 2:  Examine the Running Containers (Pods) with Container Insights

Once container insights becomes available you can view live metrics on the running of your cluster.  You will probably notice that container insights is running a lot more pods than you set running in the earlier challenges.  This is part of the Kubernetes infrastructure.  Get a detailed listing of the running containers, and then compare them to what is reported by kubectl.  Depending on how you left the cluster, parts of it may not be running.  That is okay, as we will fix this below.

For a web pods that you configured in your cluster, look at the Environment Variables and notice that the CONTENT_API_SERVICE_PORT variable that you set is listed correctly

View a billable data usage report, for all data and by namespace namespace.

Take a look at the preconfigured recommended alerts and spend some time reading about how to configure custom alerts.  Once you turn on alerting, it takes about 30 minutes to become active in the cluster, so we won't actually enable any alerting as part of this challenge.

## Part 3: Logs and Events from a Running Application

We're going to take down and redeploy the FabMedical application.  Leave the mongodb alone, just delete the content-web and content-api services and deployments.  Reinstall these using the identical yaml files that you used for Challenge 11.

Once the application is running hit it in your browser a couple of times.  You should be able to see the data from mongodb database, but if you cannot, don't worry about it.

In container insights, look at the content-web container and see that you can view both the logs from the container as well as Kubernetes events that occurred against the container.  Logs will be generated every time you access the application.  Notice how you can filter events to the pod, cluster or namespace

## Part 4: Let's Look at Other Portal Features

On the portal page for the cluster, there are two sections of functionality: "Kubernetes Resources" and "Settings".  Kubernetes Resources contains additional monitoring capabilities, while Settings allows you to access some of the features that you can access through the CLI.

Under Kubernetes Resouces -> Services and Ingresses, look for the yaml that was used to deploy the web service.  Delete the web service and attempt to deploy this one instead.  Did it work?  You could delete lines from the file until you get a working version, as this yaml is what is running, not what you need to specify to run.  Certain elements need to be specified by AKS/Kubernetes itself (and you want it to be that way).

Notice under Kubernetes Resources -> Configuration that you can examine all secrets (including certificates) that the cluster is using.

Look in Settings -> Node Pools and see how you can both scale and upgrade the cluster from thisd page.  Look and see how you can also set autoscaling from here.  Note that although AKS is a PaaS service, and Azure will handle all of the details of a cluster upgrade, we will not upgrade the cluster without a specific command to do so, but once given the command, Azure will handle all of the details.  Azure will tell you if your cluster version is no longer supported.  Note that Azure will automatically update the host images on the nodes with no interaction needed on your part.

## Success Criteria
1. You understand the monitoring features provided for AKS by the Azure Portal and Container Insights.


## Reading:
https://docs.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-overview

https://kubernetes.io/docs/reference/kubectl/overview/
