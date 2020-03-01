---
layout: post
title:  "[프로그래머스 알고리즘] 단속카메라"
date:   2020-03-01
excerpt: ""
tag:
- coding test 
comments: true
---

# 단속카메라

## 문제 설명  
고속도로를 이동하는 모든 차량이 고속도로를 이용하면서 단속용 카메라를 한 번은 만나도록 카메라를 설치하려고 합니다.  
  
고속도로를 이동하는 차량의 경로 routes가 매개변수로 주어질 때, 모든 차량이 한 번은 단속용 카메라를 만나도록 하려면 최소 몇 대의 카메라를 설치해야 하는지를 return 하도록 solution 함수를 완성하세요

  

## 제한 사항  
* 차량의 대수는 1대 이상 10,000대 이하입니다.
* routes에는 차량의 이동 경로가 포함되어 있으며 routes[i][0]에는 i번째 차량이 고속도로에 진입한 지점, routes[i][1]에는 i번째 차량이 고속도로에서 나간 지점이 적혀 있습니다.
* 차량의 진입/진출 지점에 카메라가 설치되어 있어도 카메라를 만난것으로 간주합니다.
* 차량의 진입 지점, 진출 지점은 -30,000 이상 30,000 이하입니다.

## 입출력 예  
  
|routes|return|
|:---:|:---:|
|[[-20,15], [-14,-5], [-18,-13], [-5,-3]]|2|


  
## 입출력 예 설명
-5 지점에 카메라를 설치하면 두 번째, 네 번째 차량이 카메라를 만납니다.  
-15 지점에 카메라를 설치하면 첫 번째, 세 번째 차량이 카메라를 만납니다.  



## 풀이
1. 고속도로를 나간 지점을 기준으로 2차원 배열을 정렬한다.
2. 첫 고속도로를 나간 지점을 타겟으로 하여 배열을 순회한다. 
3. 들어오는 지점과 나가는 지점 사이에 타겟이 존재하지않으면 정답을 증가시키고 타겟을 해당 인덱스의 나가는 지점으로 변경한다.



## 푸는 과정에서의 오류




## 배운점
1차원 배열의 정렬 메소드는 많이 해봤지만, 2차원배열로 기준을 삼아 정렬하는 comparator 익명함수가 떠오르지않아 다시 검색하고 풀었습니다.



## 소스코드
~~~
import java.util.*;
class Solution {
    public int solution(int[][] routes) {
        int answer = 1;
        
        Arrays.sort(routes, (o1,o2) -> {
				return Integer.compare(o1[1], o2[1]);
		});
        
        int target = routes[0][1];
        for(int i=0;i<routes.length;i++) {
        	if(target >= routes[i][0] && target <= routes[i][1]) {
        		continue;
        	} else {
        		answer++;
        		target = routes[i][1];
        	}
        }
        if(!(target >= routes[routes.length-1][0] && target <= routes[routes.length-1][1])) {
        	answer++;
        }
        System.out.println(answer);
        return answer;
    }
}
~~~
