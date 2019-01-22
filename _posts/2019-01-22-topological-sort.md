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

Java 웹개발 4년차 프로그래머다. 최근 기업의 입사 기준 코딩테스트의 벽을 느끼고 알고리즘을 공부하고 있다. 알고리즘은 SI업계 업무를 진행하면서 단 한 번도 공부한적이 없다..ㅜㅜ~~(SI업계에 들어오기전에는 공부를 했었지만 이미 머리에서 삭제된지 오래)~~  
자료구조를 공부하게 되면 매번 마지막에 그래프를 만났다.(다른 책이나 전공 수업에서도 그런지는 모르겠다) 공부를 하고 싶은 열정과 의지가 항상 그래프를 접할 즈음이나 그래프를 조금 진행하고나서 바닥이 나버려 대충 봤었다.  
그래서 알고리즘을 공부하는 첫 단계로 그래프에서 쓰이는 알고리즘을 선택했다. 그 시작은 __위상정렬 알고리즘__ 이다.

    

## 위상정렬 이란?

영어로는 __topological sorting__ 이라고 한다. __순서가 명확히 정해져있는 것__ 들의 차례대로 정렬하기 위한 알고리즘이라고 보면 쉽다.  
위상정렬은 __DAG(Directed Acyclic Graph)에만 적용이 가능__ 하다. __DAG__ 는 각 노드가 다른 노드를 가리키는 방향이 있고 그 방향이 사이클을 발생시키지 않는 그래프를 뜻한다. 다시 말해 사이클이 발생하면 위상정렬을 적용할 수 없다.  
아래 그래프를 확인해 보자.  

![예제 그래프](/assets/img/topological-sort/graph-01.png)  

사이클을 발생시키지 않는 그래프이다. 이 그래프를 큐를 이용한 위상정렬을 통해 순차적으로 탐색하는 결과를 도출해 보겠다. 스택을 이용해서도 위상정렬을 한다고 한다. 하지만 난 큐를 이용하겠다.  
시작 노드는 0번 정점 이다. 0번 정점은 1번, 4번 정점 방향으로 간선이 연결되어 있다. 0번 정점의 진입차수(in-degree)는 0이다. 진입차수는 다른 정점에서 해당 정점 방향으로 연결되어 있는 간선의 수를 의미한다.

![예제 그래프](/assets/img/topological-sort/graph-01.png)
![예제 그래프](/assets/img/topological-sort/table-01.png)
![예제 그래프](/assets/img/topological-sort/queue-01.png)
![예제 그래프](/assets/img/topological-sort/array-01.png)
  
__진입차수가 0인 정점은 모두 큐에 담아야 한다.__ 0번 정점만 탐색을 했으므로 0번 정점을 큐에 담고 출력할 배열은 비워져 있다. __만약 큐가 비워져 있다면 사이클이 발생한다는 것을 의미한다.(큐가 비워지면 안된다)__  

![예제 그래프](/assets/img/topological-sort/graph-02.png)
![예제 그래프](/assets/img/topological-sort/table-02.png)
![예제 그래프](/assets/img/topological-sort/queue-02.png)
![예제 그래프](/assets/img/topological-sort/array-02.png)
  
큐를 __enqueue__ 해서 큐에 담긴 첫번째 노드를 정렬 결과 배열에 담는다. 1번 정점은 2번 정점 방향으로 간선이 연결되어 있고 4번 정점은 5번, 8번 정점으로 간선이 연결되어 있다. 1번, 4번 정점들은 __진입차수가 전부 1__ 이였으나 0번 정점이 큐에 담기고 __큐에 담기는 정점의 개수 만큼 진입차수에서 차감__ 한다. 그리고 진입차수가 0인 정점들은 앞서 한 작업과 마찬가지로 모두 큐에 담는다.  

![예제 그래프](/assets/img/topological-sort/graph-03.png)
![예제 그래프](/assets/img/topological-sort/table-03.png)
![예제 그래프](/assets/img/topological-sort/queue-03.png)
![예제 그래프](/assets/img/topological-sort/array-03.png)
  
위와 같은 작업을 반복한 결과이다.  

![예제 그래프](/assets/img/topological-sort/graph-04.png)
![예제 그래프](/assets/img/topological-sort/table-04.png)
![예제 그래프](/assets/img/topological-sort/queue-04.png)
![예제 그래프](/assets/img/topological-sort/array-04.png)
  
2번, 5번 정점이 같이 3번 정점 방향으로 간선이 연결되어 있다. 그러므로 3번 정점은 진입차수를 2만큼 차감해야 한다.  

![예제 그래프](/assets/img/topological-sort/graph-05.png)
![예제 그래프](/assets/img/topological-sort/table-05.png)
![예제 그래프](/assets/img/topological-sort/queue-05.png)
![예제 그래프](/assets/img/topological-sort/array-05.png)
  
8번 정점이 9번 정점 방향으로 간선이 연결되어 있으므로 9번 정점의 진입차수에서 1만큼 차감한다. 하지만 9번 정점의 진입차수는 아직 1이 남아 있으므로 큐에 담지 않는다.  

![예제 그래프](/assets/img/topological-sort/graph-06.png)
![예제 그래프](/assets/img/topological-sort/table-06.png)
![예제 그래프](/assets/img/topological-sort/queue-06.png)
![예제 그래프](/assets/img/topological-sort/array-06.png)
  
반복 작업이다.~~(쓰기 귀찮다)~~

![예제 그래프](/assets/img/topological-sort/graph-07.png)
![예제 그래프](/assets/img/topological-sort/table-07.png)
![예제 그래프](/assets/img/topological-sort/queue-07.png)
![예제 그래프](/assets/img/topological-sort/array-07.png)

마침내 9번정점의 진입차수가 0이되고 큐에 담기고 다음과 같은 결과를 얻을 수 있다.  

![예제 그래프](/assets/img/topological-sort/array-08.png)
  
다음 포스팅에서 자바스크립트로 구현해 보겠다. 왠지 자바스크립로 구현하면 쉬울거 같다.