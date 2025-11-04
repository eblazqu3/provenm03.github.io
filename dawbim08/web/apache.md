


## Pas 2. Configuració IP estàtica al Servidor Ubuntu


Editem el següent fitxer:

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

```yaml
# Let NetworkManager manage all devices on this system
network:
  version: 2
  ethernets:
    enp0s3:
      addresses: [172.16.100.50/24]
      routes:
        - to: default
          via: 172.16.100.1
      nameservers:
        addresses: [8.8.8.8,8.8.4.4]
```

Apliquem la nova configuració de xarxa:

```bash
sudo netplan apply
```


<details>
<summary>

## Pas 3. Instal·lació del servidor web Apache

</summary>

Instal·lar els paquets del servidor web Apache:
```bash
sudo apt update
sudo apt install -y apache2
```

Per defecte a l'instal·lar el paquet, el servidor web Apache estarà iniciat i ja funcionant. El podem comprovar amb:
```bash
sudo service apache2 status
```

Si necessitem comprovar la versió instal·lada del nostre servidor web Apache, podem fer:
```bash
apache2 -v
```

Per comprovar el correcte funcionament, obrim un navegador web i anem a la url http://localhost/

💡 Si necessitem navegar a una web senzilla i només comprovar que carrega la web, podem utilitzar un Alpine Linux instal·lant un navegador web de consola:
```
apk update
apk add elinks
elinks http://localhost
```

💡 Si necessitem navegar a una web més complicada, podem utilitzar un S.O. amb interfície d'escriptorio o podem utilitzar un appliance de GNS3 que es diu WebTerm.

Si necessitem comprovar els mòduls actius al servidor web Apache fem:
```bash
apache2ctl -M
```

Si necessitem comprovar els espais web Apache actius (VirtualHost), fem:.
```bash
apache2ctl -S
```

⚠️ El servidor web Apache crea la següent estructura de directoris a `/etc/apache2`:
* **mods-available**: configuracions dels mòduls (seguretat, multiprocés, etc.). Aquí estaran tots els mòduls instal·lats.
* **mods-enabled**: enllaços simbòlics de les configuracions dels mòduls. Aquí estaran només els mòduls actius / habilitats.
* **sites-available**: configuracions dels espais webs. Aquí estaran tots els sites creats.
* **sites-enabled**: enllaços simbòlics de les configuracions dels espais webs. Aquí estaran només els sites actius / habilitats.
* **conf-available**: fragments de la configuració global. Aquí estaran totes les configuracions disponibles.
* **conf-enabled**: enllaços simbòlics dels fragments de la configuració global. Aquí estaran només les configuracions actives / habilitades.

</details>

<details>
<summary>

## Pas 4. Opcional: Configuracions addicionals del servidor web Apache

</summary>

### ➡️ Canviar el port per defecte

⚠️ Recordeu: és OPCIONAL. No és habitual canviar els ports per defecte si només tenim una aplicació web executant-se.

Editar el fitxer de configuració de ports del servidor web.
```
sudo nano /etc/apache2/ports.conf
```

Configurar les següents línies per canviar el port a 8080.

```bash
#Listen 80
Listen 8080
```
Reiniciar el servei.
```bash
sudo service apache2 restart
```

Comprovar el correcte funcionament: http://localhost:8080/

> :warning: No és habitual avui dia canviar el port del servidor web. Torneu-lo a canviar a port 80 que és el por defecte de http

### ➡️ Amagar la versió del servidor web Apache per temes de seguretat

Editar el fitxer de configuració principal del servidor web.
```bash
sudo nano /etc/apache2/conf-available/security.conf
```

Configurar les següents línies.
```bash
# oculta la versión de APACHE en las cabeceras de respuesta HTTP
ServerTokens Prod

# oculta la versión de APACHE en cualquier página de error
ServerSignature Off
```

Reiniciar el servei.
```bash
sudo service apache2 restart
```

### ➡️ Limitar les connexions de petició simultànies (rendiment)

Editar el fitxer de configuració del mòdul del servidor web:
```bash
sudo nano /etc/apache2/mods-available/mpm_prefork.conf
```

