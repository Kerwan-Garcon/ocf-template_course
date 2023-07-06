<!-- markdownlint-disable MD041 MD033 MD024 MD026 -->
title:Concurrence, les channels
intro:nous présentera les channels en Go.
conclusion:Etudié les channels en Go.

---

### Threads en go : channels

- C'est un type ! Un type header.

- Sa zero value est "nil"

- Comme les maps, on les crée avec "make"

  - Rappel :

  ```golang
  make(map[string]string)
  ```

- Pour les channels :

  ```golang
  make (chan [TYPE QUI PASSE DANS LE CHANNEL])
  ```

---

### Threads en go : channels

- Il existe deux types de channels :
  - Buffered channels
  - Non-buffered channels
  
Référence :

<https://golang.org/doc/effective_go.html#channels>

---

### Channels : non-buffered

- Penser à un tuyau, dans lequel on peut envoyer de l'information.

- Cette information est typée à l'avance :

```golang
ch:= make(chan int)
```

Dans ce channel, on ne peut envoyer que des int.

---

### Channels : non-buffered

- Comment écrire de l'information dans un channel ?

```golang
information := 0
ch := make(chan int)

ch <- information
```

---

### Channels : non-buffered

- Comment lire de l'information dans un channel ?

```golang
information := 0
ch := make(chan int)

ch <- information

informationLue := <-ch
```

---

### Channels : non-buffered

- L'écriture et la lecture sont bloquantes.

- Quand on écrit une valeur sur le channel, on ne peut rien faire avec tant que la valeur n'a pas été lue à l'autre bout.

- Même type de raisonnement en lecture

---

### Channels : buffered

- Tout pareil que les non buffered channels, mais on a une file d'attente à l'intérieur du channel

```golang
// On peut mettre jusqu'à 10 int dedans sans bloquer. Leur ordre est préservé.
ch := make(chan int, 10)
```

---

### Select statement

- Comment signaler qu'une goroutine a terminé son travail ?

```golang
func main() {
  ch := make(chan bool)
  go maFonction(ch)
  select {
    case <-ch:
      fmt.Println("j'ai fini !")
    case <-time.After(time.Second):
      fmt.Println("j'ai pas fini, mais tant pis !")
  }
}

func maFonction(ch chan bool) {
  time.Sleep(3*time.Second)
  ch<-true
}
```

---

### wait groups

- Un wait group est une struct, donc un type value (pas un type header !)

- Il sert à attendre qu'un groupe de goroutines aient fini leurs tâches avant d'avancer dans le programme.
  - Il bloque donc le programme.

---

### wait groups

- On commence par le déclarer :

```golang
var wg sync.WaitGroup // La zero value suffit
```

- Puis quand on lance une goroutine (juste avant),
  - on ajoute un incrément au WaitGroup
  - et on le passe en argument de la fonction :

```golang
wg.Add(1)
go maFunc(&wg)
```

---

### wait groups

- Il faut décrémenter ce compteur lorsque la goroutine a fini son execution :

```golang
func maFunc(wg *WaitGroup){
  defer wg.Done()
}
```

- Enfin pour bloquer le programme jusqu'à la fin de l'exécution des goroutines, placer un Wait() à l'endroit choisi :

```golang
func main(){
// Tout le reste ici
wg.Wait() // Cette ligne fait attendre que les goroutines soient achevées
// Suite des instructions
}
```

---

### wait groups

- Référence: <https://golang.org/pkg/sync/#WaitGroup>

---

### Closures

- Ce sont des fonctions "anonymes". Autrement dit elles n'ont pas de nom.

- Elles peuvent accéder à des variables définies en dehors de leur body :

```golang
func main() {
  l := 20
  b := 30
  func() { // C'est une closure
    var area int
    area = l - b
    fmt.Println(area)
    }()
}
```

---

### Closures

On peut directement les lancer dans des goroutines comme cela :

```golang
func main() {
  l := 20
  b := 30
  go func() { // C'est une closure lancée dans une goroutine
    var area int
    area = l - b
    fmt.Println(area)
  }()
}
```
