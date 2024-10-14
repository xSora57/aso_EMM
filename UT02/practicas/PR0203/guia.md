# PR0203: Configuración de SSH para varios usuarios
## 1. Preparación de la máquina y configuración de la red
### 1. Elegir la ip con los compañeros y configurar el archivo netplan
```bash
# This is the network config written by 'subiquity'
network:
  ethernets:
    eth0:
      dhcp4: true
      dhcp6: false
    eth1:
      addresses: [172.18.0.3/16]
  version: 2
```
### 2. En tu servidor crea una cuenta de usuario para tí y otra para cada uno de tus compañeros
```bash
vagrant@ubuntu2204:~$ sudo adduser ernesto
Adding user `ernesto' ...
Adding new group `ernesto' (1004) ...
Adding new user `ernesto' (1004) with group `ernesto' ...
Creating home directory `/home/ernesto' ...
Copying files from `/etc/skel' ...
New password: 
Retype new password:
passwd: password updated successfully
Changing the user information for ernesto
Enter the new value, or press ENTER for the default
        Full Name []:
        Room Number []:
        Work Phone []:
        Home Phone []:
        Other []:
Is the information correct? [Y/n] y
```
En el archivo /etc/passwd
```bash
raul:x:1001:1001:,,,:/home/raul:/bin/bash
ana:x:1002:1002:,,,:/home/ana:/bin/bash
alvaro:x:1003:1003:,,,:/home/alvaro:/bin/bash
ernesto:x:1004:1004:,,,:/home/ernesto:/bin/bash
```
### 3. Los usuarios no podrán conectarse por contraseña
```bash
ssh-keygen -t rsa -b 1024
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ernesto/.ssh/id_rsa): 
Created directory '/home/ernesto/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ernesto/.ssh/id_rsa
Your public key has been saved in /home/ernesto/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:yV5xqTgm3ZRwh6+HTMQxn51dA9s0PqotFIETZTo1LFM ernesto@ubuntu2204.localdomain
The key's randomart image is:
+---[RSA 1024]----+
|        .oXE...+.|
|         BB*o+=o+|
|         +Bo=.o+.|
|       o =o+o . .|
|      . So++ .   |
|       + o= +    |
|        .  + .   |
|            .    |
|                 |
+----[SHA256]-----+
```

```bash
ernesto@ubuntu2204:~/.ssh$ scp /home/ernesto/.ssh/id_rsa.pub  sora@172.18.0.4:/home/sora
The authenticity of host '172.18.0.4 (172.18.0.4)' can't be established.
ED25519 key fingerprint is SHA256:UPUCj6xEdUKBNW/HqOC+G9l+YDT9egS0D+aYlths3q4.     
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.18.0.4' (ED25519) to the list of known hosts.      
sora@172.18.0.4's password: 
id_rsa.pub                                       100%  244   145.0KB/s   00:00    
ernesto@ubuntu2204:~/.ssh$ ssh sora@172.18.0.4
sora@172.18.0.4's password: 
Last login: Mon Oct 14 10:57:38 2024 from 172.18.0.3
sora@ubuntu2204:~$ ls
id_rsa.pub
sora@ubuntu2204:~$ mkdir .ssh
sora@ubuntu2204:~$ cd .ssh
sora@ubuntu2204:~/.ssh$ cd ..
sora@ubuntu2204:~$ ls
id_rsa.pub
sora@ubuntu2204:~$ mv id_rsa.pub .ssh/authorized_keys
sora@ubuntu2204:~$ cd .ssh
sora@ubuntu2204:~/.ssh$ ls
authorized_keys
sora@ubuntu2204:~/.ssh$ ls -l
total 4
-rw-r--r-- 1 sora sora 244 Oct 14 11:33 authorized_keys
```