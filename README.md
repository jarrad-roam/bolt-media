###Run Media Server
`$ docker-compose up`

###Mount Remote Storage
####OSX
`$ mount -t smbfs 'smb://<USERNAME>:<PASSWORD>@readyshare/LaCie' 'media-storage'`

####Linux
Requires: nfs-common, cifs-utils<br>
`$ sudo mount -t cifs //10.0.0.1/LaCie ./media-storage -o username=<USERNAME>,password=<PASSWORD>,domain=WORKSPACE`

###How to setup on Udoo Bolt
Note: This assumes ssh server, git, docker, docker-compose are already setup on the host environment

1. Pull down repo `git clone https://github.com/jarrad-roam/bolt-media.git`
1. Mount remote storage to `./media-storage`
1. Run `docker-compose up -d`
1. Navigate to `bolt.home:8096` in a browser, and configure Jellyfin
1. Navigate to `bolt.home:9000` in a browser, and configure Portainer