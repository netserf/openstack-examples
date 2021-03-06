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
```
$ openstack stack create -t single-vm.yaml mystack
```

[Parameterized VM Example](heat-templates/parameterized-vm.yaml) - An example exactly the same as the first one except this one moves all the instance variables into a [parameters file](heat-templates/params1.yaml). This helps improve heat template reuse. You can now reference specific paramters file for different VM variations.
```
$ openstack stack create -t parameterized-vm.yaml -e params1.yaml mystack
```

[Network and VM Example](heat-templates/network-and-vm-with-output.yaml) - An example that creates a private network with one VM in it. To gain access to this private network a router is required which NATs a floating public address to the private IP assigned to the VM. A [parameters file](heat-templates/params2.yaml) has been included.
```
$ openstack stack create -t network-and-vm-with-output.yaml -e params2.yaml mystack
```

[Parameterized VM with Defaults Example](heat-templates/param-with-defaults-vm.yaml) - In some cases we do not want to explicitly state the parameters because this gets verbose and cumbersome. In this case having defaults is nice.
```
$ openstack stack create -t param-with-defaults-vm.yaml mystack
```

[Resource Reuse Example](heat-templates/multiple-vms.yaml) - An example that reuses our earlier [single VM example](heat-templates/single-vm.yaml). In this case we [register the resource](heat-templates/simple_resource.yaml) template and instantiate 2 identical VMs. 
```
$ openstack stack create -t multiple-vms.yaml -e simple_resource.yaml mystack
```

[Parameterized Resource Reuse Example](heat-templates/multiple-vms-with-params.yaml) - An example that uses our earlier [parameterized VM with defaults example](heat-templates/param-with-defaults-vm.yaml). In this case we [register the resource](heat-templates/params-resource.yaml) template and instantiate 2 identical VMs. One of the examples uses the default hostname and the other on overrides the hostname.
```
$ openstack stack create -t multiple-vms-with-params.yaml -e params-resource.yaml mystack
```


