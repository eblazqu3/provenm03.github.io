# Git stash

Sovint quan treballem en un projecte de desenvolupament de software, treballem en diferents aspectes d’aquest. Això el que farà és que estiguem treballant en diferents branques simultàneament. 

Ara bé, igual que fins ara, hem vist que tots els canvis es poden fer commit i avançar en el timeline del nostre projecte, també podem trobar-nos en la situació en que hem avançat en els nostres canvis però no estan llestos per fer commit, però tampoc volem descartar-los. 

Aquí és on entra el paper de stash el que farà git stash és guardar els canvis (en procés) per tal de poder treballar en una altre branca. Tot això només pasa en el vostre repositori local.

## Exemple:

Projecte git
2 branques
fer canvis en codi
fer checkout entre branques (que passa?)
fer stash 
fer checkout entre branques (que passa?)

```sh 
git stash pop vs git stash apply
```
Per tal de recuperar els canvis que hem guardat per usar més tard amb stash utilitzem git stash pop o git stash apply. Ara bé, hi ha una diferència entre els les dues comandes: 
git stash pop elimina el contingut del que s’havia fet stash i ho aplica a la zona de treball, i git stash apply, deixa una còpia del que s’havia fet stash per si es vol aplicar el canvi en una altre branca. 

Stashing de fitxer que no estan “trackejats” o fitxers ignorats. 

Per defecte git stash només guarda els canvis dels fitxers modificats que ja han estat “trackejats” per algun commit. Però no ho farà dels fitxers nous i dels fitxers ignorats a no ser que ho indiquem. 

Per tal d’indicar-ho hem de fer git stash -u -a (-u per untracked i -a per all

Treballar amb multiples stash

No estem limitats a fer un sol stash. Pots fer-ne múltiples i veure els que tens amb git stash list
```sh
$ git stash list
stash@{0}: WIP on main: 5002d47 our new homepage
stash@{1}: WIP on main: 5002d47 our new homepage
stash@{2}: WIP on main: 5002d47 our new homepage
```

Com que és una mica confus saber que estavem fent amb aquests stashes git ens dona la opció de posar un missatge usant 
```sh
git stash save "message"
```

Per tal de recuperar un dels multiples stashes ho fem usant: 

```sh
$ git stash pop stash@{2}
```

