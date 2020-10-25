---
layout: default
title: Home
permalink: /blog
---

{% if page.url == "/blog" %}

<!-- Featured
================================================== -->
<section class="feature-posts">
    <div class="section-title">
        <h2 class="text-capitalize center" style="text-align:center">Featured</h2>
    </div>
    <div class="row listrecent">

        {% for post in site.posts %}

        {% if post.featured == true %}

        {% include postbox.html %}

        {% endif %}

        {% endfor %}

    </div>
</section>

{% endif %}

<!-- Posts Index
================================================== -->
<section class="recent-posts">

    <div class="section-title">
        <h2 class="text-capitalize center" style="text-align:center">All Posts</h2>
    </div>

    <div class="row listrecent">

        {% for post in site.posts %}

        {% include postbox.html %}

        {% endfor %}

    </div>

</section>

<!-- Pagination
================================================== -->
<div class="bottompagination">
<div class="pointerup"><i class="fa fa-caret-up"></i></div>
<span class="navigation" role="navigation">
    {% include pagination.html %}
</span>
</div>

