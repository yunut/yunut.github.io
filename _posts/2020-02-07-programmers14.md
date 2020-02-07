---
layout: post
title:  "[프로그래머스 알고리즘] 다리를 지나는 트럭"
date:   2020-02-07
excerpt: ""
tag:
- coding test 
comments: true
---

# 다리를 지나는 트럭

## 문제 설명  
트럭 여러 대가 강을 가로지르는 일 차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 트럭은 1초에 1만큼 움직이며, 다리 길이는 bridge_length이고 다리는 무게 weight까지 견딥니다.
※ 트럭이 다리에 완전히 오르지 않은 경우, 이 트럭의 무게는 고려하지 않습니다.

예를 들어, 길이가 2이고 10kg 무게를 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.  

|경과시간|다리를 지난 트럭|다리를 건너는 트럭|대기 트럭|
|:---:|:---:|:---:|:---:|
|0|[]|[]|[7,4,5,6]|
|1~2|[]|[7]|[4,5,6]|
|3|[7]|[4]|[5,6]|
|4|[7]|[4,5]|[6]|
|5|[7,4]|[5]|[6]|
|6~7|[7,4,5]|[6]|[]|
|8|[7,4,5,6]|[]|[]|  

따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.

solution 함수의 매개변수로 다리 길이 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

## 제한 사항  
* bridge_length는 1 이상 10,000 이하입니다.
* 각 사람의 몸무게는 40kg 이상 240kg 이하입니다.
* truck_weights의 길이는 1 이상 10,000 이하입니다.
* 모든 트럭의 무게는 1 이상 weight 이하입니다.


## 입출력 예  
  
|bridge_length|weight|truck_weights|return|
|:---:|:---:|:---:|:---:|
|2|10|[7,4,5,6]|8|
|100|100|[10]|101|
|100|100|[10,10,10,10,10,10,10,10,10,10]|110|

  
## 입출력 예 설명




## 풀이
1. 시간과 무게를 가진 트럭배열 객체를 만들어서 시간을 관리한다.
2. 다리를 건너는 트럭에 해당되는 큐를 만들어, 각각의 시간마다 처리를 해준다.


## 푸는 과정에서의 오류
1. 처음 문제를 풀어보았을때, 각각의 트럭에 해당되는 시간을 객체로 만들어서 관리한다는 생각을 하지못하여 오래 걸렸던 문제입니다. 



## 배운점




## 소스코드
~~~
import java.util.*;

class Solution {
    public int solution(int bridge_length, int weight, int[] truck_weights) {
        int answer = 1;
        int waitIndex = 0;
        int truckIndex = 0;
        int weightCount=0;
        Queue<Truck> q1 = new LinkedList<>();
        Truck[] trucks = new Truck[truck_weights.length];
        
        for(int i=0;i<trucks.length;i++) {
        	trucks[i] = new Truck(bridge_length,truck_weights[i]);
        }
        
        q1.offer(trucks[0]);
        weightCount += truck_weights[waitIndex];
        waitIndex++;
        while(!q1.isEmpty() || waitIndex < truck_weights.length) {
        	answer++;
        	for(Truck truck : q1) {
        		truck.time--;
        	}
        	
        	if(trucks[truckIndex].time == 0) {
            	truckIndex++;
            	weightCount -= q1.poll().weight;
            } 
        	
        	if(waitIndex < truck_weights.length) {
	            if(weight >= weightCount + truck_weights[waitIndex]) {
	                q1.offer(trucks[waitIndex]);
	                weightCount += truck_weights[waitIndex];
	                waitIndex++;
	            }
        	}
            
               

        }
        return answer;
    }
}

class Truck {
	int time;
	int weight;
	
	public Truck(int time, int weight) {
		this.time = time;
		this.weight = weight;
	}
}
~~~
