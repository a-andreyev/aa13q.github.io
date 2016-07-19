---
layout: default
---

# ls posts/
{:id="posts"}

<ul>
{% for post in site.categories.posts %}

<li>
{{ post.title }}
{% if post.en %}
:: <a href="{{ post.url }}" title="{{ post.description }}">en</a>
{% endif %}
{% if post.ru %}
:: <a href="{{ post.url }}" title="{{ post.description }}">ru</a>
{% endif %}
</li>

{% endfor %}
</ul>

# ls projects/
{:id="projects"}

<ul>
{% for project in site.categories.projects %}
<li><a href="{{ project.link }}">{{ project.title }}</a> - {{ project.description }}</li>
{% endfor %}
</ul>

# ls tools/
{:id="tools"}

<ul>
{% for tool in site.categories.tools %}
<li><a href="{{ tool.link }}">{{ tool.title }}</a> - {{ tool.description }}</li>
{% endfor %}
</ul>

# ls talks/
{:id="talks"}

<ul>
{% for talk in site.categories.talks %}
<li><a href="{{ talk.link }}" title="{{ talk.description }}">{{ talk.title }}</a> at {{ talk.where }}</li>
{% endfor %}
</ul>

# cat about.md
{:id="about"}

My name is Alexey.

![photo](assets/img/aa13q.jpeg){:height="256px" width="256px"}

I'm a first-year Ph.D. student from ITMO University (Saint Petersburg, Russia).

I'm a linux/archlinux, C/C++/Qt/QML, Python/Ruby, KDE fan.
Happy Jolla SailfishOS smartphone and Pebble Time smartwatch owner.

I'm currently working on [SemIoT project](https://github.com/semiotproject) at [ISST lab](http://isst.ifmo.ru/en/).

# ls contact/
{:id="contact"}

Feel free to contact me at aa13q via telegram or ya.ru email.