---
layout: default
title: "Criptomonedas"
description: Criptomonedas y Productos Blockchain
permalink: /criptomonedas/
---

<div class="sub-topic">

  <h1 class="page-heading">{{ page.description }}</h1>

  <ul class="post-list">
    {% for post in site.categories.criptomonedas %}
      <li>
        <span class="post-meta">
            {% assign month = post.date | date: "%m" %}
            {% case month %}
    
              {% when '01' %} Enero
              {% when '02' %} Febrero
              {% when '03' %} Marzo
              {% when '04' %} Abril
              {% when '05' %} Mayo
              {% when '06' %} Junio
              {% when '07' %} Julio
              {% when '08' %} Agosto
              {% when '09' %} Septiembre
              {% when '10' %} Octubre
              {% when '11' %} Noviembre
              {% when '12' %} Diciembre
    
            {% endcase %}
          {{ post.date | date: "%-d, %Y" }}
          
        </span>

        <h2>
          <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
        </h2>
        {% if post.description %}
          <p class="post-description">
            {{ post.description }}
          </p>
        {% endif %}
      </li>
    {% endfor %}
  </ul>

</div>