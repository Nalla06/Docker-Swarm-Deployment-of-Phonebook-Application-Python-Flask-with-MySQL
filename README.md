# Docker Swarm Deployment of Phonebook Application (Python Flask) with MySQL

## Description

This project aims to deploy the Phonebook Application web application with Docker Swarm on Elastic Compute Cloud (EC2) Instances by pulling the app images from the AWS Elastic Container Registry (ECR) repository.

## Problem Statement


- Your company has recently started a project that aims to serve as phonebook web application. Your teammates have started to work on the project and developed the UI and backend part of the project and they need your help to deploy the app in development environment.

- You are, as a Cloud/DevOps engineer, requested to deploy the Phonebook Application in the development environment on Docker Swarm on AWS EC2 Instances using Terraform to showcase the project. To do that you need to;

  - Create a new public repository for the project on GitHub.

  - Create docker image using the `Dockerfile` from the base image of `python:alpine`.

  - Deploy the app on swarm using `docker compose`. To do so on the `Compose` file;

    - Create a MySQL database service with one replica using the image of `mysql:5.7`;

      - attach a named volume to persist the data of database server.

      - attach `init.sql` file to initialize the database using `configs`.

    - Configure the app service to;

      - pull the image of the app from the AWS ECR repository.

      - deploy the one app for each swarm nodes using `global` mode.

      - run the app on `port 80`.

    - Use a user-defined overlay network for the services.

  - Push the necessary files for your project from local repo to the new github repo (phonebookswarm).

- You are also requested; to use AWS ECR as image repository, to create Docker Swarm with 3 manager and 2 worker node instances, to automate the process of Docker Swarm initialization through Terraform in the development environment. To achieve this goals, you can configure Terraform configuration file using the followings;

  - The application should run on Amazon Linux 2 EC2 Instance

  - EC2 Instance type can be configured as `t2.micro`.

  - To use the AWS ECR as image repository;

    - Enable the swarm node instances with IAM Role allowing them to work with ECR repos using the instance profile.

    - Install AWS CLI `Version 2` on swarm node instances to use `aws ecr` commands.

  - To automate the process of Docker Swarm initialization;

    - Install the docker and docker-compose on all nodes (instances) using the `user-data` bash script.

    - Create the leader manager node of the swarm. Within the `user-data` script;

      - Set the first manager node hostname as `Leader-Manager`.

      - Initialize Docker swarm.

      - Create a docker service named `viz` on the manager node on port `8080` using the `dockersamples/visualizer` image, to monitor the swarm nodes easily.

      - Build the docker image from the GitHub URL of the new project repo and tag it appropriately to push it on ECR repo. (Note: Do not forget to install Git to enable Docker to work with git commands)

      - Download `docker-compose.yml` file from the repo and deploy application stack on Docker Swarm.

    - Create two manager nodes of the swarm. Within the `user-data` script;

      - Install the python `ec2instanceconnectcli` package for `mssh` command.

      - Connect from manager node to the `Leader-Manager` to get the `join-token` and join the swarm as manager node using `mssh` command.
    
    - Create two worker nodes of the swarm. Within the `user-data` script;

      - Install the python `ec2instanceconnectcli` package to use `mssh` command.

      - Connect from worker node to the `Leader-Manager` to get the `join-token` and join the swarm as worker node using `mssh` command.

  - Create a security group for all swarm nodes and open necessary ports for the app and swarm services.

  - Create an image repository on ECR for the app.

  - Tag the swarm node instances appropriately as `Docker-Swarm-Manager  <Number>/Docker-Swarm-Worker <Number>` to discern them from AWS Management Console.

  - The Web Application should be accessible via web browser from anywhere.

  - Phonebook App Website URL, Visualization App Website URL should be given as output by Cloudformation Service, after the stack created.

## Project Skeleton

```text
204-docker-swarm-deployment-of-phonebook-app-on-python-flask-mysql (folder)
|
|----readme.md            # Given to the students (Definition of the project)
|----cfn-template.yml     # To be delivered by students (Cloudformation template-Optional)
|----phonebook-app.py     # Given to the students (Python Flask Web Application)
|----requirements.txt     # Given to the students (List of Flask Modules/Packages)
|----init.sql             # Given to the students (SQL statements to initialize db)
|----main.tf              # To be delivered by students (Terraform configuration file)
|----Dockerfile           # To be delivered by students
|----docker-compose.yml   # To be delivered by students
|----templates
        |----index.html      # Given to the students (HTML template)
        |----add-update.html # Given to the students (HTML template)
        |----delete.html     # Given to the students (HTML template)
```

## Expected Outcome

![Phonebook App Search Page](./search-snapshot.png)
