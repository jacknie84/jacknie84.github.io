---
layout: post
title:  "Topological Sort(위상정렬)"
image: ''
date:   2019-01-22 13:05:31
tags: 
- topology
description: ''
categories:
- Learn Algorithm
---

## 위상정렬 알고리즘 공부

Java 웹개발 4년차 프로그래머다. 최근 기업의 입사 기준의 코딩테스트의 벽을 느끼고 알고리즘을 공부하고 있다. 알고리즘은 SI업계 업무를 진행하면서 단 한 번도 공부한적이 없다..ㅜㅜ~~(SI업계에 들어오기전에는 공부를 했었지만 이미 머리에서 삭제된지 오래)~~  
자료구조를 공부하게 되면 매번 마지막에 그래프를 만났다.(다른 책이나 전공 수업에서도 그런지는 모르겠다) 공부를 하고 싶은 열정과 의지가 항상 그래프를 접할 즈음이나 그래프를 조금 진행하고나서 바닥이 나버려 대충 봤었다.  
그래서 알고리즘을 공부하는 첫 단계로 그래프에서 쓰이는 알고리즘을 선택했다. 그 시작은 __위상정렬 알고리즘__ 이다.


## 위상정렬 이란?

영어로는 __topological sorting__ 이라고 한다. __순서가 명확히 정해져있는 것__ 들의 순서를 결정하기 위한 알고리즘이라고 보면 쉽다.  
위상정렬은 __DAG(Directed Acyclic Graph)에만 적용이 가능__ 하다. __DAG__ 는 각 노드가 다른 노드를 가리키는 방향이 있고 그 방향이 사이클을 발생시키지 않는 그래프를 뜻한다. 다시 말해 사이클이 발생하면 위상정렬을 적용할 수 없다.  
아래 그래프를 확인해 보자.

![예제 그래프](/assets/img/topological-sort-example.png)

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

var graph = [
    {//0
        "name": "시작노드"
        , "targets": [1, 4]
        , "inDegree": 0
    }
    , {//1
        "name": "1번노드"
        , "targets": [4]
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
        , "targets": [2, 5, 8]
        , "inDegree": 2
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

var topologySort = [];
var q = new Queue([graph[0]]);
var data = Object.assign([], graph.slice(1));

for (var i = 0; i < graph.length; i++) {
    if (q.empty()) {
        throw new Error("사이클 발생, data: [" + data.map(n => n.name).join(", ") + "]" + ", result: [" + topologySort.join(", ") + "]");
    }
    var frontNode = q.dequeue();
    topologySort.push(frontNode.name);

    frontNode.targets
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