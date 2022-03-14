# Docker Setup

If you have any trouble following the guide. Please send us a message in [Discord](https://discord.gg/longvinter) we are more than happy to help you out!

## Requirements and prerequisites
- [GIT](https://gitforwindows.org/) installed in your system;
- [GIT LFS](https://git-lfs.github.com/) installed in your system;
- [Docker and Docker-Compose](https://www.docker.com/products/docker-desktop) installed in your system;
- Broadband internet connection;
- Router with the ability to port forward;
- Min. 2 GB RAM;
- Min. 64-bit Windows 10 Operating System.

## System Setup

### Post-Installing Docker

After installation, restart your computer if necessary. Then open the Docker Dashboard by right-clicking the docker icon on the bottom right of your screen, or open Docker using your start menu. Depending on your OS and/or updates you may be asked to install "WSL 2". Make sure you do this as well.

## Setting up the container

### Choosing a location on your computer/server
It is recommended to place the server in your documents or on your desktop for ease of access.

Open Windows Explorer where you would like to place the server and right click somewhere. Click the option "Git Bash Here".
![image](https://user-images.githubusercontent.com/80425961/155396499-de40ebb5-f38f-441a-b545-1baad176bfe3.png)

This will open a command line in the location you right-clicked. From here, execute the commands in the next chapters.

![image](https://i.imgur.com/2QhZmXI.png)

### Downloading the container
Downloading the container can be done by either using git (recommended), or by clicking the green code button on this page and using the Download ZIP option on the [Docker Image Github](https://github.com/tvandoorn/longvinter-docker-server).

To download the container configuration using git, use the command below:
```shell
git clone https://github.com/Uuvana-Studios/longvinter-docker-server.git
```

When the clone is finished, enter the directory the git command created:
```shell
cd longvinter-docker-server
```

### Creating the data directory
In order to keep game progress between container restarts a data directory needs to be created. Create this directory in the same directory as the docker-compose.yaml file.
Use the following commands to create the directory and set the appropriate rights.

```shell
mkdir data
```

### Configuring the server settings
The server settings can be changed by opening the `docker-compose.yaml` file. Use either Notepad or a more advanced text editor such as Notepad++. Do not open the configuration using WordPad or Word! Settings that may be changed are shown below:

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

!!! info "**Note**"

    Even though changing the ports is possible, it is currently not supported by the game!

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
!!! info "**Note**"

    Remember that these commands have to be run from the same directory as the `Dockerfile`, `docker-compose.yaml` and `run.sh` files.

## Updating the Longvinter server
Updating the Longvinter server is as easy as restarting the container:

```shell
docker-compose restart
```

## Running multiple Longvinter containers on one Docker server
Running multiple Longvinter containers on one Docker server is very easy. Follow the _Setting up the container_ steps again, but this time set up the server to use a different directory for the new server.

```shell
git clone https://github.com/Uuvana-Studios/longvinter-docker-server.git new-name-here
```
The command above will download the container files in a directory named `new-name-here`. Make sure to change the server ports using the _Changing the port numbers_ step.

!!! info "**Note**"

    This works in theory but, remember, that even though changing the ports is possible, it is currently not supported by the game!

## Open Ports
You also need to open ports on your home router to allow traffic go through. This process is greatly different from what type of router you have. It is recommended to look in the manual provided by the manufacturer or contact your ISP for instructions.

!!! warning "**Warning**"

    In other tutorials it is asked to open the TCP Port 7777, do not do it. Unreal Engine doesn't use TCP connections - you would be leaving a unused port open by doing so!

- For Steam open: 27015 & 27016 (TCP and UDP)
- For Unreal Engine open: 7777 (UDP Only)

In some cases, opening the ports in your home router is not enough. In that case you usually also need to allow the ports in your software firewall. Windows has a built-in firewall, but some antivirus packages also include a firewall feature. For changing the settings on your antivirus firewall, look for instructions online or contact the developer directly.

### Setting ports rules on Windows Firewall
In order to open the ports in the included Windows Firewall, follow the steps below:

1. Open the Start Menu and search for "Windows Defender Firewall with Advanced Security", or the equivalent in your language.
2. Right-click the Inbound Rules option on the left side and choose New Rule...
![inboundrules](https://user-images.githubusercontent.com/3257540/158257002-fdd91104-b661-46f8-90de-a07c0d724c91.png)

3. In the window that opens, select the Port option and click Next. ![port](https://user-images.githubusercontent.com/3257540/158257053-1d9cab7d-666a-4bef-9871-09d6fcc445db.png)

4. Next, enter the port that you wish to forward, select the correct protocol (TCP or UDP) and click Next. ![tcpudp](https://user-images.githubusercontent.com/3257540/158257096-96d8de79-80a3-483a-98ed-da830fe89a6b.png)

5. Make sure "Allow the connection" is selected and click Next. ![allowconnection](https://user-images.githubusercontent.com/3257540/158257123-674efc62-ba90-4c96-b753-5124929500b8.png)
6. Make sure all options are selected and click Next. ![profile](https://user-images.githubusercontent.com/3257540/158257155-ebba1e05-2f33-445b-bebb-b392e038edca.png)

7. Give the firewall rule a recognizable name (Such as Longvinter [port]) and click Finish. ![name](https://user-images.githubusercontent.com/3257540/158257182-f57e43d6-0905-4db5-ba67-5cf334c27711.png)


Make sure to repeat steps 2-7 for each port that needs to be openend in the firewall.

