#Run Media Server
`$ docker-compose up`

#Mount Remote Storage
##OSX
###Non-persistent
`$ mount -t smbfs 'smb://<USERNAME>:<PASSWORD>@readyshare/LaCie' 'media-storage'`

##Ubuntu 19.10
Note, both options require: nfs-common, cifs-utils
###Non-persistent
`$ sudo mount -t cifs //10.0.0.1/LaCie ./media-storage -o uid=<OS USER ID>,gid=<OS GROUP ID>,username=<USERNAME>,password=<PASSWORD>,domain=WORKSPACE`

### Persistent
1. Create credentials file `.smb` contents:
    ```
    username=<Username>
    password=<Password>
    ```
1. Change the permissions of the file `chmod 600 .smb`
1. Edit `/etc/fstab`, modify below to suit
    ```
   //10.0.0.1/LaCie /home/bolt/bolt-media/media-storage cifs uid=<OS USER ID>,gid=<OS GROUP ID>,credentials=/home/bolt/bolt-media/.smb,domain=WORKSPACE 0 0
    ```
1. Remount file system `$ sudo mount -a`

#Deluge VPN Credentials
1. Create a credentials file `.vpn` contents:
   ```
   OPENVPN_USERNAME=<Username>
   OPENVPN_PASSWORD=<Password>
   ```
1. Change the permissions of the file `chmod 600 .vpn`

#Deluge Configure VPN Server List
1. `cd ./deluge-config`
1. `wget https://downloads.nordcdn.com/configs/archives/servers/ovpn.zip`
1. `unzip ovpn.zip`
1. `mv ovpn_tcp nord-tcp`
1. `rm -rf ovpn_udp`
1. `rm ovpn.zip`
1. `cd ../`
1. `echo OPENVPN_CONFIG=$(ls deluge-config/nord-tcp | grep nz | tr '\n' ',' | sed 's/.$//') >> .vpn`


#How to setup on Udoo Bolt

Note: This assumes ssh server, git, docker, docker-compose are already setup on the host environment

1. Ssh into the server `ssh bolt@bolt.home`
1. Pull down repo `git clone https://github.com/jarrad-roam/bolt-media.git`
1. Mount remote storage to `./media-storage` (either persistent, or non)
1. Run `docker-compose up -d`
1. Navigate to `bolt.home:8096` in a browser, and configure Jellyfin
1. Navigate to `bolt.home:9000` in a browser, and configure Portainer
1. Navigate to `bolt.home:8112` in a browser, and configure Deluge
1. Navigate to `bolt.home:9117` in a browser, and configure Jackett
1. Navigate to `bolt.home:8989` in a browser, and configure Sonarr
1. Navigate to `bolt.home:7878` in a browser, and configure Radarr
1. Navigate to `bolt.home:3579` in a browser, and configure Ombi