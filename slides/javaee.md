
# Java Serveur: JavaEE (Jakarta EE)

- Concepts essentiels de JavaEE

- Structure d’une application web

- Présentation des servlets

- Cycle de vie des servlets

- Développement de Servlets

- Présentation des JavaServer Pages

- Servlets et JSP ensemble
  
---

# Concepts JavaEE

- Une webapp est une application Java accessible via le web
- Elle s’exécute dans un environnement appelé serveur d’application
- Un serveur d’application est un environnement d’exécution offrant des services tels que:
  - Gestion des protocoles de communication (HTTP,...)
  - Gestion centralisée des connexions aux bases de données et des transactions
  - Services d’authentification des utilisateurs
  - Gestions de composants (pool d’objets, transactions) à travers les Enterprise Java Beans

- Serveurs d’applications courants:
  - Glassfish (implém de référence), JBOSS (open-source), Apache Tomcat (implém partielle: seulement Servlets & JSP)
  - IBM Websphere (commercial)


---

# Présentation des Servlets

- Les servlets permettent la communication entre la logique applicative et les clients HTTP au sein des applications Web.

- Un moteur de servlets (conteneur de servlets) masque au développeur de servlets les aspects de connexion au réseau, à la récupération des requêtes utilisateur et à la formulation des requêtes.

- Composants au cycle de vie et à l'API spécifique

---

# Structure d'une webapp

- Une webapp est packagée dans un format standard, le .war (web archive).
- Ce format est utilisé pour le packaging et le déploiement mais rarement pour l’exécution (décompression automatique par le serveur d’application)

- Structure définie par la spec servlets:

<pre>
<b>fichiers .html, .jsp, images, etc,</b> accessibles via un navigateur
<b>META-INF</b>
    <b>manifest.mf</b> (fichier décrivant le contenu du .war)
<b>WEB-INF</b> (on met dans ce répertoire tout ce qui 
ne DOIT PAS être visible depuis le web)
  <b>classes</b>   (toutes les classes de l'application ont comme base 
  ce répertoire, il est automatiquement ajouté au classpath)
  <b>lib</b> (répertoire contenant les .jar utilisés par l’application,
  automatiquement ajoutés au classpath lors du démarrage de l’application)
  <b>web.xml</b> (descripteur de déploiement de la webapp)
</pre>

---

# Descripteur de déploiement

- Le descripteur de déploiement est lu au démarrage de la webapp.
- Ses éléments sont accessibles via l'objet ServletContext

- Eléments essentiels du fichier web.xml:

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE web-app
  PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
  "web-app_2_3.dtd">
<web-app>

<servlet>
    <!-- nom logique -->
    <servlet-name>codePostServlet</servlet-name>
    <!-- nom complet de la classe -->
    <servlet-class>codepost.CodePostServlet</servlet-class>
    <!-- params de config propres à la servlet -->
    <init-param>
     <param-name>unParam</param-name>
     <param-value>uneValeur</param-value>
    </init-param>
</servlet>
```

---

# Descripteur de déploiement

- Eléments essentiels du fichier web.xml:

```xml
<servlet-mapping>
    <!-- nom logique (fait le lien avec le code correspondant)-->
    <servlet-name>codePostServlet</servlet-name>
    <!-- chemin d'accès -->
    <url-pattern>/CodePostServlet</url-pattern>
</servlet-mapping>
<session-config>
   <!-- timeout de session en secondes -->
  <session-timeout>60</session-timeout>
</session-config>
<!-- parms de config globaux à toutes les servlets de la webapp -->
<context-param>
     <param-name>db_user</param-name>
     <param-value>eleve</param-value>
