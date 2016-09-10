## Overview
This repository is for creating my setting on CentOS in VM.
After executing `vagrant up`, you can get CentOS, which has `tmux + zsh + neovim`.

## Spec
* OS: CentOS 7
* Hostname: myremote
* IP: 192.168.200.100

## How to use
```
# Create docker container on virtualbox
cd /path/to/ansible-mysetting
vagrant up
vagrant provision
```

## Attention!!!!
vagrant1.8 ver has a bug regarding ssh key so you may encounter the situation where `default: Warning: Authentication failure. Retrying...` occurs periodically during `vagrant up`.
In this case, please login centOS then modify the permission.
login password and username are vagrant.

vagrant1.8系にはssh鍵認証系のバグがあると報告されています。`default: Warning: Authentication failure. Retrying...`のエラーが`vagrant up`中にずっとおこっていた場合、centOS以下のディレクトリのパーミッションを変更してください
````
chmod 0600 .ssh/authorized_keys
````

after that, try to reload vagrant
パーミッション変更後、再度vagrantをreloadしてください。(そうしないと、このあとansibleを実行するときにssh接続がうまくいかない可能性があります。)
```
vagrant reload
```
## Confirmation
```
# login
vagrant ssh

## Confirm the command
zsh
tmux
nvim
```

## If ansible doesn't work

```
vagrant ssh
```

### Confirmation of ssh setting and hosts file
if you haven't access to centOS, please check it.
```
# if the response fails, please make sure hosts file and ssh setting
ansible -i hosts all -m ping -u vagrant --private-key=.vagrant/machines/default/virtualbox/private_key

# if you want to get more detailed information,
ansible -vvvv -i hosts all -m ping -u vagrant --private-key=.vagrant/machines/default/virtualbox/private_key

```
### Confirmation of ssh.config file
if you haven't access to centOS, please check it.
```
# if the response fails even if above response succeed, please make sure ssh.config and ansible.cfg setting
ansible all -m ping

# if you want to get more detailed information,
ansible -vvvv all -m ping
```

## Memo
### ssh setting
If you want to connect centos by `ssh`, please follow the setting.
Default host is defined as `default` so please modify it as you like.
```
# print out the setting to config file
vagrant ssh-config >> ~/.ssh/config

# modify host
vi ~/.ssh/config

# confirmation
ssh host (ex. ssh myremote)
```

### about ssh and virtual machines
VM上のゲストOSにつなぐ方法は二つある。  
ひとつはローカル自身にssh接続し、さらにVMで設定したポートフォワーディングのポートを指定することでつなぐ方法。(上)  
もう一つはゲストOSに割り振ったIPにssh接続する方法。このときにポートの指定は特に行わない。(下)  
`vagrant ssh-config`では下の情報がでてくる。しかし下の情報では`ansible all -m ping`はとおるが、playbook中で接続できなくなる。  
`~/.ssh/config`はどちらの設定でも`ssh`接続できるが、`ssh.config`の場合は上でないとうまくplaybookで接続できなくなるので、注意をする

```
Host myremote
  HostName 192.168.200.100
  User vagrant
```
```
Host myremote
  HostName 127.0.0.1
  User vagrant
  Port 2222
```
