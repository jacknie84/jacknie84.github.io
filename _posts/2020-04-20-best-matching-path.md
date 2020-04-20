---
layout: post
title:  "Springframework 에서 패턴 경로"
image: ''
date:   2020-04-20 13:37:00
tags: 
- spring
- spring boot
- spring mvc
description: ''
categories:
- Spring
---

      

## WebFlux에서 URI Path Variable

{% highlight java %}

@GetMapping("/spring5/{*path}")
public String uriVariable(@PathVariable String path) {
    System.out.println(path)
}

{% endhighlight %} 

{% highlight kotlin %}

"/spring5".nest {
    PUT("/{*path}") { ServerResponse.ok().build() }
}

{% endhighlight %} 

__위 코드로 모든 설명이 된다.__