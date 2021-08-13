# QA Cloud DevOps Projects
This project is a clone of https://github.com/DaraOladapo/flask-project - and was used as part of a teaching project. The project involves deploying a to-do list application wirtten in Python, implementing Flask.

This readme will serve as basic documentation of my process and methodology to complete the project.

## Part 1 - Creating Resource Groups and provisioning resources
This step involved using the Azure Portal to create the following resources:
- Resoruce Group - To house our other resources
- Linux Virtual Machine using Ubuntu  - To nominate as our agent for our pipeline
- MySQL Azure Database - To store the data from our finished program

### Creating the Resource Group
The resource group is a container which 'holds' other resoruces such as VMs and Databases, this enables granularity between multiple projects and allows greater transparency regarding things like cost.

![image](https://i.imgur.com/45kGYNO.png)


### Creating the Virtual Machine
The VM used Ubuntu 20.04 as the operating system, and only used limited vCPUs and RAM as our work was not compute intensive.

![image](https://i.imgur.com/NA9yznC.png)


### Creating the MySQL Database
Similar to the VM, the resources available to the database were restricted to meet our requirements and so enable cost savings.

![image](https://i.imgur.com/iIo1bUF.png)


## Part 2 - Creating a Azure DevOps Project and Connecting to a Self-Hosted Agent

This step involved creating a project on Azure DevOps, connecting it to our code repository, this would enable us to create pipelines (discussed in Step 3), which would be used in conjunction with a build agent which will operate on our agent to execute our pipeline code.

### Creating a DevOps Project

Having multiple DevOps projects allows you to seperate different pieces of work, whilst also allowing them to have unique configuration settings such as agent pools. 

![image](https://i.imgur.com/OLMWIqe.png)

### Importing the Github project

In order to run the application provided we have to import the code repository, DevOps has functinality for this which makes it easy to clone a reposiutory and integrate it into our pipelines

![image](https://i.imgur.com/RSBxQ7z.png)

### Creating a Self-Hosted Agent

An agent will unable us to execute our pipeline builds through our own resoruces as opposed to relying on DevOps resoruces which may not be available at the time of building. As such the first step was to configure a pool through DevOps.

![image](https://i.imgur.com/RS0dHQN.png)

Once this was done, we had to access our VM which was created earlier and set it up to operate as an agent. Steps for creating a self-hosted agent: https://qa-community.co.uk/~/_/learning/azure-devops-core/azure--azure-devops-build-agents

Once this was done, we just had to run the agent from the VM to activate it for our pipelines.

![image](https://i.imgur.com/rPROmxy.png)

## Part 3 - Writing our pipelines

Steps for developing the pipeline were mostly taken from:
- https://qa-community.co.uk/~/_/learning/azure-devops-core/azure--azure-devops-python-example-vm
- https://qa-community.co.uk/~/_/learning/azure-devops-core/azure--azure-devops-python-example-webapp
- https://docs.microsoft.com/en-us/azure/azure-app-configuration/push-kv-devops-pipeline

### Installing requirements via pipelines

Part of our pipeline included installing a set of requirements which were neccessary for the operation of the application we were deploying. These could have been installed directly via the agent terminal, however by integrating these into the pipeline, it ensures the requirements are satisfied in all scenarios, such as a new agent being used. In the case that the requirements are already installed, the job will not fail and the specific command will have no output and the job will continue.

![image](https://i.imgur.com/4wxFC7W.png)

Once the requirements have been installed on the virtual machine, we are able to provision resources using Azure Web Apps through the Azure CLI. This has similar benefits to the previous steps, where the resources can be automatically provisioned if they were not. As part of this code we included the 'dependsOn' parameter for this job, by specifying another job, we can ensure that this job will only run once the prior job has completed, this enables jobs to run sequentially else they may intefere with one another.

![image](https://i.imgur.com/PXt3lVL.png)

Finally, we implemented a final task to the pipeline. We wrote a script which would directly modify the configuration settings of the previously provisioned web app, this is neccessary to allow the web app to interface with our database. This again depends on the success of the previous job of deploying an Azure Web App.

![image](https://i.imgur.com/qDpfsiP.png)

## Part 4 - Troubleshooting

### Modifying Libraries

Despite completing the above steps, the application was failing to connect to the database, and we were generating errors whilst deploying the application. The errors related to some mismatch of the libraries used and their versions. To remedy this issue we used the latest versions of the libraries, and also used the PyMySQL. This change was pushed by modifying the requirements.txt file, the pipeline would read directly from this file to update the libraries as neccessary.

![image](https://i.imgur.com/gQiOWlu.png)

### Modifying project code

As we implemented a new library, we modified the code of the __init__.py file to install and connect to the database. The modified code is highlighted below:

![image](https://i.imgur.com/mapOYSI.png)

## Part 5 - Project Management

### Trello Boards

To manage my tasks, I created a Trello board. I was able to specify if a task was done, in progress, or to be done, this enabled me to visually track my progress. I was also able to add user stories to my board which I would use to test my application.

![image](https://i.imgur.com/uvmJVJ9.png)
