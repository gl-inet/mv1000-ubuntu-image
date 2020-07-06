# Application Guides

⚠️ Please follow the installation steps in the next section before installing applications on your MV1000 Brume

[Installing **Home Assistant**](HomeAssistant/InstallGuide.md)

[Installing **Pi Hole**](Pi-hole/InstallGuide.md)

[Installing **AdguardHome**](AdGuardHome/InstallGuide.md)

[Installing **Docker**](Docker/InstallGuide.md)


# Installing Ubuntu

⚠️ Installing Ubuntu on your MV1000 Brume will erase the built-in eMMC third partition, i.e. `/data` in OpenWRT. Backup any important data before installing Ubuntu. Follow the WinSCP guide here for an example of how to backup files via Windows: [WinSCP](https://docs.gl-inet.com/en/3/app/ssh/#winscp) ⚠️

**1.** Connect to your MV1000 Brume via SSH following the guide here: [https://docs.gl-inet.com/en/3/app/ssh/](https://docs.gl-inet.com/en/3/app/ssh/)

**2.** The latest uBoot version is required for Ubuntu to work. Copy the following script:

```sh
cat<<'EOF' | ash
    #!/bin/sh

    # Functions
    print()
    {
        echo -e "\n\033[0;$1m$2\033[0m\n"
    }

    # Varibles
    doUpgrade=false;

    # Check Version
    clear
    print 96 'Checking uBoot version'
    [ -n "$(strings /dev/mtd0 | grep "U-Boot 2" | grep dirty)" ] && doUpgrade=true;
    [ -z "$(strings /dev/mtd0 | grep "U-Boot 2")" ] &&  doUpgrade=true;

    # Upgrade uBoot
    if [ "$doUpgrade" = true ]; then
        print 96 'Update required, downloading'
        echo
        cd /tmp
        curl -SL https://github.com/gl-inet/mv1000-ubuntu-image/raw/master/uboot-gl-mv1000-20190901-md5-183eade39f35da8f6fc76c713754af85.bin -o /tmp/uboot.bin
        if [ "$(md5sum /tmp/uboot.bin 2>/dev/null | cut -f1 -d" ")" = "183eade39f35da8f6fc76c713754af85" ]; then
            print 96 'Updating uBoot'
            print 91 'DO NOT TURN OFF YOUR DEVICE'
            mtd erase /dev/mtd0
            mtd write /tmp/uboot.bin /dev/mtd0
            mtd erase /dev/mtd1
            print 92 'uBoot update finished, rebooting...'
            reboot
        else
            print 91 'Checksum failed, aborting!'
        fi

    # Not Needed
    else
        print 92 "No need to update uBoot"
    fi
EOF
```

**3.** Paste the script into your SSH window/session. Press enter to run the script. ⚠️ Do not reboot/turnoff your device while the update is underway ⚠️

**4.** The script will either notify you that uBoot was updated or didn't need an update. Contact support if there are any errors. The script output will be similar to this:

```sh
Checking uBoot version

Update required, downloading


  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   200  100   200    0     0    274      0 --:--:-- --:--:-- --:--:--   294
100  858k  100  858k    0     0   682k      0  0:00:01  0:00:01 --:--:-- 5763k

Updating uBoot

DO NOT TURN OFF YOUR DEVICE

Unlocking /dev/mtd0 ...
Erasing /dev/mtd0 ...
Unlocking /dev/mtd0 ...

Writing from /tmp/uboot.bin to /dev/mtd0 ...
Unlocking /dev/mtd1 ...
Erasing /dev/mtd1 ...

uBoot update finished, rebooting...
```

**5.** You are now ready to install Ubuntu. Copy the following script:

```sh
cat<<'SEOF' | ash
    #!/bin/sh
    cd /tmp
    curl -SL http://download.gl-inet.com/firmware/mv1000/ubuntu/testing/ubuntu-18.04.3-20200109.tar.gz -o /tmp/ubuntu.tar.gz
    ubuntu_upgrade -n /tmp/ubuntu.tar.gz
    cat<<'EOF' > /data/etc/apt/sources.list
deb http://ports.ubuntu.com/ubuntu-ports/ bionic main multiverse restricted universe
deb http://ports.ubuntu.com/ubuntu-ports/ bionic-security main multiverse restricted universe
deb http://ports.ubuntu.com/ubuntu-ports/ bionic-updates main multiverse restricted universe
EOF
SEOF
```

**6.** Paste the script into your SSH window/session. Press enter to run the script. The script output will be similar to this:

```sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  116M  100  116M    0     0  8247k      0  0:00:14  0:00:14 --:--:-- 8798k

ubuntu system upgrade ...

Ubuntu is installed successfully.

To switch to Ubuntu: switch_system ubuntu

After you switch the system,  the device will reboot automatically.
Then you can ssh to it: ssh root@[gateway-ip] The default password is 'goodlife'.
```

**7.** Ubuntu has now been installed, but you need to switch your system to it, **it is not automatic**. Remember to change the default password for the system after you connect to it for the first time.


# Switching Systems

To switch your system from OpenWRT to Ubuntu or vice versa, run the ```switch_system``` command in your SSH window/session as bellow:

#### OpenWRT To Ubuntu

```sh
switch_system ubuntu
```

#### Ubuntu To OpenWRT

```sh
switch_system openwrt
```

After switching systems using the command, your MV1000 Brume will reboot automatically.

Reconnect via SSH. 🔒 Ubuntu has the following defaults: IP: `192.168.8.1`,  User: `root`, Password: `goodlife` 🔒

⚠️ When switching systems you will get warning similar to: **"WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!"**. This is completely normal. Accept the warning if you are using Putty, or run the following command in your system console:

```sh
ssh-keygen -R 192.168.8.1
```

That will update the stored identification for the router. Notice this must be done every time you switch system.

⚠️ Change the default system password using the following command:

```sh
passwd
```

# Troubleshooting

If Ubuntu fails to boot after the install process, or something gets corrupted in the system and crashes, you can revert your MV1000 Brume back to OpenWRT:

**1.** Unplug all cables from the MV1000 Brume.

**2.** Press and hold the reset button, it is the button next to the switch on the side of the MV1000 Brume.

**1.** Plug in power while still pressing the reset button.

**3.** The LED's will flash briefly and turn off. The LED's will then light up in a sequence (lasting about 8 seconds) and turn off again: Power -> (Power + WAN) -> (Power + WAN + VPN).

**4.** Release the reset button.

**5.** The MV1000 Brume has now been reverted back to OpenWRT.

If OpenWRT does not work properly as well, follow the debrick guide here:

[https://docs.gl-inet.com/en/3/troubleshooting/debrick/](https://docs.gl-inet.com/en/3/troubleshooting/debrick/)


# Creating a custom Ubuntu image

## Example 1. Adding a package and config

**1.** Follow the install guide above using the GL supplied Ubuntu image

**2.** Switch into Ubuntu.

**3.** Make any configuration changes you require and install any programs you might need using apt install.

**4.** As in the previous sections, switch back to OpenWRT:

```sh
switch_system openwrt
```

**5.** By default, the Ubuntu file-system is mounted on /data. Making an image is as simple as compressing /data. This step takes time, be patient:

```sh
cd /data
tar -cvzf /tmp/ubuntu.tar.gz *
```

**6.** Download the newly created ```ubuntu.tar.gz``` image using WinSCP, FTP, SFTP or any other method you choose.

## Example 2. Installing a cross-compiled kernel image, dtb, and kernel module
**Following commands should be run on your local PC terminal (WSL2, Ubuntu preferred)**

**1.** Clone this repository, extract the GL supplied Ubuntu image:

```sh
mkdir -p ~/mv1000-ubuntu/
cd ~/mv1000-ubuntu/
git clone https://github.com/gl-inet/mv1000-ubuntu-image.git ubuntu-rootfs
cd ubuntu-rootfs
mkdir rootfs
curl -SL http://download.gl-inet.com/firmware/mv1000/ubuntu/testing/ubuntu-18.04.3-20200109.tar.gz -o ubuntu-18.04.3-20200109.tar.gz
sudo tar -xvf ubuntu-18.04.3-20200109.tar.gz -C rootfs
```

**2.** Follow the kernel compilation guide here: [https://github.com/gl-inet/mv1000-ubuntu-kernel/blob/master/README.md](https://github.com/gl-inet/mv1000-ubuntu-kernel/blob/master/README.md)

**3.** ⚠️ **Clean old modules using rm -rf with caution** ⚠️

```sh
sudo rm -rf ~/mv1000-ubuntu/ubuntu-rootfs/rootfs/lib/modules/*
```

**4.** Install the kernel image and dtb:

```sh
cd ~/mv1000-ubuntu/ubuntu-kernel
sudo cp arch/arm64/boot/Image ~/mv1000-ubuntu/ubuntu-rootfs/rootfs/boot/
sudo cp arch/arm64/boot/dts/marvell/armada-gl-mv1000-ubuntu.dtb ~/mv1000-ubuntu/ubuntu-rootfs/rootfs/boot/
sudo make ARCH=arm64 modules_install INSTALL_MOD_PATH=~/mv1000-ubuntu/ubuntu-rootfs/rootfs
```

**5.** You can optionally archive the .config and generated headers for use when compiling kernel modules locally:

```sh
cd ~/mv1000-ubuntu/ubuntu-kernel
tar -cvf compile_generated.tar include/config/ include/generated/ arch/arm64/include/generated/ Module.symvers  .config
sudo cp compile_generated.tar ~/mv1000-ubuntu/ubuntu-rootfs/rootfs/usr/src
```

Compress the new image:

```sh
cd ~/mv1000-ubuntu/ubuntu-rootfs/rootfs
sudo tar -cvzf ../ubuntu.tar.gz *
cd ..
```

**6.** Switch your MV1000 Brume back to OpenWRT using the previous steps in the guide.

**7.** Upload the newly created ```ubuntu.tar.gz``` image to the ```/tmp``` folder of your MV1000 Brume using WinSCP, FTP, SFTP or any other method you choose.

**8.** Install the new image running the following command:

```sh
ubuntu_upgrade -n /tmp/ubuntu.tar.gz
```

**9.** Finally switch to the new system:

```sh
switch_system ubuntu
```


# Installing a program that requires a kernel module - Wireguard

## Part 1 - User application
**Following commands should be run on the MV1000 Brume via SSH on Ubuntu**

**1.** Install Wiregurd (partially)

```sh
apt update && apt upgrade -y
apt install -y software-properties-common
add-apt-repository ppa:wireguard/wireguard
apt update
apt install -y wireguard
apt remove -y flash-kernel
apt autoremove -y
```

⚠️ The Wireguard kernel module will fail to install on the non-standard Ubuntu kernel, the workaround is to build kernel module from source.

⚠️ The kernel included in the GL supplied Ubuntu image was cross compiled with the compiler feature "CC_HAS_ASM_GOTO" turned on. If you compile the kernel module with a cross compiler that has the "CC_HAS_ASM_GOTO" feature turned off, the resulting kernel module will be incompatible and will crash the system, keep this in mind.

⚠️ Since you need to compile the kernel module, in the case of Wireguard, it would be best to also compile the user application so that they match in version. Above is an example only.


## Part 2 - Kernel module cross compile

**Following commands should be run on your local PC terminal (WSL2, Ubuntu preferred)**

**1.** Follow the kernel compilation guide here: [https://github.com/gl-inet/mv1000-ubuntu-kernel/blob/master/README.md](https://github.com/gl-inet/mv1000-ubuntu-kernel/blob/master/README.md)

**2.** Download a copy of the Wireguard kernel module source and extract it:

```sh
curl -SL https://git.zx2c4.com/wireguard-linux-compat/snapshot/wireguard-linux-compat-1.0.20200623.tar.xz -o wireguard-linux-compat-1.0.20200623.tar.xz
tar -xvf wireguard-linux-compat-1.0.20200623.tar.xz --strip 1 -C wireguard-linux-compat
```

**3.** Get the running kernel's .config and generated headers:

```sh
curl -SL https://github.com/gl-inet/mv1000-ubuntu-image/raw/master/compile_generated-18.04.3-20200109.tar -o ~/mv1000-ubuntu/compile_generated.tar
tar -xf ~/mv1000-ubuntu/compile_generated.tar -C ~/mv1000-ubuntu/ubuntu-kernel
```

**4.** Compile :

```sh
cd ~/mv1000-ubuntu/ubuntu-kernel
export ARCH=arm64
export CROSS_COMPILE=aarch64-linux-gnu-
make oldconfig
make modules_prepare
make -C ~/mv1000-ubuntu/ubuntu-kernel M=~/mv1000-ubuntu/wireguard-linux-compat/src clean
make -C ~/mv1000-ubuntu/ubuntu-kernel M=~/mv1000-ubuntu/wireguard-linux-compat/src
make -C ~/mv1000-ubuntu/ubuntu-kernel M=~/mv1000-ubuntu/wireguard-linux-compat/src modules_install INSTALL_MOD_PATH=~/mv1000-ubuntu/module_inst_dir
```

**5.** Copy the compiled .ko kernel module from ```~/mv1000-ubuntu/module_inst_dir/lib/modules/4.4.52-armada-17.10.3-g44275f8/extra``` to the MV1000 Brume Ubuntu ```/lib/module/``` directory and run the following command on your MV1000 Brume:

```sh
depmod
```

Original reference:

[http://wiki.espressobin.net/tiki-index.php?page=Getting+Started+Tutorials](http://wiki.espressobin.net/tiki-index.php?page=Getting+Started+Tutorials)

[http://wiki.espressobin.net/tiki-index.php?page=Software+HowTo](http://wiki.espressobin.net/tiki-index.php?page=Software+HowTo)
