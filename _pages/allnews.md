---
title: "News"
layout: textlay
excerpt: "Biomedical Genomics Group @ Health Data Science Unit"
sitemap: false
permalink: /allnews.html
---

# News

{% for article in site.data.news %}
<p>{{ article.date }} <br>
<em>{{ article.headline }}</em></p>
{% endfor %}
