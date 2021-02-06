# Linux

## apt

Disable IPv6:

```shell
apt-get -o Acquire::ForceIPv4=true update	# temperory

nano /etc/apt/apt.conf.d/99force-ipv4	# permanent: add Acquire::ForceIPv4 "true";
```

## SSH

Public Key -> Servers you want to log in

Private Key -> Client

```sh
[root@client]$ ssh-keygen	# Generate public/private key pair
...
Your identification has been saved in /root/.ssh/id_rsa.	# private key
Your public key has been saved in /root/.ssh/id_rsa.pub.	# public key
...
```

```shell
[root@server]$ cd ~/.ssh
[root@server]$ cat id_rsa.pub >> authorized_keys	# add public key as an authorized key on the server
[root@server]$ chmod 600 authorized_keys	# make sure the permissions is correct
[root@server]$ chmod 700 ~/.ssh	# make sure the permissions is correct
```

Config ssh by editing `/etc/ssh/sshd_config`:

```
RSAAuthentication yes
PubkeyAuthentication yes
PermitRootLogin yes
PasswordAuthentication no
```

