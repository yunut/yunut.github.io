---
layout: post
title:  "[프로그래머스 알고리즘] 가장 큰 수"
date:   2020-01-13
excerpt: ""
tag:
- coding test 
comments: true
---

# 나누어 떨어지는 숫자 배열

## 문제 설명  

array의 각 element 중 divisor로 나누어 떨어지는 값을 오름차순으로 정렬한 배열을 반환하는 함수, solution을 작성해주세요.
divisor로 나누어 떨어지는 element가 하나도 없다면 배열에 -1을 담아 반환하세요.

## 제한 사항  
* arr은 자연수를 담은 배열입니다.
* 정수 i, j에 대해 i ≠ j 이면 arr[i] ≠ arr[j] 입니다.
* divisor는 자연수입니다.
* array는 길이 1 이상인 배열입니다.

## 입출력 예  
  
|arr|divisor|return|
|:---:|:---:|:---:|
|[[5,9,7,10]|5|[5,10]|
|[2,36,1,3]]|1|[1,2,3,36]|
|[3,2,6]|10|[-1]|

  
## 입출력 예 설명
입출력 예#1  
arr의 원소 중 5로 나누어 떨어지는 원소는 5와 10입니다. 따라서 [5, 10]을 리턴합니다.  

입출력 예#2  
arr의 모든 원소는 1으로 나누어 떨어집니다. 원소를 오름차순으로 정렬해 [1, 2, 3, 36]을 리턴합니다.  

입출력 예#3  
3, 2, 6은 10으로 나누어 떨어지지 않습니다. 나누어 떨어지는 원소가 없으므로 [-1]을 리턴합니다.  


## 풀이
* 가장 큰 수 문제와 비교해서 정렬 기준을 삼지않아도 되고 ArrayList 배열을 하나 선언해 Collection.sort로 오름차순 정렬 시켜주면 간단히 문제가 해결됩니다.


## 푸는 과정에서의 오류



## 배운점
1. Array.sort() 는 int[] 같은 정적배열에 사용 Collection.sort() 는 ArrayList같은 리스트 배열에 사용



## 소스코드
~~~
import java.util.*;

class Solution {
  public int[] solution(int[] arr, int divisor) {
      int[] answer;
      ArrayList<Integer> tmp = new ArrayList<>();
      for(int i=0;i<arr.length;i++) {
          if(arr[i] % divisor == 0) {
              tmp.add(arr[i]);
          }
      }
      Collections.sort(tmp);
      
      if(tmp.size() == 0) {
          answer = new int[1];
          answer[0] = -1;
      } else {
         answer = new int[tmp.size()];
         for(int i=0;i<tmp.size();i++) {
            answer[i] = tmp.get(i);
         };
      }
      
      return answer;
  }
}
~~~
