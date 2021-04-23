https://www.lisenet.com/2016/samba-server-on-rhel-7/   Setting up a Samba Server with SELinux on RHEL 7


```
# yum install -y samba samba-client
# systemctl enable smb nmb
# firewall-cmd --permanent --add-service=samba
# firewall-cmd --reload

mkdir /srv/samba_pub
chmod 0777 /srv/samba_pub
useradd -s /sbin/nologin dev2

```
