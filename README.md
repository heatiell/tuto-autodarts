## 🎯 Autodarts : Guide de Configuration
Bienvenue dans ce repository dédié à l'installation et à la configuration du système Autodarts.

## 📌 Objectif du tuto
Ce dépôt a pour but de centraliser et de documenter différentes méthodes de mise en place du système de comptage automatique Autodarts. L'idée est de proposer un partage d'expériences sur les configurations logicielles et matérielles testées, afin d'offrir plusieurs alternatives selon les besoins de chacun.

## 🛠️ Une approche flexible
Il n'existe pas de solution unique pour faire tourner Autodarts. Ce guide explore donc plusieurs variantes de configuration :

- Différents systèmes d'exploitation (OS).

- Diverses méthodes d'affichage (mode application, mode kiosque, etc.).

- Des optimisations logicielles variées.

Ce repository n'est pas un dogme, mais un recueil de "recettes" possibles. Chaque méthode documentée ici représente une approche parmi d'autres, permettant à chacun de choisir celle qui correspond le mieux à son usage ou à ses compétences techniques.

## 🛠️ Méthode 1 : FullPageOS et écran tactile
Cette configuration est idéale si vous branchez votre Raspberry Pi directement sur un écran (compatible tactile) ou une TV. Elle permet de lancer l'interface d'Autodarts automatiquement au démarrage.

### 1. Matériel utilisé
- Un PC pour flasher et se connecter au Raspberry Pi (Windows, Mac ou Linux, peu importe).
- Un adaptateur pour lire une carte SD sur votre PC.
- Raspberry Pi 5 (8GB) et son alimentation.
- Une carte SD Samsung Evo Plus (la taille du stockage importe peu, mais privilégiez une carte avec de bonnes performances en termes de débit pour éviter les sensations de latence).
- Un écran tactile Anmite.
- Un hub USB 2.0 (peut ne pas être nécessaire en fonction du nombre de ports utilisés sur le Raspberry).
- Un câble USB vers USB-C entre l'écran et le Raspberry Pi.
- Un câble micro-HDMI vers mini-HDMI entre l'écran et le Raspberry Pi.

