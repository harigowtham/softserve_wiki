This note is intended to help you debug Centos regressions on a machine loaned via softserve. It's critical that you follow the instructions to the letter.

Steps
* Install Ansible. You can either use `pip install ansible` or `sudo dnf install ansible`.
* Get a machine from softserve. Let's assume that your machine has the ip 192.168.1.10.
* When the machine is ready, SSH into it and set the hostname manually with `hostnamectl: hostnamectl set-hostname    builder500.cloud.gluster.org`
* Get the ansible playbook from https://gist.github.com/deepshikhaaa/e8d9d8f934ba971c655a7dd708b1d99b (in the current working directory)
* Create the hosts file in current working directory. Add `builder500.cloud.gluster.org ansible_host=192.168.1.10` under `[mock_jenkins_builder]`
* Then run the ansible-playbook with the following command `ansible-playbook -v -i hosts mock_jenkins_setup.yml -u root`
* Verify that `/etc/hosts` has an entry that says something like this: `192.168.1.10 builder500.cloud.gluster.org.`
* Now you are ready with the setup where glusterfs source code exists in `/srv/glusterfs`
