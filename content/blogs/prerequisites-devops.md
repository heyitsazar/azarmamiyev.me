---
title: 'Some stuff that you need to know before learning DevOps'
date: 2024-03-22T01:51:11+01:00
disableshare: false
category:
cover:
  image:
  alt:
  caption:
  relative: false
draft: false
---

This blog contains the notes that I took while I was watching the freeCodeCamp video about DevOps prerequisites. By reading this blog you can get fundamental knowledge about the things you need to know before learning DevOps. I mostly focused on keywords and some small description about them, but you can research more about those by yourself to build long lasting knowledge.

You can also check the video yourself by clicking [here](https://youtu.be/Wvf0mBNGjXY?si=vwdNN43glgCvUmn_)

---
## Basic Commands of Linux
A text based command line interface which allows us to interact with operating system called **shell**. We can get the type of our shell with `echo $SHELL` $SHELL is **enviroment variable**. We have different types of shells, like **zsh and bash**.

- `echo` is a simple command allows us to print something.
- `ls` command is used to list files and directories
- `cd` used for changing directories
- `pwd` used for to show which directory that you're on
- `mkdir` is for creating folders

We can run multiple commands at once using `;`. Like, `mkdir test; cd test`. We can create multiple folders (inside of themselves) using `mkdir -p tmp/europe/hungary/budapest`. To remove a directory with its contents, we can use `rm -r /tmp/folder`. For copying one directory to another, we can use `cp -r europe /tmp/europe`.

For creating a new file, we can use `touch file_name` command. For adding content to that file we can use `cat > file_name`, after this cli will wait for our prompt, after writing something, we can press `CTRL + D` in order to save. If we use just `cat file_name`, we'll see the content of the file.

We can use `mv file_name new_name` to rename the file, or we can use same commands to move (cut and paste) files. `rm file_name` will remove file.

To get the username on linux, we can use `whoami` command. The `id` command will give more info, such as user id and group id. We can use `su` command to switch user accounts. If we're accessing the target computer remotely with ssh, we can change the hostname before ip to change users, like `newuser@192.168.6.153`.

On linux we have a file called `/etc/sudoers`, by adding a username to this file, we can give root privileges to a certain user, otherwise only root user can access to certain directories, or perform certain tasks.

We can download files from internet with `curl` or `wget` command. Like, `curl https://example.com/some_file.txt -O some_file.txt`, we need to use `-O` because otherwise it will just print to content of the file, `-O` makes it to save.

To check the OS version, we can go to `/etc/*release*` directory or on Ubuntu, you can use `lsb_release -a` command. By checking content of those files, we can get information about the OS version.
## Package managers
We have different **package managers** on linux distros, we're using them to download and install applications to our system. On CentOS and RHEL, we use RPM.
- To install a package with rpm, we need to use `rpm -i packagename` command.
- To uninstall `rpm -e package name`
- To query (search) `rpm -q packagename`

### YUM package manager
We also have YUM package manager, which downloads all of the dependencies when we try to install an application, YUM still uses RPM in background though. 

- It has `/etc/yum.repos.d` directory, which contains repos where packages are located, 
- So, when we run `yum install packagename` it goes through repos and finds everything. 

Sometimes, for finding latest version of a software or a software which is not in our default set of repos, we need to add new repos. For doing that we'll edit that directory. 

- With `yum repolist` command, we can see list of repos. 
- With `yum list` command, we can see installed packages, 
- to remove `yum remove`, 
- to show duplicates `yum --showduplicates list packagename`. 
- In order to install certain version of a package we can use, `yum install packagename-1.0` command.

## Services
When we install an application which runs in background, they are come with services, which make them to run on background without a problem even if server restarts. Services helps us to make sure that apps will run automatically even we reboot the system.

- To start a service `service servicename start`.

But we also have a package called `systemctl`, which is widely used thing for dealing with services.
- To start a service with it `systemctl start servicename`.
- To stop `systemctl stop servicename`.
- To check status `systemctl status servicename`.
- To enable/disable (start at startup) service `systemctl enable/disable servicename`.

We might face a situation, where we need a python script to start on launch of the system. For this, we need to configure the command that we're using to run that script as a service. We have directory on linux which is `/etc/systemd/system` and it contains all of the service files and we can create a file inside of if and put our command to have our script as a service. Then we can enable that service.

```ini
[Unit]
Description=My Python Script

[Service]
ExecStart=/usr/bin/python3 /path/to/myscript.py
Restart=always

[Install]
WantedBy=multi-user.target
```

For actually making it to run at startup, we need to add `WantedBy` value, it will make sure that after some service starts, this script will start. With `ExecStartPre`, we can define a service to run that before the main service starts.

## Virtual Machines
A virtual machine (VM) is a virtual environment that functions as a virtual computer system with its own CPU, memory, network interface, and storage, created on a physical hardware system (located off- or on-premises). Software called a **hypervisor** separates the machine’s resources from the hardware and provisions them appropriately so they can be used by the VM.

A **hypervisor** is software that creates and runs virtual machines (VMs). The hypervisor treats resources—like CPU, memory, and storage—as a pool that can be easily reallocated between existing guests or to new virtual machines.

All hypervisors need some operating system-level components—such as a memory manager, process scheduler, input/output (I/O) stack, device drivers, security manager, a network stack, and more—to run VMs.
### Types of hypervisors
There are 2 different types of hypervisors that can be used for virtualization.
#### Type 1
A type 1 hypervisor is on bare metal. VM resources are scheduled directly to the hardware by the hypervisor. KVM is an example of a type 1 hypervisor. KVM was merged into the Linux kernel in 2007, so if you’re using a modern version of Linux, you already have access to KVM. 
#### Type 2
A type 2 hypervisor is hosted. VM resources are scheduled against a host operating system, which is then executed against the hardware. VMware Workstation and Oracle VirtualBox are examples of type 2 hypervisors.

On VirtualBox, we have a special mode to start VMs called **headless mode**. With that mode, we can run VMs on background, so we can connect those VMs with SSH and use them on our host system. In order to connect VMs remotely, we need "Remote Desktop service" running (if VM is running Windows), or "sshd" service running (if VMs is running Linux).

For SSH, we need an ip address to connect, if VM doesn't have an ip address, we can give it with `ip addr <ip address>/<subnet mask> <dev eth0 (device name)>` command.

## Network types of VMs
On VirtualBox, we have different network types:
- **Host-only network**: with this, we can create a virtual network inside host computer, if it has DHCP enabled, VMs will get ip address and our host PC will also be the part of that network. But the VMs cannot access internet. They will only talk to each other. *We can use IP forwarding on host machine to make VMs to access internet.*
- **NAT (Network Address Translation)**: it's similar to host-only, but on NAT VMs can access to internet, but all the requests that coming out from those VMs will go through the ip address of host machine.
- **Bridge network**: on this, every VM will get ip address from out original LAN, so VMs will be part of our network like a separate machine. Every device in our network can access that VMs.

### Port Forwarding
It's also so important, for example, we're using NAT and we want to access one of the VMs via SSH, the service will listen the post 80, but our host system is also using port 80 for SSH. So for this situation, we can use port forwarding, and we can say if someone try to connect our VM with port 8080, it actually means port 80 on that VM. So this way we can use SSH.

## Clones of VMs
We have different type of cloning, **full** and **linked**. 
- Full will create the copy of the disk
- Linked will still use the disk of original VM, but will write changes on new disk. It will depend on original VM disk.

## Snapshots
We can also take snapshots of our VMs, in case if something went wrong, we can restore that snapshot and continue working. We can also clone snapshots.

## Vagrant
Vagrant is an app which helps us **to automize VM stuff**, like downloading images, making network configuration and other stuff. In Vagrant terms, every environment called **boxes**, every box contains system image, and scripts to configure that environment. 

We have `vagrant ssh` command, which helps us to connect to VMs with ssh.

When we use `vagrant init` command, Vagrant will create **Vagrantfile** which contains the configuration of our VMs. We can share this file with others and if they create VMs will that file, all VMs will be the exactly the same.

## DNS Records
- **A Record** - is how we store IPv4 addresses.
- **AAAA Record** - is for IPv6.
- **CNAME** - is for storing different names for same domain.

## Ports
Every IP address is divided to logical parts called **ports**. Someone can reach our device with the ip, but what if we want to reach specific service that runs on that computer? Then we can make that service to listen on some port and try to access to that device on that port number.