# Docker

This repo documents my understanding of Docker. The structure of my notes are as follows, and more details on each topic is presented in the Markdown files, named consistently with the first-level headings below, attached to this repo.

## 1. Introduction

### What is Docker:

Docker is a platform for **building, running, and shipping pplications** in a **consistent manner**. It solves the problem of "my application works on my machine though!:)". The three reasons behind this problem could be:

+ one or more files missing (your application is not completely deployed and misses something)
+ software version mismatch 
+ different configuration settings like different environemnt variables across different machines 

Docker comes to rescue to solve the above issues. 
 
### Virtual machines vs. Containers

A container is an isolated environment for running an application. 

Virtual machine is an abstraction of a machine (physical hardware) and we can run several virtual machines on a real physical machine. We achive tghis using a tool called **hypervisor**. In a simple term, a hypervisor is a software we use to create and manage virtual machines. There are many hypervisors out there like:

+ VirtualBox
+ VMware
+ Hyper-v (Wondows only)

Benefits of building a virtual machines is we can run an application in isolation.

The problems of using virtual machines:

+ each VM needs a **full-blown OS**, that needs to be licensed, patched, and monitored 
+ slow to start, because the entire OS has to be loaded just like starting your computer 
+ resource intensive, because each VM takes a slice of actual physical hardware resources like CPU, Memory and disk space

So due to the above issues we h+may have limited number of VMs on our machine. 

Containes compatred to VMs:

+ give us the same kind of isolation so we can run multiple applications in isolation
+ they are more lightweight compared to VMs and so do not need the full OS
+ all the containers on a machine use OS of the host, meaning all the containers share the OS of the host (more accurately, they share the kernel (the core or the engine) of the host OS) so we only need to license, patch, and monitor a single OS of the host
+ start quickly usually in seconds sometimes less
+ need less hardware resources on the host 

so due to the above benefits we can run even 100s of containers on a single machine side by side.

### Docker architecture

Docker uses a client-server architecture. So it has a client component that talks to a server component using a REST API. The server, also called docker engine, sits in the background and takes care of building and running docker containers. 

### Development workflow

In our development workflow when using docker, we take an application and dockerize it meaning we make a small change to it so that it can be run by docker. How? We just add a docker file to it.

**Docker file** is a plain text file that includes instructions that docker uses to package our application into an image. This image contains everything our application needs to run. Typically, an image contains:

+ a cut-down OS
+ a runtime environment (like Node or python)
+ application files
+ third-party libraries
+ environment variables
+  ...

so we create a docker file and give it to docker for packaging our application into an image. When we have an image we tell docker to start an application using that image. A container is just a process, a special process, because it has its own files system, which is provided by the image. So our application gets loaded inside the container or process and this is how we run our application locally on our development machine like 

    docker run ... 

this way we run our application inside a container in an isolated environment. 

Once we have the image we can push it to the docker registry like docker hub. Docker hub to docker is like GitHub to Git. It is a storage for docker images that anyone can use. So once our application's image is on docker hub then we can pull it on any machine running docker. This machine has the same image as we have in our development machine which contains a specific version of our application with everything it needs. So we can start our application the same way we started in our development machine. We just tell docker to start a container using this image. So with docker we no longer need to maintain a long complex released documents that has to be precisely followed. All the instructions for buildiong an image of our application are written in a docker file. With that we can package our application into an image and run it virtually anywhere 


Course: "The Ultimate Docker", instructor: Mosh Hamedani

