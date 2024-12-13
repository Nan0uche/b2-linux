# TP1 : Premiers pas Docker
## I. Init
### 3. sudo c pa bo
**🌞 Ajouter votre utilisateur au groupe docker** :
```bash
nanouche@Ubuntu:~$ sudo adduser nanouche docker
```
### 4. Un premier conteneur en vif
**🌞 Lancer un conteneur NGINX** :
```bash
nanouche@Ubuntu:~$ docker run -d -p 9999:80 nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
bc0965b23a04: Pull complete 
650ee30bbe5e: Pull complete 
8cc1569e58f5: Pull complete 
362f35df001b: Pull complete 
13e320bf29cd: Pull complete 
7b50399908e1: Pull complete 
57b64962dd94: Pull complete 
Digest: sha256:fb197595ebe76b9c0c14ab68159fd3c08bd067ec62300583543f0ebda353b5be
Status: Downloaded newer image for nginx:latest
fab40f22e81341c7a58e6a37d5171dda20a8f6b217e169c150ce3c78e3fd84c1
```
**🌞 Visitons** :
```bash
nanouche@Ubuntu:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                     NAMES
fab40f22e813   nginx     "/docker-entrypoint.…"   46 seconds ago   Up 45 seconds   0.0.0.0:9999->80/tcp, [::]:9999->80/tcp   heuristic_blackwell

nanouche@Ubuntu:~$ docker logs fab
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/12/11 14:22:16 [notice] 1#1: using the "epoll" event method
2024/12/11 14:22:16 [notice] 1#1: nginx/1.27.3
2024/12/11 14:22:16 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
2024/12/11 14:22:16 [notice] 1#1: OS: Linux 5.15.0-1065-intel-iotg
2024/12/11 14:22:16 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2024/12/11 14:22:16 [notice] 1#1: start worker processes
2024/12/11 14:22:16 [notice] 1#1: start worker process 29
2024/12/11 14:22:16 [notice] 1#1: start worker process 30
2024/12/11 14:22:16 [notice] 1#1: start worker process 31
2024/12/11 14:22:16 [notice] 1#1: start worker process 32
2024/12/11 14:22:16 [notice] 1#1: start worker process 33
2024/12/11 14:22:16 [notice] 1#1: start worker process 34
2024/12/11 14:22:16 [notice] 1#1: start worker process 35
2024/12/11 14:22:16 [notice] 1#1: start worker process 36
2024/12/11 14:22:16 [notice] 1#1: start worker process 37
2024/12/11 14:22:16 [notice] 1#1: start worker process 38
2024/12/11 14:22:16 [notice] 1#1: start worker process 39
2024/12/11 14:22:16 [notice] 1#1: start worker process 40
2024/12/11 14:22:16 [notice] 1#1: start worker process 41
2024/12/11 14:22:16 [notice] 1#1: start worker process 42
2024/12/11 14:22:16 [notice] 1#1: start worker process 43
2024/12/11 14:22:16 [notice] 1#1: start worker process 44

nanouche@Ubuntu:~$ docker inspect fab
[
    {
        "Id": "fab40f22e81341c7a58e6a37d5171dda20a8f6b217e169c150ce3c78e3fd84c1",
        "Created": "2024-12-11T14:22:15.792287767Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "State": {

nanouche@Ubuntu:~$ sudo ss -lnpt
State             Recv-Q            Send-Q                       Local Address:Port                       Peer Address:Port            Process                                                                      
LISTEN            0                 4096                               0.0.0.0:9999                            0.0.0.0:*                users:(("docker-proxy",pid=4614,fd=4))                                     
LISTEN            0                 4096                                  [::]:9999                               [::]:*                users:(("docker-proxy",pid=4620,fd=4))                

nanouche@Ubuntu:~$ sudo ufw allow 9999/tcp
La règle a été ajoutée
La règle a été ajoutée (v6)

nanouche@Ubuntu:~$ curl http://10.33.72.147:9999/
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
**🌞 On va ajouter un site Web au conteneur NGINX** :
```bash
nanouche@Ubuntu:~$ mkdir nginx
nanouche@Ubuntu:~$ cd nginx/
nanouche@Ubuntu:~/nginx$ cat index.html 
<h1>MEOOOW</h1>
nanouche@Ubuntu:~/nginx$ cat site_nul.conf 
server {
    listen        8080;

    location / {
        root /var/www/html;
    }
}

