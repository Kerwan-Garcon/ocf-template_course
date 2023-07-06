<!-- markdownlint-disable MD041 MD033 MD024 MD026 -->
title:L'architecture d'un programme
intro:nous détaillera comment sont architecturés les programmes Go.
conclusion:L'architecture des programmes Go.

---

### Architecture d'un programme

- Un programme Go s'organise en packages

- Il y a une fonction main, dans le package main, qui sert à lancer le programme

- Un package est un "dossier" dans le code source qui sert à organiser le code

---

### Packages

- Pour déclarer un package :

```golang
package main
```

- Pour utiliser un package :

```golang
import "NomDuModule/main"
```

---

### Packages

- Visibilité :
  - Publique : commence avec une majuscule
  
  ```golang
  func MyFunc(){}
  ```

  - Privée : minuscule

  ```golang
  func myFunc(){}
  ```

---

### Le code

- Les types de variables
- Déclarer une variable
- Définir une fonction
- Définir une méthode
- Les structures de base

---

### Les types de variables

Principaux types de données en Go:

- **bool** : true ou false
- **int** : entiers signés 32 ou 64 bits
- **float32**, **float64** : nombres à virgule flottante
- **string** : chaîne de caractères
- **array** : tableau de taille fixe
- **slice** : tableau de taille variable
- **map** : table de hachage
- **struct** : type de données personnalisé

---

### Déclarer une variable

- Déclaration simple, puis initialisation

```golang
var maChaine string
maChaine = "test"
```

- Déclaration et initialisation en une seule ligne

```golang
maChaine := "test"
```

---

<!-- _footer: "" -->

### Déclarer une fonction

- Mot clé **func**

- Nom de fonction

- Argument Type, séparés par des parenthèses

- Retours

```golang
func maFunc(arg1 string, arg2 string) (string, err){}
```

Pour les types identiques, on peut aussi écrire :

```golang
func maFunc(arg1, arg2 string) (string, error){}
```

---

### Définir une méthode

- Il faut ajouter un champ qu'on appelle "receiver"

- Attention au receiver ! Pointeur, ou valeur !

```golang
type Example struct{}
func (ex *Example) maFunc(arg1, arg2 string) (string, error){}
```

---

### Conditions en Go :

```go
if condition {
  // instructions à exécuter si la condition est vraie
} else {
  // instructions à exécuter si la condition est fausse
}
```

```go
age := 30
if age >= 18 {
  fmt.Println("Vous êtes majeur")
} else {
  fmt.Println("Vous êtes mineur")
}
```

---

### Boucles en Go : for

```go
for initialisation; condition; incrément {
  // instructions à exécuter à chaque itération
}
```

```go
for i := 0; i < 5; i++ {
  fmt.Println(i)
}
```

---

### Boucles en Go : while (presque.)

```go
for condition {
  // instructions à exécuter tant que la condition est vraie
}
```

```go
i := 0
for i < 5 {
  fmt.Println(i)
  i++
}
```

---

### Le code : avancé

- Les types de données

  - Les types "value"

  - Les types "headers"

- Les slices

- Types personnalisés

- Les "zero-value"

- Lecture des arguments passés au programme

---

### Les types de données

- Go a deux grand "types" de données :

  - Types "value"
  
  - Types "headers"
  
- Types "value"

  - Ce sont des types qui désignent directement la valeur qu'ils décrivent.

  - Exemple : les int, rune (équivalent de "char", alias de int32 en go), bytes (alias de int8 en go), bool...

---

### Les types de données

- Types "headers"

  - Ce sont des types qui comportent des références vers les valeurs décrites.

  - Exemple : Les string. C'est un groupe de deux valeurs : une adresse sur le tas, et une longueur.

  - Autre exemple : Les slices.

---

### Les slices

- Ce sont des "vues", sur des tableaux.

---

### La slice : un type qui a trois valeurs

- Un pointeur vers le premier élément du tableau

- Une longueur (length)

  - Nombre d'éléments auxquels on a accès depuis le pointeur vers le premier élément. On l'obtient avec len(maSlice)

