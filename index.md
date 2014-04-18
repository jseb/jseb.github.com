---
layout: page
title: Latest post
tagline: Supporting tagline
---
{% include JB/setup %}

<div class="latest_post">
    {% for post in site.posts limit: 1 %}
        <div class="post_info">
            <span>({{post.date | date:"%Y-%m-%d"}})<h1>{{ post.title }}</h1></span>
        </div>
        <ul class="tag_box inline">
            {% assign tags_list = post.tags %}
            {% include JB/tags_list %}
        </ul>
        <div class="post_content">
            {{ post.content }}
        </div>
        <a href="{{ post.previous.url }}" class="prev" title="{{ post.previous.title }}">&larr; Earlier Post</a>
    {% endfor %}
</div>

{% include JB/comments %}
