![filebrowser-logo](https://raw.githubusercontent.com/filebrowser/logo/master/banner.png)

# filebrowser inside a docker container

#### Introduction
filebrowser provides a file managing interface within a specified directory and it can be used to upload, delete, preview, rename and edit your files. It allows the creation of multiple users and each user can have its own directory. It can be used as a standalone app or as a middleware.

#### Table of Contents
- Preview
- Features
- Usage
- Docker
- docker-compose
- Nginx
- Ports description
- Supported environment variables
- Supported volumes
- Attaching multiple directories
- Building

  #### Preview
  ![Preview](https://user-images.githubusercontent.com/5447088/50716739-ebd26700-107a-11e9-9817-14230c53efd2.gif)

  #### Features
  - Confgurable via environment variables
  - Can be run using different user
  - Supports multiple architectures, tested on Ubuntu 18.04 (amd64), Rock64 🍍 (arm64) and Raspberry Pi 🍓 (arm32)

### Usage

#### Docker
    docker run -d --name filebrowser -p 80:8080 hurlenko/filebrowser
    
To run as current user and to map custom volume locations use:
Create Directory:
####
    mkdir -p /root/fileserver/data && mkdir -p /root/fileserver/config
####
    docker run -d \
    --name filebrowser \
    --user $(id -u):$(id -g) \
    -p 8080:8080 \
    -v /root/fileserver/data:/data \
    -v /root/fileserver/config:/config \
    -e TZ="Asia/Dhaka" \
    -e FB_BASEURL=/filebrowser \
    hurlenko/filebrowser

#### docker-compose
Minimal docker-compose.yml may look like this:
####
    version: "3"

    services:
      filebrowser:
        image: hurlenko/filebrowser
        user: "${UID}:${GID}"
        ports:
          - 443:8080
        volumes:
          - /DATA_DIR:/data
          - /CONFIG_DIR:/config
      environment:
          - FB_BASEURL=/filebrowser
      restart: always
 Simply run:
####
    docker-compose up

Ports description
8080 - default filebrowser port
Supported environment variables
The environment variables are prefixed by FB_ followed by the option name in caps. So to set "database" via an env variable, you should set FB_DATABASE. The list of avalable options can be found here.

Supported volumes
/data - Data directory to browse
/config - filebrowser.db location
Attaching multiple directories
If you want to attach multiple directories you need to mount them as subdirectories of the data directory inside of the container (/data by default):

docker run \
    -v /path/to/music:/data/music \
    -v /path/to/movies:/data/movies \
    -v /path/to/photos:/data/photos \
    hurlenko/filebrowser
Building
git clone https://github.com/hurlenko/filebrowser-docker
cd filebrowser-docker
docker build -t hurlenko/filebrowser .

  
