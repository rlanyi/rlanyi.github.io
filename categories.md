---
layout:   page
title:    Categories
---
{% comment%}
Here we generate all the categories.
{% endcomment%}

{% assign rawcats = "" %}
{% for post in site.posts %}
{% assign tcats = post.category | join:'|' | append:'|' %}
{% assign rawcats = rawcats | append:tcats %}
{% endfor %}

{% assign rawcats = rawcats | split:'|' | sort %}

{% assign cats = "" %}

{% for cat in rawcats %}
{% if cat != "" %}

{% if cats == "" %}
{% assign cats = cat | split:'|' %}
{% endif %}

{% unless cats contains cat %}
{% assign cats = cats | join:'|' | append:'|' | append:cat | split:'|' %}
{% endunless %}
{% endif %}
{% endfor %}

<div class="posts">
<p>
{% for ct in cats %}<a href="#{{ ct | slugify }}">{{ ct }}</a>&nbsp;&nbsp;&nbsp;{% endfor %}<a href="#no-category">No Category</a>
</p>

{% for ct in cats %}
<h3 id="{{ ct | slugify }}">{{ ct }}</h3>
<ul>
  {% for post in site.posts %}
  {% if post.category contains ct %}
  <li>
      <a href="{{ post.url }}">
        {{ post.title }}
        <small>{{ post.date | date_to_string }}</small>
      </a>
      {% for tag in post.tags %}
      <a href="/tag/#{{ tag | slugify }}">{{ tag }}</a>
      {% endfor %}
  </li>
  {% endif %}
  {% endfor %}
</ul>
{% endfor %}

<h3 id="no-category">No Category</h3>
<ul>
  {% for post in site.posts %}
  {% unless post.category %}
  <li>
      <a href="{{ post.url }}">
        {{ post.title }}
        <small>{{ post.date | date_to_string }}</small>
      </a>
      {% for tag in post.tags %}
      <a href="/tag/#{{ tag | slugify }}">{{ tag }}</a>
      {% endfor %}
  </li>
  {% endunless %}
  {% endfor %}
</ul>

</div>
