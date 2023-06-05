# Docker

This repo documents my understanding of Docker. The structure of my notes from the "The Ultimate Docker" course by Mosh Hamedani is as follows. For my hands-on experience with Docker, please visit the following link:


# Table of content

1. [Introduction](#1)
    1. [What is Docker](#2)
    2. [Virtual machines vs. Containers](#3)
    3. [Docker architecture](#4)
    4. [Development workflow](#5)
    5. [Docker in action](#6)

2. [The Linux Command Line](#7)
    1. [How to run/pull an image from docker hub](#8)
    2. [Managing packages](#9)
    3. [Navigating the file system](#10)
    4. [Manipulating files and directories](#11)
    5. [Editing and viewing files](#12)
    6. [Redirection](#13)
    7. [Searching for text (string) in a file using grep command](#14)
    8. [Finding files and directories](#15)
    9. [Chaining Commands](#16)
    10. [Managing environment variables](#17)
    11. [Managing processes](#18)
    12. [Managing users and groups](#19)
    13. [File permissions](#20)

3. [Building Images](#21)
    1. [Images vs. containes](#22)
    2. 
10. [Reference](#20)

<a name="1"></a>
## 1. Introduction

<a name="2"></b>
### What is Docker:

Docker is a platform for **building, running, and shipping applications** in a **consistent manner**. It solves the problem of "my application works on my machine though!:)". The three reasons behind this problem could be:

+ one or more files missing (your application is not completely deployed and misses something)
+ software version mismatch 
+ different configuration settings like different environemnt variables across different machines 

Docker comes to rescue to solve the above issues. 
 
 <a name="3"></b>
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

So due to the above issues we may have limited number of VMs on our machine. 

Containes compatred to VMs:

+ give us the same kind of isolation so we can run multiple applications in isolation
+ they are more lightweight compared to VMs and so do not need the full OS
+ all the containers on a machine use OS of the host, meaning all the containers share the OS of the host (more accurately, they share the kernel (the core or the engine) of the host OS) so we only need to license, patch, and monitor a single OS of the host
+ start quickly usually in seconds sometimes less
+ need less hardware resources on the host 

so due to the above benefits we can run even 100s of containers on a single machine side by side.

<a name="4"></b>
### Docker architecture

Docker uses a client-server architecture. So it has a client component that talks to a server component using a REST API. The server, also called docker engine, sits in the background and takes care of building and running docker containers. 

<a name="5"></b>
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

Once we have the image we can push it to the **docker registry** like **docker hub**. **Docker hub to docker is like GitHub to Git**. It is a storage for docker images that anyone can use. So once our application's image is on docker hub then we can pull it on any machine running docker. This machine has the same image as we have on our development machine which contains a specific version of our application with everything it needs. So we can start our application the same way we started in our development machine. We just tell docker to start a container using this image. So with docker we no longer need to maintain a long complex released document that has to be precisely followed. All the instructions for buildiong an image of our application are written in a docker file. With that we can package our application into an image and run it virtually anywhere 

<a name="6"></b>
## Docker in Action

Some points:
+ To package our application, we typically start from a base image. I can find the base images from dockerhub, which is a registry for docker images, (https://hub.docker.com/) and then add additional files on top of it, it is like inheritance in programming. We put all the instruction to package our application in the Dockerfile (**without an extension**) in our app directory. The following is an example of a Dockerfile:

            FROM node:alpine
            COPY . /app (we need to copy our application/program files, we want to copy all the files in the current directory into the app directory into that image, so that image has a file system and in that file system we want to create a directory called app)
            WORKDIR /app
            CMD node app.js
        
+ Then we go to terminal to tell docker to package our applciation:

            docker build -t hello-docker . (in the end we need to specidy where docker can find the Dockerfile, I put period because the Dockerfile is in the current directory)
            
+ to see all the images in the computer 

             docker images OR docker image ls
             
+ Now that I craeted an image I can run it on any computer running docker like

             docker run hello-docker (the image name in this case is hello-docker. Also it does not matter which directory I am in, because this image contains all the files for running our application) 
             
 + I can publish my image to dockerhub so that everyone can pull and run it on any machine   
 +  https://www.docker.com/play-with-docker/ is pretty cool, which gives you virtual machine which has Linux and Docker. Here I can experiment and pull the docker images already published on dockerhub like
 
            docker pull path_to_the_image_on_dockerhub
            docker images
            docker run name_of_the_image
+ to delete a docker image:

            docker rmi name_of_image
            
BUT I may need to first remove the container, to list all the containers on my system, including the stopped ones:
            
            docker ps -a
            
then 
            
            docker rm container_name
            
then I can remove the image:
            
            docker rmi hello-docker
            
When you remove a Docker image, the space it occupied on your disk is not automatically freed up. This is because Docker uses a layered file system, and each image is built on top of a base image with multiple layers. These layers are shared among images and containers, which allows for efficient storage and reduces redundancy. However, you can use the following command to clean up unused images, containers, networks, and volumes, thereby reclaiming disk space. Here's how you can do it:
            
            docker system prune 
            
        
<a name="7"></a>
## 2. The Linux Command Line

<a name="8"></a>
### How to run/pull an image from docker hub

Docker is built on basic Linux concepts. 

To run ubuntu image:

        docker run ubuntu (if we have this image locally it'll start with it if not it is pulled from docker hub)
        
now:

        docker ps (which gives me back the list of running processes or containers)

I got nothing back, but 

        docker ps -a
        
gives me back the stopped containers as well, here I can see the ubuntu container I started above.

to start a container and interact with it we have to:

        docker run -it ubuntu ("it" is short for interactive)

which gives me a shell back in terminal. Shell takes our commands and passes them to the operating system for execution. in my case I have the following **shell prompt**:

        root@944e5ad2d814:/# 
        
so I logged in as root user, the default, after the @ I have the name of the machine/container automatically generated by docker, after the colon specifies where we are in the file system, in my case it is "/" meaning I am in the root directory, the highest directory in the file system, and '#' means that I am logged in as root user with the highest privilege. If I was logged in as a noremal user instead of # I would have had $. 

<a name="9"></b>
### Managing packages

Most operating systems and development platforms come with a package manager like:

+ pip
+ npm
+ yarn
+ NuGet
+ ... 

In ubuntu we also have a package manager called **apt** which is short for **advanced packege tool**. apt is the newer package manager we also have apt-get. But we work with apt which is easier to work with.

**take away:** before installing a package I should always run **apt update** to update my package database then I can install it like **apt install package_name**. 

        apt update
        apt list (gives me back a list of all the packages in the database installed or uninstalled)
        apt install nano
        apt remove nano

we can clear the terminal window by pressing ctrl + l.

<a name="10"></b>
### Navigating the file system 

        pwd # to print the working directory
        ls # to list the files and directories
        ls -l # to print a long list
        cd / # to go to the root directory
        a quick point: in front of cd we can have relative or absolute path, a relative path is relative to where we are and in contrast the absolute path always starts with the root directory 
        cd bin # to go to the bin directory
        cd .. # to go one level up
        cd ../.. # to go two levels up
        cd ~ # to go to the home directory
        ls path  if i want to see the contents of a directory without navigating to it I can specify the path in front of ls, we can have relative or absolute path 

When you use the command cd /, you are changing to the root directory of the file system.By default, the root directory contains various system directories, including a directory called "root." This "root" directory is different from the root directory itself. It is typically used as the **home directory for the root user**, which is the superuser or administrator of the system. Only the root user can access this directory. 

Inside the root directory (cd / then ls) we have the home directory, which is where howe directories for the users are stored so at a machine with multiple users each user has a home directory here. 

<a name="11"></b>
### Manipulating files and directories

        mkdir test # to create the test directory
        mv test docker # to rename a directory
        touch file.txt # to create a file in this case file.txt, we can also create multiple files using touch command in one go like touch file1.txt file2.txt
        a quick point: to remove an entire word in one go ---> ctrl + w
        mv file.txt hello.txt # to rename a file
        rm hello.txt # to remove a file
        rm file* # we can use a pattern to remove all files start with file for example 
        rm -r docker # to recursively remove a **directory** and all its contents recursively 

<a name="12"></b>
### Editing and viewing files 

To see the contents of a file we have several options:
+ cat file1.txt (cat is short for concatenation) --> good for short files
+ for long files, sometimes we don't want to see all the contents of a file in one go (which is the case with using the cat command) and instead we want to scroll the content ---> use more and less commands: 
    + more file1.txt (this is supposed to be replaced with less command because you can only scroll down and NOT up) press enter to go down one line at a time and space to go one page. to exit we press **q** 
    + less file1.txt, up and down arrows let us go up and down, we have the same functionality for space and enter as for more command, and to exit we press **q** 
+ head -n 5 /etc/adduser.conf
+ tail -n 5 /etc/adduser.conf 

Some summary:

        nano file.txt # to edit file.txt
        cat file.txt # to view file.txt
        less file.txt # to view with scrolling capabilities
        head file.txt # to view the first 10 lines
        head -n 5 file.txt # to view the first 5 lines
        tail file.txt # to view the last 10 lines
        tail -n 5 file.txt # to view the last 5 lines

side note: to search the content of the terminal in Linux: Press Ctrl + Shift + F to open the search bar at the top of the terminal window.

<a name="13"></b>
### Redirection 

Concept of standard input and output:

The standard input represents the keyword and the standard output represents the screen. But we can always change the source of the input and output, which is called redirection. Some examples:

    cat file1.txt (prints the content on the standard output (screen)) but I can redirect to write the content to a new file like
    cat file1.txt > file2.txt 
    
we can use cat command to concatenate or combine multiple files:

    cat file1.txt file2.txt (cat reads data from  both files and print them on the terminal)
    cat file1.txt file2.txt > combined.txt (writes the results to a different file using a redirection operator)
    
The redirection operator is not limited to the cat command and we can almost use it everywhere.

    echo hello (we see the result on the terminal)
    echo hello > hello.txt 
    
if we want to write a single line to a file we may use echo instead of using nano

    echo whatever > whateverfile.txt 
    
to get the long list of the files in the etc directory and write them to a file

    ls -l /etc > file.txt

using **> operator** we can redirect standard output (pretty useful) and using **< operator** we can redirect the standard input, which is not that useful!!

<a name="14"></b>
### Searching for text (string) in a file using grep command

        grep hello file.txt # to search for hello in file.txt
        grep -i hello file.txt # case-insensitive search
        grep -i hello file1.txt file2.txt 
        grep -i hello file*.txt # to search in files with a pattern like in files whose names start with file 
        grep -i -r hello . # to search in the current directory and all its subdirectories
        grep -ir hello . (gives us the exact same results as above) 

<a name="15"></b>
### Finding files and directories

        find # to list all files and directories in the current directory recursively 
        find /etc # to list all the files and directories starting from etc 
        find -type d # to list directories only
        find -type f # to list files only
        find -name “f*” # to filter by name using a pattern
        find -type f -name "f*" # to find all the files whose names start with f, like everything else in Linux this search is also case sensitive so you got different results if you do: 
        find -type f -name "F*"
        to make the search case insensitive:
            find -type f -iname "F*"
        find / -type f -name "*.py" > python.txt 
        
<a name="16"></b>
### Chaining Commands

In linux we have different ways for chaining/combining multiple commands:

#### using semicolon:

    mkdir test;cd test;echo done
    
after pressing enter all these commands will be executed one after the other. we may add a space around these semicolons to make it more readable, which is of course optional:

    mkdir test ; cd test ; echo done 
    
in case one of these commands is not executed, like we already have a directory called test, the rest will be executed. What if we want to stop execution like if one command fails the rest is not executed:

we can use and operator (&&):

#### using and operator (&&):

    mkdir test && cd test && echo done
    
#### using or operator (||):
we also have or (||) operator:

    mkdir test || echo "directory exists"

These techniques are extremely useful when we want to deploy our app in docker!

#### using piping (|):
Another way to chain commands is piping (|), which is very powerful:

    ls /bin | less (we get the content of the bin directory and then we are creaing a pipe like what comes out of the ls /bin command goes into the second command which is less in this example, now less does not need a file name because it gets the input from the first command ls /bin in this case)
    ls /bin | head -n 5
    
sometimes when dealing with a long command our command sequence might look hard to read and so we split it into multiple lines using backslash (\):

    mkdir test;\
    cd test;\
    echo done
    
  

<a name="17"></b>
### Managing environment variables HERE

        printenv # to list all variables and their value
        printenv PATH # to view the value of PATH
        echo $PATH # to view the value of PATH
        export name=bob # to set a variable in the current session

<a name="18"></b>
### Managing processes

        ps # to list the running processes
        kill 37 # to kill the process with ID 37

<a name="19></b>
### Managing users and groups

        useradd -m john # to create a user with a home directory
        adduser john # to add a user interactively
        usermod # to modify a user
        userdel # to delete a user
        groupadd devs # to create a group
        groups john # to view the groups for john
        groupmod # to modify a group
        groupdel # to delete a group

<a name="20"></b>
### File permissions

        chmod u+x deploy.sh # give the owning user execute permission
        chmod g+x deploy.sh # give the owning group execute permission
        chmod o+x deploy.sh # give everyone else execute permission
        chmod ug+x deploy.sh # to give the owning user and group
        # execute permission
        chmod ug-x deploy.sh # to remove the execute permission from
        # the owning user and group

<a name="21"></a>
## 3. Building Images

Topics will be discussed:
+ creating docker files
+ versioning images
+ sharing images
+ saving and loading images
+ reducing the image size
+ speeding up builds

<a name="22"></a>
### Images vs. containes

An image includes everyhting (all the files and configuration settings) an application needs to run like
+ a cut-down OS
+ application files
+ third-party libraries
+ environment variables
+  ...

Once we have an image we can start a container from it. A container is kind of liek a virtual machine in a sense that:
+ it provides an isolated environment for executing an application
+ can be stopped & restarted like VMs
+ a container is technically an operating system process, a special process because it has its own file system provided by the image

to see the running processes or containes:

        docker ps
        
how to start a new container from the same image? to start a container from an image:

        docker run -it name_of_the_image

the container gets its file system from the image but each congtainer has its own write layer so what we write in a given container is invisible from other containers, of course there is a way to share data between containers and we will talk about it later in the course. But it is critical to understand that each container is an isolated environemnt for executing an application, it is an isolated universe. 






<a name="10"></a>
## 10. Reference
Course: "The Ultimate Docker", instructor: Mosh Hamedani

