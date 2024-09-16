
# Les Exceptions

- Qu’est-ce qu ’une Exception

- Comment lever une Exception

- Comment intercepter une Exception

- Comment déclarer et propager une Exception

- Comment créer une classe d'Exception

---

# Qu'est-ce qu'une Exception?

- Hiérarchie de classe permettant la gestion d'erreurs

- Permet  d’isoler une section de code du traitement des erreurs qu’elle peut générer.
  - Le code "fonctionnement nominal" devient beaucoup plus clair
  - Le code de gestion des cas exceptionnels est isolé dans des blocs _catch_

---

# Qu'est-ce qu'une Exception?


![Exceptions](/images/exceptions.png){.block .mx-auto .w-1/2}

- On doit traiter (catch) ou propager (throws) les Checked Exceptions

- Les _UnChecked Exceptions_ sont généralement des erreurs techniques irrécupérables. On n'est pas obligé de les traiter explicitement (elles remontent jusqu'à la JVM et provoquent un plantage).

---

# Lancer une Exception (throw)

```java
int calcule_prix (int nombre, int prix_u) throws NombreNegatifException {
    if (prix_u < 0 || nombre < 0) {
        // nombre ou prix est négatif on lance une	
        // NombreNegatifException
        throw new NombreNegatifException();
    }
    return prix_u * nombre;
}
```

----

# Traiter les Exceptions (catch)

- Dès qu’une anomalie survient, on est dérouté de l’exécution normale.
- Si anomalie en calcule_prix(), alors c() n’est pas appelée.

```java
try {
	a();
	calcule_prix(); //code susceptible de lever une exception
	c();
}
catch(NombreNegatifException e1){
//traitement spécifique à NombreNegatifException
}
catch (Exception e) {
	// traitement des autres types d'Exceptions
   e.printStackTrace(); //ne JAMAIS mettre un bloc catch vide!
}
finally{
//ce bloc est toujours exécuté. Qu'il y ait eu ou non une exception de levée
//on l'utilise souvent pour fermer des ressources proprement (fichiers, connexions,…)
//Il est aussi exécuté s'il y a une instrution return dans le bloc try/catch.
}
```

- Soit on traite l'exception dans un bloc _catch_, soit on la propage à l'appelant via une clause **throws** dans la signature de la méthode.

---

# Traiter les Exceptions (catch)

- Le _Multi-Catch_, apparu avec Java7
- Permet d'éviter d'avoir à dupliquer le code des blocs catch répondant à des exceptions différentes mais au comportement identique.

```java
try{
...
}catch(JsonParseException|JsonMappingException|IOException e){
	e.printStackTrace();
}
```

---

# Une levée d'Exception implique

- Recherche dans la méthode courante un bloc catch acceptant cette exception.
- Si aucun bloc catch n'est trouvé et que l'exception est une sous-classe de _java.lang.Exception_, alors la méthode doit déclarer l'exception dans sa clause _throws_, et l'exception est propagée à l'appelant.

- L'appel à une méthode pouvant lever une exception doit :
  - soit être contenu dans un bloc _try/catch_
  - soit être situé dans une méthode propageant (_throws_) cette classe d'exception
    
---

# Exceptions
Le mécanisme

<!-- ![Exceptions](/images/exceptions_anim.png){.block .mx-auto } -->

<div class="grid grid-cols-[33%_33%_33%] gap-none">
<div>
```java
Object getContent()
{
  try{
    openConnection();
  }
  catch(IOException e) {
    e.printStackTrace();
  }
  finally{
    //libère ressources
    ...
  }
  
  ...
  
}
```

</div>
<div>
```java
void openConnection() throws IOException {
  openSocket();
  sendRequest();
  receiveResponse();
}

```

</div>
<div>

```java
void sendRequest() throws IOException {
  write(header);
  write(body); // ERROR
}

```

</div>
</div>
