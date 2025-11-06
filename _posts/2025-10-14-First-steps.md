---
layout: post
title:  "Docker Project: First Steps"
date:   2025-10-14 15:30:35 -0400
categories: Docker-start
---

# Docker Project: First Steps

#### *{{ site.author }}*

## Introduction

The end goal of this project is to create an app that is created/built with docker.  This app will consist of a server and a db.  That will be built using docker compose to execute building of the app.  If you read my previous post: [Docker Project Quest](https://surferjreb.github.io/ijb_tech_blog.github.io/2025/10/14/DockerProject.html).  This describes my ranting for creating such a project.  

## Starting Point

To start with, we need a server.  So, I connect to my local proxmox instance and fire up a VM.  Going into this project there are certain things we will need to start for the VM.

VM Machine:  
- OS: Ubuntu 24.04 LTS
- CPU: 4 Core, 2 Core minimal
- RAM: 8 GB
- DISK: 300 GB

A simple spin up and install of the OS.  Just a standard install with openssh installed.  At this point don't select anything else to install.

> [!NOTE]  
> In proxmox, ssh is opened for your server and VM's.  One of the many nice features of proxmox.

A few may be lost.  If you want to know how to spin up a VM on proxmox, the have great documentation that can be found here: [proxmox documentation](https://pve.proxmox.com/pve-docs/).  


Anyways, getting back on track.  OS installed and server ready to connect too.  There are now a couple more steps before getting a project to load here.  We need Docker and a few tools.  This can be installed by a script or running a few commands.  Since this server will be running headless(no gui), we are just going to install docker eng, docker compose, and a few tools.  

Python3 is installed with the distro, part of our checklist of needs along with a few tools, `ca-certificates curl` so that can be skipped.  Just so you know I am following the docker manual on installing on linux from the documentation.  You can also find a script here to execute all this and not have to type it out or paste it in the terminal.  It is good to know if you are a dev, I find its neat.  Here is the docs: [docker manual for Ubuntu](https://docs.docker.com/engine/install/ubuntu/).  

### Next steps are..

1. Update - `sudo apt update && sudo apt upgrade -y`
2. Execute some commands to prep for install.

    ```
    sudo install -m 0755 -d /etc/apt/keyrings
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    sudo chmod a+r /etc/apt/keyrings/docker.asc

    # Add the repository to Apt sources:
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    
    # Update to load sources
    sudo apt-get update

    ```

3. Install Docker or the pieces that make up docker.

    ```
    sudo apt install docker-ce docker-ce-cli containered.io docker-buildx-plugin docker-compose-plugin
    ```

4. Check its running.
    `sudo systemctl status docker`
    It should say something to the effect of:

    ![Docker service running](/assets/images/docker_running.png)

5. Test it..
    `sudo docker run hello-world`
    Now, you should see a container startup, show you a message and then stop the container.
    ![hello world ran in docker](./assets/images/docker_hello_world.png)

6. Everything should be going great, if errors have happened at this point and you are frustrated, flush it and rebuild.  
   
   > [!NOTE]  
   >  You could always use this as a good opportunity to create a template in proxmox of your starter server.  I probably should have recommended that sooner.  Makes it easier to replace or recreate your VM.  Here's a link to the process: [proxmox templates](https://pve.proxmox.com/pve-docs/chapter-qm.html#qm_templates)

7. Now to install docker compose.  Wait, we installed the plugin already, we should be good to go for launching a project now.  Lets test: `docker compose verison`
    - You should see a version number.(if using linux...)
    - You can also, install docker compose stand alone following the documentation here: [Docker compose standalone](https://docs.docker.com/compose/install/standalone/)

Now, with that we have a kind of ready machine.  I would recommend installing some tools for networking, will help later.  Also, if you want to just develope in your environment, install your fav editor, IDE.  When using a headless setup a good thing to get confortable with is terminal editor.  No, this is not the vim/neovim call to arms.  Nano works just as good and works right out the box.  You have to install `vim` or `neovim`.  You could use `vi`, which is just as good.  Lately I have been using `vim` or `neovim` just to get out of an editor and away from auto complete too, but that is for a different blog post.

Today, the server is setup.  It is ready for a project...

#### *{{ site.author }}*