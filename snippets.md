---
layout: page
title: Snippets
---

{% for post in site.snippets %}
  * [ {{ post.title }} ]({{ post.url }})
{% endfor %}
