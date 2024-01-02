# freeIPA infrastructure exploration and automation
This is an attempt at automating a basic infrastructure centered around Red Hat's identity management system [freeIPA](https://www.freeipa.org/) using Ansible. The purpose of this project was to familiarize myself with Red Hat technologies as well as explore automation.

## Architecture
The goal is to set up a very basic infrastructure capable of managing identities and authenticate towards services through freeIPA. So far the project consists of:

* A DNS server using BIND configured as a forwarding server
* A Host for running freeIPA identity management with integrated DNS for internal hostname resolution
* A 'kerberized' NFS Server configured to automount and host user home directories
* A database server running PostgreSQL with kerberos authentication configured
* A test-client for exploring and testing various functionalities

``` 
                 ----------------
                |   dns-server   |
                |  192.168.60.5  |
                 ----------------
                         |
                 ----------------
                |   ipa-server   |
                |  192.168.60.4  |
                 ----------------
                 |       |       |
    ----------------     |     ----------------
   |    db-server   |    |    |   file-server  |
   |   192.168.60.7 |    |    |   192.168.60.6 |
    ----------------     |     ----------------
                         |      
                   ---------------
                  |  test-client  |
                  |  192.168.60.8 |
                   ---------------

```
*IP addresses and hostnames modeled after the ```Vagrantfile```*

## Prerequisites

Before you can run these playbooks you'll need Ansible installed on your system. The playbooks also use roles and collections not part of the Ansible-core package. To download all the dependencies needed run the following command from the project root folder:

```
$ ansible-galaxy install -r requirements.yml
```

To build the infrastructure locally I'm using Vagrant together with VirtualBox

## Build and configure the servers
To build the VMs and configure them using Ansible
1. Run ```vagrant up``` 
2. Run ```ansible-playbook -i inventory.yml configure.yml```

A basic playbook for setting up a user named ```test``` is available in ```playbooks/utilities/add_user.yml```.
With a valid kerberos ticket obtained through ```kinit <username>``` or simply logging in to a client machine allows you to:
* Access postgresql with: ```psql -d <database> -h db-server.ipa.test -U <username>```
* Access your own automounting /home folder on the NFS file server

##  Limitations
* ipa-client-automount configuration for some reason makes sshd refuse ansible connections.
* Yet to implement automatic home directory creation whenever a new user is added to IPA