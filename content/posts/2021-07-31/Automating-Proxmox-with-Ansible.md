---
weight: 1
title: 'Automating Proxmox with Ansible'
date: 2021-07-31T15:59:01+13:00
draft: false
author: "David"
authorlink: ""
permalink: /posts/2021/07/31/Automating-Proxmox-with-Ansible
description: "Basic Automation of Proxmox with Ansible"
featuredImage: "/images/Ansible-Proxmox.png"
featuredImagePreview: "/images/Ansible.png"

tags: ["Linux","Containers","Ansible","Automation","Proxmox","Proxmoxer"]
categories: ["How-To","DevOps","Ansible","Automation","Proxmox"]
toc:
  auto: false
---
A quick look at some basic Ansible Automation for Proxmox
<!--more-->

# Automating Proxmox with Ansible

Before I get started, I will breakdown the required packages and modules to get this working on you Ansible host machine and Target Proxmox server.

Requirements:
- Ansible Host:
    + [Community-general](https://docs.ansible.com/ansible/latest/collections/community/general/proxmox_module.html) Collections package via **ansible-galaxy**
    + [pip](https://packaging.python.org/guides/installing-using-linux-tools/#installing-pip-setuptools-wheel-with-linux-package-managers) for python version 3
    + [proxmoxer](https://pypi.org/project/proxmoxer/)
    + [requests](https://docs.python-requests.org/en/master/user/install/#install)
- Proxmox Machine
    + [pip](https://packaging.python.org/guides/installing-using-linux-tools/#installing-pip-setuptools-wheel-with-linux-package-managers) for python version 2 or 3 (Depends on your Proxmox version)
    + [proxmoxer](https://pypi.org/project/proxmoxer/)

> I Was using this Git repo [here](https://github.com/Dilden/Ansible-Proxmox-Automation/blob/master/books/create-container.yml) as an example implimentaion.

## Installation of Packages & Modules

Installation is rather staright forward.

For **Community-general** run 
``` bash
ansible-galaxy collection install community.general
```
For any issues with installation consult the install [docs](https://docs.ansible.com/ansible/latest/user_guide/collections_using.html).

For **python v2.7 pip**  run 
``` bash
sudo apt install python-pip
```
For **python v3 pip** (pip3) run
``` bash
sudo apt install python3-pip
```

> Use python3 pip on your Ansible host machine and Proxmox Machine. Only use python2.7 pip on Proxmox machine if ansible playbook throws errors around "Proxmoxer" not installed on Proxmox host.

For **Proxmoxer** run 
``` bash 
sudo pip3 install proxmoxer # For python3 pip (pip3)
# OR
sudo pip install proxmoxer # For python2.7 pip

```

## Ansible Setup

For a detailed guide on getting started with Ansible, head over to there excellent [docs](https://docs.ansible.com/ansible/latest/user_guide/index.html#writing-tasks-plays-and-playbooks), for this Guide we will jump straight to running playbooks targeting Proxmox.

I'll be assuming that you already have setup SSH and Authentication to your targeted Proxmox server. If not, I have an Article about that [here](/posts/2019-12-29-remote-server-key-login-and-lockdown) on this site.


## Ansible & Collections Modules Notes

While I would have liked to structure my Ansible folder with subfolders for playbooks & roles etc, I ran into issues with Ansible not finding the Community.General Module under Collections when running playbooks from different directoris to the default.

You can test If ansible can detect a module if its installed correctly, using the following command 

``` bash
ansible-doc module.namespace

# For community.general.proxmox
ansbile-doc community.general.proxmox
```
This should output the module reference manual in the commandline. If not, then there is an issue with Ansible finding the path to the module.

In the end to get things started, I stayed within the /etc/ansible/ directory, and just placed playbooks, with the rest of the files in the base /ansible directory.

In the future I would look to sort this issue, but for now this will be fine.

## My basic Container

I will be creating a basic Ubuntu 20.04 lts conatiner, with 2 Cpu cores, 512 MB Memory and 16 GB of storage.

The main breakdown of my parameters are in the table below:

| Spec | Value |
| --- | --- |
| OS | Ubuntu 20.04 Server lts |
| CPU | 2 Cores |
| RAM | 512 MB |
| DISK | 16 GB |
| IP | DHCP |
| VLAN | 50 |
| NAMESERVER | 10.100.0.1 |
| DOMAIN | Prod.lab |
| STORAGE Volume | mxzfs |
| TEMPLATE Volume | ISOimages_Alpha |

### Specs Explained

As I have seperate vlans on my network segregating things out, Im using the DHCP server from the Nameserver to generate the Container IP. This nameserver is at `10.100.0.1` on `vlan 50`, the `Prod.lab domain`.

My Proxmox server has a ZFS pool, where all my Container & VM disks are located. This has the Volume name `mxzfs`.

And lastly my `ISO/templates` that I will reference when deplying the OS, is located on a mounted NFS volume called `ISOimages_alpha`.

If you are storing Container & VM disks on the Local Proxmox volume then use `local` for the storage volume.
Also if your templates are local to the Machine, use `local` for the Template volume.

## Writing The Container playbook

For a detailed documentaion of all the parameters available for the Proxmox module consult the proxmox module [docs](https://docs.ansible.com/ansible/latest/collections/community/general/proxmox_module.html)

The minimum required Parameters for the module to execute is `api_host` and `api_user`.

**Node Login & Container credentials**

Node & Container values need no explaining, below table give examples.

_Breakdown of Node & Container Parameters & Values:_

| Name | Value |
| --- | --- |
|api_user|'root@pam'|
|api_password|'apiPassword'|
|api_host|'host.prod.lan'|
|vmid|'123'|
|password|'containerPassword'|
|hostname|'test.prod.lab'|
|node|'host'|

**Container Resources**

Container Resources do need explaining specifically `ostemplate` and `netif` values.

`ostemplate` parameters need to start with the `Volume Name` followed by the `vztmpl` prefix string, then the `Template` to reference. For example `isoVolume:vztmpl/template.tar.gz`.

`Storage` parameter value should be the target volume name for storing the container disk. As explained earlier, if you are storing disk volumes in the default local volume, then use `local`. For my needs I will use the volume named `mxzfs`.

`netif` parameter strings follow a format.
Starts with the network name, like `net0`, followed by comma seperated list of parameters configuring the interface.
Network interface Name `eth0`, the `ip` and `ip6` addresses. Im using DHCP for both so `ip=dhcp,ip6=dhcp` the bridge name `bridge=vmbr0`. Also as im using a vlan with a vlan number I need to add `tag=50`.
Wrap all of this in curly brackets and single quotes.
For example `'{"net0":"name=eth0,ip=dhcp,ip6=dhcp,bridge=vmbr0,tag=50"}'`

_Breakdown of Container Resource Parameters & Values:_

| Name | Value |
| --- | --- |
|cores|'1'|
|cpus|'1'|
|cpuunits|'1000'|
|ostemplate|'ISOimages_Alpha:vztmpl/ubuntu-20.04-standard_20.04-1_amd64.tar.gz'|
|storage|'mxzfs'|
|disk|'16'|
|memory|'512'|
|nameserver|'10.100.0.1'|
|netif|'{"net0":"name=eth0,ip=dhcp,ip6=dhcp,bridge=vmbr0,tag=50"}'|
|searchdomain|'prod.lab'|

Create your playbook.yml file and start the config structure as follows.

``` yaml
- name: The name of the Playbook
  hosts: 'The host target' # host.example.com
  tasks:
  - name: 'The name of this task'
    proxmox: # community.general.proxmox module reference
    ...
    # All the proxmox.module parameters go here
    ...

```

Here is an example of the fleshed out playbook.

``` yaml
---
- name: Create new LXC container in Proxmox Cygnus Host

  hosts: 'cygnus.prod.lan'
  tasks:
  - name: 'Create Container'
    proxmox:
      vmid: '300' # Specifying Container ID
      api_user: 'root@pam' # Proxmox user
      api_password: apiPassword # Password in plaintext !!!
      api_host: 'cygnus.prod.lan' # Proxmox hostname
      password: containerPassword # Password in plaintext !!!
      hostname: 'test.prod.lab' # Container hostname
      node: 'cygnus' # Name of Proxmox host
      cores: '1'
      cpus: '2'
      cpuunits: '1000'
      ostemplate: 'ISOimages_Alpha:vztmpl/ubuntu-20.04-standard_20.04-1_amd64.tar.gz' # Or use local:vztmpl/... as discussed
      storage: 'mxzfs' # Or use 'local' as discussed
      disk: '16'      
      memory: '512'
      nameserver: '10.100.0.1' 
      netif: '{"net0":"name=eth0,ip=dhcp,ip6=dhcp,bridge=vmbr0,tag=50"}'
      searchdomain: 'prod.lab'
      state: 'present'
```

## Running the Playbook

To run this playbook, simply use the command in the ansible base directory:

``` bash
ansible-playbook playbook.yml
```