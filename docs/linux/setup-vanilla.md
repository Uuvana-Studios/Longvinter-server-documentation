# Vanilla Setup

If you have any trouble following the guide, please send us a message on [Discord](https://discord.gg/longvinter) or the [Uuvana Forums](https://forum.uuvana.com/t/longvinter-questions-and-help). We are more than happy to help you out!

## Requirements and Pre-requisites

- GIT installed in your system;
- GIT LFS installed in your system;
- SteamCMD installed in your system;
- Broadband internet connection;
- Router with the ability to port forward;
- Min. 2 GB RAM;
- Min. 64-bit Linux Operating System.

## System Setup

### Creating user and group to run the server 

```shell
sudo useradd -m -d /home/steam steamcmd
```

Make sure you choose a secure password.
```shell
sudo passwd steamcmd
```

```shell
sudo usermod -aG sudo steamcmd
```

```shell
sudo su steamcmd
```

### Installing GIT, GIT Large file system and other requisits 

.pak files are large and we need to install Git LFS in order to download them.

Run the following commands according to your chosen system:

??? "**Ubuntu/Debian**"

    ```shell
    sudo apt update
    ```

    ```shell
    sudo apt install git git-lfs steamcmd
    ```

    **Note**: If you are using a 64 bit machine you will need to add multiverse.

    ```shell
    sudo add-apt-repository multiverse
    ```

    ```shell
    sudo dpkg --add-architecture i386
    ```

    ```shell
    sudo apt update
    ```

    ```shell
    sudo apt install lib32gcc-s1 steamcmd
    ```

??? "**Arch-Linux**"

    ```shell
    sudo pacman -Sy
    ```

    ```shell
    sudo pacman -S git git-lfs
    ```

    ```shell
    git clone https://aur.archlinux.org/steamcmd.git
    ```

    ```shell
    cd steamcmd
    ```

    ```shell
    makepkg -si
    ```

    ```shell
    sudo ln -s /usr/games/steamcmd /home/steam/steamcmd
    ```

??? "**Fedora**"

    ```shell
    sudo yum update
    ```

    ```shell
    sudo yum install git git-lfs steamcmd
    ```

### Installing Steam SDK

The Steam server browser needs SteamSDK. For this, we need to install SteamCMD which needs to be done under the SteamCMD user:

Make sure we are in the steamcmd user home directory:
```shell
cd ~/
```

Create the SteamCMD directory:
```shell
mkdir steamcmd-source
```

Go into the newly created SteamCMD directory:
```shell
cd steamcmd-source
```

Now download the compressed SteamCMD installer:
```shell
wget https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz
```

After that, extract it:
```shell
tar -xvzf steamcmd_linux.tar.gz
```

Finally run SteamCMD to login, install a app update and quit upon completion with the following command:
```shell
./steamcmd.sh +force_install_dir . +login anonymous +app_update 1007 +quit
```

Steam CMD will install updates automatically and login to install 64-bit SDK.

### Copying Steam SDK to the right place

We still need to move the SDK to default location where the server can interact with it:

Go to the steam folder:
```shell
cd ~/.steam
```

Create folder for the 64-bits SDK:
```shell
mkdir sdk64
```

Copy the required `steamclient.so` file from SteamCMD:
```shell
cp ~/steamcmd-source/linux64/steamclient.so ~/.steam/sdk64/
```

## Port-forwarding and Firewalls

If you are running this in your home network it will be necessary to do port forwarding or open ports in your firewall. For port forwarding instructions, please refer to the information/documentation provided by your ISP or router/modem manufacturer.

For opening ports in your linux machine use the following depending on the firewall software you are using:

!!! warning "**Warning**"

    In other tutorials it is asked to open the TCP Port 7777, do not do it. Unreal Engine doesn't use TCP connections - you would be leaving a unused port open by doing so!

??? "**IPTables**"

    ```shell
    sudo iptables -I INPUT -p udp --dport 7777 -j ACCEPT
    ```
    ```shell
    sudo iptables -I INPUT -p udp --dport 27016 -j ACCEPT
    ```
    ```shell
    sudo iptables -I INPUT -p tcp --dport 27016 -j ACCEPT
    ```
    ```shell
    sudo iptables -I INPUT -p udp --dport 27015 -j ACCEPT
    ```
    ```shell
    sudo iptables -I INPUT -p tcp --dport 27015 -j ACCEPT
    ```

??? "**Uncomplicated Firewall (UFW)**"

    ```shell
    sudo ufw allow 7777/udp
    ```
    ```shell
    sudo ufw allow 27016
    ```
    ```shell
    sudo ufw allow 27015
    ```

## Installing the server

In this step let's first return to the steamcmd user home directory:
```shell
cd ~/
```

Then we will clone the linux repository:
```shell
git clone https://github.com/Uuvana-Studios/longvinter-linux-server.git
```

Then we want to give permission for this folder to execute commands with:
```shell
sudo chmod -R ugo+rwx longvinter-linux-server/
```

## Customizing the server

Server values can be customized with Game.ini

Create the file for edit with:
```shell
nano ~/longvinter-linux-server/Longvinter/Saved/Config/LinuxServer/Game.ini
```

Add the following content there:
```ini
[/Game/Blueprints/Server/GI_AdvancedSessions.GI_AdvancedSessions_C]
ServerName=Unnamed Island
ServerTag=Default
MaxPlayers=32
ServerMOTD=Welcome to Longvinter Island!
Password=
CommunityWebsite=www.longvinter.com

[/Game/Blueprints/Server/GM_Longvinter.GM_Longvinter_C]
AdminSteamID=76561198965966997
PVP=true
TentDecay=true
MaxTents=2
ChestRespawnTime=600
```

### What does each setting mean?

| Setting name     | Used for                                                                                                                                                                                           | Default value                 |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| ServerName       | It's the name that shows up in the server browser. Please don't call your server with OFFICIAL name. We want players to clearly know if they are joining a server that is hosted by other players. | Unnamed Island                |
| ServerTag       | It's the tag that allows for easier search of the server. Please don't use your the word OFFICIAL on it. We want players to clearly know if they are joining a server that is hosted by other players. And only place one tag. | Default                |
| ServerMOTD       | Server message that is on a signs around the island.                                                                                                                                               | 32                            |
| MaxPlayer        | Maximum allowed players that can connect at any given time.                                                                                                                                        | Welcome to Longvinter Island! |
| CommunityWebsite | Allows you to promote a website on a same place where the server message is shown. This link can be opened in-game.                                                                                | www.longvinter.com            |
| Password         | Add you password here. Use only number and letters. If left empty there is no password on the server.                                                                                              | _(empty)_                     |
| ChestRespawnTime         | How much is the maximum respawn time for loot chests.                                                                                                                        |600                    |
| AdminEosID     | Here you can add all the admins that you want to have in the server. Your EOS ID can be found in the general settings tab. **If you want to add multiple** separate the ID's with single space.                                                          | 76561198965966997             |
| PVP              | Here you write `true` or `false` if you want to enable/disable Player versus Player fights.
| TentDecay              | Here you write `true` or `false` if you want to enable/disable tent decay to make sure there isn't an abundant number of abandoned tents in the server.
| MaxTents              |  Maximum allowed of tents that players that can place in the server

## Running the server

### Start the server manually with shell script

```shell
sh /home/steam/longvinter-linux-server/LongvinterServer.sh
```

### Start the server automatically with Systemd (Recommended)

```shell
sudo cp /home/steam/longvinter-linux-server/longvinter.service /etc/systemd/system/longvinter.service
```

```shell
sudo cp /home/steam/longvinter-linux-server/longvinter.socket /etc/systemd/system/longvinter.socket
```

```shell
sudo systemctl daemon-reload
```

#### How to use Systemd Management

1. How to start the server:
```shell
sudo systemctl start longvinter.service
```

2. How to stop the server:
```shell
sudo systemctl stop longvinter.service
```

3. How to restart the server:
```shell
sudo systemctl restart longvinter.service
```

4. How to enable the server to start on boot:
```shell
sudo systemctl enable longvinter.service
```

5. How to check status the server:
```shell
sudo systemctl status longvinter.service
```

6. How to see real-time logs of the server:
```shell
sudo journalctl -u longvinter -f
```

7. How to see all the logs of the server:
```shell
sudo journalctl -u longvinter
```
8. How to get the ID of the server(Requires server initialization first):
```shell
./LongvinterGetId.sh
```

If the console shows these lines at the bottom during or after startup, your server has started up correctly.
```yaml
[2022.02.22-12.51.34:514][ 13]LogOnline: Verbose: STEAM: FOnlineAsyncEventSteamServerConnectedGS ServerId: Server[0x***************]
[2022.02.22-12.51.34:782][ 21]LogOnline: Verbose: STEAM: FOnlineAsyncEventSteamServerPolicyResponseGS Secure: 1
[2022.02.22-12.51.34:849][ 23]LogOnline: Verbose: OSS: Async task 'FOnlineAsyncTaskSteamCreateServer bWasSuccessful: 1' succeeded in 2.828243 seconds
[2022.02.22-12.51.34:849][ 23]LogOnlineSession: Warning: STEAM: Server setting ,TOTPLAYING_s:0 overflows Steam SetGameTags call
[2022.02.22-12.51.34:849][ 23]LogOnlineSession: Warning: STEAM: Server setting ,ServerName_s:[EU] Uuvana 1 overflows Steam SetGameTags call
```

## Server Maintenance

### Updating the server

We have created an automated script that you can run to automatically update and restart a server.
```shell
bash /home/steam/longvinter-linux-server/LongvinterUpdate.sh
```

### Backing up your saves

We have created an automated script that you can run to automatically backup and restart a server, for now it has to be run manually and it requires user input.

```shell
bash /home/steam/longvinter-linux-server/LongvinterBackup.sh
```
