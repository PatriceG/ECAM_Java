
# Les I/O

- Deux familles de classes gérant les I/O
  - Les _Streams_ pour les flux d’octets (flux binaires en particulier)
  - Les _Readers/Writers_ pour les flux de caractères (support des différents encodages ISO).

- Il existe aussi les _non-blocking I/O_ avec le package _java.nio_ 
  - meilleure tenue en charge, manipulations de gros fichiers, copie de fichiers simplifiée,…
  
- Ainsi que les _streams_

De nombreuses classes adaptées à chaque besoin.

---

# Les I/O - Flux binaires

- _OutputStream/InputStream_
  - Flux sortant/entrant, opérations de base (lecture/écriture d’octets et de tableaux d’octets)
- _ByteArrayOutputStream/ByteArrayInputStream_
  - Flux sortant/entrant dans/depuis un tableau d’octets
  - Utile pour les I/O en mémoire (tampons, etc…)
- _FileOutputStream/FileInputStream_
  - I/O sur fichier
  - Ecriture/Ajout à un fichier
- _BufferedOutputStream/BufferedInputStream_
  - Ajoute un tampon d’écriture ou de lecture à un flux existant
  - Améliore les performances
- _File_
  - Représente un fichier ou un répertoire
  - Suppression, calcul de taille de fichier, droits d’accès,…

---

# Les I/O - flux binaires

  ![streams](/images/streams.png){.block .mx-auto .w-2/3}

---

# Ecriture dans un fichier binaire

```java
import java.io.* //* non recommandé mais c'est plus compact...
//flux d'I/O sur un fichier
FileOutputStream fos = new FileOutputStream("monFichier.txt");
//on ajoute un buffer (perfs)
BufferedOutputStream bos = new BufferedOutputStream(fos);//on a besoin d'une méthode println
PrintStream ps = new PrintStream(bos); //deprecated
ps.println("Hello World");
//on ferme le dernier flux obtenu (ça va fermer les autres automatiquement)
ps.close(); 

```

Ces opérations sont susceptibles de lancer une _IOExceptions_ ➩ besoin d'un block try/catch adapté ou d'une clause throws

---

# Lecture d'un fichier binaire en mémoire
Méthode _moderne_ avec java.nio

```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.io.IOException;
//...
Path path = Paths.get("chemin/vers/fichier.png");
byte[] fileBytes = Files.readAllBytes(path);
//utilisation de fileBytes...

```

---

# Les IO - Flux de caractères

- _Writer/Reader_
  - Flux sortant/entrant, opérations de base (lecture/écriture de caractères et de tableaux de caractères)
- _StringReader/StringWriter_
  - Flux sortant/entrant dans/depuis une chaîne de caractères
  - Utile pour les I/O en mémoire (tampons, etc…)
- _FileWriter/FileReader_
  - I/O sur fichier
  - Ecriture/Ajout à un fichier
- _PrintWriter_
  - Ecriture de chaînes de caractères
- _BufferedWriter/BufferedReader_
  - Ajoute un tampon d’écriture ou de lecture à un flux existant
  - Améliore les performances

---

# Les IO - Flux de caractères

- _OutputStreamWriter/InputStreamReader_
  - Conversions entre un flux binaire et un flux de caractère
  - Application de l’encodage de caractères par défaut de la plateforme au flux binaires
  - Utile pour utiliser les anciennes API du JSDK avec une gestion correcte des caractères (lecture/écriture sur la console et flux réseau notamment)

---

# Les I/O - Flux de caractères

  ![streams](/images/writers.png){.block .mx-auto .w-2/3}

---

# Ecriture dans un fichier texte

```java
import java.io.*
//flux d'I/O sur un fichier
FileWriter fw = new FileWriter("monFichier.txt");//on ajoute un buffer
BufferedWriter bw = new BufferedWriter(fw);
PrintWriter pw = new PrintWriter(bw);//on a besoin d'une méthode println 
//et elle n'est que dans cette classe
pw.println("Hello World");
//on ferme le dernier flux obtenu (cela ferme automatiquement les autres)
pw.close();
```

Ces opérations sont susceptibles de lancer une _IOExceptions_ ➩ besoin d'un block try/catch adapté ou d'une clause throws

---

# Lecture sur la console 
(a.k.a) 'Entrée Standard'

- méthode 1
  
```java
Scanner scanIn = new Scanner(System.in); //java.util.Scanner
String ligne= scanIn.nextLine();
scanIn.close();            
System.out.println("ligne saisie: " + ligne);
```

- méthode 2: (moderne)

```java
Console console = System.console();
//On peut afficher un prompt avant la saisie:
String ligne = console.readLine("%s", "Saisir quelque chose: ");
System.out.println("ligne saisie: " + ligne);
```

---

# Lecture d'un fichier texte en mémoire
Méthode _moderne_ avec java.nio

````md magic-move {lines: true}
```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.io.IOException;
//...
Path path = Paths.get("chemin/vers/fichier.txt");
//chargement d'un bloc de l'intégralité du fichier
List<String> lines = Files.readAllLines(path);
for (String line : lines) {
  System.out.println(line);
}

```

```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.io.IOException;
//...
Path path = Paths.get("chemin/vers/fichier.txt");
//chargement d'un bloc de l'intégralité du fichier
List<String> lines = Files.readAllLines(path);
for (String line : lines) {
  System.out.println(line);
}

//ou, plus performant (chargement progessif via un Stream):
try (Stream<String> lines = Files.lines(path)) {
  lines.forEach(System.out::println); 
} catch (IOException e) {
  e.printStackTrace();
}

```
````
