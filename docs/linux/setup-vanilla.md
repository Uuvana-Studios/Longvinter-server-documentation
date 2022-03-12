# Vanilla Setup

If you have any trouble following the guide. Please send us a message in [Discord](https://discord.gg/longvinter) we are more than happy to help you out!

## Requirements and Pre-requisites

- GIT installed in your system
- GIT LFS installed in your system
- SteamCMD installed in your system
- Broadband internet connection
- Router with the ability to port forward
- Min. 2 GB RAM

## System Setup

### Installing GIT, GIT Large file system and other requisits 

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

### Creating a dedicated user

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

### Installing Steam SDK

The Steam server browser needs steamsdk and for this we need to install SteamCMD, we will do this under the steamcmd user:

Make sure we are in the steamcmd user home directory:
```shell
cd ~/
```

Create the SteamCMD directory:
```shell
mkdir steamcmd
```

Go into the newly created SteamCMD directory:
```shell
cd steamcmd
```

Now download the compressed SteamCMD installer:
```shell
wget https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz
```

After that, extract it:
```shell
tar -xvzf steamcmd_linux.tar.gz
```

And finally run SteamCMD to login, install a app update and quit upon completion with the following:
```shell
./steamcmd.sh +force_install_dir . +login anonymous +app_update 1007 +quit
```

Steam CMD will install updates automatically and login to install 64-bit SDK.

### Copying Steam SDK to right place

We still need to move the sdk to default location where the server tries to:

Go to the steam folder:
```shell
cd ~/.steam
```

Create folder for the 64-bits SDK:
```shell
mkdir sdk64
```

And copy inside it the required `steamclient.so` file from SteamCMD:
```shell
cp ~/steamcmd/linux64/steamclient.so ~/.steam/sdk64/
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

After this we can open the required ports by executing the following commands:
```shell
sudo iptables -I INPUT -p udp --dport 7777 -j ACCEPT
```
```shell
sudo iptables -I INPUT -p udp --dport 27016 -j ACCEPT
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
MaxPlayers=32
ServerMOTD=Welcome to Longvinter Island!
Password=
CommunityWebsite=www.longvinter.com

[/Game/Blueprints/Server/GM_Longvinter.GM_Longvinter_C]
AdminSteamID=76561198965966997
PVP=true
```

What do any of them do?

- **ServerName:** It's the name that shows up in the server browser. Please don't call your server with OFFICIAL name. We want players to clearly know if they are joining a server that is hosted by other players.
- **ServerMOTD:** Server message that is on a signs around the island.
- **MaxPlayer:** Maximum allowed players that can connect at any given time.
- **CommunityWebsite:** Allows you to promote a website on a same place where the server message is shown. This link can be opened in-game.
- **Password:** Add you password here. Use only number and letters. If left empty there is no password on the server.
- **AdminSteamID:** Here you can add all the admins that you want to have in the server. **If you want to add multiple** separate id's with single space.
  - AdminSteamID=76561198965966997 11859676569976596
- **PVP:** Here you write `true` or `false` if you want to enable/disable Player versus Player fights.

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
sudo systemctl daemon-reload
```

#### How to use Systemd Management

##### How to start the server:
```shell
sudo systemctl start longvinter.service
```

##### How to stop the server:
```shell
sudo systemctl stop longvinter.service
```

##### How to restart the server:
```shell
sudo systemctl restart longvinter.service
```

##### How to enable the server to start on boot:
```shell
sudo systemctl enable longvinter.service
```

##### How to check status the server:
```shell
sudo systemctl status longvinter.service
```

##### How to see real-time logs of the server:
```shell
sudo journalctl -u longvinter -f
```

##### How to see all the logs of the server:
```shell
sudo journalctl -u longvinter
```

If the console shows these lines at the bottom after startup your server has started corretly.
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
