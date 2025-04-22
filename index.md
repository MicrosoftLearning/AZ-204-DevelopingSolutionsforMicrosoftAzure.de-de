---
title: Online gehostete Anweisungen
permalink: index.html
layout: home
---

## Inhaltsverzeichnis

Links zu den einzelnen Labs sind nachfolgend aufgelistet.

## Übungen

{% assign labs = site.pages | where_exp: "page", "page.url contains '/Instructions/Labs'" %}
| Modul | Lab |
| --- | --- |
{% for activity in labs  %}{% if activity.lab.az204Module %}| {{ activity.lab.az204Module }} | [{{ activity.lab.az204Title }}]({{ site.github.url }}{{ activity.url }}) |
{% endif %}{% endfor %}

