---
layout: post
title:  "[프로그래머스 알고리즘] 문자열 내 마음대로 정렬하기"
date:   2020-01-15
excerpt: ""
tag:
- coding test 
comments: true
---

# 문자열 내 마음대로 정렬하기

## 문제 설명  

문자열로 구성된 리스트 strings와, 정수 n이 주어졌을 때, 각 문자열의 인덱스 n번째 글자를 기준으로 오름차순 정렬하려 합니다. 예를 들어 strings가 [sun, bed, car]이고 n이 1이면 각 단어의 인덱스 1의 문자 u, e, a로 strings를 정렬합니다.

## 제한 사항  
* strings는 길이 1 이상, 50이하인 배열입니다.
* strings의 원소는 소문자 알파벳으로 이루어져 있습니다.
* strings의 원소는 길이 1 이상, 100이하인 문자열입니다.
* 모든 strings의 원소의 길이는 n보다 큽니다.
* 인덱스 1의 문자가 같은 문자열이 여럿 일 경우, 사전순으로 앞선 문자열이 앞쪽에 위치합니다.

## 입출력 예  
  
|strings|n|return|
|:---:|:---:|:---:|
|["sun", "bed", "car"]|1|["car", "bed", "sun"]|
|["abce", "abcd", "cdx"]|1|["abcd", "abce", "cdx"]|

  
## 입출력 예 설명
입출력 예#1  
sun, bed, car의 1번째 인덱스 값은 각각 u, e, a 입니다. 이를 기준으로 strings를 정렬하면 [car, bed, sun] 입니다. 

입출력 예#2  
abce와 abcd, cdx의 2번째 인덱스 값은 c, c, x입니다. 따라서 정렬 후에는 cdx가 가장 뒤에 위치합니다. abce와 abcd는 사전순으로 정렬하면 abcd가 우선하므로, 답은 [abcd, abce, cdx] 입니다.



## 풀이
* 지정된 인덱스 기준으로 정렬하기위해 comparator 함수를 사용합니다.
* 지정된 인덱스가 같을경우 사전순으로 정렬하기위해 1. 오버라이딩된 compare 메소드 안에 재정렬하는 조건문을 넣어줍니다. 2. 먼저 사전순으로 정렬한 다음, 지정한 인덱스를 기준으로 재정렬합니다.  
2번째 풀이는 한번 정렬을 수행한뒤 다시 재정렬을 하기에 시간복잡도가 더 높아집니다. 1번방법으로 정렬 기준을 정한뒤, 지정된 인덱스가 같을경우 재정렬 하는 방법이 더 효율적입니다.


## 푸는 과정에서의 오류



## 배운점
1. Comparator 함수 복습



## 소스코드
~~~
import java.util.*;
class Solution {
  public String[] solution(String[] strings, int n) {
          String[] answer;
	      
	      Arrays.sort(strings);    
      
	      Arrays.sort(strings, new Comparator<String>() {
	          @Override
	          public int compare(String o1, String o2) {
	        	  
				/*
				 * if(o1.charAt(n) == o2.charAt(n)) { return o1.compareTo(o2); } else { return
				 * o1.charAt(n) - o2.charAt(n); }
				 */
	            return o1.charAt(n) - o2.charAt(n);
	          }
	      });
	      
	      answer = new String[strings.length];
	      for(int i=0;i<strings.length;i++) {
	          answer[i] = strings[i];
	      }
	      
	      return answer;
  }
}
~~~
