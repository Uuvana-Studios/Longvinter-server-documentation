# Docker Setup 

If you have any trouble following the guide. Please send us a message in [Discord](https://discord.gg/longvinter) we are more than happy to help you out!

## Requirements and Pre-requisites

- GIT installed in your system
- GIT LFS installed in your system
- Docker installed in your system
- Docker Compose installed in your system
- Broadband internet connection
- Router with the ability to port forward
- Min. 2 GB RAM

## System Setup

### Installing GIT and GIT Large file system

.pak files are large and we need to install Git Lfs in order to download them

Run the following commands according to your chosen system:

**Ubuntu/Debian**:

```shell
sudo apt update
```

```shell
sudo apt install git git-lfs
```

**Arch-Linux**:

```shell
sudo pacman -Sy
```

```shell
sudo pacman -S git git-lfs
```

**Fedora**:

```shell
sudo yum update
```

```shell
sudo yum install git git-lfs
```

### Installing Docker

**Ubuntu/Debian**:

Begin by adding dependencies needed by the installation process:

```shell
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release
```

Begin by adding dependencies needed by the installation process:

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Begin by adding dependencies needed by the installation process:

```shell
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Now you can install Docker:

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

Start and enable docker service:

```shell
$ systemctl start docker && sudo systemctl enable docker
```

Add your user account to the docker group:

```shell
sudo usermod -aG docker $USER
```

```shell
newgrp docker
```

**Fedora**:

Add Docker’s package repository:

```shell
$ dnf -y install yum-utils device-mapper-persistent-data lvm2 dnf-plugins-core
```

```shell
$ dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
```

Install Docker:

```shell
$ dnf install docker-ce docker-ce-cli containerd.io
```

Start and enable docker service:

```shell
$ systemctl start docker && sudo systemctl enable docker
```

Add your user account to the docker group:

```shell
sudo usermod -aG docker $USER
```

```shell
newgrp docker
```

**Arch Linux**

Install Docker using an AUR-Helper:

```shell
$ paru -S --noconfirm --needed docker
```

Add your user account to the docker group:

```shell
$ usermod -aG docker $USER
```

```shell
newgrp docker
```

## Setting up the container

### Downloading the container

Downloading the container can be done by either using git (recommended), or by clicking the green Code button on this page and using the Download ZIP option on the [Docker Image Github](https://github.com/tvandoorn/longvinter-docker-server).
To download the container configuration using git, use the command below
`git clone https://github.com/tvandoorn/longvinter-docker-server.git`

### Creating the data directory

In order to keep game progress between container restarts a data directory needs to be created. Create this directory in the same directory as the docker-compose.yaml file.
Use the following commands to create the directory and set the appropriate rights.

```shell
mkdir data
```

```shell
chown -R 1200:1200 data/
```

### Configuring the server settings

The server settings can be changed by opening the `docker-compose.yaml` file. Settings that may be changed are shown below:

| Setting name          | Used for                                                                                                                                                                                                                                       | Default value                 |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| CFG_SERVER_NAME       | Setting the server name that is displayed in the server list.                                                                                                                                                                                  | Unnamed Island                |
| CFG_MAX_PLAYERS       | The maximum amount of players the server will allow at the same time.                                                                                                                                                                          | 32                            |
| CFG_SERVER_MOTD       | A Message Of The Day that will be displayed to the player.                                                                                                                                                                                     | Welcome to Longvinter Island! |
| CFG_PASSWORD          | Use this setting to require a password to join the server.                                                                                                                                                                                     | _(empty)_                     |
| CFG_COMMUNITY_WEBSITE | When the server or community has a website, enter it here to display it to the player.                                                                                                                                                         | www.longvinter.com            |
| CFG_ADMIN_STEAM_ID    | Add the SteamID64 values for the players that have admin rights to this setting. When there are multiple admins, add the SteamID64 values to this setting separated by a space.                                                                | _(empty)_                     |
| CFG_ENABLE_PVP        | When this setting is set to "true", PvP will be enabled on the server. Set to "false" to disable PvP.                                                                                                                                          | true                          |
| CFG_GAME_PORT         | This setting is used to change the game port when multiple servers are running on the same (public) IP address. When changing this setting, make sure to also change the port number under the ports section of the docker-compose.yaml file.  | 7777                          |
| CFG_QUERY_PORT        | This setting is used to change the query port when multiple servers are running on the same (public) IP address. When changing this setting, make sure to also change the port number under the ports section of the docker-compose.yaml file. | 27016                         |

With the default values above, the environment part of the `docker-compose.yaml` file should look like this:

```yaml
  environment:
    CFG_SERVER_NAME: "Unnamed Island"
    CFG_MAX_PLAYERS: "32"
    CFG_SERVER_MOTD: "Welcome to Longvinter Island!"
    CFG_PASSWORD: ""
    CFG_COMMUNITY_WEBSITE: "www.longvinter.com"
    CFG_ADMIN_STEAM_ID: ""
    CFG_ENABLE_PVP: "true"
    CFG_GAME_PORT: "7777"
    CFG_QUERY_PORT: "27016"
```

### Changing the port numbers

In order to run the server with different port numbers than the default ports `7777` and `27016`, the new port numbers have to be edited in two places in the `docker-compose.yaml` file.

```yaml
  ports:
    - "7777:7777"
    - "27016:27016"
```

```yaml
  environment:
    CFG_GAME_PORT: "7777"
    CFG_QUERY_PORT: "27016"
```

**NOTE: Even though changing the ports is possible, it is currently not supported by the game!**

## Starting the container

When the setup and configuration is done, the container is ready to be started. Open the command line and navigate to the directory (using the `cd` command) that contains the `Dockerfile`, `docker-compose.yaml` and `run.sh` files.

Start the container using the following command:

```shell
docker-compose up -d
```

This command will do the following:

1. Build the container image (if not present)
2. Create the container
3. Start the container
4. Run the included startup script (`run.sh`) inside the container
   1. Clone or update the longvinter-linux-server repository
   2. Create or update the Game.ini file with the settings provided in the `docker-compose.yaml` file
5. Start the Longvinter server

## Stopping the container

To stop the container, run the command below. Note that this removes the container from Docker, but the save data will be saved in the `data` directory and will be loaded when the server is started again next time.

```shell
docker-compose down
```

## Updating the container

When a new version of the container is released, make sure to update the files using `git pull`, or manually update the files by downloading the code as ZIP from Github. Run the command below to build the new container image and restart the container.

```shell
docker-compose up -d --build
```
Note that these commands have to be run from the same directory as the `Dockerfile`, `docker-compose.yaml` and `run.sh` files.

## Updating the Longvinter server

Updating the Longvinter server is as easy as restarting the container:

```shell
docker-compose restart
```

## Running multiple Longvinter containers on one Docker server

Running multiple Longvinter containers on one Docker server is very easy. Follow the _Setting up the container_ steps again, but this time set up the server to use a different directory for the new server.

```shell
git clone https://github.com/tvandoorn/longvinter-docker-server.git new-name-here
```
The command above will download the container files in a directory named `new-name-here`. Make sure to change the server ports using the _Changing the port numbers_ step.

**NOTE: Even though changing the ports is possible, it is currently not supported by the game!**

## Portforwarding and firewalls

When running the container it might be necessary to do port forwarding or open ports in your firewall. For port forwarding instructions, please refer to the information/documentation provided by your ISP or router/modem manufacturer. For opening ports in your software firewall use the `Windows Firewall with Advanced Security` tool for Windows systems. For Linux based systems you can use the ufw or iptables tools. Please refer to their official documentation for instructions.