Configurar les següents línies:
```xml
<IfModule mpm_prefork_module>
# De entrada inicia 5 servidores
  StartServers           5
# Después mantiene entre 5 y 10 servidores en todo momento para atender solicitudes
  MinSpareServers        5
  MaxSpareServers        10
# Inicia un máximo de 150 instancias  
  MaxRequestWorkers      150
# Número de solicitudes que el servidor maneja (0 = ilimitado)
  MaxConnectionsPerChild 0
</IfModule>
```

Desactivar el mòdul mpm_event i activar el mòdul mpm_prefork del servidor web:
```bash
sudo a2dismod mpm_event
sudo a2enmod mpm_prefork
```

Reiniciar el servei:
```bash
sudo service apache2 restart
```
</details>

<details>
<summary>

## Pas 5. Opcional: Instal·lar PHP

</summary>

Instal·lar els paquets de PHP:
```bash
sudo add-apt-repository ppa:ondrej/php # Press enter when prompted.
sudo apt update  
sudo apt install -y php8.3 libapache2-mod-php8.3 php8.3-{bz2,curl,mbstring,intl,cgi,mysql,xml,zip,sqlite3,ldap,gd,imap}

```

Reiniciar el servei:
```bash
sudo service apache2 restart
```

Per comprovar el funcionament de PHP, creem el següent arxiu:
```bash
sudo nano /var/www/html/prova.php
```

```php
<?php
phpinfo();
?>
```

Per comprovar-lo, anem al navegador web a la url http://localhost/prova.php

</details>

<details>
<summary>

## Pas 6. Opcional: Configurar APACHE per mostrar els errors de PHP (per desenvolupament)

</summary>

Editar el fitxer de configuració de PHP principal.
```bash
sudo nano /etc/php/8.3/apache2/php.ini
```

Configurar les següents línies.

```bash
display_errors = On
```

Reiniciar el servei.
```bash
sudo service apache2 restart
```
</details>

<details>
<summary>

## Pas 7. Opcional: Activar l'espai web d'usuari per APACHE (directori d'usuari public_html)

</summary>

Amb aquesta opció farem que cada usuari del sistema tingui la seva carpeta personal de www de Apache al seu home.
Crearem la carpeta **public_html** a dins del home de l'usuari (podeu ficar el caràcter ~ prement la tecla Alt dreta + 4):
```bash
mkdir ~/public_html
```

Canviar els permisos del directori d'usuari, per que qualsevol usuari pugui llegir i executar els fitxers (escriure solament el propietari).
```bash
chmod 755 ~/public_html
```

Afegim l'usuari al grup www-data:
```bash
sudo usermod -g www-data alumne
```

Activem el mòdul d'espai web d'usuari a Apache amb:
```bash
sudo a2enmod userdir
```

Editar el fitxer de configuració del mòdul PHP del servidor web.
```bash
sudo nano /etc/apache2/mods-available/php8.1.conf
```

Configurar les següents línies.
```bash
php_admin_flag engine On
```

Reiniciar el servei.
```bash
sudo service apache2 restart
```

Es podrà accedir a les webs dels usuaris del sistema mitjançant la url http://localhost/~user/
Provem d'accedir a la web de l'usuari alumne amb la url http://localhost/~alumne/

</details>

<details>
<summary>

## Pas 8. Opcional: Instal·lar BBDD MySQL

</summary>

Instal·lar els paquets.
```bash
sudo apt update
sudo apt install -y mysql-server mysql-client
```


Reiniciar els serveis MySQL i Apache:
```bash
sudo service mysql restart
sudo service apache2 restart
```

Si volem comprovar la versió instal·lada de MySQL fem:
```bash
mysql --version
```

