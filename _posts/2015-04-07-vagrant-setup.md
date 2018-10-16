---
title: Vagrant setup
layout: post
categories: devops
permalink: /2015/04/vagrant-setup.html
date:   2015-04-07 
---

You need to do 3 things. 1) Install Virtual Box and Vagrant 2) Add a Vagrant Box image and 3) Define a VM

## Install Vagrant and VirtualBox

You can install vagrant using precompiled binaries. Go to vagrantup.com website and download the installer appropriate for your platform. These are wizard type installers, so you will install them in a familiar way. Download, then double click, then follow the prompts.

Alternatively, you can use the package manager of your platform. OSX has either MacPorts or Homebrew. Windows has got chocolatey and Debian derived Linuxes has got the aptitude package manager.

### macOS with brew

```bash
brew update
brew tap caskroom/cask
brew install brew-cask
brew cask install vagrant virtualbox
```

### Windows

You need to install chocolatey first, [go here for instructions](http://bit.ly/installchoco)

```bash
cinst --y virtualbox vagrant
```

### Linux, apt-get

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install vagrant virtualbox-dkms
```

Make sure everything has been installed properly. On terminal window, run the following commands

```bash
vagrant --v
vboxmanage - v
```

If you saw the actual version numbers of vagrant and virtualbox instead of getting a "command not found" or "bad command or filename" error, that means virtualbox and vagrant are good to go.

## Add a Box

```bash
$ vagrant box add ubuntu-precise http://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-vagrant-i386-disk1.box
```

Most of these boxes are 350 - 500mb in size. Grab a coffee if you have slow internet connection

## Add a VM

Choose where you will locate your VM

```bash
mkdir -p ~/vms/myprojectname && cd ~/vms/myprojectname
vagrant init ubuntu-precise
vagrant init will create a vagrant config file. This is what you can 
```

configure and eventually commit to source control. Now, launch the VM

```bash
vagrant up
vagrant ssh # this will connect to your VM
```
