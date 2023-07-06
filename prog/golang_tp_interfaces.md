<!-- markdownlint-disable  MD013 MD041 MD033 MD034 MD024 MD026 -->
title:Les interfaces - TP
intro:nous permettra de tester les interfaces Golang.
conclusion:Testé les interfaces dans le langage Golang.

---

### Reprenez le code suivant :

```go
package main

import "fmt"

type IPAddr [4]byte

func main() {
  hosts := map[string]IPAddr{
    "loopback": {127, 0, 0, 1},
    "googleDNS": {8, 8, 8, 8},
  }
  for name, ip := range hosts {
    fmt.Printf("%v: %v\n", name, ip)
  }
}
```

---

### Interface Stringer

- Implémentez fmt.Stringer pour IPAddr, de façon à afficher l'adresse IP comme une suite de nombres séparés de points.
  Autrement dit, la slice IPAddr{1, 2, 3, 4} devrait s'afficher de la façon suivante : **"1.2.3.4"**. 
- Implémentez ceci de deux façons :
  - En utilisant le package https://golang.org/pkg/fmt/ et fmt.Sprintf()
  - En utilisant le package https://golang.org/pkg/strconv/

---

### Interface Error (1/3)

- On va implémenter une erreur "custom". Pour cela, remarquez que les erreurs en Go sont simplement des entités qui implémentent l'interface :
  https://golang.org/pkg/builtin/#error

---

### Interface Error (2/3)

- Définissez une struct "**MyError**" qui contient deux champs :
  - "When" de type time.Time (voir https://golang.org/pkg/time/#Time)
  - "What" de type string
- Implémentez l'interface Error pour votre struct "**MyError**".
- Écrivez une fonction "**run()**", dont la signature est :
  
  ```go
  func run() error
  ```
  
  qui retourne systématiquement une erreur comme celle que vous venez de créer.
- Pour donner une valeur au champ du temps, utilisez "**Now()**", du package time de la librairie standard.
- Pour donner une valeur au champ du "**What**", choisissez un texte d'erreur non vide qui vous plaît.

---

### Interface Error (3/3)

- Dans la fonction main de votre programme, exécutez la fonction "**run()**" que vous venez de définir, et récupérez l'erreur en l'assignant à une variable, comme ceci par exemple :
  
  ```go
  err := run()
  ```

- Vérifiez si votre erreur est vide (elle ne devrait jamais l'être) en écrivant un "**if**", et si elle ne l'est pas, affichez-la dans la console. Pour tester si l'erreur est "**vide**", basez-vous sur le fait qu'une erreur est une interface, et comparez la valeur de votre erreur à la zero value d'une interface.

---

### Interface vide (1/3)

On va à présent étudier l'interface vide, et la détermination des types sous-jacents :

```go
interface{}
```

- Écrivez une fonction qui a la signature suivante, et qui se contente d'exécuter fmt.Println() sur l'argument qui lui est passé :
  
  ```go
  func PrintIt(input interface{})
  ```

---

### Interface vide (2/3)

- Essayez d'exécuter cette fonction en passant un entier en argument. (c'est à dire : appelez cette fonction dans votre fonction main et lancez votre programme).
  Que se passe-t-il ?
- Essayez à présent avec une chaîne de caractères. Même question.

---

### Interface vide (3/3)

- L'interface vide est un contrat qui n'a aucune méthode. Donc pour l'implémenter, n'avoir aucune méthode sur un type suffit.
- Tous les types en Go ont 0 ou plus méthodes, donc tous les types vérifient l'interface vide.
- Modifiez à présent votre fonction **PrintIt()** pour qu'elle affiche le type de l'argument passé, et non plus sa valeur. Utilisez le switch vu en cours.
