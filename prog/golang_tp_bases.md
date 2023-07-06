<!-- markdownlint-disable  MD013 MD041 MD033 MD034 MD024 MD026 -->
title:Les bases - TP
intro:nous permettra de tester les concepts de base de Golang.
conclusion:Vu les fonctionnalités de base du langage Golang.

---

### Mise en place de l'environnement

- Sous Linux, vous pouvez installer Golang à partir du gestionnaire de paquets.
- Pour les autres systèmes, ou si la détection échoue, vous pouvez vous rendre sur cette URL et choisir le fichier qui vous correspond :
  - https://golang.org/dl/
- Vous pouvez également suivre la procédure d'installation de Golang à cette adresse :
  - https://golang.org/doc/install
- Une fois que c'est fait, vous pouvez vérifier la version avec la commande :

```shell
go version
```

---

### Enregistrez votre travail

- Utilisez votre compte Gitlab pour y stocker votre code

---

### Hello World

- Reprenez l'exemple du cours, et écrivez votre propre "Hello, World!" en Go. Écrivez-le dans un fichier **main.go**
- Testez votre programme :
  - Soit en le compilant puis en le lançant :

    ```shell
    go build ./...
    ./main.go
    
    ```

  - Soit à la manière d'un "script" :

```shell
go run main.go
```

---

### Installation

- Vous pouvez **installer** votre application sur votre machine avec la commande qui suit, il faut l'exécuter depuis la racine de votre projet :

```shell
go install
```

- Cela va placer votre binaire dans **$GOPATH/bin**. Une fois cela fait, vous pouvez exécuter votre programme depuis n'importe où.

---

### Go fmt

- Testez le formateur de code ”go fmt” en formatant ce code :

```go
   package main
import "fmt"

func main() {
 fmt.Printf( "Bad " )              ;
      fmt.Println("formating"       )
      }
```

---

### Bibliothèque standard

- Nous allons nous servir de quelques packages de la librairie standard, dont la documentation est disponible ici :
  https://golang.org/pkg/

- On va réaliser un petit programme qui compare des images et dit si elles sont identiques.

---

### Bibliothèque standard - Flag

- On va utiliser le package https://golang.org/pkg/flag/ pour permettre d'afficher la version de notre programme.
- Définissez une constante dans le fichier main.go, comme ceci :

```go
const VERSION = "1.0"
```

- Utilisez flag.Bool() et flag.Parse() pour afficher la version du programme lorsque l'on entre :

```shell
$ go run main.go -version
```

---

### Bibliothèque standard - Crypto

- Téléchargez les images disponibles sur l'espace de cours : image_1.jpg, image_2.jpg, image_3.jpg
- Créez un projet img_diff, et initialisez-le avec :

  ```shell
  go mod init img_diff
  ```

---

- Écrivez une fonction qui permet de lire un fichier complètement, et d'en retourner les octets sous forme de **[]byte**.
  Utilisez le package https://golang.org/pkg/io/ioutil/
- Écrivez une fonction qui permet de hasher un fichier donc vous lui passez le path en argument. Cette fonction retourne un **[]byte**.
  Utilisez le package https://golang.org/pkg/crypto/sha256/
- Comparez les hashes des images, et déterminez celle qui est unique.
- Affichez le résultat (le path de l'image unique tel que passé en argument) dans la console.
- Pourriez-vous imaginer une implémentation plus efficace ?
  - Implémentez-la en utilisant https://golang.org/pkg/bufio/

---

### Bibliothèque standard - Logs

- On va ajouter du logging pour les erreurs. Pour cela, utilisez :
  https://golang.org/pkg/log/
- Créez un nouveau logger, et dirigez son output sur **os.Stderr**
  (https://golang.org/pkg/os/#pkg-variables)
- Partagez votre logger dans votre programme, et loguez les erreurs éventuelles.

---

### Bibliothèque standard - Http

- On va ajouter une possibilité de récupérer une liste d'images sur internet et de les comparer.
- Ajoutez un flag, comme pour la version, qui permet de passer une chaine de caractères en argument qui est une liste d'URLs séparées par des
virgules :
  "http://url1.tld,http://url2.tld"
- Ajoutez un parsing de cette chaîne de caractères en utilisant :
  https://golang.org/pkg/strings/
  Votre parser d'argument retourne un **[]string**, qui est une slice d'URLs.

---

- Utilisez **http.Get()** pour télécharger les images. Vous pouvez lire le contenu téléchargé avec :

  ```go
  data, err := ioutil.ReadAll(resp.Body) 

  if err != nil {
    log.Fatal(err)
  }

  defer resp.Body.Close()
  ```

- Ecrivez les images sur le disque avec os.Create
- Vérifiez le bon fonctionnement de votre programme

---

### Bibliothèque standard - Time

- Ajoutez des logs qui permettent de savoir combien de temps mettent les images téléchargées à être récupérées depuis internet.
  Utilisez: https://golang.org/pkg/time/
- Et utilisez: https://golang.org/pkg/fmt/ pour les messages. Formattez avec **%v**.
