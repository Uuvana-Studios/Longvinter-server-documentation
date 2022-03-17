# Installation native

Si vous rencontrez un quelconque souci avec le guide, envoyez un message sur [Discord](https:/discord.gg/longvinter), nous vous aiderons avec plaisir !

## Prérequis et Conditions minimum d'utilisation

- [GIT](https://gitforwindows.org/) installé sur votre système;
- [GIT LFS](https://git-lfs.github.com/) installé sur votre système;
- [Steam](https://store.steampowered.com/about/) installé sur votre système;
- Connexion internet haut débit;
- Routeur avec la possibilité de rediriger les ports;
- Min. 3 GB RAM;
- Min. 64-bit Windows 10 en Système d'exploitation.

## Installer le serveur

Le plus simple pour installer le serveur est de faire un clic droit, puis sélectionner git bash dans le dossier où vous souhaitez télécharger les fichiers. Vous pouvez aussi utiliser CDM si vous avez installé GIT sans GIT BASH.

![image](https://user-images.githubusercontent.com/80425961/155396499-de40ebb5-f38f-441a-b545-1baad176bfe3.png)

Entrez la commande suivante pour cloner le dépôt GitHub dans le dossier choisit :
NOTE: Le clonage créer un dossier par défaut pour les fichiers du serveur, il n'y a donc pas besoin de créer un dossier spécialement pour le serveur. 

```
git clone https://github.com/Uuvana-Studios/longvinter-windows-server.git
```

Une fois que le clonage est terminé, vous devriez voir les fichiers suivant dans le dossier créé par la commande.

![image](https://user-images.githubusercontent.com/80425961/155396668-e4ec5308-5b1a-422f-981d-61deeaaa0ca8.png)

Le serveur est maintenant installé !

## Personnaliser le serveur

Vous pouvez personnaliser certains paramètres du serveur en éditant le fichier de configuration Game.ini

Rendez-vous dans le dossier du serveur et naviguez dans Longvinter ->Saved-> Config -> WindowsServer

Maintenant vous devez *créer un nouveau fichier* et le nommer Game.ini

![image](https://user-images.githubusercontent.com/80425961/155396792-28842ee7-dbe9-4c74-b009-c023e9fb510f.png)

Ouvrez le fichier avec n'importe quel éditeur de texte pour le modifier.
Ajoutez les informations suivantes dans ce fichier.

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

### Que signifie chacun des paramètres ?

| Nom du paramètre     |Utilisé pour                                                                                                                                                                                           | Valeur par défaut                 |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| ServerName       | C'est le nom qui s'affiche dans la fenêtre de recherche de serveur. Veuillez ne pas nommer votre serveur avec le nom d'un des serveurs officiel.  Nous souhaitons que les joueurs sâchent clairement s'ils rejoignent un serveur hébergé par un autre joueur. | Unnamed Island                |
| ServerMOTD       | Message du serveur présent sur les panneaux autour de l'île.                                                                                                                                               | Welcome to Longvinter Island!                            |
| MaxPlayer        | Nombre maximum de joueurs pouvant se connecter en même temps.                                                                                                                                        | 32 |
| CommunityWebsite | Vous permet de promouvoir un site web qui peut être ouvert en jeu. Le lien est présent au même endroit que le Message du serveur.                                                          | www.longvinter.com            |
| Password         | Ajoutez un mot de passe ici. N'utilisez que des nombres et des lettres. Si le champs est vide, il n'y aura pas de mot de passe pour accéder au serveur.                    | _(empty)_                     |
| AdminSteamID     | Dans cette section vous pouvez ajouter autant d'administrateurs que vous souhaitez. **Si vous décidez d'en ajouter plusieurs** assurez-vous de séparer les identifiants steam avec un seul espace.                                                           | 76561198965966997             |
| PVP              | Ici, écrivez `true` ou `false` afin d'activer/désactiver le Joueur contre Joueur.  | true

## Lancer le serveur

Pour facilement lancer et fermer le serveur, nous recommandons de créer un raccourci pour LongvinterServer.exe

Faire un clic droit sur l'exécutable, et cliquer sur créer un raccourci.

![image](https://user-images.githubusercontent.com/80425961/155396928-b648729a-8d1a-441a-b0cf-63ca19b17a76.png)

Ensuite, faire un autre clic droit sur le raccourci créé, et ouvrir les propriétés, vous aurez beoin de modifier le champs Cible dans l'onglet raccourci.

![image](https://user-images.githubusercontent.com/80425961/155396959-034b8948-0a33-4e1f-b305-5acd5cc9adc8.png)

Ajoutez le -log tout à droite du chemin d'accès. Le programme ouvrira une invite de commandes au démarrage du serveur et permettra de vérifier son bon fonctionnement

Après avoir édité, cliquez sur OK pour valider les modifications.

Vous pouvez maintenant lancer le serveur en double cliquant sur le raccourci. Si l'invite de commandes ne s'affiche pas, vous devrez utiliser le gestionnaire de ressource pour fermer le serveur.

![image](https://user-images.githubusercontent.com/80425961/155397007-069bbb19-8211-4559-9643-11afc7721197.png)

Pour vérifier que le serveur soit correctement lancé, cherchez ces 3 lignes dans l'invite de commandes.

![image](https://user-images.githubusercontent.com/80425961/155397051-28cc777b-39e3-4486-a66b-482fe5fa44ce.png)

Si vous ne voyez pas ces lignes, remontez l'historique pour trouver une ligne d'erreur ou d'autres avertissements pouvant être liés à Steam.

Vous pouvez aussi utiliser la recherche de serveur Steam pour voir si votre serveur est visible dans votre LAN.

![image](https://user-images.githubusercontent.com/80425961/155397100-aaa4e3e5-b7e1-4710-8035-4bab0775e08d.png)

![image](https://user-images.githubusercontent.com/80425961/155397111-d10300f6-228a-482b-b9f5-6b0b5dabd2bd.png)

Si votre serveur s'affche dans l'onglet LAN, mais qu'il n'apparaît pas dans le navigateur en jeu, la cause peut être soit le pare-feu windows, ou les ports du routeur qui ne sont pas configuré correctement.

## Ouvrir les ports

Vous devez aussi ouvrir les ports sur votre routeur pour autoriser l'entré/sortie du trafic. Les instructions sont très différente d'un fournisseur à l'autre et du type de routeur. Il est recommandé de lire les manuels fournit par les constructeurs ou de contacter votre fournisseur internet pour les instructions.

!!! "**Attention**"

    Dans d'autres tutoriels, il est écrit d'ouvrir le port 7777 en TCP, ne le faites pas.
    Unreal Engine n'utilise pas le TCP - vous laisseriez un port inutilisé ouvert à l'extérieur de votre réseau si vous le faites !

- Pour ouvrir Steam : 27015 & 27016 (TCP et UDP)
- Pour ouvrir Unreal Engine : 7777 (UDP SEULEMENT)

Dans certains cas, ouvrir les ports de votre routeur n'est pas suffisant. La cause peut aussi venir d'un programme de pare-feu. Windows vient avec un pare-feu par défaut, mais certains antivirus comprennent aussi un pare-feu intégré.
Pour changer les paramètres de pare-feu de votre antivirus, cherchez les instructions sur internet ou contactez le développeur de l'antivirus.

### Paramétrer les règles de ports dans le pare-feux

Afin d'ouvrir les ports dans le pare-feu windows, suivez les étapes suivantes :

1. Ouvrez le menu démarrer et cherchez "Pare-feu Windows Defender avec fonctions avancées de sécurité".

2. Faites un clic droit sur Règles de trafic entrant et choisir Nouvelle règle...
![inboundrules](https://user-images.githubusercontent.com/3257540/158257002-fdd91104-b661-46f8-90de-a07c0d724c91.png)

3. Dans la fenêtre qui s'ouvre, choisissez Port puis cliquez sur Suivant. ![port](https://user-images.githubusercontent.com/3257540/158257053-1d9cab7d-666a-4bef-9871-09d6fcc445db.png)

4. Choisissez ensuite le port que vous souhaitez ouvrir, sélectionnez le bon protocol (TCP ou UDP) puis cliquez sur suivant. ![tcpudp](https://user-images.githubusercontent.com/3257540/158257096-96d8de79-80a3-483a-98ed-da830fe89a6b.png)

5. Assurez-vous que "Autoriser la connexion" soit sélectionné, puis cliquer sur Suivant. ![allowconnection](https://user-images.githubusercontent.com/3257540/158257123-674efc62-ba90-4c96-b753-5124929500b8.png)

6. Vérifiez que toutes les options soient sélectionnés puis cliquez sur Suivant. ![profile](https://user-images.githubusercontent.com/3257540/158257155-ebba1e05-2f33-445b-bebb-b392e038edca.png)

7. Donnez à cette règle un nom reconnaissable (Longvinter [port]) puis cliquer sur Terminer. ![name](https://user-images.githubusercontent.com/3257540/158257182-f57e43d6-0905-4db5-ba67-5cf334c27711.png)

Assurez-vous de répéter les étapes 2-7 pour chaque port qui a besoin d'être ouvert dans le pare-feu.

## Mettre à jour à la dernière version

Il est conseillé de mettre à jour le serveur avant chaque démarrage. Si votre jeu reçoit une mise à jour sur Steam, vous devez aussi mettre le serveur à jour.

La mise à jour se fait en ouvrant le dossier contenant les fichiers du serveur, puis ouvrir GIT BASH ou CMD comme vu plus tôt. La différence par rapport à avant, vous devez forcément être à l'intérieur du dossier contenant les fichiers du serveur.

Premièrement, vérifiez que vous êtes dans le bon fichier en tapant la commande :

```
git status 
```
 
Si l'invite de commande affiche

```
fatal: not a git repository (or any of the parent directories): .git
```

Cette erreur signifie que vous êtes dans le mauvais dossier, vous ne pourrez pas mettre à jour à partir de cet emplacement.

Mettre à jour est simple, il suffit d'exécuter ces deux lignes séparément

```
git restore .
git pull
```
