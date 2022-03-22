# Installation native

Si vous rencontrez un quelconque souci avec le guide, envoyez un message sur [Discord](https:/discord.gg/longvinter), nous vous aiderons avec plaisir !

## Prérequis et Conditions minimum d'utilisation

- GIT installé sur votre système;
- GIT LFS installé sur votre système;
- SteamCMD installé sur votre système;
- Connexion internet haut débit;
- Routeur avec la possibilité de rediriger les ports;
- Min. 2 GB RAM;
- Min. 64-bit Linux système d'exploitation.

## Installer le serveur

### Création de l'utilisateur et groupe d'utilisateurs pour démarrer le serveur

```shell
sudo useradd -m -d /home/steam steamcmd
```

Soyez sûr d'avoir inséré un mot de passe sécurisé (si vous souhaitez en générer un : [lastpass](https://www.lastpass.com/fr/features/password-generator)
```shell
sudo passwd steamcmd
```

```shell
sudo usermod -aG sudo steamcmd
```

```shell
sudo su steamcmd
```

### Installation de GIT, GIT LFS et autres pré-requis

Nous utiliserons GIT LFS car les fichiers .pak sont volumineux.

Executez les commandes ci-dessous en fonction de votre système:

??? "**Ubuntu/Debian**"

    ```shell
    sudo apt update
    ```

    ```shell
    sudo apt install git git-lfs steamcmd
    ```

    **Note**: Si votre système est en 64bits, vous devrez installer le dépot Multiverse.

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

### Installation de Steam SDK

Pour apparaître dans les serveurs sur Steam il est nécéssaire d'installer SteamSDK, pour ce faire nous utiliserons SteamCMD: 

Positionnez-vous à la racine de votre utilisateur:
```shell
cd ~/
```

Créez votre répertoire Steam:
```shell
mkdir steamcmd-source
```

Accedez à votre nouveau répertoire créé:
```shell
cd steamcmd-source
```

Téléchargez l'archive de SteamCMD:
```shell
wget https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz
```

Extraction de celui-ci:
```shell
tar -xvzf steamcmd_linux.tar.gz
```

Et finalement installation grâce à SteamCMD des packages nécéssaires:
```shell
./steamcmd.sh +force_install_dir . +login anonymous +app_update 1007 +quit
```

Steam CMD installera automatiquement la dernière version.

### Copie de Steam SDK à la bonne place

Il faudra déplacer la localisation par défaut du SDK:

Allez dans le dossier Steam:
```shell
cd ~/.steam
```

Créez le dossier pour le SDK en 64 bits:
```shell
mkdir sdk64
```

Copiez à l'intérieur de celui-ci `steamclient.so` :
```shell
cp ~/steamcmd-source/linux64/steamclient.so ~/.steam/sdk64/
```

## Redirection de ports et Pare-feu

Si vous lancez le serveur chez vous, il est nécéssaire de rediriger et ouvrir vos ports sur votre pare-feu. Pour rediriger vos ports référez vous à vos manuels fournis par votre opérateur ou le constructeur de votre routeur/modem.

Pour ouvrir vos ports sur votre machine linux utilisez selon votre outil la procédure adéquate:

!!!  "**Attention**"

    Dans d'autres tutoriels, il est écrit d'ouvrir le port 7777 en TCP, ne le faites pas.
    Unreal Engine n'utilise pas le TCP - vous laisseriez un port inutilisé ouvert à l'extérieur de votre réseau si vous le faites !
    
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

## Installation du serveur

Dans un premier temps il faut retourner à la racine de l'utlisateur Steamcmd:
```shell
cd ~/
```

Ensuite nous allons cloner le github grâce à git:
```shell
git clone https://github.com/Uuvana-Studios/longvinter-linux-server.git
```

Il faut maintenant donner les droits d'exécution au dossier:
```shell
sudo chmod -R ugo+rwx longvinter-linux-server/
```

## Personnaliser le serveur

Les options du serveur peuvent être modifiés dans Game.ini

Créer et modifier votre fichier:
```shell
nano ~/longvinter-linux-server/Longvinter/Saved/Config/LinuxServer/Game.ini
```

Ajoutez les options suivantes:
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

### Que signifie chacun des paramètres ?

