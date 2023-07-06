<!-- markdownlint-disable  MD013 MD041 MD033 MD034 MD024 MD026 -->
title:Le JSON - TP
intro:nous permettra de découvrir l'intérêt du JSON en Golang.
conclusion:Appris à utiliser le JSON en Golang.

---

### Parsing pur (1/2)

Soit la structure User suivante :

```go
type User struct {
  Login string
  Password string
}
```

- Afficher dans le terminal le résultat de la sérialisation d'un utilisateur dont le login est "Paul" et le mot de passe "pass123".
- Remplacer le nom du champ "Login" dans le JSON de sortie par "userName" en utilisant les annotations.

---

### Parsing pur (2/2)

- Soit **users.json** un fichier contenant une liste d'utilisateurs au format JSON :

  ```json
  [{"userName": "matm", "Password": "123456"}, {"userName":"fake44", "Password": "azerty"}]
  ```

- Lire puis désérialiser le contenu de ce fichier dans une liste de **User**.
- Que se passe-t-il si vous renommez "Password" en "password" dans votre structure et que vous lancez à nouveau le programme précédent ?

---

### Serveur HTTP - Les données (1/2)

On va écrire dans cette partie un petit serveur HTTP qui permet d'obtenir des information sur des utilisateurs, en passant des query. Voir :
https://en.wikipedia.org/wiki/Query_string

- Ajoutez dans votre fichier users.json un champ "userID" aux utilisateurs de la liste, et donnez lui une valeur différente pour chaque utilisateur.
- Ajoutez dans la struct User définie précédemment un champ permettant de correctement récupérer le user ID lorsque l'on désérialise le JSON du fichier.

---

### Serveur HTTP - Les données (2/2)

- Créez une map, sous forme de "variable globale", que vous remplirez dès que la fonction main sera exécutée en lisant votre fichier JSON.
- Le type de cette map est :

  ```go
  map[string]User
  ```

  avec en clé le user ID et en valeur le user correspondant.
- Pour définir une "variable globale", définissez-la en dehors de toute fonction. Elle est alors accessible dans toutes les fonctions de  votre fichier **main.go**

---

### Serveur HTTP - Fonction Handler (1/3)

- Définissez un **handler** pour votre serveur. Un handler est une fonction qui a cette signature :

  ```go
  func(http.ResponseWriter, *http.Request)
  ```

  avec l'interface:
  https://golang.org/pkg/net/http/#ResponseWriter et la struct :
  https://golang.org/pkg/net/http/#Request
  comme arguments. Laissez le corps de la fonction vide pour l'instant.

---

### Serveur HTTP - Fonction Handler (2/3)

- Définissez un handler dans votre fonction main, en utilisant http.HandleFunc de :
  https://golang.org/pkg/net/http/#HandleFunc
- Utilisez le pattern "/" dans votre appel à cette fonction, et passez lui comme second argument la fonction définie au point précédent.
- Écrivez l'appel qui permettra de lancer votre serveur en utilisant :
  https://golang.org/pkg/net/http/#ListenAndServe

---

### Serveur HTTP - Fonction Handler (3/3)

- Lorsque vous faites une requête http au serveur que nous sommes en train d'écrire, le serveur appelle notre fonction handler. On a accès à la requête envoyée par le client par l'objet ***http.Request** passé en argument de votre fonction de handler, et on peut écrire notre réponse sur le **http.ResponseWriter**.
- Écrivez du code qui vérifie que le user passé dans la requête est bien dans notre map, définie en variable globale. Pour cela, utilisez :
  
  ```go
  id := r.FormValue("id")
  ```

  qui permet de récupérer le champ de query "**id**" dans une requête HTTP de type Get à l'adresse :
  http://localhost:8000/?id=id1

---

### Serveur HTTP - Sérialisation (1/3)

- Si l'id est trouvé, sérialisez le User correspondant en JSON.
  Autrement, ne faites rien.
- Écrivez le header de votre réponse avec :

  ```go
  w.Header().Set( "
    Content-Type",
    "application/json; charset=utf-8",
  )
  ```

---

### Serveur HTTP - Sérialisation (2/3)

- Ajoutez ensuite le code de la réponse avec :
  
  ```go
  w.WriteHeader(http.StatusOK)
  ```

  lorsque le user existe et :
  
  ```go
  w.WriteHeader(http.StatusNotFound)
  ```

  lorsqu'il est introuvable.

---

### Serveur HTTP - Sérialisation (3/3)

- Enfin, écrivez le JSON du User (si votre code réponse n'est pas "Not Found") dans le ResponseWriter avec :
  
  ```go
  Write([]byte) (int, error)
  ```

  de:
  https://golang.org/pkg/net/http/#ResponseWriter

---

### Serveur HTTP - Requête

- Lancez votre serveur, et testez-le avec des requêtes curl comme :

```shell
curl -i http://localhost:8000/?id="id1"
```