</context-param>
</web-app>
```

- on peut aussi y définir la méthode d'authentification, les ressources protégés, les pages d'erreur, etc...

---

# Les Servlets

- Une fois chargée dans le moteur de servlets, une servlet continue de s’exécuter et attend d’autres requêtes provenant du client.

- Une servlet peut créer et retourner une page Web HTML en réponse à la requête envoyée par le poste client (navigateur), mais c'est déconseillé car peu maintenable. On préfère lui faire générer du json ou un flux binaire (streaming, graphiques, etc…)

- Une servlet peut aussi communiquer avec d’autres ressources du serveur (objets applicatifs, objets métiers, objets d ’accès aux données)

---

# Les Servlets

<div style="display: flex; align-items: flex-start;">
  <div style="flex: 1;">
  
- Une servlet est chargé dans la mémoire du serveur d'app (init()) :
  - automatiquement au démarrage du serveur 
  - à la première sollicitation de la servlet 

- Le client envoie une requête HTTP :
  - le serveur crée un objet HttpServletRequest
  - ainsi qu ’un objet HttpServletResponse
  - le serveur appelle la méthode service() de la servlet en transmettant 
  - les objets request et response, de types: javax.servlet.http.HttpServletRequest et javax.servlet.http.HttpServletResponse.

- Lorsque la servlet n ’est plus utile, le serveur appelle la méthode destroy().

_A noter_ : une servlet **n’a pas de méthode main**

  </div>
  <div style="flex: 0 0 220px;">
    <img src="/images/servlets_cycle.png" alt="Image" style="width: 220px; margin-left: 10px;" />
  </div>
</div>

---

# Les Servlets

- Le moteur de servlets utilise un _pool de threads_.
- A chaque requête effectuées par un utilisateur, le conteneur de servlets associe cette dernière à un thread puis la méthode _service()_ est appelée.

**Attention:** on n'a qu'_une seule instance d'une servlet donnée_, accédée par les différents threads déclenchés par les clients. 
Une servlet doit donc être **thread safe** (pas de variables d'instances spécifiques à un client en particulier)

- Si la servlet doit effectuer des traitements d’initialisation, le code doit se trouver dans la méthode _init()_ de la servlet.

- Les classes et les interfaces liées aux servlets sont contenues dans deux packages Java :
  - _javax.servlet_
  - _javax.servlet.http_


---

# Exemple de Servlet
à ne pas suivre...

```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.*;

