---
layout: default
title: Meetings
permalink: /Meetings/
navpos: 3
nav: true
---

<div class="home">

  <!--<h1 class="post-heading">Posts</h1>-->
  <header class="post-header">
  <h1 class="post-title">Meetings</h1>
  </header>
  <p>Hier findet ihr die Protokolle unserer Meetings.</p>
  <hr>
  <ul class="post-list">
    {% for post in site.categories.Meetings %}
      <li>

        <h2>
          <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
        </h2>
        {{ post.excerpt }}
        <p class="post-meta">
          {{ post.date | date: "%b %-d, %Y" }}
          <a class="language" href="/{{ post.category }}/">{{ post.categories.first }}</a>
        </p>
        <p class="post-meta">
          {% if post.datasource %}
            <i class="fa fa-database" aria-hidden="true">&nbsp;
            {% if post.datasourceURL %} <a href="{{post.datasourceURL}}">{{ post.datasource }}</a>
            {% else %} {{ post.datasource }}
            {% endif %}
            </i>
          {% endif %}
          {% if post.technique %}
            <i class="fa fa-code" aria-hidden="true">&nbsp;{{ post.technique }}</i>
          {% endif %}
        </p>
      </li>
    {% endfor %}
  </ul>

  <!--- <p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | prepend: site.baseurl }}">via RSS</a></p>
  ---->
</div>
