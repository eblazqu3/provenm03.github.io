# Activitat 3 ( per parelles ) 

repo: https://github.com/eblazqu3/m08merging

Un dels dos membres de la parella feu un fork com s’indica aquí: 
https://docs.github.com/es/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo


Membre 1: 
T’has de fer una branca nova per modificar Calculadora.java i afegeix el següent codi:
Nota: intenta no copiar el bloc de codi per poder fer bé la part del diff
public int suma(int a, int b) { 
if (!Utilidades.esNumeroValido(a) || !Utilidades.esNumeroValido(b)) { throw new IllegalArgumentException("Parámetros inválidos"); } return a + b; 
}


Fes un commit 
Afegeix també: 

```java
public int multiplicacion(int a, int b) { 
   return a * b; 
}
```

Fes un altre commit
Fes un diff entre el teu ultim commit i el commit anterior (que hi veus?) 

També modificaràs la classe Main.java afegint: 

```java
System.out.println("Multiplicación: " + calculadora.multiplicacion(5, 3));
```

Esborra una linia de codi que fagi que “s’espatlli” i fes un commit 
Quina estrategia triaries per desfer aquest error? (Fes-la) 

Membre 2: 
T’has de fer una branca nova per modificar Utilidades.java i afegeix el següent codi:

```java
public static boolean esPositivo(int num) { 
    return num >= 0; 
}
```


Fes un commit 
Afegeix també: 

```java
public static boolean esNumeroValido(Object num) { 
return num instanceof Integer; 
}
```


Fes un altre commit
Fes un diff entre el teu ultim commit i el commit anterior (que hi veus?) 

També modificaràs la classe Main.java afegint: 

```java
if (Utilidades.esPositivo(5)) { 
System.out.println("El número es positivo");
 }
 ```


Esborra una linia de codi que fagi que “s’espatlli” i fes un commit 
Quina estrategia triaries per desfer aquest error? (Fes-la) 



El que acabi primer que faci un merge a main i realitzi un push, l’altre haurà de fer un pull de main i resoldre els conflictes a la seva branca entre els dos. Finalment he d’aconseguir que el codi de main estigui lliure de conflicts i amb tot el codi afegit pels dos membres de la parella
