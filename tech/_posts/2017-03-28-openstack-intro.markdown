---
layout: post
title:  "OpenStack Introduction"
date:   2017-03-28 23:12:33 +0000
categories: tech
---

This is a self compiled notes of openstack information. Including general structure, deployment, and setting up instances on a single machine.
<br>

<br>
## 1. OpenStack General Information
OpenStack is a free and open-source software platform for cloud computing, mostly deployed as an infrastructure-as- a-service (IaaS). It is a cloud operating system that controls large pools of compute, storage, and networking resources throughout a datacenter, all managed through a dashboard that gives administrators control while empowering their users to provision resources through a web interface.
<br>
<br>
Cloud Computing is a type of Internet-based computing that provides shared computer processing resources and data to computers and other devices on demand. It is a model for enabling on-demand access to a shared pool of computing resources, which can be rapidly provisioned and released.
Infrastructure as a Service (IaaS) is an online services that abstract the user from the details of infrastructure like physical computing resources, location, data partitioning, scaling, security, backup etc. 
<br>
<br>
Cloud Computing is a type of Internet-based computing that provides shared computer processing resources and data to computers and other devices on demand. It is a model for enabling on-demand access to a shared pool of computing resources, which can be rapidly provisioned and released.
Infrastructure as a Service (IaaS) is an online services that abstract the user from the details of infrastructure like physical computing resources, location, data partitioning, scaling, security, backup etc. 
<br>
 
<br>
## 2. OpenStack Overview
#### 2.1.	Core Services
Nova(Compute): Not a hypervisor, instead talks to the hypervisor to decide where vm is ran. Create VM upon request
<br>
Swift(Object Storage): flexibility of storage. Stores and retrieve files. Glance stores files into swift.
<br>
Glance(image): Deploy vm off of or create an image. Ready to run instance of vm. 
<br>
Cinder(Volume/Block Storage): persistent storage after vm instance is created. Since vm is running on nova, cinder is related to nova and not glance. 
<br>
Neutron(Networking): software defined networking. Logical switches.
 