nanouche@Ubuntu:~/nginx$ docker run -d -p 9999:8080 -v /home/nanouche/nginx/index.html:/var/www/html/index.html -v /home/nanouche/nginx/site_nul.conf:/etc/nginx/conf.d/site_nul.conf nginx
18041d0a1e9a1ed3b3cab5d0f754b7ce3053c9e05cccb1140a109fee23943740
```
**🌞 Visitons** :
```bash
nanouche@Ubuntu:~/nginx$ curl http://10.33.72.147:9999/
<h1>MEOOOW</h1>
```
**🌞 Lance un conteneur Python, avec un shell** :
```bash
nanouche@Ubuntu:~/nginx$ docker run -it python bash
Unable to find image 'python:latest' locally
latest: Pulling from library/python
fdf894e782a2: Pull complete 
5bd71677db44: Pull complete 
551df7f94f9c: Pull complete 
ce82e98d553d: Pull complete 
5f0e19c475d6: Pull complete 
abab87fa45d0: Pull complete 
2ac2596c631f: Pull complete 
Digest: sha256:220d07595f288567bbf07883576f6591dad77d824dce74f0c73850e129fa1f46
Status: Downloaded newer image for python:latest
root@e727d7d07dcd:/# 
```
**🌞 Installe des libs Python** :
```bash
root@e727d7d07dcd:/# pip install aiohttp
Collecting aiohttp
  Downloading aiohttp-3.11.10-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (7.7 kB)
Collecting aiohappyeyeballs>=2.3.0 (from aiohttp)
  Downloading aiohappyeyeballs-2.4.4-py3-none-any.whl.metadata (6.1 kB)
Collecting aiosignal>=1.1.2 (from aiohttp)
  Downloading aiosignal-1.3.1-py3-none-any.whl.metadata (4.0 kB)
Collecting attrs>=17.3.0 (from aiohttp)
  Downloading attrs-24.2.0-py3-none-any.whl.metadata (11 kB)
Collecting frozenlist>=1.1.1 (from aiohttp)
  Downloading frozenlist-1.5.0-cp313-cp313-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (13 kB)
Collecting multidict<7.0,>=4.5 (from aiohttp)
  Downloading multidict-6.1.0-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (5.0 kB)
Collecting propcache>=0.2.0 (from aiohttp)
  Downloading propcache-0.2.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (9.2 kB)
Collecting yarl<2.0,>=1.17.0 (from aiohttp)
  Downloading yarl-1.18.3-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (69 kB)
Collecting idna>=2.0 (from yarl<2.0,>=1.17.0->aiohttp)
  Downloading idna-3.10-py3-none-any.whl.metadata (10 kB)
