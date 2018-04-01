# Running Regressions on clean Centos 7 machine

This note is intended to help you debug Centos regressions on a machine loaned via softserve. It's critical that you follow the instructions to the letter.

# Steps
* Install Ansible. You can either use `pip install ansible` or `sudo dnf install ansible`.
* Setup the Ansible config file in `~/.ansible.cfg` with at least the following: 
```
[defaults]
roles_path = ~/gluster-infra-ansible/roles
```
* Clone the infrastructure repository. This code will always be kept up-to-date and will work. `git clone https://github.com/gluster/gluster.org_ansible_configuration.git gluster-infra-ansible`
* `cd gluster-infra-ansible`
* Install the roles from the galaxy by running `ansible-galaxy install -r requirements.yml -p roles -f`
* Get a machine from softserve. Let's assume that your machine has the ip `192.168.1.10`.
* When the machine is ready, SSH into it and set the hostname manually with hostnamectl: `hostnamectl set-hostname builder500.cloud.gluster.org`

* Edit the hosts file. Add `builder500.cloud.gluster.org ansible_host=192.168.1.10` under `[jenkins_builders_rax]` section.
* Then run the ansible-playbook with the following command `ansible-playbook -v -i hosts playbooks/deploy_jenkins.yml -u root -l builder500.cloud.gluster.org`
* This job will finish after some time. At this point, you need to ssh into the machine, and reboot it `reboot`.
* Once the machine is back online, run ansible again: `ansible-playbook -v -i hosts playbooks/deploy_jenkins.yml -u root -l builder500.cloud.gluster.org`. **DO NOT SKIP THIS STEP**

* Verify that /etc/hosts has an entry that says something like this: `192.168.1.10 builder500.cloud.gluster.org`.
* Now you can clone gluster, build it, and run regressions.