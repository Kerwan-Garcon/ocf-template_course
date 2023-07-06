<!-- markdownlint-disable MD041 MD033 MD024 MD026 -->
title:Golang, JSON
intro:nous présentera l'utilisation du JSON en Go.
conclusion:Etudié comment utiliser le JSON en Go.

---

### Parsing JSON

- JSON est très utilisé dans les APIs de nos jours, et existe aussi dans les logs

- Parsing intégré dans la librairie standard

- Parsing se fait par annotations

---

### Parsing JSON

- Exemple de données au format JSON :

```json
{
   "menu":{
      "id":"file",
      "value":"File",
      "popup":{
         "menuitem":[
            { "value":"New", "onclick":"CreateNewDoc()" },
            { "value":"Open", "onclick":"OpenDoc()" },
            { "value":"Close", "onclick":"CloseDoc()"}
         ]
      }
   }
}
```

---

### Parsing JSON

- Quels avantages ?

  - Simple à manipuler pour un programmeur
  
  - Lisible par un humain, léger pour les machines
  
  - "Facile à apprendre" parce que la syntaxe n'est pas extensible
  
---

### Parsing JSON

- Quels inconvénients ?

  - Syntaxe non extensible (contrairement à du XML par exemple)
  
  - Le typage limité affaiblit la sécurité
  
  - On ne peut pas toujours commenter du JSON (dépend du parser)
  
---

### Parsing JSON

- Comment générer du JSON ?

  - Créer une struct qui correspond aux données que l'on veut en sortie
  
  - Utiliser des annotations si nécessaire
  
  - Utiliser  <https://golang.org/pkg/encoding/json/#Marshal>
  
---

<!-- _class: code80 -->

- Qu'affiche ce programme ?

```golang
package main

import (
    "encoding/json"
    "fmt"
)
func main() {
  group: = ColorGroup {
    ID: 1,
    Name: "Reds",
    Colors: []string {"Crimson", "Red", "Ruby", "Maroon"},
  }
  b, err := json.Marshal(group)
  if err != nil {
    fmt.Println("error:", err)
  }
  fmt.Println(string(b))
}

type ColorGroup struct {
    ID int
    Name string
    Colors []string
}
```

```json
{"ID":1,"Name":"Reds","Colors":["Crimson","Red","Ruby","Maroon"]}
```

---

### Parsing JSON

- Comment déserialiser du JSON ?

  - Créer une struct qui correspond aux données que l'on veut lire
  
  - Utiliser des annotations si nécessaire
  
  - Utiliser <https://golang.org/pkg/encoding/json/#Unmarshal>
  
---

<!-- _class: code70 -->

```golang
package main

import (
  "encoding/json"
  "fmt"
)

func main() {
  var jsonBlob = []byte(
    `[
        {"Name": "Platypus", "Order": "Monotremata"},
        {"Name": "Quoll", "Order": "Dasyuromorphia"}
    ]`)

  var animals []Animal
  err := json.Unmarshal(jsonBlob, &animals)
  if err !=nil {
    fmt.Println("error:", err)
  }  
  fmt.Printf("%+v", animals)
}

type Animal struct{
  Name string
  Order string
}
```

---

### Parsing JSON

Importants : seuls les champs exportés seront serialisés/déserialisés.

---

### Parsing JSON

#### Les annotations

- Les annotations permettent de redéfinir les noms des champs entre JSON et struct.

- Exemple :

```golang
type Animal struct {
  Species string `json:"Name"`
  Od string `json:"Order"`
}
```

---

- On peut aussi spécifier que l'on ne veut pas que des champs vides soient ajoutés dans les JSON générés :

```golang
type Animal struct {
  Species string `json:",omitempty"`
  Order string
}
```

---

<!-- _class: code80 -->

Qu'affiche ce programme ?

```golang
package main
import (
    "encoding/json"
    "fmt"
)

func main() {
  type ColorGroup struct {
    ID int
    Name string
    Colors []string `json:",omitempty"`
  }
  group: = ColorGroup {
    ID: 1,
    Name: "Reds",
  }
  b, err := json.Marshal(group)
  if err != nil {
    fmt.Println("error:", err)
  }
  fmt.Println(string(b))
}
```

---

Résultat :

```json
{"ID":1,"Name":"Reds"}
```