public class MaServlet extends HttpServlet 
{
@Override
public void service(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException 
 {  //ceci fonctionne mais n'est pas maintenable, nécessite une recompilation à chaque modif, etc...
	res.setContentType("text/html");
	PrintWriter out = res.getWriter();
	out.println(" <HTML> ");
	out.println(" <HEAD> ");
	out.println(" <TITLE> Exercice </TITLE> ");
	out.println(" </HEAD> ");
	out.println(" <BODY> ");
	out.println(" <CENTER> <H1>Hello Servlet!</H1> </CENTER> ");
	out.println(" </BODY> ");
	out.println(" </HTML> ");
 }
}
```

---

# Les Servlets

- Les servlets HTTP permettent d’envoyer et de recevoir des données, à l ’aide d ’un formulaire HTML par exemple, ou via un service web.
- Les classes de servlets HTTP héritent de la classe javax.servlet.http.HttpServlet.

- Lorsqu ’une requête est déclenchée, la méthode _service()_ de la servlet est automatiquement exécutée, le serveur lui transmettant deux paramètres :
  - _javax.servlet.http.HttpServletRequest_ (objet requête)
  - _javax.servlet.http.HttpServletResponse_ (objet réponse)

- Ces deux objets request et response permettent à la servlet de communiquer avec le serveur mais aussi avec l ’IHM client (navigateur ou autre).

![Image Description](/images/servlets_composants.png){.block .mx-auto .w-1/4}

---

# Les Servlets

- Ces requêtes HTTP peuvent être de différents types. On parle ici de _méthodes_ : GET et POST sont les plus utilisées.

- La méthode GET, conçue pour obtenir des informations, est complétée par une liste de paramètres passés dans l'URL (http://serveur/chemin?tache=rechercher&page=1).
  - La longueur des paramètres est limitée (env 2KO)
  - Les paramètres sont en clair dans l'URL
  - Il est facile de construire des liens vers ces URL paramétrées

- La méthode POST, conçue pour poster des informations, est utilisée avec des formulaires. 
  - Les paramètres sont passés **dans le corps de la requête**.
  - La taille est illimitée (dépend généralement de paramètres du serveur)
  
- Les autres méthodes sont PUT, DELETE, HEAD, TRACE, OPTIONS.

---

# Les Servlets

- Les objets request et response peuvent être transmis aux méthodes _doGet()_ et _doPost()_ (surchargées).
- Si la méthode _service()_ n’est pas surchargée, elle appellera directement _doGet()_ ou _doPost()_ selon la _http method_ utilisée.

- Une session représente l'ensemble des requêtes effectuées sur une application entre la connexion et la déconnexion d'un utilisateur.
- On peut mémoriser des données automatiquement rattachées à l'utilisateur courant, dans la session HTTP  (_HttpSession_)
- Elle est obtenue à partir de la requête HTTP, une session HTTP s’utilise comme un dictionnaire (clé, valeur) dans lequel on stocke des informations spécifiques à l'utilisateur actuellement connecté.

---

# Exemple de Servlet (2)
Récupération de paramètres d'un formulaire

```java
/**
 * Méthode appelée à chaque envoi de requête HTTP (aiguillage)
 * @param javax.servlet.http.HttpServletRequest request
 * @param javax.servlet.http.HttpServletResponse response
 * @return void
 */
protected void service(HttpServletRequest request,
    HttpServletResponse response) throws ServletException, IOException {

    // Test du paramètre
    // (valeur saisie dans le champ "tache" du formulaire)
    if (request.getParameter("tache").equals("rechercher")) {
        // appel d'une méthode définie ailleurs dans la classe
        this.rechercherClientParId(request, response); 
    }
    else if (...) {
    ...
    }
}
```

---

# Exemple de Servlet (3)
Download / Streaming d’un fichier

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    //fichier servi par défaut si aucun param fileName n'est présent
    String fileName = "Evil_Laugh.mp3"; 
    if (request.getParameter("fileName") != null) {
        fileName = request.getParameter(fileName);
    }
    InputStream is=request.getServletContext()
    .getResourceAsStream("/WEB-INF/files/"+fileName);
    if(is == null) {
        System.out.println("file not found");
    }else {
        //version très mal optimisée d'un streaming de fichier 
        //car on charge tout le fichier en mémoire avant de
        //l'envoyer au client
        response.setContentType("application/octet-stream");
        BufferedInputStream bis = new BufferedInputStream(is);
        byte[] bytes= bis.readAllBytes();
        response.getOutputStream().write(bytes, 0, bytes.length);
    }
```

---

# Les Java Server Pages

- Les JSP sont partie intégrante de la plateforme JavaEE.

- Une page HTML contient des données dites _statiques_.
- Les JavaServer Pages (JSP) permettent d’obtenir un contenu dynamique.

- Une JSP est une page HTML dans laquelle on a des tags spécifiques ainsi que souvent du code Java inséré dans des balises spécifiques.

- On parle de _compilation à la volée_ des JSP :
  - Le serveur d'applications reçoit une requête pour un fichier JSP
  - Il le fichier JSP demandé
  - Un fichier source (.java) est généré (à partir du JSP) et est compilé (.class)

_Note:_ Le code java généré par cette compilation est en fait une servlet.

---

# Exemple de JSP
Obsolète depuis la généralisation des taglibs

```html
<HTML>
<!-- directives -->
<%@page import="monpackage.MaClasse" session="true" errorPage="error.jsp"%>
  <HEAD><TITLE> Hello </TITLE></HEAD>
  <BODY><H1>
  <% //scriptlet: fragment de Java dans le HTML
	  String nom=request.getParameter("nom");
     if (nom == null) {
       out.println(" <HL> MaServlet </HL> ");
     } else {
       out.println(" <HL> MaServlet - nom : </HL>"+ nom);
     }
  %>
  </H1> 
<!-- tags JSP -->
 <jsp:useBean id="identifiant" scope="session" class="monpackage.MaClasse"/>
  Votre dernière connexion date du 
   <jsp:getProperty name="identifiant" property="derniereConnexion" /> 	
  </BODY>
</HTML>

```
---

# Le couple JSP / Servlets

- Une page JSP est compilée à la première demande uniquement (puis exécutée) ; aux demandes suivantes, et si le source de la JSP n’a pas changé, le fichier .class existant de la servlet est exécuté.

- La création d ’un fichier JSP se fait de la façon suivante:
  - tout d’abord, création d ’un fichier HTML (outils avec composition visuelle ou non)
  - puis renommage de l’extension .html en .jsp et ajout des tags pour générer le contenu dynamique renvoyé par la servlet qui a appelé la JSP.

_Note:_ Il s'agit là de webapp _à l'ancienne_, où le contenu est généré entièrement sur le serveur. Les webapps modernes sont généralement à base de frameworks Javascript et d'un backend (webapp) de services Rest (données JSON) servent à alimenter la GUI.

---

# Le couple JSP / Servlets

- Le couplage des servlets et des JSP permet de séparer clairement la partie _traitement_ de la partie _affichage_ du résultat.

![servlets_jsp](/images/jsp_servlet.png){.block .mx-auto .w-1/3}

- Ainsi, un objet souvent appelé _JspBean_ (structure de données) est associé à la page JSP. Cette dernière peut donc y puiser les informations dont elle a besoin pour afficher le résultat dans la page HTML.

---

# Le couple JSP / Servlet

- Une requête HTTP est émise dès que l’utilisateur clique sur le bouton _Rechercher_  du formulaire.
- Le servlet récupère l’identifiant saisi par l’utilisateur dans le formulaire HTML, et effectue le traitement pour obtenir l’objet métier Client voulu puis crée un objet _JspBean_ qu’il alimente avec les données à afficher en retour.
  
```html
<html>
  <head><title>Recherche</title></head>
  <body>
   <form action="http://machine:8080/monapp/rechClient" method="POST">
    <br/>Recherche d’un client :
    <br/><input type="text" size="10" name="identifiant">
    <br/><input type="submit" name="Rechercher">
   </form>
  </body>
</html>
```

---

# Le couple JSP / Servlet

- L’objet de type RequestDispatcher est créé par le moteur de servlets et permet d’appeler une page JSP depuis une servlet, en lui transmettant les données à afficher.

```java
//Code de la servlet pour obtenir l’objet RequestDispatcher
//initialisé pour rediriger ver la page /jsp/accueil.jsp
RequestDispatcher dispatcher = request.getRequestDispatcher("/jsp/accueil.jsp");

// on attache l'objet référencé par userContext avec la clé 
// "uCtx" à la requête HTTP pour le rendre utilisable dans 
//la JSP. (Il contient les données à afficher)
request.setAttribute ("uCtx", userContext);

// appel de la méthode "forward" pour afficher la page JSP
dispatcher.forward (request, response);
```

---

# Exemple de JavaBean

```java
package data
// Classe UserContext qui va stocker les données qu'on garde 
//durant toute la session HTTP de l'utilisateur.
// Il est stocké dans la HttpSession et transmis aux JSP pour 
// en afficher le contenu
public class UserContext{
    private String userName;
    /**
     * Met à jour l'attribut userName
     * @param java.lang.String userName
     */
    public void setUserName(String userName) {
        this.userName = userName;
    }
    /**
     * Retourne la valeur de l'attribut userName
     * @return java.lang.String
     */
    public String getUserName() {
        return userName;
    }
}
```

---

# Les Taglibs

- Afin de limiter l'écriture de code java dans les JSP il existe des taglibs (tags JSP évolués) pour effectuer des actions diverses:
  - affichage de valeurs, de tableaux, gestion de formulaires,…
  
- La Java Standard TagLib (JSTL) fait partie de JavaEE

- On peut créer ses propres taglibs:
  - fichier .jar implémentant les actions décrites par les tags
  - fichier de définition des tags et de lien avec leur implémentation
  
---

# La JSTL

- Intégrée à la spec JSP
- Permet d'éviter le recours au scriptlets java dans les pages JSP et fournit un ensemble de tags répondants aux besoins les plus courants dans les JSP tels que:
  - conditions (avec le else)
  - affichage
  - formatage de données
  - boucles et autres
- Regroupe 4 taglibs: core, xml, i18n, SQL
- Dispose de son propre langage d’expressions: EL

La doc de référence de la JSTL est ici: http://docs.oracle.com/javaee/5/tutorial/doc/bnakc.html

----

# La JSTL: Installation

- La JSTL n'est pas intégrée à Tomcat. 
- Il faut télécharger les librairies disponibles ici http://tomcat.apache.org/download-taglibs.cgi
- Copier le dossier les fichiers .jar dans WEB-INF/lib de la webapp

---

# La JSTL: EL (Expression Language)

- Langage utilisé pour spécifier les valeurs des attributs.
- Syntaxe: _${obj.attrib.nestedAttrib}_
- Supporte les opérateurs _== < > != && || !_
- Recherche des objets dans les scopes page → request → session → application
- Possibilité de spécifier explicitement un scope de recherche.
- Propriétés imbriquées:
  - _${obj.attrib}_ équivalent à _${obj[attrib]}_
  - Si _obj_ est une _Map_, retourne _obj.get(attrib)_
  - Si _obj_ est une _List_, transforme _attrib_ en _int_ et retourne _obj.get(attrib)_

---

# La JSTL: Object implicites

- _pageContext_ – contexte de page (accès aux différents scopes, etc…) 
- _pageScope_ - map contenant tous les objets dans le scope page 
- _requestScope_ - map contenant tous les objets dans le scope request 
- _sessionScope_ - map contenant tous les objets dans le scope session
- _applicationScope_ - map contenant tous les objets dans le scope application 
- _param_ – équivalent de request.getParameter(String)) - récupère un paramètre passé dans l'URL ou en POST
- _header_ – équivalent à request.getheader(String)) - récupère un en-tête HTTP transmis par le navigateur
- _cookie_ – équivalent à request.getCookie(String))
- _initParam_ – équivalent à request.getInitParameter(String))

---

# La JSTL : Core

- Dédiée à l’affichage, tests, boucles,…
- Doit être **déclarée en début de JSP** afin d'être connue du moteur de rendu:
 
```html
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```

- Affichage d'une valeur:
 
```html 
Bonjour <c:out value="${uCtx.userName}"/> 
```

- On peut aussi mixer texte et EL dans l'expression à afficher:
  
```html
<c:out value="Bonjour ${uCtx.userName}"/> 
```

---

# La JSTL : Core

- Test. Si l'expression EL est **vraie** alors le corps du tag est évalué, sinon il est ignoré
 
```html
<c:if test !="${empty obj.maListe}">
La liste contient <c:out value="${fn:length(obj.maListe)}"/> éléments.
</c:if>

```

_Note:_ Ne permet pas d'avoir un _else_. Il faut utiliser le tag __c:choose__ pour cela.

- Il existe d'autres fonctions JSTL:
  - _toUpperCase, toLowerCase_
  - _substring, substringBefore, substringAfter_
  - _trim_
  - _replace_
  - _indexOf, startsWith, endsWith, contains_
  - _split, join_

Voir javadoc: http://docs.oracle.com/javaee/5/jstl/1.1/docs/tlddocs/fn/tld-summary.html

---

# La JSTL : Core
Déclaration de variable

