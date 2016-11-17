---
layout: home
permalink: /
image:
  feature: home/cover.png
---

<div class="tiles">

  {% for link in site.data.navigation %}
     <div class="tile">
        <a href="{{ site.url }}{{ link.url }}">
        {% if link.image %}<img src="{{ site.url }}/images/{{ link.image }}" alt="teaser" class="teaser">{% endif %}
        <h2 class="post-title">{{ link.title }}</h2>
        {% if link.excerpt %}<p class="post-excerpt">{{ link.excerpt }}</p>{% endif %}
        </a>
      </div><!-- /.tile -->
  {% endfor %}

</div>