# Comandes bàsiques git

https://rogerdudler.github.io/git-guide/index.es.html

Git de moment és una de les eines indispensables per a qualsevol programador, no només per programar individualment, si no per a gestió desenvolupament de codi en equip.

Inicialització d’un projecte git en local
```sh
git init 
```

veureu que al fer git init dins de la vostra carpeta hi haurà una anomenada .git que és on estan tots els fitxers de configuració per aquest projecte de git. Es pot tenir més d’un projete 
```sh
ls -la 
```

En un principi de moment no farem gaire cas a aquesta carpeta però és important que mai la borrem

Comandes bàsiques add, status i commit
```sh
git add 
```
serveix per a afegir fitxers al “trackeig” per al següent commit, és com si estiguessim dient que volem afegir els canvis del fitxer per tenir-los en compte properament. 

Això ho podem fer amb fitxers d’un en un, amb tots els fitxers de cop o amb fitxers de diferents tipus. 
```sh
git add my_file.java
```
```sh
git status 
```
serveix per a veure on ens trobem i per poder revisar quins son els fitxers que s’afegiran al commit i quins no, va bé anar fent una ullada abans de fer el commit per veure realment quins son els fitxers que pujarem al commit i que no estiguem equivocant
```sh
git status
```
```sh
git commit
```
Amb el commit el que farem es dir-li a git que guardi al canvis, això ho farà amb un identificador únic, i a més nosaltres podrem posar-li un missatge amb l’opció -m 
```sh
git commit -m "el meu missatge"
```


Anem a fer un exemple i veiem com funciona:
