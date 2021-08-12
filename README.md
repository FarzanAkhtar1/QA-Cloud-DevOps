# QA Cloud DevOps Projects
This project is a clone of https://github.com/DaraOladapo/flask-project - and was used as part of a teaching project. The project involves deploying a to-do list application wirtten in Python, implementing Flask.

This readme will serve as basic documentation of my process and methodology to complete the project.

## Step 1 - Creating Resource Groups and provisioning resources
This step involved using the Azure Portal to create the following resources:
- Resoruce Group - To house our other resources
- Linux Virtual Machine using Ubuntu  - To nominate as our agent for our pipeline
- MySQL Azure Database - To store the data from our finished program

![alt text](https://i.imgur.com/NA9yznC.png)
![alt text](https://i.imgur.com/iIo1bUF.png)

## Step 2 - Creating a Azure DevOps Project and Connecting to a Self-Hosted Agent
Steps for creating a self-hosted agent: https://qa-community.co.uk/~/_/learning/azure-devops-core/azure--azure-devops-build-agents
This step involved creating a project on Azure DevOps, connecting it to our code repository, this would enable us to create pipelines (discussed in Step 3), which would be used in conjunction with a build agent which will operate on our agent to execute our pipeline code.

