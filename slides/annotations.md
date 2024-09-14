# Les Annotations

- Apparues avec Java 5

- Utilisations multiples
  - Générer de la documentation
  - Ajouter des métadonnées détectées par le compilateur
  - Générer du code automatiquement (données persistantes, Web Services)
  - Ajouter un comportement au code (Aspect: Log, Transactions, Sécurité)

- Utilisées par de nombreuses API
  - JPA,JAXB,JAX-WS,JAX-RS,...

- On peut créer ses propres annotations

---

# Les Annotations

- Marquage de code

  - _@Deprecated_ : alertes de code déprécié
  - _@Override_ : vérifie la présence d'une méthode surchargée et empêche la compilation si ça n'est pas le cas
    - Très utile pour éviter des fautes de frappe dans la signature d'une méthode et des bugs difficiles à diagnostiquer
  - _@SuppressWarnings("unchecked")_ : supprime alertes du compilateur

---

# Les Annotations

- Déclaration de services Web _code-first_ avec JAX-WS

```java
@WebService //déclare que cette classe est exposée sous forme de web service
public class AddNumbersImpl {
  @WebMethod(action="addNumbers") //méthode exposée
  public int addNumbers(int number1, int number2) 
      throws AddNumbersException {
    if (number1 < 0 || number2 < 0) {
      throw new AddNumbersException("Negative number cant be added!", "Numbers: " + number1 + ", " + number2);
    }
    return number1 + number2;
  }
}
```

**Configure automatiquement la pile SOAP et expose le service**

---

# Les Annotations

- Gestion de la persistance en base de données (JPA)
  - Déclaration du mapping objet-relationnel
    - JPA va ensuite automatiquement générer le SQL
      - Pour créer la base de données
      - Pour accéder aux données (traduction automatique de JQL en le SQL de la BDD configurée)

```java  
@Entity //entité persistante
@Table(name="PERSONNE")
public class Personne implements Serializable {
@Id //Clé primaire
@GeneratedValue //type de clé
private int id;
@Column(name="PRENOM") //redéfinition du nom de colonne
private String prenom;
@Column(name="NOM") 
private String nom;
@Lob //objet binaire
@Basic(fetch = FetchType.LAZY) //type de chargement
private JPEG photo;
}
```
