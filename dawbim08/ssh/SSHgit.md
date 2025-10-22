# SSH Keys i Git
Pots generar una nova clau SSH a la teva màquina local. Després de generar la clau, pots afegir la clau pública al teu compte de GitHub.com per habilitar l'autenticació per a operacions de Git mitjançant SSH.
Nota: GitHub va millorar la seguretat eliminant tipus de claus antics i insegurs el 15 de març de 2022.

A partir d'aquesta data, les claus DSA (ssh-dss) ja no estan suportades. No pots afegir noves claus DSA al teu compte personal de GitHub.

Les claus RSA (ssh-rsa) amb una valid_after anterior al 2 de novembre de 2021 poden continuar utilitzant qualsevol algoritme de signatura. Les claus RSA generades després d'aquesta data han d'utilitzar un algoritme de signatura SHA-2. Alguns clients antics poden necessitar ser actualitzats per utilitzar signatures SHA-2.
Passos per generar la clau:
Obre el Terminal.

Enganxa el següent text, substituint l'email per l'adreça del teu compte de GitHub:
```sh
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Abans d'afegir una nova clau SSH al ssh-agent per gestionar les teves claus, has d'haver comprovat si ja tens claus SSH existents i haver generat una nova clau SSH.
Inicia el ssh-agent en segon pla:
```sh
$ eval "$(ssh-agent -s)" 
> Agent pid 59566
```

Afegeix la teva clau privada SSH al ssh-agent. Si has creat la teva clau amb un nom diferent o estàs afegint una clau existent que té un nom diferent, substitueix id_ed25519 pel nom del fitxer de la teva clau privada:

```sh
ssh-add ~/.ssh/id_ed25519
```

Afegeix la clau pública SSH al teu compte de GitHub. 
https://docs.github.com/es/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account


# SSH i VSCode
Instal·la al client ubunut el vs code, decarregat de la pagina oficial el .deb i fes: 

sudo apt install ./<file>.deb


Seguiu l’enllaç per a crear una connexió ssh a través d’ssh 

https://code.visualstudio.com/docs/remote/ssh
