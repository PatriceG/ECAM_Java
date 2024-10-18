
# Les Collections

- JavaSE fournit un ensemble de classes permettant de stocker et de trier des ensembles d’objets appelées Collections.
- Ces collections ont évolué au fil des versions de Java, et sont définies dans _java.util_
- Les collections sont définies par une série d'interfaces en décrivant le comportement (Set, List et Map principalement)
- Java dispose de plusieurs implémentations de ces interfaces selon l'usage envisagé.

![Collections](/images/collections.png){.block .mx-auto .w-1/2}

---

# Les Collections

- Collections de type "Java1" **(Obsolètes)**
  - _java.util.Vector, java.util.Hashtable_
  - Accès synchronized (sûr mais lent)
  - A moins d'impératif de compatibilité "Java1" on préfère utiliser les collections "Java2" plus performantes.
  - Non-Typées

- Collections de type "Java2"
  - Hiérarchie de collections adaptées à divers usages
  - Accès non synchronized (rapide mais non protégé des accès concurrents)
  - Possibilité de rendre une collection Java2 synchronized
  - Algorithmes de tri et de recherche disponibles (pré-implémentés)
  - Typées

- De nouvelles API de collections sont apparues avec Java8 (Streams)

---

# Les Collections

- Implémentation des collections disponibles:

<style>
  table {
    border-collapse: collapse;
    border: 1px solid gray;
  }
  th, td {
    border: 1px solid gray;
    padding: 8px;
  }
  th {
    text-align: center;
  }
</style>
<table>
  <tr>
    <th colspan="6" style="text-align: center;">Implémentations</th>
  </tr>
  <tr>
  <td></td>
  <td></td>
  <td><b>Hash Table</b></td>
  <td><b>Resizable Array</b></td>
  <td><b>Balanced Tree</b></td>
  <td><b>Linked List</b></td>
  </tr>
  <tr>
    <th rowspan="3">Interfaces</th>
    <td><b>Set</b></td>
    <td><b>HashSet</b></td>
    <td></td>
    <td><b>TreeSet</b></td>
    <td></td>
  </tr>
  <tr>
    <td>List</td>
    <td></td>
    <td>ArrayList</td>
    <td></td>
    <td>LinkedList</td>
  </tr>
  <tr>
    <td>Map</td>
    <td>HashMap</td>
    <td></td>
    <td>TreeMap</td>
    <td></td>
  </tr>
</table>

- Après avoir choisi le type de collection à utiliser (interface) il faut en choisir l'implémentation la plus performante en fonction de l'utilisation envisagée.
- Les implémentations les plus utilisées sont ArrayList, HashMap et TreeSet

---

# Les Collections
Exemples

- Interface java.util.Collection&lt;T&gt;:
  
```java
boolean add(T o)
boolean addAll(T o)
void clear()
boolean contains(T o)  //retourne true si la collection contient o.
boolean isEmpty()
Iterator<T> iterator() //retourne un itérateur sur la collection.
int size()
```

```java
Interface java.util.List<T> (extends java.util.Collection<T>):
T get(int index)
T remove(int index)
boolean remove(<T> o)
List<T> subList(int fromIndex, int toIndex)
```

---

# Les Collections
Exemples

- Interface java.util.Map&lt;K,V&gt;:

```java
boolean put(K key, V value)
V get(K key)
V remove(K key)
int size()
Set<Map.Entry<K,V>> entrySet() //retourne l’ensemble des entrées de la Map
boolean containsKey(K key)
boolean containsValue(V value)
boolean isEmpty()
Set<K> keySet() //retourne l’ensemble des clef de la Map
Collection<V> values() //retourne l’ensemble des valeurs de la Map
```
---

# Utilisation d'une List avec implémentation ArrayList

````md magic-move {lines: true}
```java
import java.util.List;
import java.util.ArrayList;

//On met le type de l'interface et non celui de l'implémentation
//pour ne pas lier le code à une implémentation donnée d'une collection
java.util.List<String> liste = new java.util.ArrayList<String>();
```

```java
import java.util.List;
import java.util.ArrayList;

//On met le type de l'interface et non celui de l'implémentation
//pour ne pas lier le code à une implémentation donnée d'une collection
java.util.List<String> liste = new java.util.ArrayList<String>();
liste.add("un objet");
liste.add("un autre objet");
```

