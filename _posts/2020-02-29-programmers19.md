---
layout: post
title:  "[프로그래머스 알고리즘] 다음 큰 숫자"
date:   2020-02-29
excerpt: ""
tag:
- coding test 
comments: true
---

# 다음 큰 숫자

## 문제 설명  
자연수 n이 주어졌을 때, n의 다음 큰 숫자는 다음과 같이 정의 합니다.  
조건 1. n의 다음 큰 숫자는 n보다 큰 자연수 입니다.  
조건 2. n의 다음 큰 숫자와 n은 2진수로 변환했을 때 1의 갯수가 같습니다.  
조건 3. n의 다음 큰 숫자는 조건 1, 2를 만족하는 수 중 가장 작은 수 입니다.  
예를 들어서 78(1001110)의 다음 큰 숫자는 83(1010011)입니다.  

자연수 n이 매개변수로 주어질 때, n의 다음 큰 숫자를 return 하는 solution 함수를 완성해주세요.

  
## 제한 사항  
* n은 1,000,000 이하의 자연수 입니다.

## 입출력 예  
  
|n|result|
|:---:|:---:|
|78|83|
|15|23|

  
## 입출력 예 설명




## 풀이
1. 알고리즘 자체는 문제에 다 주어져있기떄문에 구현만 하면 되는 문제입니다. 인트형 자료형 n을 2진수로바꿔 String 자료형에 저장한다음 1의 갯수를 구해줍니다.
2. n을 1씩 증가시키며 1의 갯수를 비교해 n과 갯수가같으면 answer입니다.



## 푸는 과정에서의 오류




## 배운점
다른사람의 풀이를 보고 Integer.bitCount의 메소드가 존재해 1의갯수를 더 편하게 셀수 있습니다.



## 소스코드
~~~
class Solution {
  public int solution(int n) {
      int answer = 0;
	      String binaryTmp = Integer.toBinaryString(n);
	      int binaryLen = binaryTmp.replace("0", "").length();
	      answer = n+1;
	      while(true) {

              binaryTmp = Integer.toBinaryString(answer).replace("0","");
              if(binaryTmp.length() == binaryLen) break;    
	          answer++;
	          
	      } 
	      return answer;
  }
}
~~~
