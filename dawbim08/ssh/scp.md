## SCP / Comandes / SSHFS

# Scp

L'ús bàsic de scp és el següent:
```sh
scp fitxer host:path
```

Això copia el fitxer a l'amfitrió remot. El camí de destinació és opcional, però pot ser un directori al servidor o fins i tot un nom de fitxer si estàs copiant un sol fitxer. És possible especificar múltiples fitxers; l'últim és la destinació.
Per copiar un fitxer des de l'amfitrió remot, utilitza:
```sh
scp host:fitxer path
```

Això recupera el fitxer de l'amfitrió i el col·loca al directori indicat per path. Sovint, path és només ., que indica el directori de treball actual.

Per copiar arbres de directoris sencers en lloc de fitxers individuals, afegeix l'opció -r. Per exemple:
```sh
scp -r host:path/directory .
```


Això recuperarà path/directory de l'amfitrió, copiant-lo al directori de treball actual (creant el directori dins del directori de treball actual).

# Comandes via ssh
La sintaxi per executar comandes sobre SSH és la següent:
```sh
ssh user1@server1 command1 
ssh user1@server1 'command2'
```

Per fer ús de pipes:
```sh
ssh user1@server1 'command1 | command2'
```

Per executar múltiples comandes (cal posar-les entre cometes):
```sh
ssh admin@box1 "command1; command2; command3"
```

El client SSH iniciarà sessió en un servidor anomenat server1, utilitzant el nom d'usuari user1, i executarà una comanda anomenada command1.

# SSHFS

Transferir fitxers mitjançant una connexió SSH, utilitzant SFTP o SCP, és un mètode popular per moure petites quantitats de dades entre servidors. Tanmateix, en alguns casos pot ser necessari compartir directoris sencers o sistemes de fitxers complets entre dos entorns remots. Tot i que això es pot aconseguir configurant un muntatge SMB o NFS, ambdós requereixen dependències addicionals i poden introduir problemes de seguretat o un excés de càrrega.

Com a alternativa, pots instal·lar SSHFS per muntar un directori remot utilitzant només SSH. Això té l'avantatge important de no requerir configuracions addicionals i d'heretar els permisos de l'usuari SSH al sistema remot. SSHFS és especialment útil quan necessites accedir interactivament a un conjunt gran de fitxers de manera individual.
Per fer-ho al client instal·lem el sshfs
```sh
sudo apt update
sudo apt install sshfs
```

Una vegada instal·lat el procediment d’ús és el següent 
```sh
sudo sshfs -o allow_other,default_permissions user@host-server:”directory” “client_directory_path”
```

Les opcions d'aquesta comanda funcionen de la següent manera:
**-o:** precedeix diverses opcions de muntatge (això és el mateix que quan s'executa la comanda mount normalment per a muntatges de disc no-SSH). En aquest cas, s'utilitza **allow_other** per permetre que altres usuaris tinguin accés a aquest muntatge (de manera que es comporti com un muntatge de disc normal, ja que sshfs ho impedeix per defecte) i **default_permissions** per utilitzar els permisos de sistema de fitxers normals.
user@host-server:”directory_path”: proporciona el camí complet al directori remot, incloent el nom d'usuari remot, user, el servidor remot, host-server, i el camí, en aquest cas “directory_path” per al directori d'inici de l'usuari remot. Això utilitza la mateixa sintaxi que SSH o SCP.
“client_directory_path”: és el camí al directori local que s'està utilitzant com a punt de muntatge.
Per desmuntar el directori remot només cal executar: 
```sh
sudo umount /mnt/droplet
```

Ara bé, a vegades pot ser convenient que es monti el directory remot de forma permanent. Per fer-ho hem d’utilitzar: 

Editarem el fitxer 
```sh
sudo nano /etc/fstab
```

El fitxer FSTAB o File System Table en Linux s'encarrega d'emmagatzemar informació descriptiva sobre els diferents sistemes de fitxers de l'equip.

Les particions utilitzades al sistema operatiu GNU/Linux per afegir-se al directori arrel i muntar l'arrencada es defineixen al fitxer /etc/fstab, on es guarda la informació sobre el muntatge d'aquestes particions.

Pel que fa al seu manteniment, aquest és responsabilitat de l'administrador del sistema, qui normalment l'edita mitjançant un editor de text o altres aplicacions gràfiques.

I afegirem dins del /etc/fstab 
```sh
sammy@your_other_server:~/ /mnt/droplet fuse.sshfs noauto,x-systemd.automount,_netdev,reconnect,identityfile=/home/sammy/.ssh/id_rsa,allow_other,default_permissions 0 0
```

Les muntatges permanents sovint necessiten diverses opcions com aquestes per assegurar-se que es comportin correctament. Les opcions funcionen de la següent manera:
**sammy@your_other_server:~/:** representa el camí remot, igual que abans.
**/mnt/droplet:** representa el camí local, igual que anteriorment.
**fuse.sshfs:** especifica el controlador que s'utilitza per muntar aquest directori remot.
**noauto,x-systemd.automount,_netdev,reconnect:** és un conjunt d’opcions que garanteix que els muntatges permanents cap a unitats de xarxa es comportin correctament si es perd la connexió de xarxa amb la màquina local o la remota.
**identityfile=/home/sammy/.ssh/id_rsa:** especifica un camí cap a una clau SSH local perquè el directori remot es pugui muntar automàticament. Aquest exemple suposa que tant l’usuari local com el remot són sammy; aquí es fa referència al camí local. És necessari especificar-ho perquè /etc/fstab s’executa com a root i, en cas contrari, no sabria quina configuració d’usuari SSH ha de revisar per trobar una clau confiada pel servidor remot.
**allow_other,default_permissions:** utilitza els mateixos permisos de la comanda de muntatge anterior.
**0 0:** indica que el sistema de fitxers remot no hauria de ser ni desat ni validat per la màquina local en cas d’errors. Aquestes opcions podrien ser diferents quan es munta un disc local.
Per aplicar els canvis, tanca el dcument i executa 
```sh
sudo reboot now
```
