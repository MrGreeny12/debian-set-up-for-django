# Ubuntu Server Set Up for Django Instruction

In this guide we will set up clean Ubuntu server for Python and Django projects. We will configure secure SSH connection, install from Ubuntu repositories and from sources all needed packages and ware it together for working Ubuntu Django server.

## Create user, setup SSH

Connect through SSH to remote Ubuntu server and update repositories and install some initial needed packages:

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install -y vim mosh tmux htop git curl wget unzip zip gcc build-essential make zsh tree
```

Configure SSH:
On your PC for copy ssh-keys:
```
ssh-copy-id имя_удаленного_пользователя@удаленный_IP_aдрес
```

```
sudo useradd viktor
sudo vim /etc/ssh/sshd_config
    AllowUsers viktor
    PermitRootLogin no
    PasswordAuthentication no
```

Restart SSH server, change `viktor` user password:

```
sudo service ssh restart
sudo passwd viktor
```

Install [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh):

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Configure some needed aliases:

```
vim ~/.zshrc
    alias cls="clear"
```

## Install python 3.9

```
mkdir ~/code
```

Check Python version in VPS:
```
[Install Python](https://www.linuxcapable.com/how-to-install-python-3-10-on-ubuntu-linux/#:~:text=Installing%20Python%203.10.%20To%20install,module%3A%20sudo%20apt%20install%20python3.10%2Ddev)
```

Ok, now we can pull our project from Git repository (or create own), create and activate Python virtual environment:

```
cd code
git pull project_git
cd project_dir
python3.9 -m venv env
. ./env/bin/activate
```

## Install and configure Docker, Docker-compose
Set up the repository

```
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
Install Docker Engine
```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```
