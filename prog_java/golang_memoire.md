<!-- markdownlint-disable MD041 MD033 MD024 MD026 -->
title:La gestion de la mémoire
intro:nous permettra de voir comment la mémoire est gérée en Go.
conclusion:La gestion de la mémoire.

---

### Garbage collection / Sécurité mémoire

- Pas de gestion directe des allocations mémoire
- Contrôle plus fin possible :
  - On a accès aux pointeurs, ou valeurs.

---

### Exemple 1 :

```golang  
type Example struct{}

func newExample() Example{
  return Example{}
}

func newExamplePointer() *Example{
  return &Example{}
}
```

---

### Exemple 2 :

```golang
package main
import "fmt"

func main() {
 message := "Hello Planet"

 // Pointer to string
 var pMessage *string
...
```

---

### Exemple 2 (suite) :

```golang
...
 // pMessage points to addr of message
 pMessage = &message
 fmt.Println("Message = ", *pMessage)
 fmt.Println("Message Address = ", pMessage)

 // Change message using pointer de-referencing
 *pMessage = "Hello Solar System"
 fmt.Println("Message = ", *pMessage)
 fmt.Println("Message Address = ", pMessage)
}
```

---

### Exemple 2 (suite) :

```shell
$ go run cosmiclearn.go
Message = Hello Planet
Message Address = 0xc04203e1d0
Message = Hello Solar System
Message Address = 0xc04203e1d0
```

---

### Garbage collection / Sécurité mémoire

- Pas de gestion directe des allocations mémoire

- Contrôle plus fin possible

- Sécurité mémoire : attention, les nil pointer exceptions (panics en Go) sont possibles !
