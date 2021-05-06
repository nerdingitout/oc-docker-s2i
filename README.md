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
- From the Administrator perspective, go to Projects and click 'Create Project' from the top right of the page. You can name the project 's2i-project'.
![image](https://user-images.githubusercontent.com/36239840/117288733-dc4acf00-ae7c-11eb-9a52-9d41958c0990.png)
- Once created you will be redirected to the Overview page of the project
![image](https://user-images.githubusercontent.com/36239840/117288822-f71d4380-ae7c-11eb-9cbc-46c1a720c3ca.png)

## Fork the GitHub repo and host a Dockerfile
- Fork this GitHub repository, by clicking the Fork button at the top of this repo. You will be making changes in the Dockerfile to trigger new pod deployments at a later step in this tutorial.
The repository has a Dockerfile that contains the following lines of code.
```
# simple dockerfile to test OCP s2i using Dockerfile 

FROM ubuntu:18.04
CMD ["/bin/bash", "-c", "sleep infinity"]
# CMD ["/bin/bash", "-c", "--", "while true; do sleep 30; done;"]
```
## Create a pod deployment using the Dockerfile from your GitHub repo
- In the OpenShift web console, switch from the Administrator perspective to the Developer perspective and select to create an application 'From Dockerfile'.
![image](https://user-images.githubusercontent.com/36239840/117294441-b37a0800-ae83-11eb-8264-879e2939553f.png)
- Add the URL of your forked repo in the 'Git Repo URL' field and context dir ```/ubunutu```where the Dockerfile is located.
![image](https://user-images.githubusercontent.com/36239840/117295616-1e780e80-ae85-11eb-852b-011e5be0dc13.png)
- Scroll down to Resources section and select ```Deployment Config``` and keep the 'create a route to the application' checked then click 'Create'.
![image](https://user-images.githubusercontent.com/36239840/117295963-8cbcd100-ae85-11eb-80c7-5972beb45822.png)
- You will be redirected to the topology view which shows the pods created. Once it successfully builds, you will notice that the circle around your application turns dark blue. If you click on your application, it will show the Deployment Config view where you can view details, resources and logs.
What happens here is that OpenShift builds the docker image using Dockerfile from your github repo, creates a docker image, uploads the image into OpenShift’s internal image registry and creates a Pod using that docker image.
![image](https://user-images.githubusercontent.com/36239840/117296212-d86f7a80-ae85-11eb-922e-cbafe2e2bb31.png)
## Verify that the container process matches the command specified in the Dockerfile
## Setup GitHub Webhook
## Make changes on your GitHub repository 

## Summary
