# SetUp VsCode 

## Instal·lar scoop 

Obrim el powrshell a la nostra màquina i instal·lem scoop

``` sh
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```
Una vegada instal·lat scoop instal·lem el vscode: 

``` sh 
scoop install git

scoop bucket add extras

scoop install extras/vscode
```
## Configurar Vscode per poder fes servir java 
![extensions-vscode](/assets/vscode/image-1.png)

A la pestanya d'extensions buscar: 

![java-extension](/assets/vscode/image.png)

## Començar un projecte nou amb java i vscode

![comands-vscode](/assets/vscode/image-2.png)

Prem Ctrl+Shift+P i escriu java: Create Java Project

![java-project](/assets/vscode/image-3.png)

Tria l'opció de no build tools

![no-build-tools](/assets/vscode/image-4.png)

Anomena i desa el projecte a la teva carpeta de treball 

Hauries de poder veure que s'ha creat el projecte dins l'entorn del vscode

![projecte](/assets/vscode/image-5.png)

Per executar el projete pots fer botó dret + run, o ctrl + F5

![run-project](/assets/vscode/image-6.png)