# git

## mailmap

```shell
src_username <src_mail> det_username <det_mail>
```

## SSH Access

```shell
git config --add --local core.sshCommand 'ssh -i /path/id_ed25519'
```

In `.git/config` :

```
[remote "origin"]
        url = git@github.com:Co1lin/repo.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[core]
        sshCommand = ssh -i /path/id_ed25519
[user]
        email = 
        name = 
```

