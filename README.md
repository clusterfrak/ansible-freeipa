# Ansible FreeIPA Role
-------

This is an Ansible role that will provision a fresh BIND/FreeIPA server installation. Bind is a Linux DNS resolution service, which is required for FreeIPA. FreeIPA is the Linux equivalent of Microsoft's Active Directory. It allows centalized user authentication for your kerberos realm (domain). FreeIPA also provides tools for domain wide services such as DNS, DHCP, and Certificate management, all wrapped into a web based UI.

<br>

## More Documentation
-------
[clusterfrak.com](http://clusterfrak.com/devops/ansible/ansible_freeipa/)

<br>

## Requirements
-------

__1. &nbsp;&nbsp; Install dependencies:__ <br>

> RedHat based distros (RHEL, CentOS):

```bash
sudo yum -y install epel-release
sudo yum clean all
sudo yum -y install ansible
```

<br>

__3. &nbsp;&nbsp; Create directory structure:__ <br>

Create the directory structure that you are going to use. In this tutorial we are going to set up ansible roles in __/etc/ansible/roles__

<br>

```bash
mkdir -p /etc/ansible/roles || exit 0
```
<br>

__4. &nbsp;&nbsp; Set ansible host:__

Set Ansible localhost entry so that ansible knows it will run against localhost and can talk to itself on localhost without attempting to open a TCP socket connection. 

<br>

```bash
echo localhost ansible_connection=local > /etc/ansible/hosts
```

<br>

## Role Variables
-------

The clusterfrak.freeipa role uses a few environment variables to automatically configure Bind/FreeIPA. The role is set with default values for each of the available variables. Ansible will attempt to gather shell environment variable values and use those values to over-ride the default values that are set. If no shell environment variable is available or set, then ansible will configure itself to use the default values. In order to customize the installation of Bind/FreeIPA, simply export the ansible corresponding shell variable to set the value to something other than default prior to installing the role.

<br>

> Ansible Variables:

 - domain: mydomain.local
 - bind_secondary_dns: 8.8.8.8

<br>

> Mapped Shell Environment Variables:

 - DOMAIN - FQDN that Bind and FreeIPA will be configured to provide services for. This MUST be an FQDN [default:mydomain.local]
 - SECONDARY_DNS - Secondary DNS server address. This can be configured to a second BIND/FreeIPA server or AD [default:8.8.8.8]

<br>

 > Setting Shell Environment Variables:

 To set a variable value simply export the variable prior to running the role install playbook.

<br>

```bash
export DOMAIN="mydomain.com"
```

 <br>

## Dependencies
-------

No other roles required in order to run this role

<br>

## Example Playbook With Default Values
-------

This playbook will set up Bind and FreeIPA, Bind will be configured to use the servers IP address, and automatically configured to use the mydomain.local domain. The servers hostname along with alias's such as ns1.mydomain.local, and ipa.mydomain.local will be configured using the IP of this server. The PTR zone will be automatically configured based on this servers default IPv4 Address/netmask

    - hosts: servers
      become: true
     roles:
       - clusterfrak.freeipa

## Example Playbook With Custom Values
-------

This playbook will set up Bind and FreeIPA, Bind will be configured to use the servers IP address, and automatically configured to use the customdomain.com domain. The servers hostname along with alias's such as ns1.customdomain.com, and ipa.customdomain.com will be configured using the IP of this server. The PTR zone will be automatically configured based on this servers default IPv4 Address/netmask

`export SITE_NAME="customdomain.com"`

	- hosts: servers
	      become: true
	     roles:
	       - clusterfrak.freeipa

## License
-------

BSD

## Author Information
-------

[Rich Nason](http://nason.co) <br>
[Clusterfrak Doc Site](http://clusterfrak.com) <br>
[Container Doc Site](http://appcontainers.com) <br>

