###  PostgreSQL 9.6

## Introduction

## Install database PostgreSQL and configuration pg_hba.conf to connect users at Active Directory  

## Intital setup

1. Disable selinux and firewalld

## SELinux


sed -i 's/enforcing/disabled/g' /etc/selinux/config /etc/selinux/config
reboot


## Firewall


systemctl mask firewalld
systemctl stop firewalld


## Configure sudoers on host that will receive the postgresql

ansible ALL=(root)  NOPASSWD: ALL


## Configure in the template (pg_hba.conf.j2) for ansible to be able to change the password of PostgreSQL

host    all             ansible      127.0.1.1/32       md5


2.  This is sugestion before run the playbook

### Creating Volume Group **VG00**

## Partitioning Disk
fdisk /dev/sdc

## Create Volume Group
vgcreate VG01  /dev/sdc1

## Display VG00
vgdisplay -v VG00

## Create Logical Volume
lvcreate -l100%FREE -n lvol00 VG00

## Display o Logical Volume
lvdisplay -v lvol00

## Create Filesystem
mkfs.xfs  -L POSTGRESQL /dev/VG00/lvol00

## Create directory
mkdir /pgsql

## Edit  /etc/fstab and add line below
vi /etc/fstab

LABEL=POSTGRESQL   /pgsql             xfs    defaults        1 2


## Mount filesystem

mount -av


## Run playbook to install PostgreSQL 

sudo ansible-playbook site.yml -i inventory  -e " host=postgresql installpentaho=true" 

## To verify installation of PostgreSQL

sudo ansible-playbook site.yml -i inventory  -e " host=postgresql installpentaho=false" 