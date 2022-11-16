---
layout: laravel
title: Eloquent-基本概念用法
tags: laravel
date: 2020-10-11 18:40:59
categories: PHP框架
description: Eloquent-基本概念用法
author: Stars
about: PHP框架
avatar: "/image/sidebar/avatar.jpg"
---

## 什么是 Elopquent 

Eloquent 是 Laravel 的 'ORM'，即 'Object Relational Mapping'，对象关系映射。ORM 的出现是为了帮我们把对数据库的操作变得更加地方便Eloquent 让一个 'Model类' 对应一张数据库表，并且在底层封装了很多 'function'，可以让 Model 类非常方便地调用。没错，Eloquent 就是这么屌炸天，只需要继承一下 Eloquent 类，就可以干 'first() find() where() orderBy()' 等非常非常多的事情，这就是面向对象的强大威力。

## Eloquent的基本语法

### 找到 id 为 2 的文章打印其标题

```

    $article = Article::find(2);
    
    echo $article->title;
    
```

### 查找标题为“我是标题”的文章，并打印 id

```

    $article = Article::where('title', '我是标题')->first();
    
    echo $article->id;
    
```
### 查询出所有文章并循环打印出所有标题

```

   $articles = Article::all(); // 此处得到的 $articles 是一个对象集合，可以在后面加上 '->toArray()' 变成多维数组。
   
   foreach ($articles as $article) {
   
        echo $article->title;
   
   }
    
```

### 查找 id 在 10~20 之间的所有文章并打印所有标题

```

   $articles = Article::where('id', '>', 10)->where('id', '<', 20)->orderBy('updated_at', 'desc')->get();   
   foreach ($articles as $article) {
   
        echo $article->title;
   
   }
    
```

### 基础使用要点:

1.每一个继承了 Eloquent 的类都有两个 '固定用法' 'Article::find($number)' 'Article::all()'，前者会得到一个带有数据库中取出来值的对象，后者会得到一个包含整个数据库的对象合集。
2.所有的中间方法如 'where()' 'orderBy()' 等都能够同时支持 '静态' 和 '非静态链式' 两种方式调用，即 'Article::where()...' 和 'Article::....->where()'。
3.所有的 '非固定用法' 的调用最后都需要一个操作来 '收尾'.：'->get()' 和 '->first()'。
