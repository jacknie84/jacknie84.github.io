---
layout: post
title:  "Topological Sort(위상정렬구현)"
image: ''
date:   2019-01-22 16:49:10
tags: 
- topology
description: ''
categories:
- Learn Algorithm
---

      

## 큐(Queue) 구현

위상정렬을 구현하기 앞서 큐(Queue)를 먼저 자바스크립트로 구현하겠다.~~(java나 c++로 하면 이미 구현 되어ㅏ이럽재랴ㅓㅂㅈ)~~

{% highlight javascript %}

var Queue = (function() {

    function Queue(array) {
        this.array = array;
    }

    Queue.prototype.enqueue = function(node) {
        this.array.push(node);
    }

    Queue.prototype.dequeue = function() {
        return this.array.splice(0, 1)[0];
    }

    Queue.prototype.empty = function() {
        return this.array.length <= 0;
    }

    Queue.prototype.log = function() {
        var log = this.array
            .map(json => JSON.stringify(json))
            .reduce((s, json) => s ? s + ", " + json : json, "");
        console.log(log);
    }

    return Queue;

})();

{% endhighlight %}
  
큐 마지막에 디버깅 차원에서 로그 메소드를 추가해봤다.


## 그래프 데이터 객체화

{% highlight javascript %}

var graph = [
    {//0
        "name": "시작노드"
        , "targets": [1, 4]
        , "inDegree": 0
    }
    , {//1
        "name": "1번노드"
        , "targets": [2]
        , "inDegree": 1
    }
    , {//2
        "name": "2번노드"
        , "targets": [3]
        , "inDegree": 1
    }
    , {//3
        "name": "3번노드"
        , "targets": [6]
        , "inDegree": 2
    }
    , {//4
        "name": "4번노드"
        , "targets": [5, 8]
        , "inDegree": 1
    }
    , {//5
        "name": "5번노드"
        , "targets": [3]
        , "inDegree": 1
    }
    , {//6
        "name": "6번노드"
        , "targets": [7]
        , "inDegree": 1
    }
    , {//7
        "name": "7번노드"
        , "targets": [9]
        , "inDegree": 1
    }
    , {//8
        "name": "8번노드"
        , "targets": [9]
        , "inDegree": 1
    }
    , {//9
        "name": "마지막노드"
        , "targets": []
        , "inDegree": 2
    }
];

{% endhighlight %}

__targets__ 는 다음 정점의 index 값을 의미하고 __inDegree__ 는 진입차수를 의미한다.
    

## 위상정렬

{% highlight javascript %}

var topologySort = [];
var q = new Queue([graph[0]]);
var data = Object.assign([], graph.slice(1));	// graph 배열에서 첫번째 정점은 큐에 담겨 있으므로 제외하고 복사한다.

for (var i = 0; i < graph.length; i++) {
    if (q.empty()) {
        throw new Error("사이클 발생, data: [" + data.map(n => n.name).join(", ") + "]" + ", result: [" + topologySort.join(", ") + "]");
    }
    var frontNode = q.dequeue();
    topologySort.push(frontNode.name);

    frontNode.targets
    	// "index - 1" 을 한 이유는 첫번째 정점을 먼저 큐에 넣고 첫번째 정점을 제외한 배열에 접근하기 때문
        .map(index => data[index - 1])
        .forEach(node => {
            node.inDegree--;
            if (node.inDegree === 0) {
                q.enqueue(node);
            }
        });

    q.log();
}

console.log(topologySort);

{% endhighlight %}

__수고__