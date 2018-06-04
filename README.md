## Welcome to my knowledge garden

You can use the [editor on GitHub](https://github.com/960761/960761.github.io/edit/master/README.md) to maintain and preview the content for your website in Markdown files.


## Contents


[Click to CSS+HTML part](https://960761.github.io/AboutCSS/)


[Click to JavaScript part](https://960761.github.io/AboutJS/)

## Blogs


<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
