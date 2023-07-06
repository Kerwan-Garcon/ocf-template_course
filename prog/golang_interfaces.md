<!-- markdownlint-disable MD041 MD033 MD024 MD026 -->
title:Golang, interfaces
intro:nous présentera la gestion des interfaces Go.
conclusion:Compris la gestion des interfaces Go.

---

- C'est quoi une interface ?

- Un contrat :

  - Une liste de méthodes que doit respecter une entité qui l'implémente.

  - Si l'entité implémente ces méthodes, elle est alors aussi du type que cette interface définit.

---

- Une interface permet de grouper des types concrets par fonctionnalité

- Les interfaces peuvent aider pour tester le code

- Une interface est vérifée implicitement :
  - Si toutes les méthodes d'une interface sontimplémentées pour un type, alors ce type vérifie l'interface

---

- Zero value : que se passe-t-il si on execute ce programme ?

```golang
package main

func main() {
  var ifce IfceTest
  ifce.test()
}

type IfceTest interface {
  test()
}
```

=> Panic

---

- La zero value d'une interface est un pointeur nil

- Un pointeur nil, pour rappel, c'est une absence de pointeur.

- Appeler une méthode sur un pointeur nil, c'est appeler une méthode sur "rien"

=> Panic

---

- Comment initialiser l'interface dans notre exemple précédent ?

---

```golang
package main

func main()
{
  var ifce IfceTest
  ifce = Example{}
  ifce.test()
}

type IfceTest interface {
  test()
}

type Example struct{}

func (ex Example)test(){}
```

---

- Si on a besoin de récupérer le type qui se cache sous une interface, au runtime ?

=> C'est possible avec un switch :

```golang
func printType(i interface{}) {
  switch v := i.(type) {
    case int:
      fmt.Println("The type is int !")
    case string:
      fmt.Println("The type is string !")
    default:
      fmt.Printf("I don't know about type %T!\n", v)
    }
}
```

---

<!-- _class: code80 -->

- Comment vérifier qu'une interface "contient" bien un type (sans switch) ?

```golang
package main

import
(
  "log"
)

func main()
{
  var ifce Ifce
  ifce = Example{}
  _, ok := ifce.(Example)
  if !ok {
    log.Fatal("Pas le type attendu !")
  }
}

type Ifce interface {
  test()
}

type Example struct {}

func (ex Example)test() {}
```

---

<!-- _class: code80 -->

- En écriture condensée :

```golang
package main

import "log"

func main() {
  var ifce Ifce
  ifce = Example{}

  if _, ok := ifce.(Example);!ok {
    log.Fatal("Pas le type attendu !")
  }
}

type Ifce interface {
  test()
}

type Example struct {}

func (ex Example)test() {}
```
