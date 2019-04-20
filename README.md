# Ansible FreeIPA Role

-------

This is an Ansible role that will provision a fresh FreeIPA server installation. FreeIPA is the Linux equivalent of Microsoft's Active Directory. It allows centalized user authentication for your kerberos realm (domain). FreeIPA also provides tools for domain wide services such as DNS, and Certificate management, all wrapped into a web based UI for easy domain service control.

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

__2. &nbsp;&nbsp; Create directory structure:__ <br>

Create the directory structure that you are going to use. In this tutorial we are going to set up ansible roles in __/etc/ansible/roles__

<br>

```bash
mkdir -p /etc/ansible/roles || exit 0
```

<br>

__3. &nbsp;&nbsp; Set ansible host:__ <br>

Set Ansible localhost entry so that ansible knows it will run against localhost and can talk to itself on localhost without attempting to open a TCP socket connection. 

<br>

```bash
echo localhost ansible_connection=local > /etc/ansible/hosts
```

<br>

## Role Variables

-------

The clusterfrak.freeipa role uses a few environment variables to automatically configure FreeIPA. The role is set with default values for each of the available variables. Ansible will attempt to gather shell environment variable values and use those values to over-ride the default values that are set. If no shell environment variable is available or set, then ansible will configure itself to use the default values. In order to customize the installation of FreeIPA, simply export the ansible corresponding shell variable to set the value to something other than default prior to installing the role.

<br>

> Ansible Variables:

    * domain: mydomain.local
    * dsmgr_password: Fr33IPA#DS#MGR 
    * admin_password: Fr33IPA#DS#Admin

<br>

> Mapped Shell Environment Variables:

    * ${DOMAIN}: FQDN that Bind and FreeIPA will be configured to provide services for. This MUST be an FQDN [default:mydomain.local]
    * ${MGR_PASS}: Password set for management services within the FreeIPA Console
    * ${ADMIN_PASS}: FreeIPA Administrator password

<br>

 > Setting Shell Environment Variables:

 To set a variable value simply export the variable prior to running the role install playbook.

<br>

```bash
export DOMAIN="mydomain.com"
export MGR_PASS="mysecretpassword"
export ADMIN_PASS="mysecretpassword"
```

 <br>

## Dependencies

-------

Clusterfrak.bind or a pre-existing bind installation is required to run FreeIPA

<br>

## Example Playbook With Default Values

-------

This playbook will set up FreeIPA, FreeIPA will be configured to use the servers IP address, and automatically configured to use the mydomain.local domain.

`ansible-galaxy install --ignore-certs clusterfrak.freeipa`

```bash
cat >> /etc/ansible/install.yml <<EOL
- hosts: dns-servers
  become: true
  roles:
    - clusterfrak.freeipa
EOL
```

`cd /etc/ansible && ansible-playbook install.yml`

<br>

## Example Playbook With Custom Values

-------

This playbook will set up FreeIPA, FreeIPA  will be configured to use the servers IP address, and automatically configured to use the customdomain.com domain. The Management and Admin passwords will also be set accordingly. This assumes that your DS servers are already in the [ds-servers] group in your ansible inventory file.

'''bash
export DOMAIN="customdomain.com"
export MGR_PASS="mycustompassword"
export ADMIN_PASS="mycustompassword2"
'''

`ansible-galaxy install --ignore-certs clusterfrak.freeipa`

```bash
cat >> /etc/ansible/install.yml <<EOL
- hosts: dns-servers
  become: true
  roles:
    - clusterfrak.freeipa
EOL
```

`cd /etc/ansible && ansible-playbook install.yml`

<br>

## License

-------

BSD

## Author Information

-------

[Rich Nason](http://nason.co) <br>

[Clusterfrak Doc Site](http://clusterfrak.com) <br>

[Container Doc Site](http://appcontainers.com) <br>