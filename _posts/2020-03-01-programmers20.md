---
layout: post
title:  "[프로그래머스 알고리즘] 네트워크"
date:   2020-03-01
excerpt: ""
tag:
- coding test 
comments: true
---

# 네트워크

## 문제 설명  
네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.  
  
컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

  

## 제한 사항  
* 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
* 각 컴퓨터는 0부터 n-1인 정수로 표현합니다.
* i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
* computer[i][i]는 항상 1입니다.

## 입출력 예  
  
|n|result|return|
|:---:|:---:|:---:|
|3|[[1, 1, 0], [1, 1, 0], [0, 0, 1]]|2|
|3|[[1, 1, 0], [1, 1, 1], [0, 1, 1]]|1|


  
## 입출력 예 설명




## 풀이
1. dfs 혹은 bfs로 방문한 정점을 체크해주고 완전탐색으로 진행하면 된다.



## 푸는 과정에서의 오류




## 배운점
다른사람의 풀이를 보고 Integer.bitCount의 메소드가 존재해 1의갯수를 더 편하게 셀수 있습니다.



## 소스코드
~~~
import java.util.*;
class Solution {
    private boolean[] check;
    private Queue<Integer> q;
    private int answer;
    public int solution(int n, int[][] computers) {
        answer = 0;
        check = new boolean[n];
        q = new LinkedList<>();
        for(int i=0;i<computers.length;i++) {
            if(check[i] == true) continue;
            q.offer(i);
            bfs(computers);
        }
        return answer;
    }
    
    public void bfs(int[][] computers) {
        int target;
        while(!q.isEmpty()) {
            target = q.poll();
            for(int i=0;i<computers[target].length;i++) {
                if(i == target) continue;
                if(computers[target][i] == 1 && check[i] == false) {
                    check[i] = true;
                    q.offer(i);
                }
            }
        }
        answer++;
    }
}
~~~
