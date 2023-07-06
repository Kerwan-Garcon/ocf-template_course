<!-- markdownlint-disable MD041 MD033 MD024 MD026 -->
title:Golang, les Maps
intro:nous présentera la structure de données map en Go.
conclusion:Découvert les maps en Go.

---

### Les maps

- C'est un header type

- Syntaxe :

```golang
map[Type_des_clés]Type_des_values
```

- Initialiser avec des valeurs :

```golang
m := map[string]int{"bob": 5}
```

- Initialiser vide :

```golang
m := make(map[string]int)
```

---

### Les maps

- Récuperer des valeurs (attention, si la clé est absente, on récupère la zero-value du type des valeurs !) :

```golang
valeur := m["bob"]
```

- Bonus : Récupérer une valeur et faire quelque chose si et seulement si la clé était présente :

```golang
if val, ok := m["bob"]; ok {
  //do something here
}
```
