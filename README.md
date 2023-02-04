# Archival Purposes

# WARNING - This playbook is outdated and no longer works on modern RHEL-Like distros.

This an archive of an Ansible playbook circa 2017 or so. It only targets CentOS 7 and installs the Sensu monitoring server, as well as installing the Sensu client on CentOS 7 and Ubuntu clients. It uses outdated versions of Sensu and rabbitmq. This was written before Sensu's transition to the new "Sensu GO" system.

# Deploy Sensu Server

This repo contains an Ansible playbook that will automatically install and configure a CentOS 7 system as a Sensu monitoring server. Most of the checks that are used are Nagios-compatible. Therefore, various Nagios plugin packages will be installed on the systems.

It will also install and configure the sensu monitoring client on the nodes that you specify in the `monitored-servers` hostgroup. They will be configured to talk to the sensu master server that you specify in the `sensu-server` hostgroup.

## Table of Contents
  * [Requirements](#requirements)
  * [Usage](#usage)

<a name="requirements"></a>
## Requirements

  * [Ansible Control System Requirements](https://docs.ansible.com/ansible/latest/intro_installation.html#control-machine-requirements)
  * A system with the `ansible` and `git` packages installed.
  * The ability to SSH to the target system as root with an SSH key.
  * The target system that will be the Sensu server needs to be running the CentOS 7 Linux distribution.
  * The target CentOS 7 system should have a valid FQDN (Fully Qualified Domain Name) on your local network.

<a name="usage"></a>
## Usage

This is a standalone playbook, meaning that it includes its own `ansible.cfg` file and does not need a system-level config file.

To use this playbook:

1. On your "control" system with ansible and git installed, clone this git repo.
  
2. In the [hosts](hosts) file, you will want to define the host that you want to make your sensu master server as well as the hosts that you want to monitor with said master server.
  * Define your sensu master server in the `sensu-server` hostgroup
  * Define your clients that you want to monitor in the `monitored-servers` hostgroup
  
3. In the [group_vars/all](group_vars/all) file, configure the following variables:
  * The `sensu_user` variable. This is the username of the sensu user in rabbitmq. It's also the username that's used when the sensu clients connect to the server.
  * The `sensu_password` variable. This is the password for the sensu user in rabbitmq. It's also the password that's used when the sensu clients connect to the server.

4. Now, run the ansible deploy command with this playbook:

```
ansible-playbook deploy-sensu-environment.yml
```

In a few minutes, you should have a working Sensu monitoring server that is set up to monitor your defined clients.

After the playbook is finished, you can access the web interface of your Sensu server by navigating to:

`https://your-hostname.example.com/`

This installation uses a self-signed SSL cert by default, so your browser will warn you that it can't validate the cert and that it may not be secure. Just continue through to the login page.