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

title: Java - Les Points Essentiels
class: text-center
---

# Java - Les Points Essentiels

ECAM Rennes

Patrice GODARD

patrice.godard@orange.com
<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
---

# Programme du Cours

<Toc minDepth="1" maxDepth="1"></Toc>

---

# Slide 1

  - l1
    - l1 1
  - l2
  - l3
  
---

# Code 1


````md magic-move {lines: true}
```java
// step 1
import java.util.List;
import java.util.ArrayList;

java.util.List<String> liste = new java.util.ArrayList<String>();
```

```java
// step 2
import java.util.List;
import java.util.ArrayList;

java.util.List<String> liste = new java.util.ArrayList<String>();
liste.add("un objet");
liste.add("un autre objet");
```

```java
import java.util.List;
import java.util.ArrayList;

java.util.List<String> liste = new java.util.ArrayList<String>();
liste.add("un objet");
liste.add("un autre objet");

System.out.println("second élément de la liste: " + liste.get(1));
liste.clear();

```
````

---

# Code 2

```java{monaco}
import java.util.List;
import java.util.ArrayList;

java.util.List<String> liste = new java.util.ArrayList<String>();
```

---

# Clicks Animations

<v-click>
Ceci est <span v-mark.red="1">très important</span>
</v-click>
<v-click>
Ceci <span v-mark.circle.orange="2">aussi</span>
</v-click>

---

# json annoté

<style>
/* Add your custom styles here */
.text-box {  
  position: relative;
  display: inline-block;
  font-size: 8pt;
  padding: 3px 5px;
  background-color: #FFFF66;
  border: 1px solid #aaa;
  border-radius: 3px;
}

.tb1 {
  position: relative;
  top: 0mm; left: 100mm;
}
</style>

<!-- Add your custom HTML here -->
<div class="text-box tb1">
  This is a floating text box.  
</div>


```json
{
  "name": "Slidev",
  "description": "Presentation Slides for Developers (Presentation Slides for Developers)"
}
```

---
layout: image
image: https://logos-marques.com/wp-content/uploads/2021/03/Android-Logo.png

---
# 3 column code

<v-click>
<div
  v-motion
  :initial="{ y: -80 }"
  :enter="{ y: 0 }"
  :click-1="{ y: 40 }"
  :leave="{ y: 1000 }"
>
  <Arrow x1="30mm" y1="30mm" x2="70mm" y2="30mm" width="1" color="#777"/>
</div>
</v-click>

---

# Learn More

[Documentation](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/showcases.html)

<PoweredBySlidev mt-10 />
