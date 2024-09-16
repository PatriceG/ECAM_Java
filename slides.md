---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: 
# some information about your slides (markdown enabled)

# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: true

#enable PDF generation
download: true

title: Java - Les Points Essentiels
class: text-center
---

# Java - Les Points Essentiels

ECAM Rennes

Patrice GODARD

patrice.godard@orange.com

<div class="w-75 h-px bg-gray absolute bottom-5 left-0 text-sm">Menu ci-dessous&nbsp;&nbsp;|&nbsp;&nbsp;← → pour se déplacer
</div>
<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
---

# Programme du Cours

<!-- does not work in our context: <Toc minDepth="1" maxDepth="1"></Toc> -->
1. La Plateforme Java
2. Les Packages
3. Les Collections
4. Les Annotations
5. Rappels sur les Exceptions
6. Les Entrées/Sorties
7. L'utilisation du format JSON
8. L'accès aux bases de données relationnelles
9. JavaEE: Les Servlets et les JSP
10. Développement d'une application Web
11. Les Taglibs JSP

---
src: slides/pf_java.md
---

---
src: slides/packages.md
---

---
src: slides/collections.md
---

---
src: slides/annotations.md
---

---
src: slides/exceptions.md
---

---
src: slides/io.md
---

---
src: slides/json.md
---

---
src: slides/jdbc.md
---

---
src: slides/javaee.md
---

---
src: slides/annexe.md
---
