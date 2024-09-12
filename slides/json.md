
# JSON
## (JavaScript Object Notation)

- Format de données très populaire
- Sous-ensemble de Javascript
- Interprété ‘nativement’ par les navigateurs
- Facile à lire et à générer
- Auto-descriptif
- Extensible sans casser l’existant

- Nombreuses API disponibles pour le manipuler
- Jackson (la référence)
- Gson, json-java,Json-simple,…
- Les API de JavaEE (JSON-P & JSON-B) sont peu utilisées car trop complexes...

---

# JSON - Exemple de document

![jsondoc](/images/jsondoc.png){style="width: 75%;"}

---

# JSON - Mode d'utilisation

- Il existe une API intégrée au JSDK (JSR 353) mais dans les faits elle est peu utilisée.
- On utilise de préférence des librairies permettant de mapper directement un graphe d’objets Java sur un flux JSON => _Jackson_ en particulier, mais aussi _JSON-Java_ qui est plus simple à utiliser, mais ne dispose pas d’un mode Data Binding bidirectionnel.

- Jackson dispose de 3 modes d’utilisation: 
  - Streaming – Lecture/Ecriture de JSON sous forme d’événements
    - Performant, peu consommateur de ressources, mais parfois complexe à implémenter. Sert surtout de fondation aux autres modes d’utilisation.
  - Tree Model – Fournit une représentation d’un document JSON sous la forme d’un arbre en mémoire.  
    - Option la plus flexible, mais non-typée.
  - Data Binding – Mapping bidirectionnel entre un graphe d’objets java et un flux JSON

---

# Jackson - Tree Model - Lecture

```java
ObjectMapper mapper = new ObjectMapper();
//flux JSON d'exemple
String jsonString = "{\"nom\":\"John Doe\", \"age\":23,\"notes\":[20,15,17]}";

//crée un arbre depuis le flux JSON
JsonNode rootNode = mapper.readTree(jsonString);

//recherche un attribut par son nom
//Tous les nœuds de l'arbre sont de type JsonNode
JsonNode nameNode = rootNode.path("nom");
System.out.println("Nom: "+ nameNode.asText());
 
JsonNode marksNode = rootNode.path("notes");
Iterator iterator = marksNode.elements();
//reste à parcourir l'Iterator pour récupérer les "notes"

```
---

# Jackson - Tree Model - Ecriture

```java
try{ 
   ObjectMapper mapper = new ObjectMapper();

   JsonNode rootNode = mapper.createObjectNode(); //racine
   //ajout élément "notes", enfant du rootNode
   ArrayNode marksNode = ((ObjectNode)rootNode).putArray("notes"); 
   //ajout d'éléments au tableau "notes"      
   ((ArrayNode)marksNode).add(100);
   ((ArrayNode)marksNode).add(90);
   ((ArrayNode)marksNode).add(85);
   //ajout d'attributs     
   ((ObjectNode) rootNode).put("nom", "John Doe");
   ((ObjectNode) rootNode).put("age", 23);
   //sérialisation de l'arbre JSON sur disque
   mapper.enable(SerializationFeature.INDENT_OUTPUT);
   mapper.writeValue(new File("personne.json"), rootNode);

}catch(JsonParseException|JsonMappingException|IOException e){
	e.printStackTrace();
}

```
---

# Jackson - DataMapping - Lecture

- Conversion d’un flux JSON vers un graphe d'objets Java:
- Jackson sait convertir automatiquement les types par défaut (String, Integer, tableaux)

```java
  ObjectMapper mapper = new ObjectMapper();
  try {
		//Lecture d’un flux JSON depuis un fichier
		//On spécifie la classe vers laquelle mapper le flux lu
		User user = mapper.readValue(new File("c:\\personne.json"), User.class);

		//affichage sur la console du flux dé-sérialisé (mappé)
		System.out.println(user);

		//affichage sur la console du flux re-sérialisé
		mapper.enable(SerializationFeature.INDENT_OUTPUT);
		System.out.println(mapper.writeValueAsString(user));

	} catch (JsonGenerationException|JsonMappingException|IOException e) {
		e.printStackTrace();
	}
```
---

# Jackson - DataMapping - Types

![jsontypes](/images/jsontypes.png){style="width: 75%;"}

---

# Jackson - DataMapping - Ecriture

- Conversion d’un objet Java User en flux JSON:

```java
User user = new User();
ObjectMapper mapper = new ObjectMapper();
try {
    //sérialise l'object User en un flux JSON dans un fichier
    mapper.writeValue(new File("/tmp/user.json"), user);

    //affiche le flux sur JSON la console, indenté
    mapper.enable(SerializationFeature.INDENT_OUTPUT);
    System.out.println(mapper.writeValueAsString(user));
	} catch (JsonGenerationException|JsonMappingException|IOException e) {
		e.printStackTrace();
	} 

```

---

# Jackson - Support de JSON non-standard

- Par défaut Jackson se conforme strictement à la norme JSON.
- Cependant comme beaucoup de flux JSON ne respectent pas cette norme, certaines fonctionnalités sont activables afin de lui faire accepter de tels flux.

La liste de ces fonctionnalités est consultable ici:
http://wiki.fasterxml.com/JacksonFeaturesParser

---

# JSON-Java - Lecture

```java
//flux JSON d'exemple
String jsonString = "{\"nom\":\"John Doe\", \"age\":23,\"notes\":[20,15,17]}";

//crée un arbre depuis le flux JSON
JSONObject root = new JSONObject(jsonString)

//recherche un attribut par son nom, selon le type de Noeud
String nom = root.getString("nom");
int age = root.getInt("age");
JSONArray notes = root.getJSONArray("notes");
```

---

# JSON-Java - Tableaux

```java
JSONArray notes = root.getJSONArray("notes");

//iterator() est non-typé, il faut donc utiliser un opérateur 
//de cast dans la bouche
Iterator<Object> notesIt = notes.iterator();
while (notesIt.hasNext()) {
	JSONObject note = (JSONObject) notesIt.next();
    System.out.println(note.getInt());
}

```

---

# JSON-Java - Création d'objet

```java
//crée un objet simple (pas un JSONArray)
JSONObject adresse = new JSONObject();

//ajoute un attribute de type String	
adresse.put("ville", "Bruz");

//Ajoute l’objet créé à son parent, avec un attribut adresse
root.put("adresse",adresse)
```

_Note:_ Si on crée un _JSONArray_ alors la méthode _put_ à utiliser pour y ajouter des éléments ne prend qu’un seul argument: l’élément à ajouter au tableau

---

# JSON-Java - Ecriture

```java
try{
    FileWriter fw = new FileWriter("monFichier.json");
    //sérialise le flux json vers le writer fw
    //avec une indentation de 4 espaces par niveau
    root.write(fw,4,0);
    fw.close();
}catch(JSONException | IOException e){
   e.printStackTrace();
}
```

Une excellente introduction à JSON-Java:
https://www.baeldung.com/java-org-json

