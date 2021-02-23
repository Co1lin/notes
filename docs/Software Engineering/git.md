# Git

Ref: [Git 多用户配置](https://blog.csdn.net/yuanlaijike/article/details/95650625)

Unset global configuration:
```sh
git config --global --unset user.name
git config --global --unset user.email
```

Check:

```shell
git config --global --list
```

## Use ssh key for git authentication

Generate key file:

```shell
ssh-keygen -t rsa -C "colin@gmail.com" # -C: comment to identify the key
```

Add public key to GitHub ,GitLab, or etc.

Add private key to localhost:

```sh
ssh-add ~/.ssh/id_rsa_github
```

Let's check:

```shell
ssh-add -l
```

## Multiple git user:

Edit `~/.ssh/config`:

```
Host github
HostName github.com
User jitwxs
IdentityFile ~/.ssh/id_rsa_github

Host gitlab
HostName gitlab.mygitlab.com
User lemon
IdentityFile ~/.ssh/id_rsa_gitlab
```

Test:

```shell
ssh -T git@github.com
ssh -T git@gitlab.mygitlab.com
```

## Set commit user name for repo

```shell
git config --local user.name "name"
git config --local user.email "colin@gmail.com"
```

Check:

```shell
git config --local --list
```

