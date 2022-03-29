# Installation Docker

Si vous rencontrez un quelconque souci avec le guide, envoyez un message sur [Discord](https:/discord.gg/longvinter), nous vous aiderons avec plaisir !

## Prérequis et Conditions minimum d'utilisation

- GIT installé sur votre système;
- GIT LFS installé sur votre système;
- Docker et Docker-Compose installé sur votre système;
- Connexion internet haut débit;
- Routeur avec la possibilité de rediriger les ports;
- Min. 2 GB RAM;
- Min. 64-bit Linux système d'exploitation.

## Installer le serveur


### Installation GIT et GIT LFS

Nous utiliserons GIT LFS car les fichiers .pak sont volumineux.

Executez les commandes ci-dessous en fonction de votre système:

??? "**Ubuntu/Debian**"

    ```shell
    sudo apt update
    ```

    ```shell
    sudo apt install git git-lfs
    ```

??? "**Arch-Linux**"

    ```shell
    sudo pacman -Sy
    ```

    ```shell
    sudo pacman -S git git-lfs
    ```

??? "**Fedora**"

    ```shell
    sudo yum update
    ```

    ```shell
    sudo yum install git git-lfs
    ```

### Installation Docker

??? "**Ubuntu/Debian**"

    Pour commencer ajoutons les paquets nécéssaires à l'installation:

    ```shell
    sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release
    ```

    Pour commencer ajoutons les paquets nécéssaires à l'installation:

    ```shell
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    ```

    Pour commencer ajoutons les paquets nécéssaires à l'installation:

    ```shell
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```

    Maintenant vous pouvez installer Docker:

    ```shell
    sudo apt-get install docker-ce docker-ce-cli containerd.io
    ```

    lancez et activez le service Docker:

    ```shell
    sudo systemctl start docker && sudo systemctl enable docker
    ```

    Ajoutez votre utilisateur au groupe Docker:

    ```shell
    sudo usermod -aG docker $USER
    ```

    ```shell
    newgrp docker
    ```

??? "**Fedora**"

    Ajoutez le dépot Docker:

    ```shell
    sudo dnf -y install yum-utils device-mapper-persistent-data lvm2 dnf-plugins-core
    ```

    ```shell
    sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
    ```

    Installez Docker:

    ```shell
    sudo dnf install docker-ce docker-ce-cli containerd.io
    ```

    lancez et activez le service Docker:

    ```shell
    sudo systemctl start docker && sudo systemctl enable docker
    ```

    Ajoutez votre utilisateur au groupe Docker:

    ```shell
    sudo usermod -aG docker $USER
    ```

    ```shell
    newgrp docker
    ```

