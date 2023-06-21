# Docker

This repo documents my understanding of Docker. The followings are my notes from the course "The Ultimate Docker" by Mosh Hamedani. For my hands-on experience with Docker, please visit the following link:


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
    12. [Managing users](#19)
    13. [Managing groups](#20)
    14. [File permissions](#21)

3. [Building Images](#22)
    1. [Images vs. containes](#23)
    2. [Running an application on a brand new machine](#24)
    3. [Dockerfile Instructions](#25)
    4. [Choosing the Right Base Image](#26)
    5. [Copying Files and Directories](#27)
    6. [Excluding Files and Directories](#28)
    7. [Running Commands](#29)
    8. [Setting Environment Variables](#30)
    9. [Exposing Ports](#31)
    10. [Setting the User](#32)
    11. [Defining Entrypoints](#33)
    12. [Speeding Up Builds](#34)
    13. [Removing Images](#35)
    14. [Tagging Images](#36)
    15. [Sharing Images](#37)
    16. [Saving and loading Images](#38)

 4. [Working with Containers](#39)
    1. [Introduction](#40)
    2. [Starting containers](#41)
    3. [Viewing the Logs](#42)
    4. [Publishing Ports](#43)
    5. [Executing Commands in Running Containers](#44)
    6. [Stopping and Starting Containers](#45)
    7. [Removing Containers](#46)
    8. [Containers File System](#47)
    9. [Persisting Data using Volumes](#48)
    10. [Copying Files between the Host and Containers](#49)
    11. [Sharing the Source Code with a Container](#50)

 5. [Running Multi-container Applications](#51)
    1. [Introduction](#52)
    2. [Installing Docker Compose](#53)
    3. [Cleaning Up our Workspace](#54)
    4. [The Sample Web Application](#55)
    5. [JSON and YAML Formats](#56)
    6. [Creating a Compose File](#57)
    7. [Building Images](#58)
    8. [Starting and Stopping the Application](#59)
    9. [Docker Networking](#60)
    10. [Viewing Logs](#61)
    11. [Publishing Changes](#62)
    12. [Migrating the Database](#63)
    13. [Running Tests](#64)
   
6. [Deploying Applications](#65)
    1. [Introduction](#66)
    2. [ Deployment Options](#67)
    3. [Getting a Virtual Private Server](#68)
    4. [Installing Docker Machine](#69)
    5. [Provisioning a Host](#70)
    6. [Connecting to the Host](#71)
    7. [Defining the Production Configuration](#72)
    8. [Reducing the Image Size](#73)
    9. [Deploying the Application](#74)
    10. [Troubleshooting Deployment Issues](#75)
    11. [Publishing Changes](#76)
    
7. [Reference](#77)

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
            
            docker system prune (if you have any stopped containers that you didn't remove explicitly, running docker system prune will remove those containers along with other unused resources.)
            
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

To exit from the container just run **exit** command. Now, how to start an stopped container?

        docker ps -a

        docker start -i name_of_container_you_want_to_start
    
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
### Managing environment variables 

Just like variables in programming, we have environment variables in Linux which we can set for storing configuration settings for our application. So our application can read the configuration settings from these environment variables. Let's view and set them:

        printenv # to list all environment variables and their value on our machine 

We have a very important environment variable called **PATH**. Sometimes when we run a program from the command line and we get the error of program/command was not found, even though we installed it. It happens quite often because our operating system cannot find that program. To find a program our operating system is not going to our entire hard drive and instead it only looks at specific directories and those directories are specified using the PATH environemnt variable, which is set to a list of directories separated by a colon. To see a value of a particular variable:

        printenv PATH
or

        echo $PATH (we have to prefix the variable name with $ to specify that the variable is an environment variable) 

to set a variable we use export command:

        export DB_USER=danial 
        
This only sets a variable in the **current terminal session** meaning if we start over another session/terminal we lose this variable. To solve this issue and set the variable permanently we have to write it to a special file called **.bashrc** (this file is the user personal start off file meaning everytime the user logs in Linux loads this command from the user's home directory) which is in the home directory. So this is where we need to write permanent environment variables:

        cd ~
        nano .bashrc
        DB_USER=danial

another approach is to use redirection of standard output that we learned. But here there is a **very important point**: I do need to use **>>** to make sure I append to the .bashrc, I do NOT have to use only one single > which overwrites the content of .bashrc file:

        echo DB_USER=danial >> .bashrc
        cat .bashrc
        
One last point: the changes we make to the .bashrc file are only effective in the next terminal session. This is because the .bashrc file only loads once when we start a terminal session and so to make the changes effective I have to either terminate the session and start over another terminal session or source the .bashrc file (just remember we need to execute the following command from our home directory where the .bashrc file lives):

        source .bashrc
        
if not in home directory:

        source ~/.bashrc 

Point: you should never store sensitive info in the environment variables because at the end of the day these are stored in the plain text files and everyone who has access the machine can find them. 

<a name="18"></b>
### Managing processes

A process is an instance of a running program.

        ps # to list all the running programs/processes
        
the TIME shown as in the output is the amount of CPU time each process consumed, in the future if you notice your system gets slow it may be because some processes take so much of CPU time and in that case you may want to kill that process:

        kill 37 # to kill the process with ID 38

<a name="19"></b>
### Managing users

How to create a new user and log in as that user?

        useradd -m john # to create a new user with a home directory, the name of user as john

where is this user? in the configuration file in the etc directory. We can use cat to look at that

        cat /etc/passwd # the name passwd is misleading the passwords are not here and here we only have user account information

if I want to have bash instead of sh (which is the old original shell program (bash is the enhanced version of this program)) as the shell program used when this user logs in, I can use usermod to modify this record:

        usermod -s /bin/bash john

where are the passwords? in the file named shadow in the same directory, this is where passwords are stored in an encrypted format. This file is only accessible to the root user:

        cat /etc/shadow

how to login to our container as john?

in a new terminal, first to see the running containers:

        docker ps
       
we want to execute a bash session inside the container:

        docker exec -it container_id bash
        
 but again we will be logged in as root! But we wanted to login as john:
 
        docker exec -it -u john container_id bash  

here at the end of the shell prompt, I have $ rather than # because john is not the root user but a regular user, in contrast in my other terminal which I logged in as root to my container I have # at the end of the prompt which indicates I am logged in as root user with extra priviledges.

In the terminal logged in as john if :

        cat /etc/shadow

I got the permission denied error!

Also in the terminal logged in as john, if:

        cd ~
        pwd

I go bask:

        /home/john
 
 in this directory we can keep john's files. when we are done with this user we can delete it using 
 
        userdel john 
        
        
We also have another command adduser. What is the difference between useradd and adduser?

useradd is the original API that was built but adduser is more interactive and uses useradd under the hood. Using adduser we can set a password and specify additional info about the user like full name, room number, work phone etc. Quite often when using docker for deploying our application we don't want to use adduser because we don't want to interact with this command

<a name="20"></b>
### Managing groups 

We use groups to manage permissions like all the users in the same group have the same permission. In the same way as with users, we have three commands:

+ groupadd
+ groupmod
+ groupdel

Let's create a new group called developers

        groupadd developers

where is this group:

        cat /etc/group # to see all the groups in our image
        
let's add the user john to this group:

we have two tyopes of groups: primary and supplementary: every Linux user has one primary group and zero or more supplementary groups, every file created by a user will be owned by one user with one group (which is the primary group). The primary group is automatically created when we create a new user it is a group with the same name as the new user. 
        
to set a group as the supplementary group for john:

        usermod -G developers john 

to see the groups of a user:

        groups name_of_user
        
now if i want to create another group and add john to it:

        groupadd artists
        usermod -G artists john
        
then

        groups john
        
i got back:

        john : john artists
        
point: The usermod -G command in Linux **replaces the existing supplementary groups of a user** with the specified group(s) provided. To keep the previous supplementary groups I need to use -a as append:

        usermod -aG developers john
        
now 

        groups john
        
I got back
        
        john : john artists developers

to delete a group:

        groupdel group_name # to delete a group


<a name="21"></b>
### File permissions

side note: shell scripts are the files with extension .sh and can contain any Linux commands which will be executed one after the other (we can combine the commands we would like to create a deployment script). Let's create a simple shell script file (I logged in as root):

        cd /home
        echo echo hello > deploy.sh 
        
to see the permissions for this file:

        ls -l

some quick notes:

+ as a first letter when we have **d** it indicates directory and - indicates file (having execute permission for directory means that we can go inside it like doing cd to go inside it)
+ then we have 9 letters grouped as 3 to specify permissions (the first 3 is for user who owns the file, the second for the group who owns the file and the third 3 is for others): in each group we have r --> read w --> write x --> execute
 
        ./deploy.sh
        
i got permission denied error even I as a root user created the file. So I need to change permission using change mode command (chmod):

        chmod u+x deploy.sh

now I can 

        ./deploy.sh

I got back

        hello
        
now I log in as john like in new terminal

        docker exec -it -u john name_of_container bash 
        cd /home
        ls -l
        
now

        ./deploy.sh
 
I got the permission denied error. So if I want to give permission to others for this file like others like john to be able to execute it I go back to the terminal where I logged in as root and then:

        chmod o+x deploy.sh 

now if I go back to the other terminal logged in as john I will be able to execute the deploy.sh file

we can remove permission using -. 

        chmod u-x whateverfile.whateverextension
        
        OR
        
        chmod og+x+w-r *.sh
        
some quick summaries:

        chmod u+x deploy.sh # give the owning user execute permission
        chmod g+x deploy.sh # give the owning group execute permission
        chmod o+x deploy.sh # give everyone else execute permission
        chmod ug+x deploy.sh # to give the owning user and group execute permission
        chmod ug-x deploy.sh # to remove the execute permission from the owning user and group

            
**Some Exercises:**
1. Get the long list of files in the etc directory and write the output to a file:
    
        ls -l /etc > files.txt
            
2. Find all the python files in this image and write it to a file.txt
    
        find / -type f -name "*.py" > python.txt
        find -type f -name "*.py" > python.txt (only looks for the python files in the current directory and if I want to find all the python files in this image I have to specify the path which is the root directory (/) as above.
            
<a name="22"></a>
## 3. Building Images

**The first step to use docker to build and deploy applications is to create images.** So solid understanding of images is crucial. 

Topics will be discussed:

+ creating docker files
+ versioning images
+ sharing images
+ saving and loading images
+ reducing the image size
+ speeding up builds

<a name="23"></a>
### Images vs. containes

An image includes **everyhting (all the files and configuration settings) an application needs to run** like
+ a cut-down OS
+ application files
+ third-party libraries
+ environment variables
+  ...

To build an image:
        
        docker build -t desired_name_of_image . 

**Once we have an image we can start a container from it.** A container is kind of like a virtual machine in a sense that:

+ it provides an isolated environment for executing an application
+ can be stopped & restarted like VMs
+ a container is technically an operating system process, a special process because it has its own file system provided by the image

to see the running processes or containes:

        docker ps
        
how to start a new container from the same image? to start a container from an image:

        docker run -it name_of_the_image (if image is in local it uses it if not pulls it from docker hub)

the container gets its file system from the image but each container has its own write layer so what we write in a given container is invisible from other containers, of course there is a way to share data between containers and we will talk about it later in the course. But it is critical to understand that each container is **an isolated environemnt** for executing an application, it is an **isolated universe.**

Next, we dockerize a front end application, built with react, and package it into an image once we have that image we can deploy the application virtually anywhere. Let's see how!

<a name="24"></a>
### Running an application on a brand new machine

There are a number of steps we need to follow to run our app on a brand new machine:

+ install node (sudo apt install nodejs)
+ install dependencies of the application using npm install (which looks into the package.json file to find the required dependencies and automatically download and store all the thrid party libraries for our project). After that we will have a new directory called node_modules where we have all the dependencies, also we will have a new file package-lock.json 
+ from the project directory: npm start (to start a project)

we will have the same steps as above for other development stacks whether to use csharp, java, python, etc. we have some tools to manage the dependencies of our application and then we have a way to start our application. 

This starts at development server listening to the port 3000, if we go to port 3000 at localhost (localhost:3000) we can see our react application. So we saw how to run our app in a new machine, we will see how to use Docker so we don't have to repeat all these steps everytime we want to deploy our app on a new machine. So next we will dockerize our application and package it into an image. Once we have that image we can deploy our application virtually anywhere. 

some quick notes:
+ When you start a React app using npm start, it typically runs a development server that hosts your application locally on your computer. By default, the development server uses the address localhost and port 3000.
+ localhost: localhost is a hostname that refers to the current device you're using. It's commonly used to access services running on the same device. When you access localhost in your browser, you're essentially referring to your own computer.
+ Port 3000: Ports are used to identify specific processes or services running on a computer. Port 3000 is a commonly used port for local development servers. So when you start your React app and it runs on localhost:3000, it means the development server is listening on port 3000 of your local machine.

<a name="25"></a>
### Dockerfile Instructions

The first step to dockerize an application is to add a Dockerfile to it. **A Dockerfile contains instructions for building an image**. The following is a complete list of those instructions:

+ FROM to specify the base image 
+ WORKDIR to specify the working directory
+ COPY for copying files and directories
+ ADD for copying files and directories
+ RUN for executing operating system commands (we can execute all the Linux commands using RUN, if we are on windows we can execute windowns commands using RUN as well) 
+ ENV for setting environment variables
+ EXPOSE to tell docker our container starts at a given port 
+ USER to specify the user that should run the application, typically we want to run our application using a user with limited priviledges
+ CMD to specify the commands that should be executed when we start a container
+ ENTRYPOINT to specify the commands that should be executed when we start a container

<a name="26"></a>
### Choosing the Right Base Image

To add a Dockerfile into our app, in the root directory we create a new file named Dockerfile and inside it we specify the base image. **The base image can be an operating system like Linux or windows or the operating system plus the runtime environement like Node.js Runtime Environment or Python Runtime Environment etc.**. 

Some notes:

+ You can see Dockerfile samples for different technology stacks on docs.docker.com. 
+ Some of the images are not hosted on Docker hub like the microsoft images, which are hosted on microsoft container registry (mcr) in these cases we need to specify the full URL in front of FROM instruction, an image can be in any registry, the default registry that Docker uses is Docker hub but we can specify different registries using the full URL (don't blindly take the URL and always double check it because the URL or the version can change).

        FROM node:14.16.0-alpine:3 (here we specified the specific tag (14.16.0-alpine:3) and did not use 'latest' tag which is the default tag assumed by Docker. Never use latest tag because if you build your application against the latest vesion of node, then next time there is a new version of node and if you rebuild your application image, your application will be built with a different version of node and things can get unpredictable so ALWAYS use a specific version). 

side note: when you pull an image, Docker automatically downloads the right Docker image for your CPU architecture so we only need to specify tag and not the CPU architecture. 

to build an image, in the terminal:

        docker build -t react-apt . (we tell Docker to find the Dockerfile in the current directory) 
        
now to start a container with our image:  

        docker run -it react-app 
        
which gives me a node environment where we can write Java Script code, which is not what I wanted I wanted to run bash to be able to look at file system. So:

        docker run -it react-app bash (at the end we can specify the command to be run at the start of the container and here we specified bash) 
        
which gives me error since alpine Linux does not come with bash and alpine only has original shell program, so:

        docker run -it react-app sh
        
Now I got the shell.

In this image we only have alpine Linux and node v14.16.0 and we don't have our application files, next we will copy our application files into this image. 

<a name="27"></a>
### Copying Files and Directories

For copying applications' files and directories into the image we have two instructions:

+ COPY
+ ADD 

They have the exact same syntax but ADD has additional features:

        FROM node:14.16.0-alpine:3
        COPY package.json /app (to copy package.json file into app directory in the image if the app directory does not exist Docker automatically creates it) 

to copy more files:

        COPY package.json README.md /app/ (when we want to copy with more than one source files we need to end the destination directory with a forward slash) 

we can also use patterns:

        COPY package*.json /app/ 

to copy everything in the current directory into the app directory:

        COPY . /app/
 
 so far we used an absolute path for the destination (/app) (because our path starts with a forward slash), we can also use a relative path if we set the working directory first: using the WORKDIR instruction we can set the working directory and so all the instrctions come after will be executed inside this working directory and so we can replace the absolute path with the relative path:
 
        FROM node:14.16.0-alpine:3
        WORKDIR /app
        COPY . .  (copy everything from the current directory into the working directory specified above and indicated with a period here) 
        
what if we want to copy a file that has a space in it? let's say a file called **hello word.txt**. We need to use the other form of the COPY instruction, where we can pass an array of strings, each string represents an argument of the COPY instruction:

        COPY ["hello word.txt", "."] # will not be used that often but be aware of it

 As discussed we also have ADD which has the exact same syntax as COPY but with additional 2 features:
 
 + with ADD we can add a file from a URL
 
        ADD http://.../file.json .
      
 + we can pass a compressed file and ADD automatically uncompress the file into the directory 

        ADD file.zip . 
 
So the best practice is to use COPY, which has no magic around it and it is more staightforward, unless you need one of the above two additional features only provided by ADD. 
        
side note: all the files from the **current directory**, which is the root directory of our app, will be copied/added, since Docker engine does not have access to any files or directories outside the current directory. 

so my updated Dockerfile is:

        FROM node:14.16.0-alpine:3
        WORKDIR /app
        COPY . .  
        
let's rebuild our image:

        docker build -t react-app . 

then

        docker run -it react-app sh
 
 now on the shell we are inside the /app directory because in the Dockerfile we set this directory as the current working directory. What if we want to exclude some files or directories? we see next!

when mosh does ls he has a directory called node_modules that I don't have also i don't have package-lock.json! This should be because I did not initially run the app locally on my machine! The package-lock.json file is created after running the npm install command in a Node.js application. I went back and repeat steps in "Running an application on a brand new machine" and now I also have node_modules directory and package-lock.json file.


 
<a name="28"></a>
### Excluding Files and Directories

As we saw when we build our application, docker client takes everything in our app root directory, which is called the build context or build context directory, and send it to docker engine or docker daemon. For this simple application the size was aropund 150 MB which is mainly because of the node_modules directory. As our application gets more complex and we have more third party libraries this node_modules directory gets larger and larger. Also later when we want to deploy our app  our docker client will talk to a docker engine on a different machine which means whatever we have in the build context has to be transfered over the network. So if we have a large build context with a million files in it all these files have to be sent to the docker engine on the remote machine. We don't want that, we don't really need to transfer this node_modules directory because all these dependencies are defined in package.json file. So we can simply exclude this directory and copy everything else and then restore these dependencies on the target image. This has 2 benefits:

+ our build context is smaller so we transfer less data over the network 
+ our build will be faster and we don't have to wait for all these files to be transfered to docker engine

How to do that?

in the root directory of our app we create a file named **.dockerignore**, everything in lower case, (like .gitignore by whcih we exclude some files and directories from Git) inside it we list the files and directories we want to exclude in this case node_modules/. Doing so, then building an image, and then starting a new container from our image our container does not have node_modules directory in the file system of the container because we excluded it. So we will do npm install to install all the dependencises, as shown next:

<a name="29"></a>
### Running Commands
 
So the next step is to provide instructions in our Dockerfile to install our project dependencies using npm. To do so, we use RUN in our Dockerfile. With RUN command we can execute any command that we normally execute in a terminal session. So our updated Dockerfile will be:

        FROM node:14.16.0-alpine:3
        WORKDIR /app
        COPY . . 
        RUN npm install #(it finds the required dependencies from package.json file) 
 
 if for example we want to install Python on this image as well we will add RUN apt install python in the Dockerfile (side note is that alpine Linux does not have apt as the package manager and so this will not work we have to instead use apk as package manager, be aware of these details). 
 
rebuild an image:
 
        docker build -t react-app .
        
then

        docker run -it react-app sh
        
now if I do ls I do have node_modules directory and package-lock.json file in my file system. Perfect, next we will set the environemt variable!

<a name="30"></a>
### Setting Environment Variables

Sometimes we need tos et environemnt variables for example let's say our front-end application needs to talk to a backend or an API. Quite often we set the URL of an API using an environment variable. To do so we use ENV instruction. Our updated Dockerfile will be:

        FROM node:14.16.0-alpine:3
        WORKDIR /app
        COPY . . 
        RUN npm install
        ENV API_URL=http://api.myapp.com/ 
        
rebuild our image:

        docker build -t react-app .
        
then start a new container form it:

        docker run -it react-app sh
        
then I can

        prinvtenv # to see all the environment variables
        or
        printenv API_URL
        or
        echo $API_URL 
        
 **So whenever we start this container this environemnt variable is automatically set for us.**

<a name="31"></a>
### Exposing Ports

If we want to start our app outside docker, we go to the project directory and then npm start, which starts the development server on port 3000 and so if we go to localhost:3000 we can access our application. In the future when we run this application inside a Docker container this port 3000 will be open on the **container NOT on the host**. Understanding of this is important. So on the same machine we can have multiple containers running the same image all these containers will be listening to port 3000 but the port 3000 on the host is not going to be automatically mapped to these containers. So next we will see how to map a port on the host to a port on these containers. But for now, let's go back to the Dockerfile and modify it as follows:

        FROM node:14.16.0-alpine:3
        WORKDIR /app
        COPY . . 
        RUN npm install
        ENV API_URL=http://api.myapp.com/
        EXPOSE 3000 

with EXPOSE command we specify what port this container will be listening on. It is important to understand that the EXPOSE command does not automatically publish the port on the host and it is just a form of documentation to tell us this container will eventually listen on port 3000 so later when we properly run this application inside a docker container we know that we should map a port on the host to port 3000 on the container, which we will do next.
 
 <a name="32"></a>
### Setting the User

By default, Docker runs our application with root user, which can open up secutiry holes in our system. So to run this application we should create a regular user with limited previlidges. 

A side note: in alpine Linux which is the base of my image, specified below in the Docker file, we don't have groupadd and useradd command and instead we have addgroup and adduser.

        FROM node:14.16.0-alpine:3
        WORKDIR /app
        COPY . . 
        RUN npm install
        ENV API_URL=http://api.myapp.com/
        EXPOSE 3000
        RUN addgroup app && adduser -S -G app app 
        USER app 

some notes on above:
with -S we specify the user is system user for running our app and not a real user and with -G we set the primary group of the user, also keep in mind before creating a user we need to create a group to be able to add the user to it which we did with addgroup command. The last app is the name of the user and the one before that is the name of the group the user belongs to, the best practice in Linux is to have the same name for the user and its primary group. After creating the group and the user, we can use USER command to set the user, meaning all the follwoing commands will be executed using the set user. So after saving the Docker file let's rebiuild the image:

        docker build -t react-app . 

let's start a new container from this image and make sure the current user is the app user:

        docker run -it react-app sh

then to print the name of the user:

        whoami         
        
which prints 

        app
        
when we do 

        ls -l
        
we see that the current user, which is app, cannot write to any file in our application and just can read files and execute some of them which is good in terms of security. 

 <a name="33"></a>
### Defining Entrypoints

As we saw earlier, to start our application, in the project directory, we do npm start (which starts our application outside docker). So this is the command we should execute when starting a container. 

        docker run react-app npm start 
        
Above, I did not use -it because I did not want to interact with this container (did not want to run a shell session), we just want to start the application

Doing so we get permission error, because in our Docker file we set the user at the end, we executed all these previous instructions as the root user then  we switched to a regular user with limited privilidges so we should revise Docker file as follows:

        FROM node:14.16.0-alpine:3
        RUN addgroup app && adduser -S -G app app 
        USER app 
        WORKDIR /app
        COPY . . 
        RUN npm install
        ENV API_URL=http://api.myapp.com/
        EXPOSE 3000
        
now let's rebuild the image:

        docker build -t react-app . 
        
Unlike Mosh, I got the permission denied error because the /app directory has incorrect permission/ownership so I needed to revise the Docker file as follows:

        FROM node:14.16.0-alpine:3
        RUN addgroup app && adduser -S -G app app
        WORKDIR /app
        COPY . .
        RUN chown -R app:app /app
        RUN chmod -R 755 /app
        USER app
        RUN npm install 
        ENV API_URL=http://api.myapp.com/
        EXPOSE 3000
        
Doing so solves the permission error issue, then to rebuild the image:

        docker build -t react-app . 

now let's start a new container from  our image: 

        docker run react-app npm start
        
now I got the message successfully compiled and our web server started on port 3000, but as mentioned before this is **port 3000 of the container not the local host** so if we go to this address http://localhost:3000 we won't access the application. Next we see **how to map a port from the host to a port on the container.**

We don't want to specify the command npm start in 'docker run react-app npm start' everytime we start a container, this is where we use the command instructions (CMD) so:

        FROM node:14.16.0-alpine:3
        RUN addgroup app && adduser -S -G app app
        WORKDIR /app
        COPY . .
        RUN chown -R app:app /app
        RUN chmod -R 755 /app
        USER app
        RUN npm install 
        ENV API_URL=http://api.myapp.com/
        EXPOSE 3000
        CMD npm start 

using **the CMD instructuon we specify the default command to be executed**.  Now after rebuilding the image we can start a container from it using 'docker run react-app' (no need for npm start anymore).

remember because the CMD instruction is for supplying the **default command** it does not make sense to have multiple command instructions in the docker file, if we have multiple command instructions only the last one will take effect. 

Difference between CMD and RUN instructios:

using both we can execute commands. The RUN instruction is a **build-time instruction meaning it is executed at the time of building the image**, in contrast the **CMD instruction is a run-time instruction meaning it is executed when we start a container.**

the CMD instruction has two forms:

        # the shell form
        CMD npm start

this form is executed by the docker inside a separate shell that is why it is called the shell form, on Linux that shell is /bin/sh (the original shell program) and on Windows is cmd (command prompt) 

        # the execute form
        CMD ['npm', 'start'] 

with this form we can execute the command directly with no need to spin up another shell process also it makes it easier and faster to clean up the resources when you stop containers so always use this form as the best practice. So:
 
        FROM node:14.16.0-alpine:3
        RUN addgroup app && adduser -S -G app app
        WORKDIR /app
        COPY . .
        RUN chown -R app:app /app
        RUN chmod -R 755 /app
        USER app
        RUN npm install 
        ENV API_URL=http://api.myapp.com/
        EXPOSE 3000
        CMD ['npm', 'start'] 

We also have another instruction called ENTRYPOINT which is pretty similar to CMD and likewise, it has two forms: the shell and the execute forms.

        ENTRYPOINT npm start
        
or 

        ENTRYPOINT ["npm", "start"] 
        
Difference between ENTRYPOINT and CMD instructions:

We can always overwrite the default command when starting a container like:

        docker run react-app echo hello 

this echo hello command overwrites the default command which is CMD ['npm', 'start'] in my Docker file. In contrast, we cannot easily overwrite the ENTRYPOINT instruction when running the container like if you want to overwrite the ENTRYPOINT instruction we have to use --entrypoint option like the following, which is harder to overwrite the ENTRYPOINT instruction:

        docker run react-app --entrypoint echo hello 

in practical terms, we often want to use ENTRYPOINT when we know for sure this is the command that should be executed whenever we start a container with no exception, in contrast with the CMD instruction we have a bit more flexibility we can always overwrite it. So choosing between CMD and ENTRYPOINT is a matter of personal preference.

So far our Docker file is in good shape but it is slow we will optimize it next:

 <a name="34"></a>
### Speeding Up Builds

So far our builds are fairly slow. Every time we make a small change and we have to rebuild the image we have to wait almost half a minute. Let's optimize our build. The first thing to understand is the **concept of layers** in Docker. An image is essentially a collection of layers you can think of a layer as a small file system that only includes modified files. When Docker tries to build an image it executes each of the instructions in the Dockerfile and creates a new layer. That layer only includes the files that were modified as a result of that instruction. So for the first instruction, Docker takes the node image and put it in a layer (more accurately the node image itself is a couple of layers but for now don't worry about that). Similarly, for every instruction in the Dockerfile, Docker creates a new layer. To have a quick look at these layers:

        docker history react-app # react-app is the name_of_image

Now we can see all the layers and we have to read this list from bottom to up. An image is essentially a collection of these layers. 

Docker has an optimization mechanism built into it. Next time we ask Docker to build this image it looks at the first instruction and see if the instruction changed or not. If not it is not going to rebuild this layer and instead it is going to reuse it from its cache. That is the optimization. Then Docker looks at the second instruction and again if there is no change Docker reuses it from its cache and is not going to rebuild it. In contrast, if we make a tiny change in one instruction let's say 

        RUN addgroup app2 && adduser -S -G app app 

then Docker has to rebuild this layer.

        FROM node:14.16.0-alpine:3
        RUN addgroup app && adduser -S -G app app
        WORKDIR /app
        COPY . .
        RUN chown -R app:app /app
        RUN chmod -R 755 /app
        USER app
        RUN npm install 
        ENV API_URL=http://api.myapp.com/
        EXPOSE 3000
        CMD ['npm', 'start'] 
        
**COPY . .** is a special instruction. Because with this instruction Docker cannot tell if anything has changed or not. And it has to look at the content of files as well. And that means if we make a tiny change in our application Docker cannot use this layer from its cache and it has to rebuild it. And this is where the problem happens: once the layer is rebuilt all the following layers have to rebuild as well. So Docker cannot reuse the following layers e.g., RUN npm install from its cache and it has to reinstall all the npm dependencies. And this is exactly where we have a bottleneck. We have to wait half a minute for all these dependencies to be installed. So to optimize our build we have to separate the installation of the third-party dependencies from copying our application files:


        FROM node:14.16.0-alpine:3
        RUN addgroup app && adduser -S -G app app
        WORKDIR /app
        COPY package*.json . 
        RUN chown -R app:app /app
        RUN chmod -R 755 /app
        USER app
        RUN npm install 
        COPY . .
        ENV API_URL=http://api.myapp.com/
        EXPOSE 3000
        CMD ['npm', 'start'] 

with this new setup if we have not changed any of the application dependencies Docker is going to reuse this layer (COPY package*.json .) from its cache because package*.json is not modified. Similarly, Docker is not going to reinstall all npm dependencies because this instruction is  not changed. If we changed it to npm update then yes Docker had to rebuild this layer. The we have **COPY . .** and of course this layer should always be rebuilt and that is totally fine. 

With this new setup our build was super fast. Here is a takeaway: **to optimize your build, you should organize your Dockerfile in such a way that the instructions that do not change frequently (stable instructions) should be on the top and the changing instructions should be down the bottom.**

 <a name="35"></a>
### Removing Images

We may want to remove **dangling images** or **proper images**:

when we run

        docker images

we found out that we have a bunch of images that have no name and no tag. These are dangling images meaning loose images: these are layers that have no relationship with a tagged image. As we are changing Dockerfile and rebuilding our image Docker was creating these layers and at some point, these layers lost their relationship with our react-app image, as we work with Docker we see all these dangling images popping up all the time. To get rid of them we have to use prune command:

        docker image prune 

but we may have a stopped container running on the older react-apt image, which needs to be removed first:

        docker ps -a

so we want to get rid of these stopped containers:

        docker container prune

now I don't have any stopped containers then:

        docker image prune 

will delete all of my dangling images.

        docker images

returns me back all the proper images. 

Now let's say I want to delete a proper image (not the dangling one):

        docker image

which, provides me with all the sub-commands available. A side note: all image management operations start with **docker image**. 

        docker image rm image_name # or image_id, to remove multiple images we can separate their names or ids by a space 

a side note: docker rmi vs. docker image rm 

Both docker rmi and docker image rm commands are used to remove Docker images. 

<a name="36"></a>
### Tagging Images

Whenever we build an image or pulled it from Docker hub by default Docker uses the **latest** tag. This latest tag is JUST a label there is nothing special about it: it does not necessarily mean this is the latest version of your image. If you don't tag your images properly latest can point to an older image. Keep in mind that **latest** is totally fine for development but you should NOT use it in production you don't want to put an image with the latest tag in production or staging server because if something goes wrong you cannot easily troubleshoot issues because you don't know what version you are really running in production if you want to do a rollback or an upgrade how can you tell what version you are running in production if it is always latest! So we should always use explicit tags to identify what version you are running on each environment: in your test environment, staging or production. For development, latest tag is totally fine. 

let's tag our image properly:

 we have two ways to tag an image:

1. to tag an image while building it:

        docker build -t react-app:1 .

different groups have different preferences to choose the tagging style. some prefer semantic versioning like 3.1.2, which is common among the teams that do not release that often. Teams that release frequently prefer build numbers like 77. 

        docker images

we see that an image can have multiple tags. what if we made a mistake and want to remove the tag:

        docker image remove react-app:1 

the tag will be gone.

2. tag an image after it is already built:

we use the tag command:

        docker image tag react-app:latest react-app:1 

takeaway: the latest tag does not necessarily point to the latest image we have to explicitly apply it to the latest image:

        docker image tag react-app:2 react-app:latest 


<a name="37"></a>
### Sharing Images

To share our images with others we can push them to the **hub.docker.come**. In there, we can create a repository, similar to the GitHub repository. We can have a repository and in this repository, we can have multiple images with different tags. When creating a repository I chose the name "react-app" for my repository. doing so the name of our repo would be our username/image_name in my case: danialarab/react-app. To push our image to this repo we have to give it this tag i.e., danialarab/react-app. 

a side note: optionally we can connect our account to the GitHub account and so every time we do a push to GitHub DockerHub automatically pulls the latest code and builds a new image. 
 
back to the terminal:

        docker image ls

gives me back:

        REPOSITORY   TAG                  IMAGE ID       CREATED       SIZE
        react-app    2                    bf832e81e05d   2 days ago    301MB
        react-app    latest               bf832e81e05d   2 days ago    301MB
        alpine       latest               5e2b554c1c45   5 weeks ago   7.33MB
        ubuntu       latest               3b418d7b466a   7 weeks ago   77.8MB
        node         14.16.0-alpine3.13   50bfd284aa0d   2 years ago   117MB

let's say I want to publish the latest version of image with ID bf832e81e05d, so I need to first:

        docker image tag bf danialarab/react-app:2

then

        docker images

gives me back:

        REPOSITORY             TAG                  IMAGE ID       CREATED       SIZE
        danialarab/react-app   2                    bf832e81e05d   2 days ago    301MB
        react-app              2                    bf832e81e05d   2 days ago    301MB
        react-app              latest               bf832e81e05d   2 days ago    301MB
        alpine                 latest               5e2b554c1c45   5 weeks ago   7.33MB
        ubuntu                 latest               3b418d7b466a   7 weeks ago   77.8MB
        node                   14.16.0-alpine3.13   50bfd284aa0d   2 years ago   117MB

now the image with ID bf832e81e05d has three tags. All these tags point to the same image on my machine. Now we are ready to push this image to DockerHub. First, we have to login so in terminal:

        docker login

After providing username and password to successfully login,

        docker push danialarab/react-app:2 

now Docker is pushing each of the layers on my image. The first time takes a little while because the layer that includes all the npm dependencies is fairly large, once we pushed this image our future push would be faster assuming we don't change our application dependencies though. Now If I go back to my docker hub profile and refresh I see the pushed image there. 

Let's make a small change in the project like adding a ! in the readme file, then rebuild the image:

        docker build -t react-app:3 .

then 

        docker images

gives me:

        REPOSITORY             TAG                  IMAGE ID       CREATED              SIZE
        react-app              3                    b2ec5490256a   About a minute ago   301MB
        danialarab/react-app   2                    bf832e81e05d   2 days ago           301MB
        react-app              2                    bf832e81e05d   2 days ago           301MB
        react-app              latest               bf832e81e05d   2 days ago           301MB
        alpine                 latest               5e2b554c1c45   5 weeks ago          7.33MB
        ubuntu                 latest               3b418d7b466a   7 weeks ago          77.8MB
        node                   14.16.0-alpine3.13   50bfd284aa0d   2 years ago          117MB

then again to be able to push it to docker hub i need to give my image an appropriate tag that starts with my username: danialarab/react-app:

        docker image tag react-app:3 danialarab/react-app:3

now

        docker images

gives me back:

        REPOSITORY             TAG                  IMAGE ID       CREATED         SIZE
        react-app              3                    b2ec5490256a   4 minutes ago   301MB
        danialarab/react-app   3                    b2ec5490256a   4 minutes ago   301MB
        danialarab/react-app   2                    bf832e81e05d   2 days ago      301MB
        react-app              2                    bf832e81e05d   2 days ago      301MB
        react-app              latest               bf832e81e05d   2 days ago      301MB
        alpine                 latest               5e2b554c1c45   5 weeks ago     7.33MB
        ubuntu                 latest               3b418d7b466a   7 weeks ago     77.8MB
        node                   14.16.0-alpine3.13   50bfd284aa0d   2 years ago     117MB

so

        docker push danialarab/react-app:3 
 
so this time because some of these layers already exist my push is super fast. now back to my repo on docker hub I do have two tags! Now that my image is on docker hub I can pull it on any machine that runs docker just like pulling any other image on the docker hub.

<a name="38"></a>
### Saving and loading Images

What if I want to share my image with a colleague without going it through the docker hub? I can save it as a compressed file and then load it on another machine.

        docker image save --help

its help:

        Usage:  docker image save [OPTIONS] IMAGE [IMAGE...]
        
        Save one or more images to a tar archive (streamed to STDOUT by default)
        
        Options:
          -o, --output string   Write to a file, instead of STDOUT

so

        docker image save -o react-app.tar react-app:3 # saves my image react-app:3 to a compressed file named react-app.tar in the durrent directory

after extracting the content of the compressed file we have several folders each of which represents a layer of our image, each folder/layer contains three files: a layer.tar, json, and VERSION file. In each folder, the file layer.tar contains all the files of that layer for example the layer for our app, for Linux, for node, and so on.

We also have load command:

        docker image load --help

its help:

        Usage:  docker image load [OPTIONS]
        
        Load an image from a tar archive or STDIN
        
        Options:
          -i, --input string   Read from tar archive file, instead of STDIN
          -q, --quiet          Suppress the load output

to demonstrate this let's remove the image with a tag 3 from my machine then load it:

        docker images

gives me:

        REPOSITORY             TAG                  IMAGE ID       CREATED          SIZE
        danialarab/react-app   3                    b2ec5490256a   36 minutes ago   301MB
        react-app              3                    b2ec5490256a   36 minutes ago   301MB
        danialarab/react-app   2                    bf832e81e05d   2 days ago       301MB
        react-app              2                    bf832e81e05d   2 days ago       301MB
        react-app              latest               bf832e81e05d   2 days ago       301MB
        alpine                 latest               5e2b554c1c45   5 weeks ago      7.33MB
        ubuntu                 latest               3b418d7b466a   7 weeks ago      77.8MB
        node                   14.16.0-alpine3.13   50bfd284aa0d   2 years ago      117MB


so

        docker image rm react-app:3 

then

        docker images

gives me:

        REPOSITORY             TAG                  IMAGE ID       CREATED          SIZE
        danialarab/react-app   3                    b2ec5490256a   37 minutes ago   301MB
        react-app              2                    bf832e81e05d   2 days ago       301MB
        react-app              latest               bf832e81e05d   2 days ago       301MB
        danialarab/react-app   2                    bf832e81e05d   2 days ago       301MB
        alpine                 latest               5e2b554c1c45   5 weeks ago      7.33MB
        ubuntu                 latest               3b418d7b466a   7 weeks ago      77.8MB
        node                   14.16.0-alpine3.13   50bfd284aa0d   2 years ago      117MB

we still have this image in danialarab/react-app, let's also remove it which also points to the image with ID b2ec5490256a:

        docker image rm danialarab/react-app:3

or using the image ID which is easier

        docker image rm b2

then

        docker images

so:
       
        REPOSITORY             TAG                  IMAGE ID       CREATED       SIZE
        danialarab/react-app   2                    bf832e81e05d   2 days ago    301MB
        react-app              2                    bf832e81e05d   2 days ago    301MB
        react-app              latest               bf832e81e05d   2 days ago    301MB
        alpine                 latest               5e2b554c1c45   5 weeks ago   7.33MB
        ubuntu                 latest               3b418d7b466a   7 weeks ago   77.8MB
        node                   14.16.0-alpine3.13   50bfd284aa0d   2 years ago   117MB

so that image is gone from my machine let's load it:

        docker image load -i react-app.tar 

now

        docker images

so

        REPOSITORY             TAG                  IMAGE ID       CREATED          SIZE
        react-app              3                    b2ec5490256a   45 minutes ago   301MB
        danialarab/react-app   2                    bf832e81e05d   2 days ago       301MB
        react-app              2                    bf832e81e05d   2 days ago       301MB
        react-app              latest               bf832e81e05d   2 days ago       301MB
        alpine                 latest               5e2b554c1c45   5 weeks ago      7.33MB
        ubuntu                 latest               3b418d7b466a   7 weeks ago      77.8MB
        node                   14.16.0-alpine3.13   50bfd284aa0d   2 years ago      117MB

so here the loaded image has the same image ID that we had earlier.

<a name="39"></a>
## 4. Working with Containers

<a name="40"></a>
### Introduction

Topics to be discussed:

+ starting and stopping containers
+ publishing ports
+ viewing container logs
+ executing commands in running containers
+ removing containers
+ persisting data using volumes
+ sharing source code with containers so we don't have to rebuild our image every time we make a change in our code

<a name="41"></a>
### Starting containers

        docker images

        REPOSITORY   TAG                  IMAGE ID       CREATED       SIZE
        react-app    latest               bf832e81e05d   4 days ago    301MB
        alpine       latest               5e2b554c1c45   5 weeks ago   7.33MB
        ubuntu       latest               3b418d7b466a   7 weeks ago   77.8MB
        node         14.16.0-alpine3.13   50bfd284aa0d   2 years ago   117MB

        docker ps 

        CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

so there is no running container, let's run a new container:

        docker run react-app

which gives me back:

        > react-app@0.1.0 start /app
        > react-scripts start
        
        ℹ ｢wds｣: Project is running at http://172.17.0.2/
        ℹ ｢wds｣: webpack output is served from 
        ℹ ｢wds｣: Content not from webpack is served from /app/public
        ℹ ｢wds｣: 404s will fallback to /
        Starting the development server...
        
        Browserslist: caniuse-lite is outdated. Please run:
        npx browserslist@latest --update-db
        
        Why you should do it regularly:
        https://github.com/browserslist/browserslist#browsers-data-updating
        Compiled successfully!
        
        You can now view react-app in the browser.
        
          Local:            http://localhost:3000
          On Your Network:  http://172.17.0.2:3000

        Note that the development build is not optimized.
        To create a production build, use yarn build.

but here there is a problem: I cannot type any additional command on the terminal window. If I press ctrl + C to get out of this my container stops! So to deal with this we can run the container in a detached mode meaning in the background:

        docker run -d react-app

now my container is run in the background and my terminal window is free and I can do whatever I want. Of course, it takes a bit for the container to start because that is the time for the web server to start. 

when I do 

        docker ps

I got back:

        CONTAINER ID   IMAGE       COMMAND                  CREATED         STATUS         PORTS      NAMES
        3dcfa5499ca0   react-app   "docker-entrypoint.s…"   5 seconds ago   Up 4 seconds   3000/tcp   reverent_leakey

the last column which is NAMES is the names that docker automatically associates each container with a random name and so we can reference a container using its ID or name. I also can give my container a name when starting it:

        docker run -d --name blue-sky react-app

now let's look at the running containers:

        docker ps

        CONTAINER ID   IMAGE       COMMAND                  CREATED         STATUS         PORTS      NAMES
        c8432f36a87e   react-app   "docker-entrypoint.s…"   5 seconds ago   Up 4 seconds   3000/tcp   blue-sky
        3dcfa5499ca0   react-app   "docker-entrypoint.s…"   5 minutes ago   Up 5 minutes   3000/tcp   reverent_leakey

<a name="42"></a>
### Viewing the Logs

now that I have the two containers running in the background as:

        CONTAINER ID   IMAGE       COMMAND                  CREATED         STATUS         PORTS      NAMES
        c8432f36a87e   react-app   "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes   3000/tcp   blue-sky
        3dcfa5499ca0   react-app   "docker-entrypoint.s…"   9 minutes ago   Up 9 minutes   3000/tcp   reverent_leakey

here there is a problem: I do not know what is going on inside these containers! what if something goes wrong? what if the server generates an error? that is where I need to use the **logs** command:

let's take a look at the logs for the congtainer with ID c8432f36a87e:

        docker logs container_id
        docker logs c8

gives me back:

        > react-app@0.1.0 start /app
        > react-scripts start
        
        ℹ ｢wds｣: Project is running at http://172.17.0.3/
        ℹ ｢wds｣: webpack output is served from 
        ℹ ｢wds｣: Content not from webpack is served from /app/public
        ℹ ｢wds｣: 404s will fallback to /
        Starting the development server...
        
        Browserslist: caniuse-lite is outdated. Please run:
        npx browserslist@latest --update-db
        
        Why you should do it regularly:
        https://github.com/browserslist/browserslist#browsers-data-updating
        Compiled successfully!
        
        You can now view react-app in the browser.
        
          Local:            http://localhost:3000
          On Your Network:  http://172.17.0.3:3000
        
        Note that the development build is not optimized.
        To create a production build, use yarn build.

which is the output of my web server, which is exactly the same output as what we saw when we started the container in the foreground. this logs command has additional oprions and like any other docker commands always use --help to learn about various options:

        docker logs --help 

which gives me back:

        Usage:  docker logs [OPTIONS] CONTAINER
        
        Fetch the logs of a container
        
        Options:
              --details        Show extra details provided to logs
          -f, --follow         Follow log output
              --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative
                               (e.g. 42m for 42 minutes)
          -n, --tail string    Number of lines to show from the end of the logs (default "all")
          -t, --timestamps     Show timestamps
              --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or
                               relative (e.g. 42m for 42 minutes)

the option -f or --follow is useful if your container is continuously producing output. and instead of running docker logs container_id multiple times i can do docker logs -f container_id then whatever it is written to the log you can see it in real time on the terminal of course I have to press ctrl + C to get out of that. 

another option is -n or --tail with which we can specify the number of lines, from the bottom, to show. like

        docker logs -n 5 c8

so

          On Your Network:  http://172.17.0.3:3000
        
        Note that the development build is not optimized.
        To create a production build, use yarn build.

this is useful if we have a really long log. 

we can use -t to see the timestamps:

        docker logs -t c8

which gives me back

        2023-06-18T22:25:17.507519341Z 
        2023-06-18T22:25:17.507551456Z > react-app@0.1.0 start /app
        2023-06-18T22:25:17.507554516Z > react-scripts start
        2023-06-18T22:25:17.507556272Z 
        2023-06-18T22:25:20.328568863Z ℹ ｢wds｣: Project is running at http://172.17.0.3/
        2023-06-18T22:25:20.328594128Z ℹ ｢wds｣: webpack output is served from 
        2023-06-18T22:25:20.328597077Z ℹ ｢wds｣: Content not from webpack is served from /app/public
        2023-06-18T22:25:20.328598853Z ℹ ｢wds｣: 404s will fallback to /
        2023-06-18T22:25:20.328965984Z Starting the development server...
        2023-06-18T22:25:20.328973663Z 
        2023-06-18T22:25:20.546970785Z Browserslist: caniuse-lite is outdated. Please run:
        2023-06-18T22:25:20.546987271Z npx browserslist@latest --update-db
        2023-06-18T22:25:20.546989341Z 
        2023-06-18T22:25:20.546990832Z Why you should do it regularly:
        2023-06-18T22:25:20.546992399Z https://github.com/browserslist/browserslist#browsers-data-updating
        2023-06-18T22:25:38.350132519Z Compiled successfully!
        2023-06-18T22:25:38.350159374Z 
        2023-06-18T22:25:38.350162950Z You can now view react-app in the browser.
        2023-06-18T22:25:38.350165726Z 
        2023-06-18T22:25:38.350168086Z   Local:            http://localhost:3000
        2023-06-18T22:25:38.350170613Z   On Your Network:  http://172.17.0.3:3000
        2023-06-18T22:25:38.351164308Z 
        2023-06-18T22:25:38.351179220Z Note that the development build is not optimized.
        2023-06-18T22:25:38.351182796Z To create a production build, use yarn build.
        2023-06-18T22:25:38.351185506Z 

where we see the timestamps in front of each message.

so if you encounter any issues when running an applciation inside docker, the first thing you need to look at is **LOGS**.

<a name="43"></a>
### Publishing Ports

Currently, I have two containers running react-app application, if I go to localhost port 3000, localhost:3000, I cannot access the application. This is because port 3000 is published on the container not on the host so on the same machine we have multiple containers each of which is listening to port 3000 but the host itself is NOT listening to port 3000. So this port is currently closed and there is no way to send traffic into local host at this port. This is where we need to publish a port. If we take a look at the running containers:

        docker ps

I have

        CONTAINER ID   IMAGE       COMMAND                  CREATED             STATUS             PORTS      NAMES
        c8432f36a87e   react-app   "docker-entrypoint.s…"   55 minutes ago      Up 55 minutes      3000/tcp   blue-sky
        3dcfa5499ca0   react-app   "docker-entrypoint.s…"   About an hour ago   Up About an hour   3000/tcp   reverent_leakey

I have a column named PORTS where we can see the ports and their mapping, here both of my containers are listening to port 3000. Now let's start a new container and publish a port at the same time:

        docker run -d -p 80:3000 --name c1 react-app # here I publish a port on the host, port 80, to port 3000 of the container 

now if I go to localhost port 80, localhost:80, I can see my react app running. Let's look at the running containers one more time:

        docker ps

so

        CONTAINER ID   IMAGE       COMMAND                  CREATED              STATUS              PORTS                                   NAMES
        4599ef42b129   react-app   "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:80->3000/tcp, :::80->3000/tcp   c1
        c8432f36a87e   react-app   "docker-entrypoint.s…"   About an hour ago    Up About an hour    3000/tcp                                blue-sky
        3dcfa5499ca0   react-app   "docker-entrypoint.s…"   About an hour ago    Up About an hour    3000/tcp                                reverent_leakey


here for my c1 container, I have the following port mapping

        0.0.0.0:80->3000/tcp, :::80->3000/tcp

here I can see that port 80 of the host is mapped to port 3000 of the container. We don't have this notation for other containers. 

<a name="44"></a>
### Executing Commands in Running Containers

I learned that when I start a container it executes the default command specified using CMD instruction in the docker file, which is:

        FROM node:14.16.0-alpine3.13
        RUN addgroup app && adduser -S -G app app
        WORKDIR /app
        COPY package*.json ./ 
        RUN chown -R app:app /app
        RUN chmod -R 755 /app
        USER app  
        RUN npm install 
        COPY . .
        ENV API_URL=http://api.myapp.com/ 
        EXPOSE 3000
        CMD ["npm", "start"] 

but what if we want to execute a command in a running container later on? let's say we want to troubleshoot something and we want to look at the file system of that container? we can use **docker exec** to execute a command in a running container. 

**Difference between docker exec and docker run:**

with docker run we start a new container and run a command but with docker exec we execute a command in a running container, we do not start a new container. 

we can do

        docker exec c1 here_we_can_run_any_operating_system_command

let's do

        docker exec c1 ls

i got back:

        Dockerfile
        README.md
        node_modules
        package-lock.json
        package.json
        public
        src
        yarn.lock

which is the content of my app directory and this is becasue in the Docker file I set the working directory to the app directory through **WORKDIR /app** instruction and so when I run ls I see the content of this directory. 

using the same command we can open up a shell session in an interactive mode:

        docker exec -it c1 sh

which gives me back a shell session inside my c1 container:

        /app $ 

where I can do anything I want. **When I am done I can type exit and this does not impact the state of my container and my container is still running**. 

        docker ps

        CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                                   NAMES
        4599ef42b129   react-app   "docker-entrypoint.s…"   30 minutes ago   Up 30 minutes   0.0.0.0:80->3000/tcp, :::80->3000/tcp   c1
        c8432f36a87e   react-app   "docker-entrypoint.s…"   2 hours ago      Up 2 hours      3000/tcp                                blue-sky
        3dcfa5499ca0   react-app   "docker-entrypoint.s…"   2 hours ago      Up 2 hours      3000/tcp                                reverent_leakey


So using the **exec** command we can run any command in a runnign container. 

<a name="45"></a>
### Stopping and Starting Containers

As we discussed containers are like lightweight virtual machines, which can be stopped and restarted. To stop a running container:

        docker stop name_of_container

Now if I go to the localhost where my app was running and refresh it, my app is not available. Because the container that was serving the application is no longer running and it stopped and we need to bring it back up.

to start a stopped container:

        docker start name_of_container

Difference between docker run and docker start:

with docker start we start a stopped container but with docker run we start a new container. 


<a name="46"></a>
### Removing Containers

There are 2 ways to remove a container: 

        docker container rm container_name

or

        docker rm container_name

but if the container is running we cannot remove it via the above commands and we have two options: 

1. first stop the container then use the above commands
2. or force remove it using force option

        docker rm -f container_name

now if we do docker ps -a we don't see the container that we removed.

a cool trick to search for a specific container using piping:

        docker ps -a | grep container_name

to delete all the stoped containers in one go: 

        docker container prune 

        
<a name="47"></a>
### Containers File System

        docker ps 
        
        CONTAINER ID   IMAGE       COMMAND                  CREATED         STATUS         PORTS      NAMES
        479ec4c7b2b5   react-app   "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes   3000/tcp   c1
        ed3528ca9d50   react-app   "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes   3000/tcp   blue_sky

now let's start a shell session in one container:

        docker exec -it c1 sh

then let's create a file in the app directory:

        echo hello > file1.txt

        ls

        Dockerfile         file1.txt          package-lock.json  public             yarn.lock
        README.md          node_modules       package.json       src

now let's start a new shell session in another container:

        docker exec -it blue_sky sh

now let's see if the file1.txt file is in the app directory of the container blue_sky.

        ls | grep file1

which gives nothing back to me. 

So each container has its own file system, that is invisible to other containers. That means if we delete a container its file system is also gone with it and we lose our data. So we should never store our data in a container's file system. Instead, we use volumes.

<a name="48"></a>
### Persisting Data using Volumes

<a name="49"></a>
### Copying Files between the Host and Containers

<a name="50"></a>
### Sharing the Source Code with a Container







<a name="51"></a>
## 5. Running Multi-container Applications

<a name="52"></a>
### Introduction


<a name="53"></a>
### Installing Docker Compose

<a name="54"></a>
### Cleaning Up our Workspace

<a name="55"></a>
### The Sample Web Application

<a name="56"></a>
### JSON and YAML Formats](#56)

<a name="57"></a>
### Creating a Compose File

<a name="58"></a>
### Building Images

<a name="59"></a>
### Starting and Stopping the Application

<a name="60"></a>
### Docker Networking


<a name="61"></a>
### Viewing Logs

<a name="62"></a>
### Publishing Changes


<a name="63"></a>
### Migrating the Database

<a name="64"></a>
### Running Tests






<a name="65"></a>
## 6. Deploying Applications

<a name="66"></a>
### Introduction

<a name="67"></a>
### Deployment Options

<a name="68"></a>
### Getting a Virtual Private Server

<a name="69"></a>
### Installing Docker Machine

<a name="70"></a>
### Provisioning a Host

<a name="71"></a>
### Connecting to the Host

<a name="72"></a>
### Defining the Production Configuration

<a name="73"></a>
### Reducing the Image Size

<a name="74"></a>
### Deploying the Application


<a name="75"></a>
### Troubleshooting Deployment Issues

<a name="76"></a>
### Publishing Changes


    

<a name="77"></a>
## 7. Reference
Course: "The Ultimate Docker", instructor: Mosh Hamedani (https://codewithmosh.com/p/the-ultimate-docker-course)

