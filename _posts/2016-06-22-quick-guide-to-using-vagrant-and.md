---
title: Quick guide to using Vagrant and VirtualBox
layout: post
categories: devops
permalink: /2016/06/quick-guide-to-using-vagrant-and.html 
date:   2016-06-22 
---

What is Vagrant? It's a;

- A tool for developers, QA and IT people
- A virtual machine management tool
- Remove frictions on setting up development environment
- Remove frictions on reproducing errors and runtime anomalies due to machine configuration differences

For the full pep-talk, go to [vagrantup.com/intro](https://www.vagrantup.com/intro/index.html)

Before we go any further, let's have some definition of terms.

- **vm** what I mean by vm is both the virtualBox and vagrant. If I need to refer to either Vagrant or VirtualBox, I'll use their respective names, but **vm** means virtualBox + vagrant
- **host machine.** The actual or physical PC. This could be your macOS, Windows or Linux PC
- **guest machine.** The vm 
- **box.** is some sort of an operating system image. If you have performed complete disk backups of your operating system before, a box is similar to that. It is a snapshot of an operating system. Snapshots are taken and saved typically because you would like to restore them at a later time, either on the same hardware or a different one

## Install Vagrant and VirtualBox

Vagrant requires requires either VirtualBox, VMWare or any other hypervisor virtualization. In our case, we’ll use VirtuaBox. The installation process is (a) install VirtualBox then (b) install Vagrant.

There are a couple of ways to install these software. It depends on your operating system and what kind of package manager you are using. macOS has HomeBrew and MacPorts, Linux has aptitude, rpm, yum etc. and Windows has chocolatey.

The most straightforward way is to install virtualbox and vagrant using pre-compiled binaries. The process involves nothing more than downloading these binaries, double-clicking them and following the wizards. Just like how you would install any other app in your system.

In this article, we won't use the precompiled binaries. We'll install vagrant using the package managers. Examples 1,2 and 3 shows how to install vagrant in macOS, Windows and Linux respectively.

**Example 1. Installation on a macOS with brew**

~~~bash
brew update
brew tap caskroom/cask
brew cask install vagrant
brew cask install virtualbox
~~~

For Windows, you need to install chocolatey first, go [here for instructions](bit.ly/installchoco)

**Example 2. Installation on a Windows PC**

~~~bash
cinst --y virtualbox vagrant
~~~

**Example 3. Installation on Linux using apt-get**

~~~bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install vagrant virtualbox-dkms
~~~

Make sure everything has been installed properly. On terminal window, run the following commands

~~~bash
vagrant --v
vboxmanage - v
~~~

If you saw the actual version numbers of vagrant and virtualbox instead of getting a "command not found" or "bad command or filename" error, that means virtualbox and vagrant are good to go.

Vagrant uses the term "box" to refer to an OS image. Before we can create a vagrant instance, we need to get a box. You can get boxes from vagrantup.com and many other places, but we'll get ours from ubuntu.com. The command below downloads the ubuntu precise box to our workstation

```
$ vagrant box add ubuntu-precise http://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-vagrant-i386-disk1.box
```

Most of these boxes are 350 - 500mb in size. Grab a coffee if you have slow internet connection

## Vagrant project

A vagrant box is not the same as a vagrant project. Remember that a box is just an image of the operating system. You can create many projects using the same image. But we will need a box before we can create project.

You can use the ubuntu-precise box we downloaded earlier for multiple projects. This is actually one of the main use cases for vagrant. You can isolate the libraries and configurations of your projects. That way, you always have a clean baseline box.

To create our first project, we need to do (a) create a folder for our project (b) run `vagrant init` inside the project folder and c) optionally, edit the vagrant file if we want to alter some of the default settings of vagrant

To create a project, follow the commands below.

```bash
$ mkdir -p ~/vms/myProjectName && cd ~/vms/myProjectName
$ vagrant init ubuntu-precise
```

`vagrant init` will create a vagrant config file. This is what you can configure and eventually commit to source control. 

Then we can start using the virtual machine

````bash
cd ~/vms/myProjectName

vagrant up   # starts the vm
vagrant ssh  # login to the vm
````

<aside class="aside"><strong>TIP</strong>

If you are used to working with virtualbox and any other hypervisor, you might be wondering why we don’t see the usual window of virtualbox. Vagrant, by default runs in headless mode that is why we don’t see any GUI interfaces to it. You can work with the virtualbox GUI if you really want (need) to, but working on the CLI is the idiomatic way of working with vagrant</aside>

Other commands to manage vm

```bash
vagrant halt     # like a shutdown
vagrant suspend  # puts the vm to sleep
vagrant destroy  # like halt, but also destroys the vm
```

<aside class="aside"><strong>TIP</strong>

If you still want to see the GUI of the virtualbox session, you can do 1) exit the existing virtual machine session, if you are in one 2) edit the Vagrantfile configuration and look for the `config.vm.provider` entry and uncomment that section. Finally 3) reload the virtual machine using the command vagrant reload</aside>

