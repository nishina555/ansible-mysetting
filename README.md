## Overview
This repository is for creating my setting on centOS in VM.
After executing `vagrant up`, you can get centOS, which has `tmux + zsh + neovim`.

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
