## {{include.title}} ({{include.category}})

##### **Problem Statement**

{{include.problem}}

##### **Solution**

{{include.solution}}

{% capture carousel_images %}

{% for image in include.images %}
{{image}}
{% endif %}

{% endcapture %}
{% include elements/carousel.html %}

