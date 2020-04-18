#Run Media Server
`$ docker-compose up`

#Mount Remote Storage
##OSX
`$ mount -t smbfs 'smb://<USERNAME>:<PASSWORD>@readyshare/LaCie' 'media-storage'`

##Ubuntu 19.10
Note, both options require: nfs-common, cifs-utils
###Non-persistent
`$ sudo mount -t cifs //10.0.0.1/LaCie ./media-storage -o username=<USERNAME>,password=<PASSWORD>,domain=WORKSPACE`

### Persistent
1. Create credentials file `.smb` contents:
    ```
    username=<Username>
    password=<Password>
    ```
1. Change the permissions of the file `chmod 600 .smb`
1. Edit `/etc/fstab`, modify below to suit
    ```
   //10.0.0.1/LaCie /home/bolt/bolt-media/media-storage cifs credentials=/home/bolt/bolt-media/.smb,domain=WORKSPACE 0 0
    ```
1. Remount file system `$ sudo mount -a`

#How to setup on Udoo Bolt
Note: This assumes ssh server, git, docker, docker-compose are already setup on the host environment

1. Pull down repo `git clone https://github.com/jarrad-roam/bolt-media.git`
1. Mount remote storage to `./media-storage`
1. Run `docker-compose up -d`
1. Navigate to `bolt.home:8096` in a browser, and configure Jellyfin
1. Navigate to `bolt.home:9000` in a browser, and configure Portainer