```java
import java.util.List;
import java.util.ArrayList;

//On met le type de l'interface et non celui de l'implémentation
//pour ne pas lier le code à une implémentation donnée d'une collection
java.util.List<String> liste = new java.util.ArrayList<String>();
liste.add("un objet");
liste.add("un autre objet");

System.out.println("second élément de la liste: " + liste.get(1));
liste.clear();

```
````

---

# Utilisation d'une Map avec implémentation HashMap

````md magic-move {lines: true}
```java
import java.util.Map;
import java.util.HashMap;

//On met le type de l'interface et non celui de l'implémentation
//pour ne pas lier le code à une implémentation donnée d'une collection
java.util.Map<String,String> myMap = new new java.util.HashMap<String,String>();
```

```java
import java.util.Map;
import java.util.HashMap;

//On met le type de l'interface et non celui de l'implémentation
//pour ne pas lier le code à une implémentation donnée d'une collection
java.util.Map<String,String> myMap = new new java.util.HashMap<String,String>();
myMap.put("clef1", "Hello");
myMap.put("clef2", "World");


```

```java
import java.util.Map;
import java.util.HashMap;

//On met le type de l'interface et non celui de l'implémentation
//pour ne pas lier le code à une implémentation donnée d'une collection
java.util.Map<String,String> myMap = new new java.util.HashMap<String,String>();
myMap.put("clef1", "Hello");
myMap.put("clef2", "World");
System.out.println(myMap.get("clef2")); //affiche "World"

myMap.remove("clef2") //enlève l'élément ayant la clé spécifiée 

```
````

---

# Les Collections
Parcours

L'interface _java.util.Iterator&lt;T&gt;_ permet de parcourir une _java.util.Collection&lt;T&gt;_ simplement et éventuellement de modifier celle-ci en cours de parcours.

```java
java.util.List<String> liste = new java.util.ArrayList<String>();
liste.add("un objet");
liste.add("un autre objet");

java.util.Iterator<String> it = liste.iterator();
while(it.hasNext())
{
	String s = it.next(); //on n'a pas besoin de "caster" en String car la collection est typée lors de sa déclaration
	System.out.println(s);
} 
```
---

# Les Collections
Parcours

En pratique on préfère souvent cette structure de boucle for qui permet de parcourir encore plus simplement n'importe quel objet implémentant l'interface _Iterable_

```java
java.util.List<String> liste = new java.util.ArrayList<String>();
liste.add("un objet");
liste.add("un autre objet");

//boucle sur tout le contenu de « liste »
for(String element : liste)
{
	System.out.println(element);
} 
```

**Note:** On n’a plus accès à l’index de l’élément dans la liste. Si on en a besoin on peut passer par une variable locale incrémentée à chaque passage dans la boucle

Exemples en live: https://www.codiva.io/p/956abd10-52a8-4a22-9d3f-b28bc73bfd87

---

# Les Collections
Tri

Il existe plusieurs façons d'obtenir une collection triée:
Soit les objets dans la collection implémentent _java.lang.Comparable_:

```java
java.util.TreeMap
java.util.Arrays.sort(Object[] o)
java.util.Collections.sort(java.util.List list)
```

Soit on développe une classe implémentant _java.util.Comparator_ spécifique, que l'on passe ensuite en argument aux méthodes de tri:

```java
java.util.Collections.sort(java.util.List maListe, java.util.Comparator monComparateur)
```

**On préfère cette seconde option** car elle permet de séparer le critère de tri de l'implémentation des objets, donc de changer facilement de critère de tri.

_Note:_
Le package _java.util.collections_ contient beaucoup d'autres méthodes utilitaires pour remplir, mélanger, trouver les valeurs min et max d'une Collection

---

# Les Collections
Tri

L'interface _java.lang.Comparable_ est implémentée par de nombreux objets Java (String,…)

```java
public interface Comparable
{
/**
* Compare deux objets
* @param o – objet à comparer
* @return entier négatif, zéro, ou positif selon que l'objet courant est
* inférieur, égal, ou supérieur à l'objet spécifié
* @throws ClassCastException – si l'objet spécifé n'est pas du bon type
*/
public int compareTo(Object o) throws ClassCastException;
}
```

