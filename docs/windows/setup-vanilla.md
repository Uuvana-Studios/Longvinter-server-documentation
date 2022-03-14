# Vanilla Setup

If you have any trouble following the guide. Please send us a message in [Discord](https://discord.gg/longvinter) we are more than happy to help you out!

## Requirements and prerequisites
- [GIT](https://gitforwindows.org/) installed in your system
- [GIT LFS](https://git-lfs.github.com/) instaled in your system
- [Steam](https://store.steampowered.com/about/) installed in your system.
- Broadband internet connection
- Router with the ability to port forward
- Min. 3 GB RAM
- Min. 64-bit Windows 10 Operating System

## Port-forwarding and Firewalls

If you are running this in your home network it will be necessary to do port forwarding or open ports in your firewall. For port forwarding instructions, please refer to the information/documentation provided by your ISP or router/modem manufacturer.

!!! warning "**Warning**"

    In other tutorials it is asked to open the TCP Port 7777, do not do it. Unreal Engine doesn't use TCP connections - you would be leaving a unused port open by doing so!

For Steam open: 27015 & 27016 (TCP and UDP)
For Unreal Engine open: 7777 (UDP Only)

## Installing the server

The easiest way to install the server is to right-click and git bash to the folder where you want to install the server files. You can also use CMD if you installed just GIT and not the GIT BASH.

![image](https://user-images.githubusercontent.com/80425961/155396499-de40ebb5-f38f-441a-b545-1baad176bfe3.png)

Enter the following command to clone the GitHub repository to your wanted folder:
NOTE: cloning creates a new folder for the files so no need to make a folder for the repository beforehand.

```
git clone https://github.com/Uuvana-Studios/longvinter-windows-server.git
```

Once the cloning is done you should see the following files inside the folder command created.

![image](https://user-images.githubusercontent.com/80425961/155396668-e4ec5308-5b1a-422f-981d-61deeaaa0ca8.png)

You now have installed the server!

## Customizing the server

You can customize some server parameters by editing the Game.ini config file

Go to server folder and navigate to Longvinter ->Saved -> Config -> WindowsServer

Here you should _create a new file_ and name it Game.ini

![image](https://user-images.githubusercontent.com/80425961/155396792-28842ee7-dbe9-4c74-b009-c023e9fb510f.png)

Open it for edit. You can use any text editor you want.
Add the following content inside this file.

```ini
[/game/blueprints/server/gi_advancedsessions.gi_advancedsessions_c]
ServerName=Unnamed Island
MaxPlayers=32
ServerMOTD=Welcome to Longvinter Island!
Password=
CommunityWebsite=www.longvinter.com

[/game/blueprints/server/gm_longvinter.gm_longvinter_c]
AdminSteamID=97615967659669198
```

### What does each setting mean?

| Setting name     | Used for                                                                                                                                                                                           | Default value                 |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| ServerName       | It's the name that shows up in the server browser. Please don't call your server with OFFICIAL name. We want players to clearly know if they are joining a server that is hosted by other players. | Unnamed Island                |
| ServerMOTD       | Server message that is on a signs around the island.                                                                                                                                               | 32                            |
| MaxPlayer        | Maximum allowed players that can connect at any given time.                                                                                                                                        | Welcome to Longvinter Island! |
| CommunityWebsite | Allows you to promote a website on a same place where the server message is shown. This link can be opened in-game.                                                                                | www.longvinter.com            |
| Password         | Add you password here. Use only number and letters. If left empty there is no password on the server.                                                                                              | _(empty)_                     |
| AdminSteamID     | Here you can add all the admins that you want to have in the server. **If you want to add multiple** separate the ID's with single space.                                                          | 76561198965966997             |
| PVP              | Here you write `true` or `false` if you want to enable/disable Player versus Player fights. 

## Running the server

For easy starting and closing, I recommend creating a shortcut for LongvinterServer.exe

Right-click the file and press create a shortcut.

![image](https://user-images.githubusercontent.com/80425961/155396928-b648729a-8d1a-441a-b0cf-63ca19b17a76.png)

Then right-click the shortcut and open the Properties panel, here you need to edit the Target field in the shortcut tab.

![image](https://user-images.githubusercontent.com/80425961/155396959-034b8948-0a33-4e1f-b305-5acd5cc9adc8.png)

Add the -log to the right side of the path. This is telling the program that we want to open a command panel for the server program. This helps us verify that the server starts correctly

After edit just Press "OK" to save the edit.

Then you can start the server with the shortcut. If the console is not opened you need to close the server using Task Manager.

![image](https://user-images.githubusercontent.com/80425961/155397007-069bbb19-8211-4559-9643-11afc7721197.png)

To verify that server started correctly look for these 3 lines in the console window.

![image](https://user-images.githubusercontent.com/80425961/155397051-28cc777b-39e3-4486-a66b-482fe5fa44ce.png)

If these lines are not present look further up the command line for any error or other warnings that are Steam related.

You can also use the Steam server tool to see if your server is visible in the LAN space.

![image](https://user-images.githubusercontent.com/80425961/155397100-aaa4e3e5-b7e1-4710-8035-4bab0775e08d.png)

![image](https://user-images.githubusercontent.com/80425961/155397111-d10300f6-228a-482b-b9f5-6b0b5dabd2bd.png)

If your server shows up on the LAN tab but not in the game's server browser. This indicates that there is either firewall problem with your windows or the ports of the router are not configured correctly.

## Updating To Latest build

I suggest you update the server every time you start it. The best rule of thumb is that if your game has updated in steam. This means you also have to update the server.

Updating is done by opening the server folder in GIT BASH or in CMD as I showed earlier. The difference is that you now have to be inside the folder and not where it is located.

First, verify that you are in the folder by typing:

```
git status 
```
 
If console prints:
```
fatal: not a git repository (or any of the parent directories): .git
```

This means you are in the wrong folder and you cannot pull the update from here.

Updating is simple. Just execute these two lines separately

```
git restore .
git pull
```


