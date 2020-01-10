# Docker Installation Guide

## Before Install Docker on Ubuntu of MV1000

### Check firmware date compiled before installing Docker

If your MV1000 is runing **openwrt system** and it's already installed **ubuntu**.Please use the following instructions to check the firmware compilation date:

```
root@GL-MV1000:/# cat /data/etc/version.date 
2020-1-9 10:25:18
```

If your MV1000 is runing **ubuntu system**.Please use the following instructions to check the firmware compilation date:

```
root@GL-MV1000-Ubuntu:~# cat /etc/version.date 
2020-1-9 10:25:18
```

### Upgrade ubuntu firmware

**Caution:**

   	**If the firmware date is 2020-1-9 10:25:18 and later, skip the firmware upgrade.Otherwise, follow the steps below to upgrade the ubuntu system**.

If your MV1000 is runing ubuntu system.Please run **'switch_system openwrt'** to switch to openwrt system. 

1.switch to openwrt system

	switch_system openwrt

2.download ubuntu firmware

```
curl -SL http://download.gl-inet.com/firmware/mv1000/ubuntu/testing/ubuntu-18.04.3-20200109.tar.gz -o /tmp/ubuntu-18.04.3-20200109.tar.gz

```

3.upgrade 

```
ubuntu_upgrade -n /tmp/ubuntu-18.04.3-20200109.tar.gz
```

4.switch to ubuntu system

```
switch_system ubuntu
```



## Install docker

### Install the curl tool as shown below

```
sudo apt-get update
sudo apt-get install curl
```

### Download docker script and execute

```
curl -fsSL https://get.docker.com -o get-docker.sh
bash get-docker.sh
```

### Check if the installation was successful

After executing the docker installation script, the following information will be output

```
root@GL-MV1000-Ubuntu:~# bash get-docker.sh 

# Executing docker install script, commit: f45d7c11389849ff46a6b4d94e0dd1ffebca32c1

- sh -c 'apt-get update -qq >/dev/null'
- sh -c 'DEBIAN_FRONTEND=noninteractive apt-get install -y -qq apt-transport-https ca-certificates curl gnupg >/dev/null'
- sh -c 'curl -fsSL "https://download.docker.com/linux/ubuntu/gpg" | apt-key add -qq - >/dev/null'
  Warning: apt-key output should not be parsed (stdout is not a terminal)
  curl: (35) OpenSSL SSL_connect: SSL_ERROR_SYSCALL in connection to download.docker.com:443 
  gpg: no valid OpenPGP data found.
  root@GL-MV1000-Ubuntu:~# 
  root@GL-MV1000-Ubuntu:~# bash get-docker.sh 

# Executing docker install script, commit: f45d7c11389849ff46a6b4d94e0dd1ffebca32c1

- sh -c 'apt-get update -qq >/dev/null'
- sh -c 'DEBIAN_FRONTEND=noninteractive apt-get install -y -qq apt-transport-https ca-certificates curl >/dev/null'
- sh -c 'curl -fsSL "https://download.docker.com/linux/ubuntu/gpg" | apt-key add -qq - >/dev/null'
  Warning: apt-key output should not be parsed (stdout is not a terminal)
- sh -c 'echo "deb [arch=arm64] https://download.docker.com/linux/ubuntu bionic stable" > /etc/apt/sources.list.d/docker.list'
- sh -c 'apt-get update -qq >/dev/null'
- '[' -n '' ']'
- sh -c 'apt-get install -y -qq --no-install-recommends docker-ce >/dev/null'
  debconf: unable to initialize frontend: Dialog
  debconf: (Dialog frontend requires a screen at least 13 lines tall and 31 columns wide.)
  debconf: falling back to frontend: Readline
  debconf: unable to initialize frontend: Readline
  debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/aarch64-linux-gnu/perl/5.26.1 /usr/local/share/perl/5.26.1 /usr/lib/aarch64-linux-gnu/perl5/5.26 /usr/share/perl5 /usr/lib/aarch64-linux-gnu/perl/5.26 /usr/share/perl/5.26 /usr/local/lib/site_perl /usr/lib/aarch64-linux-gnu/perl-base) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7, <> line 3.)
  debconf: falling back to frontend: Teletype
- sh -c 'docker version'
  Client: Docker Engine - Community
   Version:           19.03.5
   API version:       1.40
   Go version:        go1.12.12
   Git commit:        633a0ea
   Built:             Wed Nov 13 07:27:46 2019
   OS/Arch:           linux/arm64
   Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.5
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.12
  Git commit:       633a0ea
  Built:            Wed Nov 13 07:26:16 2019
  OS/Arch:          linux/arm64
  Experimental:     false
 containerd:
  Version:          1.2.10
  GitCommit:        b34a5c8af56e510852c35414db4c1f4fa6172339
 runc:
  Version:          1.0.0-rc8+dev
  GitCommit:        3e425f80a8c931f88e6d94a8c831b9d5aa481657
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
If you would like to use Docker as a non-root user, you should now consider
adding your user to the "docker" group with something like:

  sudo usermod -aG docker your-user

Remember that you will have to log out and back in for this to take effect!

WARNING: Adding a user to the "docker" group will grant the ability to run
         containers which can be used to obtain root privileges on the
         docker host.
         Refer to https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface
         for more information.
```

