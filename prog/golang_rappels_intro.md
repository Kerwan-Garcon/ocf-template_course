<!-- markdownlint-disable MD041 MD033 MD024 MD026 -->
title:Golang, rappels
intro:récapitulera certaines notions vues précédemment.
conclusion:Rappelé certaines notions déjà abordées.

---

### Rappels

- On a deux catégories de types en Go : value et headers
  - Un type value décrit directement la valeur correspondante (un bool est bien un boolean, et pas une adresse vers un boolean)
  - Un type header contient des informations sur les valeurs qu'il décrit. On peut bien sûr récupérer ces valeurs.

---

### Rappels

- Exemple de type header : les slices (trois valeurs: pointeur vers le premier élément, longueur, capacité)
- Les zero values : ce sont les valeurs par défaut des types en Go.

---

### Rappels pointeurs

- Les pointeurs sont des adresses mémoire
- Ils désignent un emplacement, où trouver "autre chose"
- Cela peut être une valeur, ou un autre pointeur etc...

Mais alors pourquoi utiliser des pointeurs ?

---

### Qu'affiche ce code ?

```golang  
package main
import (
  "fmt"
)
func main() {
  ex:= Example{age: 10}
  modifAge(ex, 43)
  fmt.Println(ex)
}

type Example struct{
  age int
}

func modifAge (ex Example, age int){
  ex.age=age
}
```

---
Résultat :

```shell
{10}
```

---

### Pourquoi ?

- Go est "pass by value"
  - C'est à dire que quand on passe une variable à une fonction :
    - une copie est faite
    - c'est cette copie qui est utilisée dans la fonction

- Comment "réparer" l'exemple précédent ?
  => Avec des pointeurs !

---

```golang  
package main
import (
  "fmt"
)
func main() {
  ex:= Example{age:10}
  modifAge(&ex, 43)
  fmt.Println(ex)
}

type Example struct{
  age int
}

func modifAge(ex *Example, age int){
  ex.age=age
}
```