<br>
#### 2.2.	Detailed Module
Nova:
Interface to the hypervisor. Support many hypervisors. Direct placement of vm
<br><br>
Swift:
Elastic alternative to local storage. Application create binary objects, which are dispersed in a replicated way between storage nodes. (they are replicated so if one node goes down its still fine. Needs 3 nodes) Swift proxy keeps track of object location.
<br><br>
Neutron:
Software defined networking. 
Creating one logical broadcast domain on an infrastructure that contains many routers. 
Separate data from different customers on the same machine.
<br><br>
Glance:
An image of a vm. Not installed but deployed into an instance. 
Default images can be downloaded (cirros) or comes with distribution. Can also be created. 
With the image, a template is used to specify the hardware. 
<br><br>
Cinder:
After creating an instance, cinder block storage is used for persistent storage. 
Uses object store as physical storage. 
<br><br>
Horizon:
Dashboard. Web User Interface. 
Used by admins. Allow options, command line access for full control.
<br><br>
Keystone:
Allow users and roles to access services in tenant environment. 
Uses authentication tokens.
<br><br>
Magnum: (unimportant)
Run containers on top of openStack. Uses bay and bay models. 
<br><br>
Sahara:
Integrate hadoop into OpenStack. (big data)
Hadoop clusters are used. 
<br><br>
Trove: (DBaaS)
Foundation for provisioning and management of sql and nosql databases.
Prepackaged configurations for deploying databases. 
<br><br>
Designate:
DNS as a Service. Creates DNS records. 
Pools are used as discrete name servers. 
<br><br>
Heat:
Orchestration of stacks to make deployment easier.
Stack is a collection of vm, template contains definition for what makes up the stack. (HeatOrchestrationTemplate)
Resources: contains the objects. 
Properties: specifics of template (flavor)
Parameters: properties of resource.
Output: Passed back to the user(shown on horizon)
<br><br>
Message Broker:
Interprocess communication, OpenStack projects talking
Messages to be sent across cloud services using auth token.
Time needs to be sync
<br><br>
Mandatory Backend Services
Databases. RabbitMQ
Needs to be running all the time. 
<br><br>
Ceilometer, Ironic (Bare metal deployment, deploy openstack), Oslo (standardization)
<br><br>
## 3.	Deployment
#### 3.1.	General Information
Red Hat OpenStack distribution at rdoproject.org
Red Hat OpenShift, using containers to deploy cloud soln
Red Hat CloudForms, manage openstack and vm platforms.Deployment methods
<br><br>
#### 3.2.	Deployment Methods
Devstack, for development usages.<br>
Fuel, for production development. Requires at least 3 nodes.<br>
Packstack is deployment method in rdo. Uses puppet to deploy a state on the openstack nodes.
Needs to generate answer files.<br>
In this case OpenStack is installed on top of Red Hat. RHEL can be installed on a physical machine or a virtual machine. 
<br><br>
#### 3.3.	Installation Guide
3.3.1.	Install OS (RHEL)<br>
Installing red hat developer subscription (15 to 20 mins) (either on vmware or physical machine)
https://developers.redhat.com/products/rhel/hello-world/#fndtn-vmware<br>
Requirement: 4GB RAM (6GB preferable), 20GB storage, RHEL 7.0+<br>
To use the repositories and yum commands, Use a developer subscription. One developer subscription can be used on one installation on a physical machine or unlimited installation on virtual machines. 
<br><br>
3.3.2.	Installation Documents
Follow the guides here: <br>
[https://www.rdoproject.org/install/quickstart/][guide3.3.2]<br>
and here:<br>
[https://www.rdoproject.org/networking/neutron-with-existing-external-network/][guide3.3.2-2]<br>
The steps are outlined below.
<br><br>
3.3.3.	Enable Repositories<br>
`subscription-manager repos --enable=rhel-server-rhscl-7-rpms`<br>
`subscription-manager repos --enable=rhel-7-server-optional-rpms`<br>
`subscription-manager repos --enable=rhel-7-server-extras-rpms`<br>
`subscription-manager repos --enable=rhel-7-server-rh-common-rpms`<br>
<br><br>
3.3.4.	Setup<br>
`systemctl disable NetworkManager`<br>
`systemctl stop NetworkManager`<br>
`systemctl enable network`<br>
`systemctl start network`<br>
<br><br>
3.3.5.	Install Files<br>
`yum install -y https://rdoproject.org/repos/rdo-release.rpm`<br>
`yum update -y`<br>
`yum install -y openstack-packstack`<br>
`packstack --allinone  (this is no longer used as it is the default installation)`<br>
`packstack --allinone --provision-demo=n --os-neutron-ovs-bridge-mappings=extnet:br-ex --os-neutron-ovs-bridge-interfaces=br-ex:eth0 --os-neutron-ml2-type-drivers=vxlan,flat`<br>
(replace the eth0 with the primary nic name on your station. Use “ip ad” to see the list of ports and use the one with connectivity.)<br>
(this creates an ovs bridge and maps it to the existing port)<br>
(takes 20 to 60 mins to install)<br>
`openstack-status` //if doesn’t work use “yum install openstack-utils”<br>
<br><br>
3.3.6.	Setup Network Configurations<br>
Edit the `/etc/sysconfig/network-scripts/ifcfg-br-ex` file<br>
Change or edit the content to match<br>
{% highlight ruby %}
DEVICE=br-ex
DEVICETYPE=ovs
TYPE=OVSBridge
BOOTPROTO=static
IPADDR=192.168.1.32 (this should be the current network ip address)
NETMASK=255.255.255.0
GATEWAY=192.168.1.1  (current network gateway)
DNS1=8.8.8.8
ONBOOT=yes
{% endhighlight %}
(this move the network parameters from eth0/ens192/.. to br-ex.)<br>
<br><br>
Edit the `/etc/sysconfig/network-scripts/ifcfg-eth0` file (replace eth0 with the system’s port name)<br>
{% highlight ruby %}
DEVICE=eth0 (replace with port name)
TYPE=OVSPort
DEVICETYPE=ovs
OVS_BRIDGE=br-ex
ONBOOT=yes
{% endhighlight %}
<br>
Reboot the system.<br>
<br><br>

## 4.	Creating an Instance (Using Horizon Dashboard)
#### 4.1.	Basic Introduction
The following are needed to start an instance:<br>
<br>
Image – glance<br>
An image is delivered by glance. It boots off of a live cd into an OS.
<br><br>
Flavor – nova<br>
Defines the hardware settings that the instance will use. How much VCPU, RAM, storage etc.
<br><br>
Networking – neutron<br>
Configure the internal, external networks, routers and IP addresses.
<br><br>
Security group – nova<br>
Set of firewall rules associated to the instance.
<br><br>
Ssh keys – nova<br>
Key pair for login authentication.
<br><br>
Nova boot<br>
To start the instance. 
<br><br>
Floating ip address – neutron<br>
Used to expose the instance to external networks. 
<br><br>
Volume – cinder<br>
Stores information from inside the instance in a permanent way. 
<br><br>
#### 4.2.	Create Project
Identity->Projects (project is aka tenant):<br>
Create Project, add project information.<br>
Project members consists of services.<br>
Quota tab, resources available within the project. <br>
Example configuration: 4 VCPU, 4 Instances, 2048 Ram, 5 Floating IP, 2 network.
<br><br>
#### 4.3.	Create User
Identity->Users:<br>
User name, password, primary project that it belongs to and the role (member, admin)
<br><br>
#### 4.4.	Create Image
(The following can be done by member)<br>
Project->Compute->Images<br>
Create Image, Image name. <br>
Image Source can only be uploaded locally. Chose the file and format. <br>
(example: [https://docs.openstack.org/image-guide/obtain-images.html][guide4.4] )<br>
(download the cirros test, cirros-0.3.5-x86_64-disk.img. The recommended format is qcow2)
<br><br>
#### 4.5.	Create Flavor
(The following needs to be done by admin user or above)<br>
Admin->System->Flavors<br>
Create flavor, name (m0.little etc), 1 VCPU, 256 RAM, 1 Root Disk
<br><br>
#### 4.6.	Create Network
4.6.1.	Installation Document<br>
Follow: [https://www.rdoproject.org/networking/neutron-with-existing-external-network/][guide4.6.1]<br>
(Steps outlined below in CLI) <br>
(before you move on to the next steps, allocate some ip address on your network. 3 to 5+)<br>
(continue as admin)
<br><br>
4.6.2.	Create the External Network (Using Command Line)<br>
cd to `/root` and `source keystonerc_admin` to gain access to openstack.<br>
`neutron net-create external_network --provider:network_type flat --provider:physical_network extnet  --router:external`<br>
(this creates the external network under admin->networks)<br>
`neutron subnet-create --name public_subnet --enable_dhcp=False --allocation-pool=start=192.168.1.245,end=192.168.1.250 \ --gateway=192.168.1.1 external_network 192.168.1.0/24`<br>
(replace the allocation pool with the allocated IP address. Replace gateway and external network accordingly)
(once configured, the external network will use 3 IP addresses. One for the DHCP, one for router gateway, and one for floating IP.)
<br><br>
4.6.3.	Create the External Network (Using Horizon Dashboard)<br>
(not recommended as there is no documentation on this)<br>
Admin->System->Networks<br>
Create Network, name external_network, Project, Provider network type flat, physical network extent, segmentation id none, check external network<br>
Project->Network->Networks<br>
Click on external_network, Subnets, Create Subnet<br>
Subnet name, network address is the system’s ip address in CIDR, gateway ip accordingly. <br>
Disable DHCP, in allocation pool enter the allocated ip addresses. DNS name server 8.8.8.8<br>
<br>
4.6.4.	Create the Internal Network<br>
(as member)<br>
Project->Network->Network<br>
Create Network, name int-net.<br>
Subnet, name int-sub. <br>
Network address 10.0.0.0/24 (doesn’t need to match anything on physical)<br>
Gateway IP 10.0.0.1 (used to connect to outside network. Default is 1st one)<br>
Subnet details , Allocation Pools “10.0.0.100,10.0.0.150”, DNS Name Servers “8.8.8.8”<br>
<br>
#### 4.7.	Create Router
(login as member)<br>
Project->Network->Routers<br>
Create router, name router1, set external network external_network<br>
Click into router1, go to interfaces, click add interface.<br>
Subnet, int-net, ip address(optional): 10.0.0.1. (might want to not set this originally and not fill this)<br>
<br>
#### 4.8.	Launch Instance
Project->Compute->Instances<br>
Launch Instance<br>
Instance name, availability zone, count<br>
Source, select image<br>
Flavor, select flavor (m0.little)<br>
Networks, select int-net<br>
Key Pair, Create Key Pair, enter a name. Will automatically download .pem file which will be used to access the instance.<br>
Launch Instance. <br>
(the instance will be assigned a private ip address in the int-net)<br>
<br>
#### 4.9.	Connect to an Instance
4.9.1.	Create Floating IP Address<br>
Project->Computer->Instances<br>
Click on the drop down list under actions, and click on associate floating IP. <br>
Create a floating IP under the external_network and associate it with the instance’s private ip address. This floating ip can now be used to access the instance.<br>
	You can access the Instance in the following three ways:<br>
<br>
4.9.2.	Horizon Dashboard<br>
Project->Compute->Instances<br>
Click on the instance<br>
Go to Console<br>
<br>
4.9.3.	Linux SSH<br>
ssh -i keypair.pem cirros@10.0.0.1 (the username and floating ip of the instance)<br>
<br>
4.9.4.	Windows Putty<br>
Follow: [https://github.com/naturalis/openstack-docs/wiki/Howto:-Creating-and-using-OpenStack-SSH-keypairs-on-Windows][guide4.9.4]<br>
(Steps outlined below)<br>
Download puttyGen, run.<br>
Load the .pem key. Need to select see all files. <br>
Save private key. Into ppk file.<br>
Run putty. <br>
Put cirros@10.0.0.1 (the username and floating ip of the instance)<br>
On the left hand side menu, go to Connection->SSH->Auth<br>
Click browse to load the ppk file. <br>
Open and connect.<br>
<br>
## 5.	Creating an Instance (Using Command line Interface)
Not recommended. Use horizon dashboard instead. This was used for default installation, does not include connection with existing network. However, do read through the commands.<br>
<br>
{% highlight ruby %}

Navigate to /root

Source keystonerc_admin 
 
//openstack --help | grep “keyword” 
//this can be used to search help to find what you need.
 
 
openstack service list
openstack endpoint list
neutron net-list (list of networks)
keystone –help
nova list (list of instances)
nova hypervisor-list (list of hypervisors)
  
openstack service show ID
  

credential files:
PROJECT-openrc.sh
https://docs.openstack.org/user-guide/common/cli-set-environment-variables-using-openstack-rc.html
  
project:
openstack project create PROJECTNAME
openstack project list
  
openstack domain list
openstack domain create DOMAINNAME
  
openstack group create --domain DOMAINNAME GROUPNAME
openstack group add user GROUPNAME USERNAME
  
openstack user list
openstack user create USERNAME
openstack user create --password secret USERNAME
openstack role list
openstack user role list
openstack role add --project PROJECTNAME --user USERNAME ROLENAME
  
openstack flavor list
openstack flavor create --ram 256 --disk 1 --vcpu 1 --public FLAVORNAME(m1.micro)
  
wget https://download.cirros-cloud.net/0.3.4/cirros-0.3.4-x86_64-disk.img | --no-check-certificate
  
//import img to glance
glance image-create --name IMGNAME --disk-format qcow2 --container-format bare --visibility public --file cirros-0.3.4-x86_64-disk.img
glance image-list
  
//to switch user
cp keystonerc_demo keystone_USERNAME
//edit username, password, ps1, tenant_name(set to projectname)
  
//start the instance
nova boot --flavor FLAVORNAME --image IMGNAME INSTANCENAME
  
//ssh keys
nova keypair-add KEYNAME
nova keypair-list
  
//security groups
nova secgroup-list
nova secgroup-create SECNAME(ssh-and-web) “for ssh and web traffic”
nova secgroup-add-group-rule SECNAME
nova secgroup-add-rule SECNAME tcp 80 80 0.0.0.0/0
nova secgroup-add-rule SECNAME tcp 20 20 0.0.0.0/0
  
//quotas
openstack quota show USERNAME
openstack quota set --ram 10240 --instance 4 --gigabytes 100 PROJECTNAME
openstack quote show PROJECTNAME
  
//service endpoint
openstack service create TYPE --name NAME
openstack endpoint create --public http://192.168.1.x:5678 --adminurl https://192.168.1.x:5678 --internalurl http://192.168.1.x:5678 SERVICEID
  
//troubleshoot
cd /var/log
grep -v INFO keystone.log
  
{% endhighlight %}


[guide3.3.2]: https://www.rdoproject.org/install/quickstart/
[guide3.3.2-2]: https://www.rdoproject.org/networking/neutron-with-existing-external-network/
[guide4.4]: https://docs.openstack.org/image-guide/obtain-images.html
[guide4.6.1]: https://www.rdoproject.org/networking/neutron-with-existing-external-network/
[guide4.9.4]: https://github.com/naturalis/openstack-docs/wiki/Howto:-Creating-and-using-OpenStack-SSH-keypairs-on-Windows




