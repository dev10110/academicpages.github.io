---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single-pub.html %}
{% endfor %}


{% for post in site.publications reversed %}
    {{ post.date | default: "1900-01-01" | date: "%Y" }}
    {{ post.venue }}
{% endfor %}
