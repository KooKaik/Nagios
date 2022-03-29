# TPs Monitoring Nagios

## Sommaire

- TP 1
- TP 2
- TP 3
- TP 4

## TP 1

Nagios Core a été installé sur un serveur Ubuntu 20.04

![Voir l'interface web](https://github.com/KooKaik/Nagios/blob/master/Capture%20Ecran/NagiosCore.png)

Définir les alias de commande :
- Un alias de commande permet d'associer une commande à un nom afin d'éviter de la taper plusieurs fois
- Réduit les erreurs (fautes de frappes) et gain de temps


Répertoire Nagios :
- bin -> Application binaire
- etc -> Fichier de configuration de Nagios (SERVER, HOSTS, etc...)
- libexec -> Extension / Plugins
- sbin -> Binaire Nagios important
- share -> Fichier de l'interface Web Nagios Core
- var -> Fichier variable comme les logs

## TP 2

### Liste des commandes

check_ping :

```
cd /usr/local/nagios/libexec/
```

```
./check_ping
```

---

check_ping local avec Warning à 40ms et Critical à 30% :

```
cd /usr/local/nagios/libexec/
```

```
./check_ping -H 192.168.116.129 -w 40,10% -c 40,30%
PING OK -  Paquets perdus = 0%, RTA = 0.07 ms|rta=0.068000ms;40.000000;40.000000;0.000000 pl=0%;10;30;0
```

---

check_ping sur www.google.fr avec Warming à 5ms et Critical à 20ms :

```
cd /usr/local/nagios/libexec/
```

```
./check_ping -H www.google.fr -w 5,10% -c 20,30%
CRITICAL - Network Unreachable
```

```
./check_ping -H 8.8.8.8 -w 5,10% -c 20,30%
PING OK -  Paquets perdus = 0%, RTA = 19.44 ms|rta=19.438000ms;20.000000;20.000000;0.000000 pl=0%;10;30;0
```

---

### Supervision du serveur

Créer une commande check_ping :
- Se rendre dans [commands.cfg](https://github.com/KooKaik/Nagios/blob/master/Fichiers%20de%20Congifuration/objects/commands.cfg)
```
define command {

    command_name    check_ping_localhost
    command_line /usr/local/nagios/libexec/check_ping -H localhost -w 40,40% -c 60,60%
}
```

Supervisez son serveur nagios en créant un fichier "serveur_nagios.cfg" :
- Création du fichier de configuration [serveur_nagios.cfg](https://github.com/KooKaik/Nagios/blob/master/Fichiers%20de%20Congifuration/objects/serveur_nagios.cfg)

- Déclaration du fichier dans [cgi.cfg](https://github.com/KooKaik/Nagios/blob/master/Fichiers%20de%20Congifuration/cgi.cfg)
```
cfg_file=/usr/local/nagios/etc/objects/serveur_nagios.cfg
```

Vérifier le bon fonctionnement sur l'interface web
![interface web](https://github.com/KooKaik/Nagios/blob/master/Capture%20Ecran/Services.png) :
- On peut voir qu'un service "PING" est associé à l'hôte "serveur_nagios"

## TP 3

### Coté Client

Une VM clients linux a été mise en place pour ce TP (Ubuntu Server 20.04)

La procédure suivante a été suivi afin d'installer NRPE sur le client

[Procédure NRPE](https://support.nagios.com/kb/article/nrpe-how-to-install-nrpe-v4-from-source-515.html)

### Coté Serveur

- Création du fichier de configuration [linux_clients.cfg](https://github.com/KooKaik/Nagios/blob/master/Fichiers%20de%20Congifuration/objects/linux_clients.cfg)

- Déclaration du fichier dans [cgi.cfg](https://github.com/KooKaik/Nagios/blob/master/Fichiers%20de%20Congifuration/cgi.cfg)
```
cfg_file=/usr/local/nagios/etc/objects/linux-clients.cfg
```

Vérification sur l'interface web
![Hotes](https://github.com/KooKaik/Nagios/blob/master/Capture%20Ecran/Hosts.png)
![Services](https://github.com/KooKaik/Nagios/blob/master/Capture%20Ecran/Services.png)

On peut voir qu'un nouvel hote est apparu (linux-clients) et que plusieurs services sont associés à celui-ci

## TP 4