| Nom du paramètre     |Utilisé pour                                                                                                                                                                                           | Valeur par défaut                 |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| ServerName       | C'est le nom qui s'affiche dans la fenêtre de recherche de serveur. Veuillez ne pas nommer votre serveur avec le nom d'un des serveurs officiel.  Nous souhaitons que les joueurs sâchent clairement s'ils rejoignent un serveur hébergé par un autre joueur. | Unnamed Island                |
| ServerMOTD       | Message du serveur présent sur les panneaux autour de l'île.                                                                                                                                               | Welcome to Longvinter Island!                            |
| MaxPlayer        | Nombre maximum de joueurs pouvant se connecter en même temps.                                                                                                                                        | 32 |
| CommunityWebsite | Vous permet de promouvoir un site web qui peut être ouvert en jeu. Le lien est présent au même endroit que le Message du serveur.                                                          | www.longvinter.com            |
| Password         | Ajoutez un mot de passe ici. N'utilisez que des nombres et des lettres. Si le champs est vide, il n'y aura pas de mot de passe pour accéder au serveur.                    | _(empty)_                     |
| AdminSteamID     | Dans cette section vous pouvez ajouter autant d'administrateurs que vous souhaitez. **Si vous décidez d'en ajouter plusieurs** assurez-vous de séparer les identifiants steam avec un seul espace.                                                           | 76561198965966997             |
| PVP              | Ici, écrivez `true` ou `false` afin d'activer/désactiver le Joueur contre Joueur.  | true                                                                                                  | true                          |

## Lancer le serveur

### Lancer le serveur manuellement

```shell
sh /home/steam/longvinter-linux-server/LongvinterServer.sh
```

### Lancer le serveur avec Systemd (Recommandé)

```shell
sudo cp /home/steam/longvinter-linux-server/longvinter.service /etc/systemd/system/longvinter.service
```

```shell
sudo cp /home/steam/longvinter-linux-server/longvinter.socket /etc/systemd/system/longvinter.socket
```

```shell
sudo systemctl daemon-reload
```

#### Comment utiliser Systemd 

1. Comment lancer le serveur:
```shell
sudo systemctl start longvinter.service
```

2. Comment arrêter le serveur:
```shell
sudo systemctl stop longvinter.service
```

3. Comment redémarrer le serveur:
```shell
sudo systemctl restart longvinter.service
```

4. Comment mettre le serveur en démarrage auto à chaque redémarrage:
```shell
sudo systemctl enable longvinter.service
```

5. Comment vérifier le status de votre serveur:
```shell
sudo systemctl status longvinter.service
```

6. Comment voir en temps réel le journal du serveur
```shell
sudo journalctl -u longvinter -f
```

7. Comment voir tous les journaux du serveur:
```shell
sudo journalctl -u longvinter
```

8. Comment obtenir l'identifiant serveur(Requière d'avoir lancer le serveur):
```shell
./LongvinterGetId.sh
```

Si votre console montre ces lignes au départ, votre serveur est démarré correctement.
```yaml
[2022.02.22-12.51.34:514][ 13]LogOnline: Verbose: STEAM: FOnlineAsyncEventSteamServerConnectedGS ServerId: Server[0x***************]
[2022.02.22-12.51.34:782][ 21]LogOnline: Verbose: STEAM: FOnlineAsyncEventSteamServerPolicyResponseGS Secure: 1
[2022.02.22-12.51.34:849][ 23]LogOnline: Verbose: OSS: Async task 'FOnlineAsyncTaskSteamCreateServer bWasSuccessful: 1' succeeded in 2.828243 seconds
[2022.02.22-12.51.34:849][ 23]LogOnlineSession: Warning: STEAM: Server setting ,TOTPLAYING_s:0 overflows Steam SetGameTags call
[2022.02.22-12.51.34:849][ 23]LogOnlineSession: Warning: STEAM: Server setting ,ServerName_s:[EU] Uuvana 1 overflows Steam SetGameTags call
```

## Maintenance serveur

### Mise à jours du serveur

Nous avons créés un script permettant de mettre à jours automatiquement en l'exécutant et redémarrer votre serveur.
```shell
bash /home/steam/longvinter-linux-server/LongvinterUpdate.sh
```

### Sauvegarde de vos données Longvinter

Nous avons créés un script permettant de sauvegarger automatiquement votre serveur et de rédémarrer celui-ci, pour l'instant il doit être lancé manuellement et requiert des informations de votre part.

```shell
bash /home/steam/longvinter-linux-server/LongvinterBackup.sh
```
