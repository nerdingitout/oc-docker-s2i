# Deploying OpenShift application from Dockerfile using S2I capability
## Introduction
OpenShift Source-to-Image (S2I) is a framework that makes it easy to write images that take application source code as an input and produce a new image that runs the assembled application as output. The main advantage of using S2I for building reproducible Docker images is the ease of use for developers. A Dockerfile is a recipe (or blueprint) for building Docker images.<br>
In this tutorial, you will learn how to use OpenShift S2I feature to build a docker image from a Dockerfile hosted in github and deploy a Pod using that docker image. You will also learn how to setup the github webhook to notify OpenShift of new code push/commit events in github, such that OpenShift will auto rebuild and redeploy Pods using the latest code/Dockerfile changes in your github.
## Prerequisites
For this tutorial you will need:
- Red Hat OpenShift Cluster 4.3 on IBM Cloud.
- oc CLI (can be downloaded from this link or you can use it at http://shell.cloud.ibm.com/.
## Estimated Time
It will take you around 30 minutes to complete this tutorial.
## Steps
- [Create Project](https://github.com/nerdingitout/oc-docker-s2i#create-project)
- [Create a GitHub repo and host a Dockerfile](https://github.com/nerdingitout/oc-docker-s2i#create-a-github-repo-and-host-a-dockerfile)
- [Create a pod deployment using the Dockerfile from your GitHub repo](https://github.com/nerdingitout/oc-docker-s2i#create-a-pod-deployment-using-the-dockerfile-from-your-github-repo)
- [Verify that the container process matches the command specified in the Dockerfile](https://github.com/nerdingitout/oc-docker-s2i#verify-that-the-container-process-matches-the-command-specified-in-the-dockerfile)
- [Setup GitHub Webhook](https://github.com/nerdingitout/oc-docker-s2i#setup-github-webhook)
- [Make changes on your GitHub repository](https://github.com/nerdingitout/oc-docker-s2i#make-changes-on-your-github-repository)
## Create Project
## Create a GitHub repo and host a Dockerfile
## Create a pod deployment using the Dockerfile from your GitHub repo
## Verify that the container process matches the command specified in the Dockerfile
## Setup GitHub Webhook
## Make changes on your GitHub repository 

## Summary
