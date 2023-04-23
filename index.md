---
title: Online gehostete Anweisungen
permalink: index.html
layout: home
ms.openlocfilehash: 8cc220d44fe56f19385b25675c29e391b610db8e
ms.sourcegitcommit: d2d374fffa4fcbf92b9c4bdc9c9ecc470152e033
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132626025"
---
## <a name="content-directory"></a>Inhaltsverzeichnis

Links zu den einzelnen Labs sind nachfolgend aufgelistet.

## <a name="labs"></a>Labs

{% assign labs = site.pages | where_exp: "page", "page.url contains '/Instructions/Labs'" %}
| Modul | Labor |
| --- | --- |
{% for activity in labs  %}{% if activity.lab.az204Module %}| {{ activity.lab.az204Module }} | [{{ activity.lab.az204Title }}]({{ site.github.url }}{{ activity.url }}) |
{% endif %}{% endfor %}

## Demos

{% assign demos = site.pages | where_exp:"page", "page.url contains '/Instructions/Demos'" %}
| Modul | Demo |
| --- | --- | 
{% for activity in demos  %}| {{ activity.demo.az204Module }} | [{{ activity.demo.az204Title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
