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


<ul>
{% for post in site.publications reversed %}
  {% include archive-single-pub.html %}
{% endfor %}
</ul>


{% comment %}
<table>
{% tablerow post in site.publications reversed cols:1 %}
    {{ post.date | date: "%Y" }} </td><td> {{ post.venue }} </td><td> <a href="{{post.paperurl}}">{{post.title}} </a>  
{% endtablerow %}
</table>
{% endcomment %}
