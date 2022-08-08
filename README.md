# Truenas_Core_Deluge_Setup
Setup steps for Deluge on Truenas Core


Create a user in the Jail
```
pw useradd -n deluge -u [ID] -c "Deluge BitTorrent Client" -s /sbin/nologin -w no
```

Make the config dir and change the owner to the Deluge user
```
mkdir -p /home/deluge/.config/deluge
chown -R deluge:deluge /home/deluge/
```

Install pkg and download vim
```
pkg
pkg install vim
```

Edit the repository to include the one with deluge.
```
vim /etc/pkg/FreeBSD.conf
# create a /usr/local/etc/pkg/repos/FreeBSD.conf file:
#
#   mkdir -p /usr/local/etc/pkg/repos
#   echo "FreeBSD: { enabled: no }" > /usr/local/etc/pkg/repos/FreeBSD.conf
#
FreeBSD: {
 url: "pkg-http://pkg.FreeBSD.org/s{ABI}/release_1".
 mirror_type: "srv",
 signature_type: "fingerprints",
 fingerprints: "/usr/share/keys/pkg",
 enabled: yes

```

Install Deluge and enable the services.
```
pkg install deluge

sysrc "deluged_enable=YES"
sysrc "deluged_user=deluge"
sysrc "deluge_web_enable=YES"
sysrc "deluge_web_user=deluge"
```

Test everything by starting the services
```
service deluged start
service deluge_web start
```
