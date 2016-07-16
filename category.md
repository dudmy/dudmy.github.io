---
layout: page
title: Category
---

<div>
{% for category in site.categories %}

    {% capture cat %}{{ category | first }}{% endcapture %}

        <h5>{{ cat | capitalize }}</h5>
        <ul>
        {% for post in site.categories[cat] %}
          <li><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></li>
        {% endfor %}
        </ul>

{% endfor %}