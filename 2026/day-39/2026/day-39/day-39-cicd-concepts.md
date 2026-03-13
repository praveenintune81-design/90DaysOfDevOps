# Day 39 – CI/CD Concepts

## Introduction

CI/CD is a DevOps practice that automates the process of building, testing, and deploying applications.
Instead of manually building and deploying software, CI/CD pipelines perform these tasks automatically whenever code changes are pushed to a repository.

CI/CD helps teams release software faster, reduce human errors, and maintain stable applications.


# Task 1 – The Problem

Imagine a team of 5 developers working on the same repository and deploying code manually to production.

## What Can Go Wrong?

Several problems can occur when deployment is done manually:

-Developers may overwrite each other’s code.
-Bugs can reach the production environment.
-Manual deployment can introduce human errors.
-Deployment becomes slow and inconsistent.
-It becomes difficult to track which version is deployed.

Manual deployment processes are unreliable and do not scale well when teams grow.


## What Does "It Works on My Machine" Mean?

"It works on my machine" is a common phrase used when code runs correctly on a developer’s local computer but fails in another environment such as testing or production.

This usually happens because of differences in:

-Operating systems
-Installed dependencies
-Environment variables
-Configuration settings

CI/CD helps solve this issue by ensuring code is built and tested in a consistent environment.


## How Many Times Can a Team Safely Deploy Manually?

Manual deployments are risky and slow.
Most teams can safely deploy only **1–2 times per day** using manual processes.

With CI/CD automation, teams can deploy **many times per day** safely and reliably.


# Task 2 – CI vs CD

## Continuous Integration (CI)

Continuous Integration is the practice where developers frequently merge their code changes into a shared repository.
Every code change automatically triggers a build and automated tests.

The goal of CI is to detect problems early and ensure that new code integrates correctly with the existing codebase.

Example:
A developer pushes code to GitHub and the pipeline automatically installs dependencies, builds the application, and runs unit tests.


## Continuous Delivery

Continuous Delivery ensures that the application is always in a **deployable state**.

The pipeline automatically builds and tests the application and prepares a release artifact, but the final deployment to production requires **manual approval**.

Example:
The pipeline builds a Docker image and pushes it to a container registry where it is ready for deployment.


## Continuous Deployment

Continuous Deployment goes one step further than Continuous Delivery.

Every change that passes automated tests is automatically deployed to production without manual intervention.

Example:
A developer pushes code → tests pass → the application is automatically deployed to production.


# Task 3 – Pipeline Anatomy

A CI/CD pipeline is composed of several important components.

## Trigger

A trigger is the event that starts a pipeline.

Common triggers include:

-Code push to repository
-Pull request creation
-Scheduled execution
-Manual workflow trigger


## Stage

A stage is a logical phase of the pipeline.

Typical stages include:

-Build
-Test
-Deploy

Each stage represents a group of related tasks.



## Job

A job is a unit of work executed within a stage.

A stage can contain one or multiple jobs.

Example jobs include:

-Build job
-Test job
-Docker build job


## Step

A step is a single command or action executed within a job.

Examples include:

-Installing dependencies
-Running tests
-Building Docker images

Example commands:

--------------

npm install
npm test
docker build 

-------------

## Runner

A runner is the machine that executes pipeline jobs.

Runners can be:

-GitHub-hosted runners
-Self-hosted runners

They provide the environment where pipeline commands run.


## Artifact

Artifacts are files generated during the pipeline process.

Examples include:

-Compiled applications
-Docker images
-Test reports
-Build packages

Artifacts are often used in later stages of the pipeline.


# Task 4 – CI/CD Pipeline Diagram

Example pipeline scenario:

A developer pushes code to GitHub.
The application is tested, built into a Docker image, and deployed to a staging server.

Pipeline flow:


Developer
   |
   | git push
   v
GitHub Repository
   |
   v
Pipeline Trigger
   |
   v
------------------------
BUILD STAGE
- Install dependencies
- Compile application
------------------------
        |
        v
------------------------
TEST STAGE
- Run unit tests
- Run integration tests
------------------------
        |
        v
------------------------
DOCKER BUILD STAGE
- Build Docker image
- Push image to registry
------------------------
        |
        v
------------------------
DEPLOY STAGE
- Deploy to staging server
------------------------



# Task 5 – Open Source Repository Example

Repository explored:
FastAPI

GitHub repository:
https://github.com/tiangolo/fastapi

Folder explored:

```
.github/workflows
```

Example workflow file:

```
test.yml
```

---

## What Triggers the Workflow?

The workflow is triggered when code is pushed to the repository or when a pull request is created.

Example configuration:

```
on:
  push
  pull_request
```

---

## How Many Jobs Does It Have?

The workflow includes multiple jobs such as:

* Testing job
* Linting job

Each job runs different validation tasks.

---

## What Does the Workflow Do?

The workflow performs several automated tasks including:

* Installing project dependencies
* Running automated tests
* Checking code formatting and quality

This ensures that code changes do not break the application.

---

# Conclusion

CI/CD pipelines help automate the software development lifecycle.

Benefits include:

- Faster software delivery
- Reduced human error
- Reliable deployments
- Early detection of bugs
- Better collaboration among developers


