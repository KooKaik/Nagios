## TP 1

Nagios Core a été installé sur un serveur Ubuntu 20.04

Voir Screen : NAGIOSCORE.png

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

Check_Ping :

```
cd /usr/local/nagios/libexec/
```

```
./check_ping
```

---

Check Ping local avec Warning à 40ms et Critical à 30% :

```
cd /usr/local/nagios/libexec/
```

```
./check_ping -H 192.168.116.129 -w 40,10% -c 40,30%
PING OK -  Paquets perdus = 0%, RTA = 0.07 ms|rta=0.068000ms;40.000000;40.000000;0.000000 pl=0%;10;30;0
```

---

Check_Ping sur www.google.fr avec Warming à 5ms et Critical à 20ms :

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

Créer une commande check_ping :
- Se rendre dans [Lien](https://github.com/KooKaik/Nagios/blob/master/Fichiers%20de%20Congifuration/objects/commands.cfg "commands.cfg")
```
define command {

    command_name    check_ping_localhost
    command_line /usr/local/nagios/libexec/check_ping -H localhost -w 40,40% -c 60,60%
}
```

Supervisez son serveur nagios en créant un fichier "serveur_nagios.cfg" :
- Création du fichier de configuration [Lien](https://github.com/KooKaik/Nagios/blob/master/Fichiers%20de%20Congifuration/objects/serveur_nagios.cfg "serveur_nagios.cfg")

- Déclaration du fichier dans cgi.cfg [Lien](https://github.com/KooKaik/Nagios/blob/master/Fichiers%20de%20Congifuration/cgi.cfg "cgi.cfg")

Vérifier le bon fonctionnement sur [Lien](https://github.com/KooKaik/Nagios/blob/master/Capture%20Ecran/Services.png "l'interface web") :
- On peut voir qu'un service "PING" est associé à l'hôte "serveur_nagios"

