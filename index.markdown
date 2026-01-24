---
layout: default
title: WorkingDev.net | The Engine
---

# workingdev.net
**Technical references, code snippets, and implementation patterns.**

---

## 🛠 Active Projects
*A repository of practical solutions for specific tech stacks.*

**Android & Kotlin** Practical implementation of Jetpack Compose, Coroutines, and modern architecture.

**Java & Spring** Patterns for Spring Boot 3.x, Jakarta EE, and backend security.

**Full-Stack & DevOps** Deployment strategies for Node.js, TypeScript, and Docker.

---

## 📑 Latest Briefs
*The most recent additions to the repository.*

<ul>
  {% for post in site.posts limit:15 %}
    <li>
      <code>{{ post.date | date: "%Y-%m-%d" }}</code> — 
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>


---

## ⚓ About the Author
**workingdev.net** is maintained by **Ted Hagos**.

* **Books:** Technical author at **Apress**.
* **Courses:** Training and workshops at [TheLogBox.com](https://thelogbox.com).
* **Writing:** Essays on the seniority gap and the economics of code at [TedHagos.com](https://tedhagos.com).