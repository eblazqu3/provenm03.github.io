# Git tag 
Git tag és una eina que basicament ens permet marcar punts històrics amb una etiqueta. Per expmle, podriem senyalar el commit en el qual s’ha fet el release de la versió v0.2.4

Els tags seran com branques estatiques, que no evolucionen més. Per tant podrem retornar al tag però no registrar nou històric. 

Farem servir: 

Per afegir un tag 
```sh
git tag <tagname>
```

Per verue quants tags tenim
```sh
git tag
```

per fer push del tag 
```sh
git push origin <tagname>
```
