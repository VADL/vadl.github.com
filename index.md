---
title: Hello World!
layout: page
tagline: Supporting tagline
---
{% include JB/setup %}

Welcome to the Vanderbilt Aerospace Design Lab's site!  On this site is where we keep track of our design/development process as well as maintain best practices / guides / tutorials for new members and for reference. 
 
## Posts

Below are the posts from the VADL across various topics, note that as the list grows, it may be easier to find what you're looking for if you go to the categories or tags pages which allow you to filter relevant posts by those fields.

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>