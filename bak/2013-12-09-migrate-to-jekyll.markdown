---
layout: post
title: "迁移到jekyll"
tags: [Jekyll]
categories: [Other]
---


把博客从Octopress迁移到Jekyll， 虽然Octopress也是基于Jekyll的，但是感觉有点臃肿，而且之前的主题也不怎么喜欢，于是也换了一个简约的主题。

## 1. Create a Post

```ruby

$ rake post title="Hello World"

```


## 2. Create a Page

```ruby

$ rake page name="about.md"

```

## 3. Create a nested page

```ruby

$ rake page name="pages/about.md"

```