??? "**Arch Linux**"

    Installez Docker en utilisant AUR-Helper:

    ```shell
    sudo paru -S --noconfirm --needed docker
    ```

    Ajoutez votre utilisateur au groupe Docker:

    ```shell
    sudo usermod -aG docker $USER
    ```

    ```shell
    newgrp docker
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

## Configurez le conteneur

### Téléchargez le conteneur

Le téléchargement du conteneur se fait via GIT (recommandé), ou en cliquant le bouton vert CODE et download ZIP [Docker Image Github](https://github.com/tvandoorn/longvinter-docker-server).
Pour télécharger en utilisant GIT, utilisez la commande suivante
```shell
git clone https://github.com/tvandoorn/longvinter-docker-server.git
```

### Créez le répertoire

Afin de garder la progression du jeu ainsi que le conteneur pendant un redémarrage vous devrez créer un dossier data. Créez le répertoire au même endroit que se situe le fichier docker-compose.yaml .
Utilisez les commandes suivant afin de créer le dossier et appliquer les droits à celui-ci.

```shell
mkdir data
```

```shell
chown -R 1200:1200 data/
```

### Configurez les options du serveur

The server settings can be changed by opening the `docker-compose.yaml` file. Settings that may be changed are shown below:

| Nom du paramètre          | Utilisé pour                                                                                                                                                                                                                                       | Valeur par défaut                 |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| CFG_SERVER_NAME       | C'est le nom qui s'affiche dans la fenêtre de recherche de serveur. Veuillez ne pas nommer votre serveur avec le nom d'un des serveurs officiel.  Nous souhaitons que les joueurs sâchent clairement s'ils rejoignent un serveur hébergé par un autre joueur.                                                                                                                                                                                 | Unnamed Island                |
| CFG_MAX_PLAYERS       | Nombre maximum de joueurs pouvant se connecter en même temps.                                                                                                                                                                         | 32                            |
| CFG_SERVER_MOTD       | Message du serveur présent sur les panneaux autour de l'île.                                                                                                                                                                                      | Welcome to Longvinter Island! |
| CFG_PASSWORD          | Ajoutez un mot de passe ici. N'utilisez que des nombres et des lettres. Si le champs est vide, il n'y aura pas de mot de passe pour accéder au serveur.                                                                                                                                                                                     | _(empty)_                     |
| CFG_COMMUNITY_WEBSITE | Vous permet de promouvoir un site web qui peut être ouvert en jeu. Le lien est présent au même endroit que le Message du serveur.                                                                                                                                                         | www.longvinter.com            |
| CFG_ADMIN_STEAM_ID    | Dans cette section vous pouvez ajouter autant d'administrateurs que vous souhaitez. **Si vous décidez d'en ajouter plusieurs** assurez-vous de séparer les identifiants steam avec un seul espace.                                                                | _(empty)_                     |
| CFG_ENABLE_PVP        | Ici, écrivez `true` ou `false` afin d'activer/désactiver le Joueur contre Joueur.                                                                                                                                          | true                          |
| CFG_GAME_PORT         | Ce paramètre est utilisé pour changer le port du jeu si plusieurs serveur fonctionnent sur la même adresse IP publique. Si vous changez ceci, soyez sûr de changer le port également dans le fichier docker-compose.yaml .  | 7777                          |
| CFG_QUERY_PORT        | Ce paramètre est utilisé pour changer le port d'entrée si plusieurs serveur fonctionnent sur la même adresse IP publique. Si vous changez ceci, soyez sûr de changer le port également dans le fichier docker-compose.yaml . | 27016                         |

Par défaut les valeurs de `docker-compose.yaml` ressembleront à ceci:

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

### Changer les ports

Dans un premier temps lancez le serveur avec les ports `7777` and `27016`, Il faudra ensuite changer dans `docker-compose.yaml` les ports à 2 endroits différents.

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

    Même s'il est possible de changer les ports, le jeu n'accepte pas pour l'instant ceci!


## Lancez le conteneur

Lorsque votre installation et configuration sera faite, lancez le docker. Ouvrez votre console et accédez à votre dossier (en utilisant la commande `cd` ) contenant les fichiers `Dockerfile`, `docker-compose.yaml` et `run.sh` .

Lancez votre conteneur en utilisant la commande:

```shell
docker-compose up -d
```

Cette commande fera:

1. Construction de votre image Docker (si non présente)
2. création du conteneur
3. Lancement du conteneur
4. Lancement du script (`run.sh`) dans votre conteneur
   1. Cloner ou mettre à jours votre dépot longvinter-linux-server 
   2. Créer ou mettre à jours votre Game.ini grâce à `docker-compose.yaml`
5. Lancement de votre serveur Longvinter

## Arrêt de votre conteneur

Pour arrêter votre conteneur, utilisez la commande ci-dessous. Pour information votre conteneur sera supprimé mais le répertoire `data` sera gardé et réutilisé à votre prochain lancement.

```shell
docker-compose down
```

## Mettre à jours votre conteneur

Lorsqu'une mise à jours sera publiée, Mettez à jours celle-ci à l'aide de `git pull`, ou manuellement en cliquant sur CODE et en téléchargeant le ZIP. Exécutez la commande ci-dessous pour mettre à jours le conteneur ensuite.

```shell
docker-compose up -d --build
```
Cette commande est à utiliser dans le même répertoire que les fichiers `Dockerfile`, `docker-compose.yaml` et `run.sh` .

## Mettre à jours votre serveur Longvinter

Mettre à jours votre serveur Longvinter est aussi facile que de redémarrer votre conteneur, faites ceci:

```shell
docker-compose restart
```

## Lancement de plusieurs serveurs Longvinter sur le même serveur de conteneurs

Lancer plusieurs serveurs Longvinter sur le même serveur de conteneur est assez simple. Suivez les instructions de _Configurez le conteneur _  étape par étape, mais veillez cette fois ci à utiliser un autre répertoire que `data`.

```shell
git clone https://github.com/tvandoorn/longvinter-docker-server.git new-name-here
```
Cette précédente commande créera un conteneur avec le nom `new-name-here`. Soyez sûrs d'avoir changer les ports en vous référant à _Changer les portss_ .

!!! info "**Note**"

    Dans la théorie cela fonctionne, rappelez vous, il n'est pour l'instant pas possible de changer les ports, le jeu ne le support pas encore!
