This note is intended to help you debug Centos regressions on a machine loaned via softserve. It's critical that you follow the instructions to the letter.

Steps
* Install Ansible. You can either use `pip install ansible` or `sudo dnf install ansible`.
* Get a machine from softserve. With migration from RAX to AWS, you cannot SSH into the machine using root user because of security issues.

   **USAGE INSTRUCTION: SSH to the instance and login as 'centos' using the key specified at launch**

  Let's assume that your machine has the ip 192.168.1.10. 
* When the machine is ready, SSH into it as centos user and set the hostname manually with `hostnamectl set-hostname    builder500.cloud.gluster.org`
* Logout from the instance and clone the softserve repo and cd into the playbooks directory on your local machine. `git clone https://github.com/gluster/softserve && cd softserve/playbooks`.
* Update inventory file in current working directory. Add `builder500.cloud.gluster.org ansible_host=192.168.1.10` under `[softserve]`
* Then run the ansible-playbook with the following command `ansible-playbook -v -i inventory regressions-final.yml --become -u centos`
* Now you are ready with the setup where glusterfs source code exists in `/root/glusterfs`.