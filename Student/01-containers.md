# Challenge 1: Got Containers?

[< Previous Challenge](./00-prereqs.md) - **[Home](../README.md)** - [Next Challenge >](./02-acr.md)

## Introduction

The first step in our journey will be to take our application and package it as a container image using Docker.

## Demo
Your coach will demonstrate running the application natively without using containers.

## Description

In this challenge we'll use Dockerfiles to build container images of our app, and we'll test them locally.

### First a demo.

The instructor will demo the application running on a Linux virtual machine, non-containerized.

### Building the Docker Image

Our Fab Medical App consists of two components, a web component and an api component.  Your task is to build containers for each of these.  You have been provided source code for the application (no need to edit), but you need to create the Dockerfiles (one for each).  Dockerfiles are the standard build definition tool for containerizing applications. 

### Success Criteria

- Build Docker images for both `content-api` and `content-web`
- Run both containers you just built and verify that the app is working. 
	- **Hint:** Run the containers in 'detached' mode so that they run in the background.
	- The content-web app expects an environment variable named `CONTENT_API_URL` that points to the API app's URL.
	- **NOTE:** The containers need to run in the same network to talk to each other. 
		- Create a Docker network named "fabmedical"
		- Run each container using the "fabmedical" network
		- **Hint:** Each container you run needs to have a "name" on the fabmedical network and this is how you access it from other containers on that network.
		- **Hint:** You can run your containers in "detached" mode so that the running container does NOT block your command prompt.

## Learning Resources

See Docker documentation:  
[Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
[Docker command line reference](https://docs.docker.com/engine/reference/commandline/cli/)
[Docker multi-container reference](https://docs.docker.com/get-started/07_multi_container/)

Reference articles on how to Dockerize a Node.js app:
- <https://nodejs.org/en/docs/guides/nodejs-docker-webapp/>
- <https://buddy.works/guides/how-dockerize-node-application>
- <https://www.cuelogic.com/blog/why-and-how-to-containerize-modern-nodejs-applications>