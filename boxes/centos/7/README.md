*NOTE: Base images are used to build other vagrant images. They are the very basic building block of other images. Normally they consist of only an operating system with only the packages needed to build the vagrant base image.*

# CentOS 7 Base Image

## Instructions to Build a Base Image
To build a base box you will need to follow the instructions below. There is no
way to code these in a repeatable fashion.

*NOTE: [Virtualbox](https://www.virtualbox.org/wiki/Downloads) is required
to build a base Vagrant image*

**BEFORE BEGINNING**  
Download a [CentOS 7 ISO](https://wiki.centos.org/Download). For this guide CentOS 7 x86-64 Minimal was used.

### Virtual Machine
1. Create new virtual machine in Virtualbox
  -   Open Settings for new virtual machine and disable USB and Audio
2. Install CentOS 7
  -   Load ISO into CD-ROM of new virtual machine
  -   Start virtual machine and follow CentOS 7 installation instructions

### Installing CentOS 7
1. Set root password as `vagrant`
2. Create vagrant user:
  -   Username: `vagrant`
  -   Password: `vagrant`
3. Install Virtualbox Guest Additions  
~~~~
rpm -Uvh http://ftp.linux.ncsu.edu/pub/epel/7/x86_64/e/epel-release-7-2.noarch.rpm
yum install -y gcc kernel-devel kernel-headers dkms make bzip2 perl
shutdown -r now
mkdir /media/VirtualBoxGuestAdditions
mount -r /dev/cdrom /media/VirtualBoxGuestAdditions
cd /media/VirtualBoxGuestAdditions
./VBoxLinuxAdditions
~~~~
4. Install required packages for Vagrant  
`yum install -y openssh-clients wget curl ntp`
5. Turn on services  
~~~~
systemctl enable sshd.service
systemctl enable ntpd.service
~~~~
6. Update sudoers file `/etc/sudoers`
  -   Disable require tty  
  `Default !requiretty`
  -   Add the following line to the end of the sudoers file  
  `vagrant ALL=(ALL) NOPASSWD: ALL`
7. Set selinux to disabled
8. Install vagrant insecure key
~~~~
mkdir /home/vagrant/.ssh
chmod 0700 /home/vagrant/.ssh
curl https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant.pub >> /home/vagrant/.ssh/authorized_keys
chmod 0600 /home/vagrant/.ssh/authorized_keys
chown -R vagrant:vagrant /home/vagrant
~~~~
9. Shutdown Virtual machine  
`shutdown -h now`
10. Package new box  
`vagrant package --output <NAME-OF-BOX>.box --base <NAME-OF-VIRTUAL-MACHINE>`