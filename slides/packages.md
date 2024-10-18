
# Les Packages

- Les packages permettent de regrouper les classes par domaine fonctionnel ou technique.
- Un package regroupe un ensemble de classes sous un même espace de nommage, empêchant ainsi les conflits de noms de classes.
- Les noms des packages suivent un schéma hiérarchique: _name.subname..._
- Une classe _Watch_ appartenant au package _time.clock_ doit se trouver dans le fichier _time/clock/Watch.java_
- Les packages permettent au compilateur et à la JVM de localiser les fichiers contenant les classes à charger.
- L'instruction _package_ indique à quel package appartient la ou les classe(s) de l'unité de compilation (le fichier).

---

# Les Packages

- Les répertoires contenant les packages doivent être présents dans la variable d'environnement **CLASSPATH**
- En dehors du package (sans directive import), les noms des classes doivent être indiqués sous leur forme complète: _packagename.subpackagename.ClassName_ (java.io.File)
- L'instruction _import packagename.*_ permet d'utiliser des classes sans les préfixer par leur nom de package.
- Les API du JDK sont organisées en packages (java.lang, java.io, ...)

---

# Les Packages
## Le CLASSPATH

- La variable d’environnement **CLASSPATH** indique à la JVM les chemins pour la recherche des classes (répertoires parents de la racine des packages).
- Si les classes sont dans un fichier _.jar_ alors il faut mettre **le chemin complet du fichier** dans le CLASSPATH.
- Exemples:
  - Classe _fr.ecam.Test_ située dans le répertoire _/home/ecam/_
  - Le CLASSPATH doit contenir _/home/ecam/_

  - Classe _fr.ecam.Test_ située dans un fichier _Test.jar_ dans _/home/ecam/_
  - Le CLASSPATH doit contenir le fichier _/home/ecam/Test.jar_ (et non le dossier dans lequel il est situé)

---

# Les Packages
## Le CLASSPATH

- Le réflexe à avoir
  - Une **java.lang.NoClassDefFoundError** indique un problème de CLASSPATH (vérifier qu'il contient bien tous les .jar de l'application et le dossier parent des éventuels fichiers .class)
  - Cette erreur est très fréquente lors du déploiement d’applications Java.
