---
layout: default
---

## ls posts/
{:id="posts"}

{% for random in site.categories.posts %}
{% if random.lang == "meta" %}

{% assign current_title = random.title %}

### {{ random.title }}

<ul>

{% for random in site.categories.posts %}

{% if random.lang != "meta" and random.title == current_title %}
<li>
<a href="{{ random.url }}" title="{{ random.title }}"> {{ random.lang }} </a>
</li>
{% endif %}

{% endfor %}

</ul>

{% endif %}

{% endfor %}

## ls projects/
{:id="projects"}

<ul>
{% for project in site.categories.projects %}
<li><a href="{{ project.link }}">{{ project.title }}</a> - {{ project.description }}</li>
{% endfor %}
</ul>

## ls tools/
{:id="tools"}

<ul>
{% for tool in site.categories.tools %}
<li><a href="{{ tool.link }}">{{ tool.title }}</a> - {{ tool.description }}</li>
{% endfor %}
</ul>

## ls talks/
{:id="talks"}

<ul>
{% for talk in site.categories.talks %}
<li><a href="{{ talk.link }}" title="{{ talk.description }}">{{ talk.title }}</a> at {{ talk.where }}</li>
{% endfor %}
</ul>

## ls random/
{:id="random"}

{% for random in site.categories.random %}
{% if random.lang == "meta" %}

{% assign current_title = random.title %}

### {{ random.title }}

<ul>

{% for random in site.categories.random %}

{% if random.lang != "meta" and random.title == current_title %}
<li>
<a href="{{ random.url }}" title="{{ random.title }}"> {{ random.lang }} </a>
</li>
{% endif %}

{% endfor %}

</ul>

{% endif %}

{% endfor %}



## cat about.md
{:id="about"}

My name is Alexey.

![photo](assets/img/aa13q.jpeg){:height="256px" width="256px"}

I'm a Ph.D. student from ITMO University (Saint Petersburg, Russia).

I'm a linux/archlinux, C/C++/Qt/QML, KDE fan.
Happy Jolla SailfishOS smartphone and Pebble Time smartwatch owner.

I'm currently working at [Open Mobile Platform](http://omprussia.ru/) and looking for additional earnings for personal projects.

## cat contacts.md
{:id="contact"}

Feel free to contact me at `aa13q` via telegram, freenode irc or ya.ru email.

Donation links:

+ [paypal](https://paypal.me/aa13q)
+ [flattr](https://flattr.com/profile/aa13q)
+ [rocketbank.ru](https://rocketbank.ru/aa13q-alexey-andreyev)