---

# Les Collections
Tri

L'interface java.util.Comparator&lt;T&gt; permet de comparer des objets n'implémentant pas _java.lang.Comparable_ en implémentant soi-même l'algorithme de comparaison (l'algorithme de tri est lui déjà implémenté dans le JDK)

```java
public interface Comparator<T>
{
/**
*  Compare deux objets
*  Mêmes règles que java.lang.Comparable.compareTo()
* @return entier négatif, zéro, ou positif selon que l'objet courant est
* inférieur, égal, ou supérieur à l'objet spécifié
* @throws ClassCastException – si l'objet spécifé n'est pas du bon type
*/
public int compare(T o1, T o2);

/**
* Permet de comparer deux "Comparator" entre eux.
* Il n'est généralement pas nécessaire d'implémenter cette méthode
* (l'implémentation par défaut dont on hérite via java.lang.Object est suffisante)
*/
public boolean equals(Object o);
}
```
---

# Les Collections
Tri

La méthode int compare(T o1, T o2) doit être implémentée de sorte que:

- Si o1 est égal à o2, elle retourne 0
- Si o1 est inférieur à o2 elle retourne un nombre négatif (-1 par convention)
- Si o1 est supérieur à o2 elle retourne un nombre positif (+1 par convention)

Le sens de "supérieur" ou "inférieur" dépend du contexte métier:

- Ordre lexicographique pour une chaine de caractères
- Valeur numérique pour un nombre
- Combinaison spécifique de champs pour un objet plus complexe

---

# Les Collections
Tri - Exemple Concret

Soit la classe Point:

```java
public class Point{
	 integer x; integer y;
    ..//constructeur omis
}
```

Voici un Comparateur qui permet de les classer par x croissant:

```java
public class PointXComparator implements Comparator<Point>
{
public int compare(Point p1, Point p2) {
  int r = 0;  //valeur par défaut renvoyée quand p1.x == p2.x
  if(p1.x > p2.x){
    r = 1;  
  }else{
    if(p1.x < p2.x){
      r = -1;  
    }
  }
  return r;
}
}
```

---

# Les Collections
Tri - Exemple Concret

Tri d'une List&lt;Point&gt;

````md magic-move {lines: true}
```java
List<Point> lst = new ArrayList<Point>();
lst.add(new Point(5,6));
lst.add(new Point(2,1));
lst.add(new Point(7,5));
```

```java
List<Point> lst = new ArrayList<Point>();
lst.add(new Point(5,6));
lst.add(new Point(2,1));
lst.add(new Point(7,5));

//tri (la liste lst est triée "en place", sans recopie)
Collections.sort(lst,new PointXComparator());

```

```java
List<Point> lst = new ArrayList<Point>();
lst.add(new Point(5,6));
lst.add(new Point(2,1));
lst.add(new Point(7,5));

//tri (la liste lst est triée "en place", sans recopie)
Collections.sort(lst,new PointXComparator());

//affichage de la liste triée en utilisant un iterator
Iterator<Point> it = lst.iterator();
while(it.hasNext()){
	Point p = it.next();
	System.out.println("Point: "+p.x+","+p.y);
}
//affichage de la liste triée en utilisant for
for(Point p : lst){
  System.out.println("Point: "+p.x+","+p.y);
}
```
````
---

# Les Collections
Fonctionnement de contains(Object o)

- La méthode _contains(Object o)_ invoque _equals()_ sur tous les objets de la collection.
- L’implémentation de _equals()_ n’est correcte que pour les types standards (String, Integer),… son implémentation par défaut dans _Object_ compare les références en mémoire (et non l’identité des objets).
- Pour les types définis par l’utilisateur il est nécessaire de surcharger la méthode _equals()_ si on souhaite pouvoir utiliser la métode _contains()_ de Collection.

```java
public class Point{
    integer x; integer y;

public boolean equals(Object o){
  Point p = (Point) o;
  return (x == p.x && y == p.y) //compare l'identité des deux Points et non leur référence mémoire.
  }
}
```