Accedim a MySQL amb qualsevol password (no en té ara mateix cap password l'usuari root):
```bash
sudo mysql -u root -p
```

Modifiquem la contrasenya de l'usuari root, canviant a les següents ordres XXXXX pel password que volem ficar.
```bash
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'XXXXX';
mysql> FLUSH PRIVILEGES;
```

Provem a accedir a MySQL amb l'usuari root i el nou password:
```bash
sudo mysql -u root -p
```
</details>

<details>
<summary>

## Pas 9. Opcional: Instal·lar gestor web BBDD phpMyAdmin

</summary>

Instal·lar els paquets.
```bash
sudo apt update
sudo apt install -y phpmyadmin
```

A l'instal·lador ens sortirà una pantalla que demanarà una configuració de phpMyAdmin. Seleccionar les següent opcions amb les fletxes del teclat per moure entre opcions, Tab per seleccionar un altre component (p.ex. el botó) i la barra espaiadora per seleccionar una opció o clicar un botó.
```
Servidor web que desea reconfigurar automáticamente: apache2
Configure database for phpmyadmin with dbconfig-common? <No>
```

Editar el fitxer de configuració principal del servidor web.
```bash
sudo nano /etc/apache2/apache2.conf
```

Afegir al fitxer la següent línia abans de l'últim comentari del fitxer (els comentaris comencen per #):
```bash
Include /etc/phpmyadmin/apache.conf
```

Reiniciar el servei.
```bash
sudo service apache2 restart
```

Per accedir a phpMyAdmin, obrim el navegador i anem a la url http://localhost/phpmyadmin/
- Usuari: root
- Password: el que hem ficat a XXXXX

</details>

<details>
<summary>

## Pas 10. Opcional: Instal·lació i configuració d'un site / espai web virtual amb APACHE

</summary>

Per defecte, al realitzar una instal·lació d'Apache web server ens instal·la un VirtualHost amb la configuració al fitxer `/etc/apache2/sites-enabled/000-default.conf`. Ara farem VirtualHost personalitzats.

En el següent exemple crearem un VirtualHost per a **dominio.foo**

Crear la següent estructura de directoris:
```bash
sudo mkdir -p /var/www/dominio
```

Canviar el propietari (recursivament) de l'estructura de directoris (de "root" a usuari web d'APACHE "www-data").
```bash
sudo chown -R www-data:www-data /var/www
```

Canviar els permisos (recursivament) dels directoris web, per que qualsevol usuari pugui llegir i executar els fitxers (escriure solament el propietari).

```bash
sudo chmod -R 755 /var/www
```

Crear una pàgina HTML o PHP d'inici a l'espai web.
```bash
sudo nano /var/www/dominio/index.html
```
```bash
sudo nano /var/www/dominio/index.php
```

Crear el fitxer de configuració de l'espai web a partir d'un altre per defecte.
```bash
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/dominio.conf
```

Editar el fitxer de configuració de l'espai web.
```bash
sudo nano /etc/apache2/sites-available/dominio.conf
```

Configurar les següents línies.
```xml
<VirtualHost *:80>
  ServerAdmin admin@dominio.foo
  ServerName dominio.foo
  ServerAlias www.dominio.foo
  DocumentRoot /var/www/dominio

  <Directory /var/www/dominio>
      Options FollowSymLinks
      AllowOverride All
      Require all granted
  </Directory>

  ErrorLog ${APACHE_LOG_DIR}/dominio-error.log
  CustomLog ${APACHE_LOG_DIR}/dominio-access.log combined
</VirtualHost>
```

Desactivar l'espai web per defecte (000-default.conf) i activar el nou espai web de dominio.foo (dominio.conf):
```bash
sudo a2dissite 000-default.conf
sudo a2ensite dominio.conf
```

Comprovar que el fitxer de configuració és correcte.
```bash
sudo apache2ctl configtest
```

Reiniciar el servei.
```bash
sudo service apache2 restart
```

Editar el fitxer de configuració de màquines del sistema operatiu, per afegir la màquina on està l'espai web.
```bash
sudo nano /etc/hosts
```

Afegim la següents línia a continuació de les 2 primeres que ja n'hi han:
```bash
172.16.100.50 dominio.foo www.dominio.foo
```

Comprovar els directoris dels espais webs disponibles i activats.
```bash
sudo ls -l /etc/apache2/sites-available/
sudo ls -l /etc/apache2/sites-enabled/
```

Podem comprovar el correcte funcionament del nou VirtualHost que hem creat obrint un navegador web i accedint a la url http://www.dominio.foo

</details>

<details>
<summary>

## Pas 11. Opcional: Permetre o denegar el llistat de fitxers

</summary>

Editar el fitxer de configuració de l'espai web per denegar el llistat de fitxers a l'arrel del domini.
```bash
sudo nano /etc/apache2/sites-available/dominio.conf
```

### Desactivar el llista de fitxers d'un directori

Configurar les següents línies.

```xml
<VirtualHost *:80>
  ServerAdmin admin@dominio.foo

  ServerName dominio.foo
  ServerAlias www.dominio.foo
  DocumentRoot /var/www/dominio/

  <Directory "/var/www/dominio/">
    Options -Indexes
    AllowOverride All
  </Directory>

  ErrorLog ${APACHE_LOG_DIR}/dominio-error.log
  CustomLog ${APACHE_LOG_DIR}/dominio-access.log combined
</VirtualHost>
```