- Une capacité (capacity)

  - Nombre d'éléments existant dans le tableau sous-jascent. On l'obtient avec cap(maSlice)
  
---

```golang
package main
import (
  "fmt"
)
func main() {
  orig:=[]string{"one", "two", "three", "four"}
  sl1:=test[0:2]
  sl2:=test[1:3]
  modifSlice(sl1, 1)
  fmt.Printf("Ma slice 2: %v", sl2)
}

func modifSlice(sl []string, idx int){
  sl[idx] = "modified"
}
```

---

### Vrai tableau ?

Est-ce qu'on peut faire de "vrais" tableaux, et pas des slices ?

Oui ! Comme ça :

```golang
vraiTableau:=[2]int{1,8}
```

---

### Définir son propre type

- On peut définir des types qui contiennent plusieurs champs, à la manière des classes.

- Attention, les types n'ont (presque toujours) que de la donnée à l'intérieur, pas de méthodes !

- Exemple :

```golang
type ExempleType struct{
  age int
  nom string
}
```

---

### Définir son propre type

On peut aussi définir des alias de types:

- types interchangeables :

```golang
type T = string
```

- nouveaux types :

```golang
type T string
```

---

### Les "zero-value"

- En go, l'initialisation des variables est automatique si elle n'est pas explicite

- Chaque type a une "zero-value", qui correspond à la valeur par défaut du type

- Exemples : 0 pour int, false pour bool, etc.

- Fonctionne pour tous types y compris structs, et "types header" !

---

### Lecture des arguments

- On peut lire les arguments passés à un programme par une commande comme :

```shell
go run main.go "image.png"
```

---

### Lecture des arguments

- Pour celà, il faut importer le package "os", et récupérer la slice de string "Args" :

```golang
argsWithProg := os.Args
argsWithoutProg := os.Args[1:]
arg := os.Args[3]
```

---

### Premier programme

- Dossier du projet

```shell
mkdir hello && cd hello
```

---

### Premier programme

- Initialisation d'un module : création du fichier go.mod

```shell
go mod init hello
```

---

### Premier programme

- Déclaration du package, et fonction main

```golang
package main
func main(){}
```

---

### Premier programme

- Imports et appel

```golang
package main
import "fmt"
func main(){
  fmt.Println("Hello, World!")
}
```

---

### On itère

- Est-ce qu'on pourrait faire exécuter une commande système à notre programme ?

- Essayons avec la commande "date", pour afficher la... date.

---

### On itère

- Le package os/exec :
  - <https://golang.org/pkg/os/exec/>
- Le type Cmd:
  - Cmd represents an external command being prepared or run.
  - A Cmd cannot be reused after calling its
    - Run
    - Output
    - CombinedOutput
  methods

---

- La fonction Command :

```golang
func Command(name string, arg ...string) *Cmd
```

- La méthode Output :

```golang
func (c *Cmd) Output() ([]byte, error)
```

---

- Revenons à notre exemple :

```golang
package main
import "fmt"
func main() {
  fmt.Println("Hello, World!")
}
```

---

- On modifie la fonction :
  Création de Cmd

```golang
package main
import "fmt"
func main(){
  out, err := exec.Command("date").Output()
  fmt.Println("Hello, World!")
}
```

---

- Gestion des erreurs :

```golang
package main
import "fmt"
func main(){
  out, err := exec.Command("date").Output()
  fmt.Println("Hello, World!")
}
```

- Ce code ne compile pas !

---

- Affichage du message de sortie :

```golang
package main
import (
  "fmt"
  "log"
  "os/exec"
)

func main(){
  out, err := exec.Command("date").Output()
  if err!=nil {
    log.Fatal(err)
  }
  fmt.Printf("The date is: %s\n", out)
}
```

---

### Pourquoi ça fonctionne ?

- "date" fonctionne parce qu'elle est disponible sous Windows et sur les systèmes type Unix.

- Comment faire pour une commande qui dépend de l'OS ?
  - Réponse : la cross-compilation, et les instructions de compilation
