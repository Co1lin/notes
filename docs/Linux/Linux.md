# Linux

## tmux

[Tmux 使用教程 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2019/10/tmux.html)

```shell
tmux list-session

tmux new -s my_session

tmux attach -t 0

tmux kill-session -t 0

tmux switch -t 0

tmux rename-session -t 0 <new-name>
```

下面是一些会话相关的快捷键。

- `Ctrl+b d`：分离当前会话。
- `Ctrl+b s`：列出所有会话。
- `Ctrl+b $`：重命名当前会话。

划分窗格：

```shell
tmux split-window -h
```

- `Ctrl+b c`：创建 window 。
- `Ctrl+b n`：显示下一个 window 。
- `Ctrl+b p`：显示上一个 window 。
- `Ctrl+b :rename-window <name>`：重命名 window 。
- `Ctrl+b &`：kill 当前 window 。
- `Ctrl+b q`：显示窗格编号。
- `Ctrl+b x`：关闭当前窗格。
- `Ctrl+b {`：当前窗格与上一个窗格交换位置。
- `Ctrl+b }`：当前窗格与下一个窗格交换位置。
- `Ctrl+b !`：将当前窗格拆分为一个独立窗口。

pane 相关：

- `Ctrl+b %`：划分左右两个 pane 。
- `Ctrl+b "`：划分上下两个 pane 。
- `Ctrl+b Alt+Up/Down`： `resize-pane -U/D 10` 调整 pane 的大小

## Disk Related

```shell
lsblk

fdisk -l

df -hT
```

Format disk:

```shell
mkfs -t ext4 /dev/sdb2
mount -t ext4 /dev/sdb2 /data
```

Permanently mount:

```shell
sudo nano /etc/fstab

/dev/sdb2 /data ext4 defaults 1 1
```

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

### SSH Tunnel

```sh
ssh -N -f -L 8008:localhost:8008 user@serverip
```

## xvfb

xvfb enables you to run graphical applications without a display.

```shell
xvfb-run
```

## find

https://www.runoob.com/linux/linux-comm-find.html

```shell
# find <path> <option>

find . -name "*.c"
find . -type f
```

删除空文件夹：先找到空的，然后再借助 `xargs` 删除：

```shell
find -type d -empty | xargs -n 1 rm -rf
```

## xargs

```shell
# <some cmd> | xargs <options> <cmd>

# -n <num> : 每次执行 <cmd> 时用前面管道给的几个参数（空格隔开）；默认是全部
# 下面 -n 1 使得 rm -rf 每次用前面的一个参数，执行多次直到把参数用完。
find -type d -empty | xargs -n 1 rm -rf
```

## ln

Hard link: \~ shared_ptr in C++; 只有当最后一个链接被删除后才能删除。

```shell
ln <src> <dest>
```

Symbolic link:

```shell
ln -s <src> <dest>
```

## Compress

### tar

```shell
# -z   filter the archive through gzip
# -v   verbosely list files processed
# -f, --file=ARCHIVE         use archive file or device ARCHIVE

# -c  create a new archive
tar -czvf <target archive> <dir to compress>

# -x  extract files from an archive
tar -xzvf <archive>

# -t  list the contents of an archive
tar -tzvf <archive>
```

### pigz

!!! tip "Tips about the usage"
  `pigz` itself is dedicated to deal with files, not directory!

```shell
# -9  best quality
# -p<num_processes>  processes
tar cf - <dir> | pigz -9 -p4 > output.tar.gz

# -d decompress a file
# -k keep the original file
pigz -dk -p4 <archive.tar.gz>

# -l list the content
pigz -l <archive.tar.gz>
```

