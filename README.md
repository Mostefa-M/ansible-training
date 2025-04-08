# ansible-training

Working with Ansible and Version Control Git


We install git (yum/dnf install git -y) 
To work with Ansible seamlessly we need to get firt a GIT repository so we create one on GitHub for example,
Then we make an ssh-passwordless (preferably -t  ed25519 and add comment with -C " ")  between our Ansible Control-Host and others servers we want to manage.
We coppy the ed25519 publickey created, on our Github under Profile/Setting/SSH and GPG key"

We return to the repository create on GitHub and under the Green rectangle named "code" 
we choose clone ssh and we copy the link git@github.com:Mostefa-M/ansible-training.git
On the Ansible Control-Host (server5) we type " git clone" and we paste the link from GitHub clone SSH
So it should looks like this : sudo git clone git@github.com:Mostefa-M/ansible-training.git" and press "yes"

A directory named after the name we choose will be create with that README.md"

We need also to provide a name and and email address otherwise we will get errors"
To do that we type:
git config --global user.name "Mostefa"
git config --global user.mail "mous94400@hotmail.com"
a ".gitconfig" file will be created containing the provided information" under ~/

With "git status" command it will show us in red color  anything that might have been changed and with  "git diff README.md  it will shows us the line where changed happened.

With "git add README.md" it will make changes available on the README.md locally.

If we do again "git status we will see modified: README.md in green color.

With "git commit -m "update readme file, initial commit" we will validate the change on the README.md
the "-m" option  is to write message to the commit.

And most importantly to make change on the MASTER Branch on the  actual README.md on our GitHub repository we need to used this following command

git push origin main 


......................................
We create file called "inventory" that will contain all IP addresses of the server we want to manage.
Then use git add inventory, git commit -m "First version of the inventory".


To make sure that all servers are reachable we run this command 

ansible all --key-file /root/.ssh/ansible_key.pub  -i /root/ansible-training/inventory -m ping

(explanation we run the ansible command against all, we use the path of the public key, -i is to specify the inventory file path, -m is for choosing the module ping that will make a real ssh connection to test the connectivity).

We shorten the command by creating inside the repository on the local machine  a file called "ansible.cfg"
We insert as follow
[defaults]

inventory = inventory
private_key_file= /root/.ssh/ansible_key.pub 

So by typing the command:
ansible all -m ping 

We will get the same result as the previous command because ansible will check inside the local ansible.cfg and will found the name of the inventory that
is also named inventory and the path for the public key.
..........................................................................................................................................................
ansible all --list-hosts (shows us the numbers of hosts) must be run inside the GitHub repository folder 
ansible all -m gather_facts (Gathered all information related to the Remote Servers).
ansible all -m gather_facts --limit 192.168.1.91  (Gathered all information related to one specific server).

...............................................................................................

ansible all -m dnf -a update_cache=true --become --ask-become-pass

This Ansible command does the following:

ansible all: Targets all hosts defined in your Ansible inventory.
-m dnf: Specifies the dnf module to be used. This module is used to manage packages on systems that use the DNF package manager (like Fedora, CentOS 8+, RHEL 8+).
-a update_cache=true: Passes the argument update_cache=true to the dnf module. This tells DNF to update its package cache, ensuring it has the latest information about available packages.
--become: Instructs Ansible to use privilege escalation (like sudo or other configured methods) to execute the task as a different user, typically root.
--ask-become-pass: Tells Ansible to prompt you for the password required for privilege escalation (the become password, often your sudo password).
In summary, when you run this command:

Ansible will connect to all the hosts listed in your inventory.
For each host, it will attempt to run the dnf module with the update_cache=true argument.
Because of the --become flag, Ansible will try to execute this command with elevated privileges (usually as root).
The --ask-become-pass flag will cause Ansible to prompt you on your control machine to enter the sudo (or other privilege escalation) password. You will need to enter this password for Ansible to successfully execute the dnf update --refresh (or equivalent) command on the remote hosts.
This command is commonly used as a preliminary step in Ansible playbooks or ad-hoc commands to ensure that the managed hosts have an up-to-date package cache before installing, updating, or removing software.