Downloading aiohttp-3.11.10-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (1.7 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.7/1.7 MB 24.2 MB/s eta 0:00:00
Downloading aiohappyeyeballs-2.4.4-py3-none-any.whl (14 kB)
Downloading aiosignal-1.3.1-py3-none-any.whl (7.6 kB)
Downloading attrs-24.2.0-py3-none-any.whl (63 kB)
Downloading frozenlist-1.5.0-cp313-cp313-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl (267 kB)
Downloading multidict-6.1.0-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (131 kB)
Downloading propcache-0.2.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (227 kB)
Downloading yarl-1.18.3-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (339 kB)
Downloading idna-3.10-py3-none-any.whl (70 kB)
Installing collected packages: propcache, multidict, idna, frozenlist, attrs, aiohappyeyeballs, yarl, aiosignal, aiohttp
Successfully installed aiohappyeyeballs-2.4.4 aiohttp-3.11.10 aiosignal-1.3.1 attrs-24.2.0 frozenlist-1.5.0 idna-3.10 multidict-6.1.0 propcache-0.2.1 yarl-1.18.3
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager, possibly rendering your system unusable.It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv. Use the --root-user-action option if you know what you are doing and want to suppress this warning.

root@e727d7d07dcd:/# pip install aioconsole
Collecting aioconsole
  Downloading aioconsole-0.8.1-py3-none-any.whl.metadata (46 kB)
Downloading aioconsole-0.8.1-py3-none-any.whl (43 kB)
Installing collected packages: aioconsole
Successfully installed aioconsole-0.8.1
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager, possibly rendering your system unusable.It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv. Use the --root-user-action option if you know what you are doing and want to suppress this warning.

root@e727d7d07dcd:/# python
Python 3.13.1 (main, Dec  4 2024, 20:40:27) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import aiohttp
>>> 
```
## II. Images
### 1. Images publiques
**🌞 Récupérez des images** :
```bash
nanouche@Ubuntu:~$ docker images
REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
linuxserver/wikijs   latest    863e49d2e56c   5 days ago      465MB
python               latest    3ca4060004b1   7 days ago      1.02GB
nginx                latest    66f8bdd3810c   2 weeks ago     192MB
wordpress            latest    c89b40a25cd1   2 weeks ago     700MB
mysql                latest    56a8c14e1404   8 weeks ago     603MB
hello-world          latest    d2c94e258dcb   19 months ago   13.3kB
```
**🌞 Lancez un conteneur à partir de l'image Python** :
```bash
nanouche@Ubuntu:~$ docker run -it python bash
root@d1dd7c9b3c8d:/# python3
Python 3.13.1 (main, Dec  4 2024, 20:40:27) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```
### 2. Construire une image
**🌞 Ecrire un Dockerfile pour une image qui héberge une application Python** :
```bash
nanouche@Ubuntu:~/python_app_build$ cat Dockerfile 
FROM debian

RUN apt update -y && apt install -y python3
RUN apt install python3-emoji
RUN mkdir /app

WORKDIR /app

COPY app.py /app/app.py

ENTRYPOINT ["python3", "/app/app.py"]
```
**🌞 Build l'image** :
```bash
nanouche@Ubuntu:~/python_app_build$ docker build . -t python_app:version_de_ouf
[+] Building 6.8s (11/11) FINISHED                                                                                                                                             docker:default
 => [internal] load build definition from Dockerfile                                                                                                                                     0.0s
 => => transferring dockerfile: 231B                                                                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/debian:latest                                                                                                                         0.7s
 => [internal] load .dockerignore                                                                                                                                                        0.0s
 => => transferring context: 2B                                                                                                                                                          0.0s
 => [1/6] FROM docker.io/library/debian:latest@sha256:17122fe3d66916e55c0cbd5bbf54bb3f87b3582f4d86a755a0fd3498d360f91b                                                                   0.0s
 => [internal] load build context                                                                                                                                                        0.0s
 => => transferring context: 27B                                                                                                                                                         0.0s
 => CACHED [2/6] RUN apt update -y && apt install -y python3 python3-pip                                                                                                                 0.0s
 => [3/6] RUN apt install python3-emoji                                                                                                                                                  2.5s
 => [4/6] RUN mkdir /app                                                                                                                                                                 0.4s 
 => [5/6] WORKDIR /app                                                                                                                                                                   0.1s 
 => [6/6] COPY app.py /app/app.py                                                                                                                                                        0.1s 
 => exporting to image                                                                                                                                                                   2.8s 
 => => exporting layers                                                                                                                                                                  2.8s 
 => => writing image sha256:953a357e6934af1b1bc6759913225f2899e294263e32b81c060e664ad400d354                                                                                             0.0s 
 => => naming to docker.io/library/python_app:version_de_ouf 
```
**🌞 Lancer l'image** :
```bash
nanouche@Ubuntu:~$ docker run python_app:version_de_ouf
Cet exemple d'application est vraiment naze 👎
```
## III. Docker compose
**🌞 Créez un fichier docker-compose.yml** :
```bash
nanouche@Ubuntu:~/compose_test$ cat docker-compose.yml 
version: "3"

services:
  conteneur_nul:
    image: debian
    entrypoint: sleep 9999
  conteneur_flopesque:
    image: debian
    entrypoint: sleep 9999
```
**🌞 Lancez les deux conteneurs avec docker compose** :
```bash
nanouche@Ubuntu:~/compose_test$ docker compose up -d
WARN[0000] /home/nanouche/compose_test/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
[+] Running 3/3
 ✔ conteneur_flopesque Pulled                                                                                                                                                            2.4s 
 ✔ conteneur_nul Pulled                                                                                                                                                                  2.4s 
   ✔ fdf894e782a2 Already exists                                                                                                                                                         0.0s 
[+] Running 3/3
 ✔ Network compose_test_default                  Created                                                                                                                                 0.2s 
 ✔ Container compose_test-conteneur_nul-1        Started                                                                                                                                 0.5s 
 ✔ Container compose_test-conteneur_flopesque-1  Started   
```
**🌞 Vérifier que les deux conteneurs tournent** :
```bash
nanouche@Ubuntu:~/compose_test$ docker compose ps
WARN[0000] /home/nanouche/compose_test/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
NAME                                 IMAGE     COMMAND        SERVICE               CREATED          STATUS          PORTS
compose_test-conteneur_flopesque-1   debian    "sleep 9999"   conteneur_flopesque   30 seconds ago   Up 30 seconds   
compose_test-conteneur_nul-1         debian    "sleep 9999"   conteneur_nul         30 seconds ago   Up 30 seconds 
```
**🌞 Pop un shell dans le conteneur conteneur_nul** :
```bash
nanouche@Ubuntu:~/compose_test$ docker exec -it e4bb bash
root@e4bbdf22d0d9:/# apt-get update -y
apt-get install -y iputils-ping
root@e4bbdf22d0d9:/# ping conteneur_flopesque
PING conteneur_flopesque (172.18.0.2) 56(84) bytes of data.
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=1 ttl=64 time=0.131 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=2 ttl=64 time=0.100 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=3 ttl=64 time=0.078 ms
^C
--- conteneur_flopesque ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2030ms
rtt min/avg/max/mdev = 0.078/0.103/0.131/0.021 ms
```