## Shared folder with host

There are many reasons to use vagrant. Some use it as test or staging server and some use it as a development server.  When you use it as a development server, you need to have a way to share data between the host (the machine where you installed the vm) and the guest machine (the vm itself).   

By default, Vagrant already shares the folder `/vagrant` on the guest machine with the the folder `/vms/myProjectName` (this is the directory where you created your vagrant project). If you want the guest and host machine to share files in folder other than the default (/vagrant), you need to configure that in **Vagrantfile**

## Managing boxes

When you longer need the OS image, you can delete the boxes in order to free up disk space. The physical location of the image files (boxes) are shown in Table 2.

**Table 2. Location of boxes**

```
+--------------------+-------------------------------+
| OS                 | Location of boxes             |
+--------------------+-------------------------------+
| Windows            | %USERHOME%\.vagrant.d\boxes   |
| macOS or Linux     | ~/.vagrant.d/boxes            |
+--------------------+-------------------------------+
```


To delete a box, do the following. 

```bash
vagrant box list
vagrant box remove <name of the box>
```

If you simply want to rename a box, you can do the following

```bash
vagrant box repackage
```

## Vagrantfile

*Vagrantfile* is a configuration file. It was created when you issued the command `vagrant init`. It’s written in the Ruby language, so it' s very easy to read. There are a lot of defaults that are already defined in this config file. You simply need to uncomment them if you need to enable any particular functionality e.g. port forwarding etc. The Vagrantfile has a simple structure, everything is inside the `Vagrant configure` block. See the code example below

```bash
Vagrant configure(2) do |config|
   # various configurations
end
```

## Networking

One of the common reasons why we run vagrant boxes is so that we can run server applications in them, either for testing or for development or something else. No matter what the reasons are why you setup vagrant, you will need a way to access the servers that are running on your guest machine. There are three ways to do this (1) setup your vm as a public network (2) set it up as private network and lastly (3) use port forwarding. Each of these options have their advantages and disadvantages. Your general security situation and project goals will largely determine which approach is right for you.

## Public networking

This is known as *bridge* networking. If you go for this setup, the guest computer, your vm, will actually appear as another node in the current network segment. It will appear as another computer in your network neighborhood. In a public network, you may be able to refer to your guest machine via its hostname because your DNS server might be able to pick up the hostname of the vm when the DHCP server gives it an IP address. 

The Vagrantfile for a publicly accessible vm (with static IP) looks like the following 

```bash
Vagrant.configure(2) do |config|
   config.vm.network "public_network", ip: "192.168.1.210"
end
```

This type of configuration will boot up your machine and assign itself a static IP address of 192.168.1.210. It will appear as another node in the network segment

<aside class="aside"><strong>WARNING</strong>

Consult your network administrator if it is okay to use any specific IP addresses. Most network admins configure the workstation endpoints to get their IP addresses dynamically from a DHCP server

</aside>

If you want to set up a public vm with dynamic IP, you can do it with the following Vagrantfile

```bash
Vagrant.configure(2) do |config|
   config.vm.network "public_network", type: "dhcp"
end
```

In this type of configuration, the guest machine will act like it is another node of the network segment. It will listen to any DHCP broadcast, if it finds any, it will fetch an IP address from the server

## Port forwarding

Vagrant boxes are insecure out of the box. That’s why the **public_network** setting is commented by default. Think of it this way, a computer on a private network is usually behind a firewall and/or a router. Most computers on an office setting belong to this category. Your office workstation is not reachable from the internet because it is behind a firewall.

When you create a vagrant project, the vm is on a private network. That means you cannot reach any of its ports from the host machine, unless you define a public network. Having a vm that is not accessible from your host machine is not very useful, so we need a way to reach the applications running on the vm. One way to do it via port forwarding. Basically, we will map a port number on the host machine to a port of on the guest machine (your vm). That way, when we try to communicate to a port on the host machine, that request will be forwarded to the port of the guest machine, thus making the services of the guest machine reachable.

 To enable port forwarding, you need to edit the Vagrantfile, and uncomment the following line (remove the # sign on the left)

```bash
config.vm.network "forwarded_port", guest:80,  host:8080, id: "whatever"
```

This line has 4 parts. The first one is the name of the configuration, the 2nd, 3rd and 4th part are as follows

- **guest:80**. The vm will listen on port 80, this is usually a web server like Apache or NGinx or any other webserver that you want to use
- **host:8080**. This is the port on the host machine (your actual PC where the vagrant is installed). When you launch a browser and go to http://localhost:8080, you are actually hitting the port 80 of your vm
- **id: whatever**. This is optional, but it is always a good idea to to put an id on your port mapping definition

You can forward more than one port. If you need to open more ports on the guest machine, just define each mapping on separate lines, like this

```bash
config.vm.network "forwarded_port", guest:80,  host:8080, id: "apache"
config.vm.network "forwarded_port", guest:4000,  host:3000, id: "node"
```