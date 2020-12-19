# Linux

## apt

Disable IPv6:

```shell
apt-get -o Acquire::ForceIPv4=true update	# temperory

nano /etc/apt/apt.conf.d/99force-ipv4	# permanent: add Acquire::ForceIPv4 "true";
```

