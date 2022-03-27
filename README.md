# Debian Server Set Up for Django Instruction

In this guide we will set up clean Ubuntu server for Python and Django projects. We will configure secure SSH connection, install from Ubuntu repositories and from sources all needed packages and ware it together for working Ubuntu Django server.

## Create user, setup SSH

Connect through SSH to remote Ubuntu server and update repositories and install some initial needed packages:

```
sudo apt-get update ; \
sudo apt-get install -y vim mosh tmux htop git curl wget unzip zip gcc build-essential make
```

Configure SSH:

```
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

## Init — must-have packages & ZSH

```
sudo apt-get install -y zsh tree redis-server nginx zlib1g-dev libbz2-dev libreadline-dev llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev liblzma-dev python3-dev python-pil python3-lxml libxslt-dev python-libxml2 libffi-dev libssl-dev python-dev gnumeric libsqlite3-dev libpq-dev libxml2-dev libjpeg-dev libfreetype6-dev libcurl4-openssl-dev supervisor
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

mkdir ~/code

Build from source python 3.9, install with prefix to ~/.python folder:

```
wget https://www.python.org/ftp/python/3.9.12/Python-3.9.12.tgz ; \
tar xvf Python-3.9.* ; \
cd Python-3.9.12 ; \
mkdir ~/.python ; \
./configure --enable-optimizations --prefix=/home/viktor/.python ; \
make -j8 ; \
sudo make altinstall
```

Now python3.7 in `/home/viktor/.python/bin/python3.9`. Update pip:

```
sudo /home/viktor/.python/bin/python3.9 -m pip install -U pip
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
