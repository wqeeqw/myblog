---
title: wordpress-格式化标题
tags: wordpress
date: 2020-10-09 18:40:59
categories: PHP框架
description: wordpress格式化标题
author: Stars
about: PHP框架
avatar: "/image/sidebar/avatar.jpg"
---

## 页面使用

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title><?php wp_title('-',true,'right');?></title>
   ......
```

## functions.php 使用

```
    
<?php 
    /**
    * 想要wp_title()函数实现，访问首页显示“站点标题-站点副标题”
    * 如果存在翻页且正方的不是第1页，标题格式“标题-第2页”
    * 当使用短横线-作为分隔符时，会将短横线转成字符实体&#8211;
    * 而我们不需要字符实体，因此需要替换字符实体
    * wp_title()函数显示的内容，在分隔符前后会有空格，也要去掉
    */
    add_filter('wp_title', 'lingfeng_wp_title', 10, 2);
    function lingfeng_wp_title($title, $sep) {
        global $paged, $page;
    
        //如果是feed页，返回默认标题内容
        if ( is_feed() ) { 
            return $title;
        }
    
        // 标题中追加站点标题
        $title .= get_bloginfo( 'name' );
    
        // 网站首页追加站点副标题
        $site_description = get_bloginfo( 'description', 'display' );
        if ( $site_description && ( is_home() || is_front_page() ) )
            $title = "$title $sep $site_description";
    
        // 标题中显示第几页
        if ( $paged >= 2 || $page >= 2 )
            $title = "$title $sep " . sprintf( '第%s页', max( $paged, $page ) );
    
        //去除空格，-的字符实体
        $search = array('&#8211;', ' ');
        $replace = array('-', '');
        $title = str_replace($search, $replace, $title);
    
        return $title;  
    }

?>

```