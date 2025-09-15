## Repositòris 
```sh
git push
```
El que hem vist fins ara va molt bé per a gestionar el nostre propi codi en local, ara bé a vegades o més aviat sovint, ens interessarà tenir guardat el codi en un repositori online.

En el cas de que estiguem afegint el repo per primera vegada hauriem de seguir aquest esquema: 
```sh
git 

git add .

git commit -m "first commit"

git branch -M main

git remote add origin https://github.com/NOMBRE_USUARIO/NOMBRE_PROYECTO.git

git push -u origin master
```

Nota: Si volem assegurar que el push el fem amb un usuari en concret millor posar a remote 
```sh
 git remote set-url origin https://user@github.com/user/repo_name.git
```
Com que ja tenim fetes les primeres parts només hem d’afegir el git branch,  git remote i fer el git push 

- Creem un repositori nou a github
- Afegim els canvis al repositori

Proveu de fer canvis en local en un altre fitxer nou, fer el commit i afegir-lo al repo. 

```sh
git fetch i git pull 
```

Igual que volem pujar els nostres canvis a un repositori, també hem de poder fer la inversa. Això ens interesarà per quan hem fet canvis desde una altra màquina o si un company ha desenvolupat una part desde el seu lloc de treball, i poder obtenir els canvis realitzats. 

Per ara hem de saber que per tal de no tenir molts problemes per fer pull, els fitxers que volem actualitzar no els hauriem d’haver fet canvis nosaltres. Si això passés tindriemm un conflict. 

Per tant per tal de recuperar els canvis fets haure de fer: 
```sh
git fetch 

git pull 
```

git fetch verifica si hi ha canvis però sense baixar-los, i git pull verifica si hi ha canvis i si els hi ha els baixa. 


- Feu un canvi de fitxer o creeu un fitxer nou desde github i actualitzeu el vostre repo local

## La importancia del .gitignore

.gitinore és un fitxer especial que afegim al proecte de git per tal d’indicar-li quins directoris o tipus de fitxer NO volem “trackejar” 

Per exemple si creem un hello world amb java i el compilem, se’ns genera un .class. Aquest .class no volem que estigui trackejat, doncs haurem d’incloure els .class al git ignore

Afegir a .gitignore
.gitignore
*.<type>
/<dir>


- Anem a afegir al nostre repo un projecte de netbeans i que només ens trackegi el codi font. 

- Més d’un projecte de netbeans al mateix repo
