# Deploying OpenShift application from Dockerfile using S2I capability
## Introduction
OpenShift Source-to-Image (S2I) is a framework that makes it easy to write images that take application source code as an input and produce a new image that runs the assembled application as output. The main advantage of using S2I for building reproducible Docker images is the ease of use for developers. A Dockerfile is a recipe (or blueprint) for building Docker images.<br>
In this tutorial, you will learn how to use OpenShift S2I feature to build a docker image from a Dockerfile hosted in github and deploy a Pod using that docker image. You will also learn how to setup the github webhook to notify OpenShift of new code push/commit events in github, such that OpenShift will auto rebuild and redeploy Pods using the latest code/Dockerfile changes in your github.
## Prerequisites
For this tutorial you will need:
- Red Hat OpenShift Cluster 4.3 or above on IBM Cloud.
## Estimated Time
It will take you around 30 minutes to complete this tutorial.
## Steps
- [Create a GitHub repo and host a Dockerfile](https://github.com/nerdingitout/oc-docker-s2i#create-a-github-repo-and-host-a-dockerfile)
- [Create Project](https://github.com/nerdingitout/oc-docker-s2i#create-project)
- [Create a pod deployment using the Dockerfile from your GitHub repo](https://github.com/nerdingitout/oc-docker-s2i#create-a-pod-deployment-using-the-dockerfile-from-your-github-repo)
- [Verify that the container process matches the command specified in the Dockerfile](https://github.com/nerdingitout/oc-docker-s2i#verify-that-the-container-process-matches-the-command-specified-in-the-dockerfile)
- [Setup GitHub Webhook](https://github.com/nerdingitout/oc-docker-s2i#setup-github-webhook)
- [Make changes on your GitHub repository](https://github.com/nerdingitout/oc-docker-s2i#make-changes-on-your-github-repository)

## Fork the GitHub repo and host a Dockerfile
- Fork this GitHub repository, by clicking the Fork button at the top of this repo. You will be making changes in the Dockerfile to trigger new pod deployments at a later step in this tutorial.
The repository has a Dockerfile that contains the following lines of code.
```
# simple dockerfile to test OCP s2i using Dockerfile 

FROM ubuntu:18.04
CMD ["/bin/bash", "-c", "sleep infinity"]
# CMD ["/bin/bash", "-c", "--", "while true; do sleep 30; done;"]
```
## Create Project
- From the Administrator perspective, go to Projects and click 'Create Project' from the top right of the page. You can name the project 's2i-project'.
![image](https://user-images.githubusercontent.com/36239840/117288733-dc4acf00-ae7c-11eb-9a52-9d41958c0990.png)
- Once created you will be redirected to the Overview page of the project
![image](https://user-images.githubusercontent.com/36239840/117288822-f71d4380-ae7c-11eb-9cbc-46c1a720c3ca.png)

## Create a pod deployment using the Dockerfile from your GitHub repo
- In the OpenShift web console, switch from the Administrator perspective to the Developer perspective and select to create an application 'From Dockerfile'.
![image](https://user-images.githubusercontent.com/36239840/117294441-b37a0800-ae83-11eb-8264-879e2939553f.png)
- Add the URL of your forked repo in the 'Git Repo URL' field and context dir ```/ubunutu```where the Dockerfile is located.
![image](https://user-images.githubusercontent.com/36239840/117295616-1e780e80-ae85-11eb-852b-011e5be0dc13.png)
- Scroll down to Resources section and select ```Deployment Config``` and keep the 'create a route to the application' checked then click 'Create'.
![image](https://user-images.githubusercontent.com/36239840/117295963-8cbcd100-ae85-11eb-80c7-5972beb45822.png)
- You will be redirected to the topology view which shows the pods created. Once it successfully builds, you will notice that the circle around your application turns dark blue.
- If you click on your application, it will show the Deployment Config view where you can view details, resources and logs. What happens here is that OpenShift builds the docker image using Dockerfile from your github repo, creates a docker image, uploads the image into OpenShift’s internal image registry and creates a Pod using that docker image.
![image](https://user-images.githubusercontent.com/36239840/117296212-d86f7a80-ae85-11eb-922e-cbafe2e2bb31.png)

## Verify that the container process matches the command specified in the Dockerfile
- From the Topology view, click on the `DeploymentConfig`. 
![dc](https://user-images.githubusercontent.com/36239840/122884433-38938080-d34f-11eb-85ac-2ab375c57ffc.JPG)
- Click on `Pods` to list the running Pod
![pods](https://user-images.githubusercontent.com/36239840/122884614-611b7a80-d34f-11eb-9453-146ce278ce0f.JPG)
- Select the running pod to enter the pod details view.
![select pod](https://user-images.githubusercontent.com/36239840/122884761-81e3d000-d34f-11eb-8e89-71c0bc252bf3.JPG)
- Click on `Terminal`, which brings up a Terminal (Shell) inside your running Ubuntu Container.
![terminal](https://user-images.githubusercontent.com/36239840/122886209-cfad0800-d350-11eb-80f5-b635e3563795.JPG)
- Verify that the pod is running `sleep infinity` process as specified in the Dockerfile by typing in the terminal the following command as shown in the screenshot.
```
ps aux | grep sleep
```
![sleep](https://user-images.githubusercontent.com/36239840/122886717-421de800-d351-11eb-93eb-b2a9f4893d60.JPG)

## Setup GitHub Webhook
Github Webhooks allow external services to be notified when certain events happen.
For auto-deploy (updates to the github Dockerfile auto deploys new Pods) to work, you will need to configure our github repo with the webhook that OpenShift provides as part of the `BuildConfig`. This is best achieved using the GUI.
- From menu to the left, go to 'Builds' then select the build config of your application.
![build](https://user-images.githubusercontent.com/36239840/122889220-a80b6f00-d353-11eb-8d09-a7d3a77d516c.JPG)
- You will be redirected to the details view of the build config. Scroll down till you get to the webhooks section and copy the URL with secret for generic webhooks.
![github webhook](https://user-images.githubusercontent.com/36239840/122907130-e14bdb00-d363-11eb-8e91-a1e9f0f696d5.JPG)
- Go to your GitHub repo, under Settings, click on Webhook option then click 'add Webhook' button.
![webhook](https://user-images.githubusercontent.com/36239840/122892643-9e373b00-d356-11eb-8fdd-43b92559db0a.JPG)
- Paste the copied URL into `Payload URL` field and select application/json option in `Content type` field, leave everything rest to defaults and click on `Add webhook`
![url](https://user-images.githubusercontent.com/36239840/122893252-359c8e00-d357-11eb-9784-5852eb16b802.JPG)
- Click on your newly added webhook to see details. Scroll down to see the `Recent Deliveries` section with an entry for PING test prefixed with a tick mark , which indicates the ping test was successful. Click on the entry to get more details about the REST API call and the associated response for the ping test.
A successful PING test would mean Github is able to connect with your OpenShift cluster
![ping](https://user-images.githubusercontent.com/36239840/122895324-0f77ed80-d359-11eb-8a94-e323c6541a0f.JPG)

## Make changes on your GitHub repository 
In this section, you will make a small change in Dockerfile, you will change the CMD used to keep the Container alive and commit the change. This should trigger a push event from Github to the OpenShift which will cause OpenShift to re-build the Docker image and re-deploy the pod using the newly built Docker image
- Go to the Dockerfile in github, edit the file, comment the first command and uncomment the second command as shown in the screenshot below. Then commit your changes
![image](https://user-images.githubusercontent.com/36239840/122897495-1e5f9f80-d35b-11eb-8ac5-dbdd93855301.png)
- Switch to your OpenShift GUI, go to Administrator view and click on `Builds`. You will notice a new Build is initiated automatically as shown in the following image. Once this build is done, a new Pod will be created using the new docker image that was built.
![buildss](https://user-images.githubusercontent.com/36239840/122904339-4b16b580-d361-11eb-86f8-f2f84a963323.JPG)
- Click on `Pods` view to see that the old Pod is being Terminated and new Pod is being created
![podss](https://user-images.githubusercontent.com/36239840/122904544-80bb9e80-d361-11eb-8c6a-b0886ca1837b.JPG)
- Click on the new Pod and go to the Terminal to verify that the new Container indeed is running the new process specified in the Dockerfile. Use the following command in the terminal.
```
ps aux | grep sleep
```
- You will get output similar to the following.
![new](https://user-images.githubusercontent.com/36239840/122905023-ead44380-d361-11eb-96c6-33f84c8e9b4a.JPG)
- If you go to your Github webhooks view, you will see a new entry under `Recent Deliveries` which maps to the recent notification delivered by Github to OpenShift in response to the new commit in the repo.
![image](https://user-images.githubusercontent.com/36239840/122908220-f07f5880-d364-11eb-8a7a-6235c61a52b7.png)

## Summary
In this tutorial, you have successfully demonstrated source-to-image (S2I) capability using Dockerfiles. This use case shows how to deploy a pod from GitHub hosted Dockerfile, setup the connection between OpenShift and GitHub using webhooks and ensure that new code changes to the repository results in new pod being deployed on the OpenShift cluster.