Reiniciar el servei.
```bash
sudo service apache2 restart
```

### Activar el llistat de fitxers d'un directori

Crear un fitxer de configuració de directoris per permetre el llistat de fitxers en un directori públic.
```bash
sudo mkdir /var/www/dominio/public/
sudo nano /var/www/dominio/public/.htaccess
```

Configurar les següents línies.
```bash
Options +Indexes
```

Crear un fitxer de configuració de directoris per denegar el llistat de fitxers en un directori privat.
```bash
sudo mkdir /var/www/dominio/private/
sudo nano /var/www/dominio/private/.htaccess
```
<details>
<summary>

### Opció usuaris

</summary>

Configurar les següents línies al fitxer **/var/www/dominio/private/.htaccess**

```bash
AuthName "User authentication" 
AuthType Basic
AuthUserFile /etc/apache2/.htpasswds

# Autenticar todos los usuarios
Require valid-user

# Autenticar algunos usuarios
#Require user username1 username2

Options +Indexes
```

Crear el fitxer d'usuaris i contrasenyes. Afegir usuaris i contrasenyes.
```bash
sudo htpasswd -cb /etc/apache2/.htpasswds username1 XXXXX
sudo htpasswd -b /etc/apache2/.htpasswds username2 XXXXX
```

Comprovar el fitxer d'usuaris i contrasenyes.
```bash
sudo cat /etc/apache2/.htpasswds
```

Reiniciar el servei.
```bash
sudo service apache2 restart
```
</details>

<details>
<summary>

### Opció grups

</summary>

Configurar les següents línies al fitxer **/var/www/dominio/private/.htaccess**
```bash
AuthName "Group authentication" 
AuthType Basic
AuthUserFile /etc/apache2/.htpasswds
AuthGroupFile /etc/apache2/.htgroups

#### Autenticar un grup
Require group groupname

Options +Indexes
```

Editar el fitxer de grups
```bash
sudo nano /etc/apache2/.htgroups
```

Configurar les següents línies.
```bash
groupname: username1 username2
```

Activar el mòdul d'autenticació de grups del servidor web.
```bash
sudo a2enmod authz_groupfile
```

Reiniciar el servei.
```bash
sudo service apache2 restart
```
</details>
</details>

<details>
<summary>

## Pas 12. Opcional: Configurar l'espai web amb connexions segures SSL/TLS (HTTPS)

</summary>

Instal·lar els paquets.
```bash
sudo apt update
sudo apt install -y openssl
```

Generar la clau privada del domini propi.

> :warning: **Les claus privades solen tenir permisos 600 i root com a propietari.**

```bash
sudo openssl genrsa -out /etc/ssl/private/clave_dominio.key 2048
```

  * **genrsa** és l'algoritme d'encriptació utilitzat.
  * **2048** és la longitud en bits de la clau.

Generar la solicitut de signatura del certificat (CSR - Certificate Signing Request) del domini propi a partir de la clau privada.

```bash
sudo openssl req -new -key /etc/ssl/private/clave_dominio.key -out /etc/ssl/certs/solicitud_dominio.csr
```
```
Country Name (2 letter code) [AU]:ES
State or Province Name (full name) [Some-State]:Barcelona
Locality Name (eg, city) []:Hospitalet
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Proven
Organizational Unit Name (eg, section) []:Smx
Common Name (e.g. server FQDN or YOUR name) []:www.dominio.foo
Email Address []:admin@dominio.foo
  
Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

  * El **Common Name** és el nom del domini propi. No és el mateix registrar dominio.foo que www.dominio.foo (si es registra un certificat amb una de les dues opcions, no funcionarà per l'altra). 

Mostrar la solicitut de signatura del certificat del domini propi codificat.
```bash
cat /etc/ssl/certs/solicitud_dominio.csr
```

Mostrar la solicitut de signatura del certificat del domini propi sense codificar.
```bash
openssl req -text -noout -in /etc/ssl/certs/solicitud_dominio.csr
```

**SI ES COMPRA EL CERTIFICAT, AQUEST PAS NO CAL.** Generar el certificat SSL/TLS autosignat del domini propi a partir de la clau privada i de la solicitut de signatura del certificat (CSR).

**Els certificats solen tenir permisos 644 i root com a propietari.**

```bash
sudo openssl x509 -req -days 365 -in /etc/ssl/certs/solicitud_dominio.csr -signkey /etc/ssl/private/clave_dominio.key -out /etc/ssl/certs/certificado_dominio.crt
```

  * **x509 -req** és el format o estàndard utilitzat per CSR.
  * **-days** és la data d'expiració del certificat.

Mostrar el certificat SSL/TLS autosignat del domini propi.
```bash
sudo openssl x509 -text -noout -in /etc/ssl/certs/certificado_dominio.crt
```

Activar el mòduls **ssl**, **rewrite** i **headers**.
```bash
sudo a2enmod ssl
sudo a2enmod rewrite
sudo a2enmod headers
```

Editar el fitxer de configuració de l'espai web.
```bash
sudo nano /etc/apache2/sites-available/dominio.conf
```

Configurar les següents línies.
```xml
# apache version: 2.4.29
# openssl version: 1.1.1d
# requires apache modules: ssl, rewrite, headers

