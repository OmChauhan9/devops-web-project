# Java Web App Deployment with AWS 

Welcome to my completed **7-Day DevOps Challenge** project! 🚀

This repository hosts a Java web application that is automatically built, tested, and deployed using a fully automated **CI/CD Pipeline** on AWS.

<br>

## Table of Contents
- [Introduction](#introduction)
- [Architecture & Technologies](#architecture--technologies)
- [How It Works](#how-it-works)
- [Contact](#contact)
- [Conclusion](#conclusion)

<br>

## Introduction
The goal of this project was to move away from manual deployments and build a robust "Supply Chain" for software delivery.

Starting with a basic Java app, I progressively implemented **Infrastructure as Code (IaC)**, **Dependency Management**, **Continuous Integration (CI)**, and **Continuous Deployment (CD)**. The result is a system where a single `git push` triggers a series of automated steps that result in a live production update minutes later.

<br>

## Architecture & Technologies
Here is the complete tech stack used to build this pipeline:

- **Amazon EC2**: Used two separate environments—a **Dev Server** for coding/testing and a **Production Server** for hosting the live application.
- **GitHub**: Stores the source code and acts as the trigger for the pipeline via Webhooks.
- **AWS CodeArtifact**: A secure, private repository that caches Maven dependencies. This protects the supply chain from external downtime and ensures consistent builds.
- **AWS CodeBuild**: A fully managed build service that compiles the Java source code, runs tests, and packages the application into a `.war` file and zipped artifact.
- **AWS CodeDeploy**: Automates the deployment to the EC2 fleet. It handles the lifecycle events (Stop Server -> Install -> Start Server) using `appspec.yml` and Bash scripts.
- **AWS CodePipeline**: The "Conductor" of the orchestra. It connects GitHub, CodeBuild, and CodeDeploy into a single visual workflow.
- **Amazon S3**: Acts as the artifact store, holding the zipped build outputs before they are deployed.
- **AWS CloudFormation**: Used to define the entire infrastructure (VPC, EC2, Security Groups) as code, allowing the environment to be spun up or destroyed in minutes.

<br>

## How It Works
Instead of running manual commands, this project uses an automated workflow:

1. **Source**: I push a code change (e.g., updating `index.jsp`) to the `main` branch on GitHub.
2. **Trigger**: AWS CodePipeline detects the commit and pulls the latest source code.
3. **Build**: CodeBuild starts a new container, authenticates with **CodeArtifact** to fetch libraries, compiles the Java code using Maven, and produces a build artifact stored in **S3**.
4. **Deploy**: CodeDeploy pulls the artifact from S3 and pushes it to the **Production EC2 Instance**.
5. **Live**: The shell scripts (`install_dependencies.sh`, `start_server.sh`) automatically restart the Tomcat/Apache server with the new version.

<br>

## Conclusion
This project demonstrates the power of modern DevOps tools to eliminate "it works on my machine" issues. By automating the build and deploy process, I can focus on writing code while AWS handles the delivery.

A big shoutout to **[NextWork](https://learn.nextwork.org/app)** for the guidance on this 7-Day DevOps Challenge. [You can check out the challenge here.](https://learn.nextwork.org/projects/aws-devops-vscode?track=high)
