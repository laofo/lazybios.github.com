---
layout: default
comments: false
---

<div class="content">
    {% for post in site.posts %}
        {% capture y %}{{ post.date | date: "%Y"}}{%endcapture%}
        {% if year != y %}
            {% assign year = y %}
            <h3 class="year">{{ year }}</h3>
        {% endif %}
        <ul class="article-list">
            <li>
                <small>{{ post.date | date: "%Y-%m-%d" }}</small>
                <a href="{{site.url}}{{post.url}}" title="{{ post.title }}">{{ post.title }}</a>
            </li>
        </ul>
    {% endfor %}
</div>
