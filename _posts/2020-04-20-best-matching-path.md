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

      

## Spring WebFlux에서 URI Path Variable

### Java Controller

{% highlight java %}

@GetMapping("/spring-webflux/{*path}")
public String uriVariable(@PathVariable String path) {
    System.out.println(path)
}

{% endhighlight %} 

### Kotlin RouteFunction

{% highlight kotlin %}

"/spring5-webflux".nest {
    PUT("/{*path}") { ServerResponse.ok().build() }
}

{% endhighlight %} 

__위 코드로 모든 설명이 된다.__

## Spring MVC에서 URI Path Variable

하지만 Spring MVC에서는 ```{*path}```와 같은 syntax를 사용할 수 없다.

{% highlight java %}

// Controller RequestMapping
@GetMapping("/spring-mvc/**")
public String uriVariable(HttpServletRequest request) {
    String path = HttpServletRequestUtils.extractBestMatchingPath(request);
    System.out.println(path)
}

// 전역 메소드(HttpServletRequestUtils.java)
public static String extractBestMatchingPath(HttpServletRequest request) {
    ResourceUrlProvider urlProvider = (ResourceUrlProvider) request.getAttribute(ResourceUrlProvider.class.getCanonicalName());
    String pattern = (String) request.getAttribute(HandlerMapping.BEST_MATCHING_PATTERN_ATTRIBUTE);
    String path = (String) request.getAttribute(HandlerMapping.PATH_WITHIN_HANDLER_MAPPING_ATTRIBUTE);
    String extractedPath = urlProvider.getPathMatcher().extractPathWithinPattern(pattern, path);
    return cleanSubPath(extractedPath);
}

{% endhighlight %}

경로 패턴을 ```'/**'```와 같이 하위 모든 경로가 매핑해 주고 ```{*path}```와 같은 기능을 제공하는 전역 메소드를 작성해서 사용한다. 
위 샘플 코드와 같이 사용하게 되면 항상 _상대 경로_ 를 제공하는 것에 주의해야 한다.