<VirtualHost *:80>
  ServerName dominio.foo
  ServerAlias www.dominio.foo

  # Redireccionar las peticiones HTTP a HTTPS
  RewriteEngine on
  RewriteRule ^(.*)$ https://%{HTTP_HOST}$1 [R=301,L]
</VirtualHost>

<VirtualHost *:443>
  ServerAdmin admin@dominio.foo
 
  ServerName dominio.foo
  ServerAlias www.dominio.foo
  DocumentRoot /var/www/dominio/
 
  <Directory /var/www/dominio>
    Options FollowSymLinks
    AllowOverride All
    Require all granted
  </Directory>
  
  # Habilitar SSL/TLS
  SSLEngine on
  
  # Habilitar certificados SSL/TLS
  SSLCertificateFile       /etc/ssl/certs/certificado_dominio.crt
  SSLCertificateKeyFile    /etc/ssl/private/clave_dominio.key
  #SSLCACertificateFile    /etc/ssl/certs/certificado_ca.crt
  #SSLCertificateChainFile /etc/ssl/certs/certificado_ca.crt

  # Habilitar HTTP/2 y HTTP/1.1
  Protocols h2 http/1.1
 
  # Habilitar protocolos más seguros y deshabilitar inseguros
  SSLProtocol all -SSLv3 -SSLv2 -TLSv1 -TLSv1.1

  # Forzar cifrados del servidor para que el cliente no pueda elegir
  SSLHonorCipherOrder on
  
  # Habilitar cifrados más fuertes
  SSLCipherSuite "ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384"

  # Deshabilitar compresión y sesiones SSL/TLS para un mejor rendimiento
  SSLCompression    off
  SSLSessionTickets off

  # Habilitar HSTS (63072000 segundos = 2 años) para forzar el uso de HTTPS desde la primera visita
  Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains; preload"
 
  ErrorLog ${APACHE_LOG_DIR}/dominio-error.log
  CustomLog ${APACHE_LOG_DIR}/dominio-access.log combined
</VirtualHost>
```

  * [SSL Configuration Generator](https://mozilla.github.io/server-side-tls/ssl-config-generator/)
  * [SSL Server Test](https://www.ssllabs.com/ssltest/index.html)
  * [SSL and TLS Deployment Best Practices](https://github.com/ssllabs/research/wiki/SSL-and-TLS-Deployment-Best-Practices)

  * **certificado_dominio.crt** és el certificat SSL/TLS autosignat del domini propi. **PROPORCIONAT PER L'AUTORITAT CERTIFICADORA (CA), SI ES COMPRA EL CERTIFICAT.**
  * **clave_dominio.key** és la clau privada del domini propi.
  * **certificado_ca.crt** és el certificat intermediari del domini propi generat per una autoritat certificadora (CA). **PROPORCIONAT PER L'AUTORITAT CERTIFICADORA (CA), SI ES COMPRA EL CERTIFICAT.**

Desactivar i activar l'espai web.
```bash
sudo a2dissite 000-default.conf
sudo a2ensite dominio.conf
```

Comprovar que el fitxer de configuració és correcte.
```bash
sudo apache2ctl configtest
```

Reiniciar el servei.
```bash
sudo service apache2 restart
```

Comprovar el correcte funcionament.

https://www.dominio.foo/

</details>