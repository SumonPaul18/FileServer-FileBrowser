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
  Confgurable via environment variables
Can be run using different user
Supports multiple architectures, tested on Ubuntu 18.04 (amd64), Rock64 🍍 (arm64) and Raspberry Pi 🍓 (arm32)

### Usage

#### Docker
    docker run -d --name filebrowser -p 80:8080 hurlenko/filebrowser

To run as current user and to map custom volume locations use:
####
    docker run -d \
    --name filebrowser \
    --user $(id -u):$(id -g) \
    -p 8080:8080 \
    -v /DATA_DIR:/data \
    -v /CONFIG_DIR:/config \
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
 

  
