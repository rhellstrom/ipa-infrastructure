# freeIPA infrastructure exploration and automation
This is my attempt at automating a basic infrastructure centered around RedHat's identity management system [freeIPA](https://www.freeipa.org/) using Ansible.

## Architecture
The infrastructure consists of 5 virtual machines set up through Vagrant:

* DNS server running BIND configured as a forwarding server
* Host for running freeIPA identity management with integrrated DNS for internal hostname resolution
* A 'kerberized' NFS Server configured to automount and host user home directories
* A database server running PostgreSQL with kerberos authentication configured
* A client machine for testing the various functionalities and services through freeIPA

``` 
                -----------------
               |    dns-server   |
               |   192.168.60.5  |
                -----------------
                         |
                 ----------------
                |   IPA-server   |
                |  192.168.60.4  |
                 ----------------
                 |       |       |
    ----------------     |      ----------------
   |    db-server   |    |     |   file-server  |
   |   192.168.60.7 |    |     |  192.168.60.6  |
    ----------------     |      ----------------
                         |
                 ---------------
                |   test-user   |
                |  192.168.60.8 |
                 ---------------

```
*IP addresses and hostnames modeled after the ```Vagrantfile```*

## Prerequisites

The project is built locally using Vagrant together with VirtualBox and in order to run the playbooks you also need Ansible installed as well as the Ansible [freeipa role](https://galaxy.ansible.com/ui/repo/published/freeipa/ansible_freeipa/docs/)

```
$ ansible-galaxy collection install freeipa.ansible_freeipa
```

## Build and configure the servers
