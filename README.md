# OpenStack Examples
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE.txt)

Some helpful examples and usage of OpenStack technologies that I've been playing with.

## General

[OpenStack Client](https://docs.openstack.org/python-openstackclient/latest/) - Notes on how to use the OpenStack command line client.

[OpenStack Client Install](https://docs.openstack.org/newton/user-guide/common/cli-install-openstack-command-line-clients.html) - Notes on how to install python OpenStack client tools.

## Command Line Cheat Sheet

Some useful OpenStack commands for getting started:
```
openstack catalog list
openstack image list
openstack image show IMAGE

openstack server list
openstack flavor list
openstack server show NAME
openstack console log NAME
openstack server stop NAME
openstack server start NAME
openstack server reboot NAME

openstack network list
openstack network create NETWORK_NAME
openstack subnet create --subnet-pool SUBNET --network NETWORK SUBNET_NAME

openstack keypair list

openstack stack create -t single-vm.yml mystack2
openstack stack delete mystack
```
[More cheat sheets](https://docs.openstack.org/ocata/user-guide/cli-cheat-sheet.html)


## HEAT
[Single VM Example](heat-templates/single-vm.yaml) - An example of how to instantiate a single VM in a pre-existing OpenStack network. This isn't ideal for reuse since the parameters need to be replaced for the OpenStack environment, but its an easy starting point.

[Parameterized VM Example](heat-templates/parameterized-vm.yaml) - An example exactly the same as the first one except this one moves all the instance variables into a [parameters file](heat-templates/params1.yaml). This helps improve heat template reuse. You can now reference specific paramters file for different VM variations.

[Network and VM Example](heat-templates/network-and-vm-with-output.yaml) - An example that creates a private network with one VM in it. To gain access to this private network a router is required which NATs a floating public address to the private IP assigned to the VM. A [parameters file](heat-templates/params2.yaml) has been included.

[Resource Reuse Example](heat-templates/simple_host.yaml) - An example that reuses our earlier [single VM example](heat-templates/single-vm.yaml). In this case we register the resource template and instantiate 2 identical VMs. Try the following:
```
$ openstack stack create -t multiple-vms.yaml -e simple_resource.yaml mystack
$ openstack stack list
$ openstack stack delete mystack
```
