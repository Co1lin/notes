# Linux

## Monitoring or Error Tracing

```shell
glances	# install by pip

# display the system message buffer
# can be used to find OOM message
dmesg
```

## tmux

[Tmux 使用教程 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2019/10/tmux.html)

https://www.rushiagr.com/blog/2016/06/16/everything-you-need-to-know-about-tmux-copy-pasting-ubuntu/

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
- `Ctrl+b D`：list all connected clients to detach

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
blkid

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

Ignore `The following signatures were invalid`:

```shell
# add [trusted=yes], e.g.
echo "deb [trusted=yes]  http://dist.alphadrive.ai/apt ./" > /etc/apt/sources.list.d/alphadrive.list
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

Flatten files recursively

```bash
find original_dir -mindepth 1 -type f -name '*.js' -exec mv -i '{}' target_dir ';'
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

## netplan

```shell
cd /etc/netplan
sudo netplan generate
```

```yaml
network:
    ethernets:
        enp1s0f0:
            addresses:
                - 192.168.2.222/24
            gateway4: 192.168.2.1
            nameservers:
                addresses:
                    - 8.8.8.8
    version: 2
```

```shell
sudo netplan apply
```

gateway to the Internet:

```shell
sudo sysctl -p
#net.ipv4.ip_forward = 1
sudo iptables -A FORWARD -i enp1s0f0 -j ACCEPT
sudo iptables -A FORWARD -i enp1s0f1 -j ACCEPT
sudo iptables -t nat -A POSTROUTING -o enp1s0f0 -j MASQUERADE
sudo iptables -t nat -A POSTROUTING -o enp1s0f1 -j MASQUERADE
```

## iperf

```shell
iperf -s <ip>
iperf -c <ip>
```

## rsync

```shell
rsync -r src dst
rsync -r source1 source2 destination

# -a	to preserve every thing (archive mode), such as hidden files, meta info (last updated time, permissions)
rsync -a source destination

# -n / --dry-run	simulate the result of operation
# -v	verbose
rsync -anv source/ destination

# --delete	delete files only exist in dst
rsync -av --delete source/ destination

# --include '*.txt'
# --exclude '*.txt'
rsync -av --include="*.txt" --exclude='*' source/ destination
```

### Remote

```shell
# SSH protocol	username@host:/path/to/file
# -e specify the remote shell to use
rsync -av -e 'ssh -p port' source destination

# rsync protocol	username@host::module/destination

# --append-verify (verify checksum of existing data), --append
# -P = --partial + --progress ; --info=progress2 ???
rsync -azhP --append-verify -e 'ssh -p xxxxx' user@somewhere:/path .
```

## User management

```shell
sudo adduser colin
sudo usermod -aG sudo colin

sudo visudo
$USER ALL=(ALL) NOPASSWD: ALL
```

## swap

```shell
swapon -s
swapon -a
dd if=/dev/zero of=/swapfile bs=1M count=32768
mkswap /swapfile
swapon /swapfile
swapon -s
```

## curl

```shell
# resume downloading
curl -C - -O link
# execute remote sh script
curl -L link | bash
bash <(curl -L link)

       -L, --location
              (HTTP)  If  the  server reports that the requested page has moved to a different
              location (indicated with a Location: header and a 3XX response code),  this  op‐
              tion will make curl redo the request on the new place.
       -C, --continue-at <offset>
              Continue/Resume a previous file transfer at the given offset. 
              Use "-C -" to tell curl to automatically find out where/how to resume the transfer. It then uses the given output/input files to figure that out.
       -O, --remote-name
              Write output to a local file named like the remote file we get. (Only the file part of the remote file is used, the path is cut off.)

              The file will be saved in the current working directory. If you want the file saved in a different directory, make sure you change the current working directory
              before invoking curl with this option.

```

## htop configuaration

```shell
# Beware! This file is rewritten by htop when settings are changed in the interface.
# The parser is also very primitive, and not human-friendly.
fields=0 48 17 18 38 39 40 2 46 47 49 1
sort_key=46
sort_direction=1
hide_threads=0
hide_kernel_threads=1
hide_userland_threads=0
shadow_other_users=0
show_thread_names=0
show_program_path=1
highlight_base_name=0
highlight_megabytes=1
highlight_threads=0
tree_view=0
header_margin=1
detailed_cpu_time=0
cpu_count_from_zero=0
update_process_names=0
account_guest_in_cpu_meter=0
color_scheme=0
delay=15
left_meters=Memory Swap CPU Load LoadAverage
left_meter_modes=1 1 1 1 1
right_meters=Tasks LoadAverage Uptime
right_meter_modes=2 2 2
```

## git

```shell
GIT_SSL_NO_VERIFY=1 git clone https://mydomain.com/myrepo.git
```

## bash profile

```bash
export SHELL=/bin/zsh
exec /bin/zsh -l
```













