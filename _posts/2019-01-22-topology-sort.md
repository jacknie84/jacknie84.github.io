---
layout: post
title: "Topology Sort"
image: ''
date: 2019-01-22 13:05:31
tags: 
- topology
description: ''
categories:
- Learn Algorithm
---

---
layout: post
title:  "Sharding in MongoDB"
image: ''
date:   2016-09-12 00:06:31
tags:
- mongodb
description: ''
categories:
- Learn Jekyll 
---

위상정렬 알고리즘
================

```
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
```