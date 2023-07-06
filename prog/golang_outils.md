<!-- markdownlint-disable MD041 MD033 MD024 MD026 -->
title:Les outils intégrés
intro:nous présentera les outils fournis avec le langage Golang.
conclusion:Les outils intégrés au langage.

---

### Go fmt

- Formatage automatique du code :
  - Aide à la standardisation de l'apparence du code
  - Évite les commits à "+800" parce que quelqu'un a reformaté dans son IDE
  - Permet aux autres outils go de manipuler le code

---

### Go mod

- Vendoring :
  - Permet de s'affranchir de l'ancienne architecture (avec le $GOPATH)
  - Plus besoin de go get
  - Les dépendances sont définies dans un seul fichier "go.mod", et importées dans le code, "par fichier"

---

### Go doc

- Documentation :
  - Est-ce que vous documentez vos scripts ?
  - Possibilité de récupérer de la documentation avec :

```shell
go doc [Symbol]
```

---

### Go test

- Les tests :
  - Est-ce que vous testez vos scripts ?
  - Il suffit de placer ses tests dans des fichiers "\[nom_fichier\]\*test.go", et de nommer les fonctions de test "Test\*\[nom_fonction\]".
  
  - Lancer les tests avec :

```shell
go test ./...
```

---

### Go install

- Installation des paquets :
  - Même chose qu'un package manager classique type npm, cargo, pip...

```shell
go install https://github.com/FiloSottile/age
```
