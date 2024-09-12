
# Java DataBase Connectivity - Objectifs

- Fournir un accès _homogène_ aux SGBDR
- Abstraction des SGBDR cibles
- Requêtes SQL
- Simple à mettre en œuvre

C'est la fondation utilisée par les API de plus haut niveau utilisées généralement (JPA, Spring Data JPA)

---

# JDBC - Fonctionnement

![jdbc_archi](/images/jdbcarchi.png){style="width: 75%;"}

---

# JDBC - Communication avec la BDD

![jdbc_archi2](/images/jdbcarchi2.png){style="width: 75%;"}

---

# JDBC - Accès aux Données

- Charger le driver
  
- Connexion à la base
  
- Création d'un _statement_

- Exécution de la requête

- Lecture des résultats

---

# JDBC - Chargement du Driver

Utiliser la méthode _forName_ de la classe Class :

```java
//chargement driver mySQL par le ClassLoader
//le nom de classe à utiliser est donné par la doc du driver 
//(fichier .jar fourni par l'éditeur de la BDD, qu'on a mis dans le CLASSPATH)
Class.forName("com.mysql.cj.jdbc.Driver");
```

---

# Connexion à la base de données

- Accès via un URL qui spécifie :
  - l'utilisation de JDBC
  - le driver ou le type du SGBDR
  - l'identification de la base
- Exemples d’URL de connexion :
  - _jdbc:mysql://serveur/ma_base_, _jdbc:localhost/codepost_ 
- Ouverture de la connexion :
  
```java
import java.sql.Connection
Connection conn = DriverManager.getConnection(url, user, password);
```

_Note:_ Toutes les méthodes d'accès à JDBC sont susceptibles de lever une _java.sql.SQLException_, il faut donc soit les traiter dans un bloc try/cach, soit les propager via une clause throws.

---

# Création d'un _Statement_

- 3 types de statements :
  - _statement_ : requêtes simples
  - _prepared statement_ : requêtes précompilées
  - _callable statement_ : procédures stockées
  
- On préfère les _prepared statements_ aux _statements_, plus performants (pré-compilation) et plus sûrs (protégés contre l'injection SQL)

---

# Requêtage

- On spécifie le SQL au moment de la création du _PreparedStatement_. Le SQL est alors précompilé par le Driver.
- Puis on spécifie la valeur des paramètres (indiqués par "?") au moment d'effectuer la requête

```java
//conn est la connexion obtenue précédemment
//la requête a un paramètre, indiqué par ?, valorisé plus tard
PreparedStatement prepStmt = conn.prepareStatement("select * from villes where code_postal like ?");
```

---

# Exécution d'une requête

- 3 types d ’exécutions :
  - _executeQuery_ : pour les requêtes qui retournent un ResultSet
  
  - _executeUpdate_ : pour les requêtes INSERT, UPDATE, DELETE, CREATE TABLE et DROP TABLE
  
  - _execute_ : pour quelques cas rares (procédures stockées)

---

# Exécution d'une requête

- Exécution d'une requête pré-compilée:

```java
//prépare la requête
PreparedStatement prepStmt = conn.prepareStatement("select * from villes where code_postal like ?");

//valorise les paramètres selon leur type et leur position dans la requête (le premier a le N° 1)
prepStmt.setString(1,"35000"); 
//effectue la requête de sélection => ExecuteQuery()
ResultSet rs = prepStmt.executeQuery();
//parcours les résultats
```
---

# Lecture des Résultats

- _executeQuery()_ renvoie un _ResultSet_
  
- Le RS se parcourt itérativement rangée par rangée (row)
  
- Les colonnes sont référencées par leur numéro ou par leur nom
  
- L'accès aux valeurs des colonnes se fait par les méthodes _getXXX()_ où XXX représente le type de l'objet
  
- Pour les très gros rows, on peut utiliser des streams.

---

# Lecture des Résultats

```java
PreparedStatement prepStmt = conn.prepareStatement ("select * from villes where code_postal like ?");
prepStmt.setString(1,"35000"); // valorise le premier paramètre
ResultSet rs = prepStmt.executeQuery(); //rs est un "curseur" initialemnt placé juste avant le premier résultat retourné
while (rs.next()) // pour chaque ligne de résultat
{
   // affiche les valeurs de la ligne courante
    int id = rs.getInt("id"); 
    String nom = rs.getString("nom");
    
    System.out.println("id: " + id + " , nom: " + nom);
}
rs.close(); //ne pas oublier de fermer le ResultSet
conn.close(); //et la connexion
```

---

# Requête de Modification de données

```java
PreparedStatement prepStmt = conn.prepareStatement ("UPDATE Table1 SET b=? WHERE b=?");
prepStmt.setString(1,"nouvelle valeur");
prepStmt.setString(2,"ancienne valeur");

int r = prepStmt.executeUpdate(); 
System.out.println("Nombre d'enregistrements modifiés: "+r);
prepStmt.close();
conn.close();
```

---

# Requête de Suppression de données

```java
PreparedStatement prepStmt = conn.prepareStatement ("DELETE FROM Table1 where b=?");
prepStmt.setString(1,"valeur");

int r = prepStmt.executeUpdate(); 
System.out.println("Nombre d'enregistrements supprimés: "+r);
prepStmt.close();
conn.close();
```

----

# Exemple Complet

```java
package fr.ecam.jdbc;
import java.sql.*;

public class TestJDBC {
	public static void main(String[] args) throws Exception {
		Class.forName("com.mysql.cj.jdbc.Driver");
		Connection conn = DriverManager.getConnection("jdbc:mysql://mon_serveur/ma_base","monlogin", "monpassword");
	    PreparedStatement prepStmt = conn.prepareStatement("SELECT * from villes where nom=?");
        prepStmt.setString(1,"35000"); //valorise le 1er paramètre
        ResultSet rs = prepStmt.executeQuery();
		while (rs.next()) { //boucle affichant toutes les villes ayant le code postal spécifié dans la requête
			String nom = rs.getString("nom");
            String nom = rs.getString("code_postal");
			String id = rs.getInt("id");
   		    String code_postal = rs.getString("code_postal");
            System.out.println("id: " + id + " , nom: " + nom + " , code_postal: " + code_postal);
 		}
	}
}

```

---

# Gestion des Transations

- Les connexions sont en auto-commit par défaut (commit automatique après chaque requête SQL)

- Lors de mises à jour faisant intervenir plusieurs tables il est nécessaire que toutes le mises à jour s’effectuent dans une même transaction afin de garantir l’intégrité des mises à jour en cas d’échec de l’une d’elles.

- La gestion des transaction (validation, annulation) se fait de manière explicite avec JDBC.

- Avec l’API JTA de JavaEE, il est possible d’avoir:
  - Une transaction faisant intervenir plusieurs sources de données simultanément (commit à deux phases).
  - Une gestion déclarative des transactions via des annotations.

---

# Gestion des Transations

```java
Connection conn = null;
try{
  conn = DriverManager.getConnection(url, user, password);

  conn.setAutoCommit(false);
  //requêtes de type insert/update
  …
  //validation 
  conn.commit();
}catch(SQLException e){
	//annulation en cas d’erreur
	conn.rollback();
}
```
