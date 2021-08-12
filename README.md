# QA Cloud DevOps Projects
This project is a clone of https://github.com/DaraOladapo/flask-project - and was used as part of a teaching project. The project involves deploying a to-do list application wirtten in Python, implementing Flask.

This readme will serve as basic documentation of my process and methodology to complete the project.

## Step 1 - Creating Resource Groups and provisioning resources
This step involved using the Azure Portal to create the following resources:
- Resoruce Group - To house our other resources
- Linux Virtual Machine using Ubuntu  - To nominate as our agent for our pipeline
- MySQL Azure Database - To store the data from our finished program

### Creating the Resource Group
The resource group is a container which 'holds' other resoruces such as VMs and Databases, this enables granularity between multiple projects and allows greater transparency regarding things like cost.

![alt_text](https://i.imgur.com/45kGYNO.png)


### Creating the Virtual Machine
The VM used Ubuntu 20.04 as the operating system, and only used limited vCPUs and RAM as our work was not compute intensive.

![alt text](https://i.imgur.com/NA9yznC.png)


### Creating the MySQL Database
Similar to the VM, the resources available to the database were restricted to meet our requirements and so enable cost savings.

![alt text](https://i.imgur.com/iIo1bUF.png)


## Step 2 - Creating a Azure DevOps Project and Connecting to a Self-Hosted Agent

This step involved creating a project on Azure DevOps, connecting it to our code repository, this would enable us to create pipelines (discussed in Step 3), which would be used in conjunction with a build agent which will operate on our agent to execute our pipeline code.

### Creating a DevOps Project

Having multiple DevOps projects allows you to seperate different pieces of work, whilst also allowing them to have unique configuration settings such as agent pools. 

![alt text](https://i.imgur.com/OLMWIqe.png)

### Importing the Github project

In order to run the application provided we have to import the code repository, DevOps has functinality for this which makes it easy to clone a reposiutory and integrate it into our pipelines

![alt text](https://i.imgur.com/RSBxQ7z.png)

### Creating a Self-Hosted Agent

An agent will unable us to execute our pipeline builds through our own resoruces as opposed to relying on DevOps resoruces which may not be available at the time of building. As such the first step was to configure a pool through DevOps.

![alt_text](https://i.imgur.com/RS0dHQN.png)

Once this was done, we had to access our VM which was created earlier and set it up to operate as an agent. Steps for creating a self-hosted agent: https://qa-community.co.uk/~/_/learning/azure-devops-core/azure--azure-devops-build-agents

Once this was done, we just had to run the agent from the VM to activate it for our pipelines.

![alt_text](https://i.imgur.com/rPROmxy.png)

## Step 3 - Writing our pipelines

Steps for developing the pipeline were mostly taken from:
- https://qa-community.co.uk/~/_/learning/azure-devops-core/azure--azure-devops-python-example-vm
- https://qa-community.co.uk/~/_/learning/azure-devops-core/azure--azure-devops-python-example-webapp
- https://docs.microsoft.com/en-us/azure/azure-app-configuration/push-kv-devops-pipeline

### Installing requirements via pipelines

Part of our pipeline included installing a set of requirements which were neccessary for the operation of the application we were deploying. These could have been installed directly via the agent terminal, however by integrating these into the pipeline, it ensures the requirements are satisfied in all scenarios, such as a new agent being used.

![alt_text](https://i.imgur.com/4wxFC7W.png)

## Step 4 - Troubleshooting / Configuring our application