### 2. Flasher la carte SD
Pour notre installation, nous allons utiliser [FullPageOS](https://github.com/guysoft/FullPageOS). L'intérêt de ce système d'exploitation est qu'il est léger et ouvre nativement des fenêtres Chromium au lancement en plein écran.

Pour commencer, nous allons utiliser le [Raspberry Pi Imager](https://www.raspberrypi.com/software/). Téléchargez-le en faisant attention à utiliser le lien adapté à l'OS de votre PC.

Branchez votre carte SD sur votre PC.

Lancez Raspberry Pi Imager, vous devez voir ceci :
![First View](2-Flash/first_view.png?raw=true "First View")

Dans Appareil, nous allons choisir Raspberry Pi 5 et cliquer sur Suivant :
![Device](2-Flash/device.png?raw=true "Device")

Dans OS, nous allons ensuite descendre jusqu'à trouver "Other specific-purpose OS" et cliquer dessus :
![Os Other specific-purpose os](2-Flash/os-other-specific-purpose-os.png?raw=true "Os Other specific-purpose os")

On va ensuite encore faire défiler la nouvelle liste jusqu'à trouver "Digital signage and kiosks" et cliquer dessus :
![Os Digital signage and kiosks](2-Flash/os-digital-signage-and-kiosks.png?raw=true "Os Digital signage and kiosks")

Vous verrez apparaître FullPageOS, cliquez dessus et sélectionnez FullPageOS (Stable), puis cliquez sur Suivant :
![Os FullPageOS](2-Flash/os-fullpageos.png?raw=true "Os FullPageOS")
![Os FullPageOS Stable](2-Flash/os-fullpageos-stable.png?raw=true "Os FullPageOS Stable")

On va maintenant sélectionner la carte SD que l'on a préalablement branchée au PC et cliquer sur Suivant :

![Stockage](2-Flash/stockage.png?raw=true "Stockage")

Dans personnalisation, nous allons entrer les informations suivantes :
- Dans nom d'hôte : autodarts et cliquez sur Suivant :
![Hostname](2-Flash/hostname.png?raw=true "Hostname")
- Dans localisation, choisissez votre localisation (dans mon cas Paris).
- Dans utilisateur, j'ai choisi autodarts en nom d'utilisateur et renseigné un mot de passe (il est important de conserver ce mot de passe, nous en aurons besoin pour la suite) :
![User](2-Flash/user.png?raw=true "User")
- Si vous souhaitez utiliser le Wi-Fi plutôt qu'un câble Ethernet, vous devrez le configurer plus tard. Nous allons passer la configuration du Wi-Fi en cliquant sur Suivant :
![Wifi](2-Flash/wifi.png?raw=true "Wifi")
- Dans "Accès à Distance", il est important d'activer SSH car nous allons nous en servir pour installer Autodarts :
![SSH](2-Flash/wifi.png?raw=true "SSH")
- Nous allons maintenant écrire l'image sur la carte SD, cliquez sur Écrire :
![Write](2-Flash/write.png?raw=true "Write")

Cela peut prendre quelques minutes !
Vous devriez voir cette page à la fin :
![Finish](2-Flash/finish.png?raw=true "Finish")

### 3. Configurer FullPageOS
Après avoir flashé l'image, nous allons rebrancher la carte SD sur le PC. Nous allons ouvrir l'explorateur de fichiers, et naviguer sur la carte SD. Chez moi la carte SD est nommée bootfs.
À l'intérieur, nous allons modifier deux fichiers :
- fullpageos.txt
- wifi.nmconnection (si besoin du Wi-Fi)

Ouvrez fullpageos.txt avec un éditeur de texte :
![FullPageOS TXT](3-FullPageOSConfig/fullpageos_txt.png?raw=true "FullPageOS TXT")

Remplacez l'URL préconfigurée par https://play.autodarts.io, sauvegardez et fermez le fichier :
![FullPageOS TXT updated](3-FullPageOSConfig/fullpageos_txt_updated.png?raw=true "FullPageOS TXT updated")

En cas d'utilisation du Wi-Fi, ouvrez wifi.nmconnection avec un éditeur de texte :
![Wifi nmconnection](3-FullPageOSConfig/wifi_nmconnection.png?raw=true "Wifi nmconnection")

Commencez par supprimer tous les # comme suit :
![Wifi nmconnection uncomment](3-FullPageOSConfig/wifi_nmconnection_uncomment.png?raw=true "Wifi nmconnection uncomment")

Remplacez ensuite `ssid=set your wifi ssid here` par le nom de votre Wi-Fi (SSID) ainsi que `psk=set your password here` par le mot de passe de votre Wi-Fi, sauvegardez et fermez le fichier :
![Wifi nmconnection ssid](3-FullPageOSConfig/wifi_nmconnection_ssid.png?raw=true "Wifi nmconnection ssid")
![Wifi nmconnection password](3-FullPageOSConfig/wifi_nmconnection_password.png?raw=true "Wifi nmconnection password")

Votre carte SD est prête à être installée sur le Raspberry Pi 5 !

### 4. Installation du serveur Autodarts via SSH
Une fois la carte SD insérée dans le Raspberry Pi 5, vous pourrez brancher celui-ci avec son alimentation. Il démarrera de manière automatique, et si vous êtes déjà branché sur un écran, vous devriez voir le logo de FullPageOS puis la page d'Autodarts s'ouvrir automatiquement en plein écran.

Sur votre PC, ouvrez maintenant un terminal de commande pour lancer la connexion SSH au Raspberry Pi 5. Vous devriez pouvoir trouver un bon nombre de tutos sur comment utiliser SSH si vous n'êtes pas à l'aise pour le faire.

Dans le terminal, commencez par cette commande :

```Bash
ssh autodarts@autodarts.local
```
ou alors :
```Bash
ssh autodarts@autodarts
```
Si aucune de ces deux commandes ne fonctionne, vous devez vous connecter sur l'interface d'administration de votre box internet afin de retrouver l'IP locale du Raspberry Pi. De même que pour SSH, vous trouverez des tutos sur comment faire cela sur Internet en fonction de votre fournisseur et de votre box. Une fois l'IP identifiée, lancez en remplaçant `X.X.X.X` :
```Bash
ssh autodarts@X.X.X.X
```

À la première connexion, vous devriez avoir un message similaire :
```Bash
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Tapez `yes`, votre mot de passe vous sera demandé ensuite, entrez le mot de passe que nous avons défini précédemment au moment de configurer l'image.

```Bash
autodarts@autodarts.local's password:
```

Si vous êtes connecté correctement, vous devriez voir quelque chose de ce type :
![SSH connected](4-AutodartsInstall/ssh_connected.png?raw=true "SSH connected")

On va maintenant suivre les instructions d'installation d'Autodarts pour Linux : [https://autodarts.diy/getting-started/installation/](https://autodarts.diy/getting-started/installation/).

On commence par lancer :
```Bash
bash <(curl -sL get.autodarts.io)
```
![Autodarts command](4-AutodartsInstall/autodarts_command.png?raw=true "Autodarts command")
![Autodarts command done](4-AutodartsInstall/autodarts_command_done.png?raw=true "Autodarts command done")

Si vous prévoyez d'utiliser un hub USB, il est possible que vous ayez besoin de lancer une seconde commande :
```Bash
bash <(curl -sL get.autodarts.io/uvc)
```
![Autodarts uvc done](4-AutodartsInstall/uvc_command.png?raw=true "Autodarts uvc done")

Redémarrez le Raspberry Pi !

### 5. Installer une extension Chrome pour avoir un clavier virtuel pour l'écran tactile
Vous allez avoir besoin d'un clavier et d'une souris connectés au Raspberry Pi. Vous devrez aussi connecter votre écran tactile au Raspberry Pi pour tester le clavier virtuel.

Lorsque le Raspberry Pi a démarré, vous devriez voir au bout de quelques secondes la page de connexion d'Autodarts :
![Autodarts login page](5-ChromeExtension/autodarts_login_page.png?raw=true "Autodarts login page")

Avec votre clavier, vous allez taper `ctrl` + `t` afin d'ouvrir un nouvel onglet Chrome.
Vous allez rechercher `simple-virtual-keyboard chrome extension`.

Vous devriez trouver cette extension :
![Simple Virtual Keyboard Extension](5-ChromeExtension/simple_virtual_keyboard_extension.png?raw=true "Simple Virtual Keyboard Extension")

Installez l'extension puis redémarrez le Raspberry Pi !

Une fois de retour sur la page de connexion d'Autodarts, vous devriez voir le clavier virtuel apparaître lors du remplissage du champ `Username or email` ou `Password` :

![Virtual Keyboard Login Page](5-ChromeExtension/virtual_keyboard_login_page.png?raw=true "Virtual Keyboard Login Page")

Astuce : Il est possible de changer le format du clavier virtuel pour le passer en français dans la configuration de l'extension. Vous devrez redémarrer le Raspberry Pi pour que cela soit pris en compte.

### 6. Configurer votre jeu de fléchettes

Branchez maintenant vos caméras sur le Raspberry Pi !

Vous devrez créer un compte sur Autodarts, puis vous connecter avec celui-ci sur le Raspberry Pi.
Une fois connecté, vous devriez voir ceci :
![Logged Autodarts](6-ConfigureAutodarts/logged_autodarts.png?raw=true "Logged Autodarts")

Pour lancer la configuration, allez dans `My boards` dans le menu à gauche.
Vous devriez voir la fenêtre suivante :
![My boards Autodarts](6-ConfigureAutodarts/my_boards.png?raw=true "My boards Autodarts")

Cliquez sur `Claim` pour ajouter le système Autodarts du Raspberry Pi à votre compte Autodarts.

Une fois claim, vous pourrez accéder au board manager :
![Board manager](6-ConfigureAutodarts/board_manager.png?raw=true "Board manager")

Il est possible que vous deviez cliquer plusieurs fois avant d'arriver sur le board manager (au moins 2 avec mes tests), je n'ai pas vraiment trouvé d'explication.

Une fois sur le board manager, suivez les instructions de la documentation d'Autodarts pour configurer vos caméras et l'autodétection : [https://autodarts.diy/getting-started/first-startup/](https://autodarts.diy/getting-started/first-startup/)