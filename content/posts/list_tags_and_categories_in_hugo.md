+++
title =  "List tags and categories in hugo"
date = 2018-06-14T14:15:18+02:00
featured_image = ""
description = ""
tags = []
categories = ['hugo']
+++

For example, to create a list

    <p class="posts-name">Tags:</p>
    <ul>
        {{ range $key, $value := .Site.Taxonomies.tags }}
        <li>
            <a href="/tags/{{ $key | urlize }}">{{ $key | title }}</a>
        </li>
        {{ end }}
    </ul>
    <hr>

    <p class="posts-name">Categories:</p>
    <ul>
        {{ range $key, $value := .Site.Taxonomies.categories }}
        <li>
            <a href="/categories/{{ $key | urlize }}">{{ $key | title }}</a>
        </li>
        {{ end }}
    </ul>
    <hr>
