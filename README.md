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
"ansible all --key-file ~/.ssh/id_ed25519 -i /home/mostefa/ansible-training/inventory -m ping"

We shorten the command by creating inside the repository on the local machine  a file called "ansible.cfg"
We insert as follow
[defaults]

inventory = inventory
private_key_file= ~/.ssh/id_ed25519


