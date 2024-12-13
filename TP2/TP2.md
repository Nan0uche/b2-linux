TP2 Commun : Stack PHP
I. Packaging de l'app Web
🌞 docker-compose.yml
Penser à ouvrir les ports du docker-compose en tcp.
docker build -t webpython .
docker compose up -d
Attendre 5 secondes le temps que tout se lance
http://VOTREIPLOCAL:8080/index.php?route=/
TADA, profitez de votre db !

TP2 admins : Web stack
I. Good practices
🌞 Limiter l'accès aux ressources
docker run --memory 1G
🌞 No root et 🌞 Gestion des droits du volume qui contient le code
Voir Dockerfile
🌞 Gestion des capabilities sur le conteneur NGINX
Voir Docker compose
🌞 Mode read-only
$ docker run --read-only
⭐ BONUS :
Voir Docker compose mais j'arrive pas à read only dans le compose sa fais tout crash

II. Reverse proxy buddy
A. Simple HTTP setup
Voir Docker Compose
nanouche@Ubuntu:/media/nanouche/SANDISK USB/Ynov/Cours/b2-linux/TP2/TP2-Commun$ cat /etc/hosts
127.0.0.1 www.supersite.com
127.0.0.1 pma.supersite.com 