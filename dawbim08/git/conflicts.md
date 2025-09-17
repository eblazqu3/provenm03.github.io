## Conflicts
Que passa si main ha avançat fitxer diferent

Ara bé, fins ara hem explorat el camí de la felicitat dins de git, però no tot és tan fàcil i bonic quan programem en equip…

Es possible que abans de que nosaltres acabem el nostre canvi, alguns membres de l’equip ho hagin fet abans i per tant hagin afegit el seu codi a main. Com que estem treballant en màquines diferents, ho hauran pujat al repositòri. 

Quan nosaltres haguem acabat de programar la nostra part ens haurem d’assegurar abans de passar el codi a main, que no hi ha problemes (conflicts) amb el codi que ha pujat la resta de comanys.

Primer haurem de fer un pull del codi que han pujat a main, i serà convenient que el pull el fem a la branca main. Per tant primer ens anirem a la branca main, tornarem a la branca new i farem un merge a la nostra branca
```sh
git checkout main
git pull 
git checkout new
git merge main
```


Com que no hem modificat els mateixos fitxers no hem tingut cap mena de problema. 

Ara ja podriem fer un merge segur cap a main

Que passa si main ha avançat mateix fitxer (conflict)

Ara si fem el mateix procediment d’abans però modificant el mateix fitxer en dues branques diferents veurem que arribem a un conflict: 

```sh
<<<<<<< HEAD
hola hola hola
=======
hola hola mal
>>>>>>> main
```

Per sort el git no ens deixa completar el merge fins que no haguem resolt aquets conflicts i ens indica quina part del codi és la que es veu afectada. 

Haurem de decidir segons el cas quina part de codi ens quedem. A vegades ens interessarà quedar-nos una, l’altre o les dues. 

Es cert que si es treballa en documents diferents aquest probleme tendeix a sortir poc, però en projectes grans o relativament grans, és més comú del que sembla modificar el mateix fitxer per a diferents features sense adonar-se i que passin aquest tipus de coses. 

És millor resoldre el conflict a la branca en que estem treballant, mantenint el main el més net possible. 

## Activitat 1

Per parelles creeu un repo, feu un canvi a main i pujeu-lo. Proposta de codi podeu fer un Hello world amb java.

Una vegada tingueu main amb un commit fet, feu-vos cadascú una branca que es digui feature_elvostrenom i feu respectivament una classe que es digui ElVostreNom (ex Pep) i que tingui un mètode que sigui printName() i que printi per pantalla el nom. Aquesta classe s’haurà de fer servir al main. 

Es a dir els dos per separat haureu creat un fitxer diferent i modificat el main. 

Heu d’anar amb compte a l’hora de posar els canvis a main ja que un dels dos ho haurà fet abans. 

Si hi ha conflicts els haureu de resoldre. 

Quan tingueu tot a main funcionant, heu de fer un diff entre el commit actual i el primer commit per veure les diferents coses que s’han modificat. Feu-ho amb fitxers i codi que s’han modificat

