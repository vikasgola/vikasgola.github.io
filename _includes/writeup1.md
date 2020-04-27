## {{include.title}} ({{include.category}})

##### **Problem Statement**

{{include.problem}}

##### **Solution**

{{include.solution}}

{% capture carousel_images %}

{% for image in include.images %}
{{image}}
{% endfor %}

{% endcapture %}

{% include elements/carousel.html %}