```html
<!-- foo est initialisé avec une constante -->
<c:set var="foo" scope="session" value="valeur"/>

<!-- foo est initialisé avec le résutat de l'évaluation du corps du tag-->
<c:set var="foo">
valeur
</c:set>  

<!-- foo est initialisé avec le résultat d'une expression EL -->
<c:set var="foo" value="${obj.attrib}"/>
```

---

# La JSTL : Core
Itération

```html
<table>
  <!-- varStatus définit un nom de variable -->
  <!-- permettant d'avoir des infos sur la boucle en cours d'exécution -->
  <c:forEach var="ville" items="${villes}" varStatus="status" >
    <tr>
    <td>
        ligne: <c:out value="${status.index}"/>
    </td>
    <td>
        nom: <c:out value="${ville.nom}"/>
    </td>
    <td>
        code postal: <c:out value="${ville.code_postal}"/>
    </td>
    </tr>
  </c:forEach>
</table>
```

---

# La JSTL : i18n

- Fichiers de ressources:
  - _/nls/applicationResources.properties_ (défaut)
  - _/nls/applicationRessources_fr.properties_ (Locale "fr")

- Déclaration de la taglib:

```html
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt" %>
```

- Délaration des fichiers de ressources dans les paramètres de contexte de l'application:
  
```xml
<context-param>
 <para-name>javax.servlet.jsp.jstl.fmt.localizationContext</param-name>
 <param-value>nls.applicationResources</param-value> 
</context-param>
```

---

# La JSTL : i18n

- La sélection de la Locale est automatique (suivant préfs envoyées par le navigateur). 
- Il est possible de passer outre via le tag _&lt;fmt:setLocale&gt;_

- Affichage multi-langue:

```html
<fmt:bundle basename="nls.applicationResources">
<fmt:message key="label.hello"/>
…
</fmt:bundle>
```

---

# La JSTL : i18n
Formatage de dates

- Utilisation d'un style prédéfini:

```html
<fmt:formatDate value="${now}" type="date"   dateStyle="full"/>
```

- Spécification du format:

```html
<fmt:formatDate value=${now} pattern="dd/mm/yyyy hh:MM"/>
 ```
---

# JSP / Servlets
Frameworks Applicatifs

- Dans la pratique on utilise généralement un framework basé sur les servlets/JSP et fournissant des services de plus haut niveau:
  - MVC2 (séparation Modèle, Vue & Contrôleur)
  - Validation de formulaires
  - Contrôle de séquencement des pages
- Exemple de tels frameworks: Spring-MVC, Struts (obsolète)

- Pour l’accès aux données on utilise soit un framework de persistance objet-relationnel (JPA, Spring Data JPA), soit des EJB (Enterprise JavaBeans)
