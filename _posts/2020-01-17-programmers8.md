---
layout: post
title:  "[프로그래머스 알고리즘] 소수 찾기"
date:   2020-01-17
excerpt: ""
tag:
- coding test 
comments: true
---

# 소수 찾기

## 문제 설명  

1부터 입력받은 숫자 n 사이에 있는 소수의 개수를 반환하는 함수, solution을 만들어 보세요.

소수는 1과 자기 자신으로만 나누어지는 수를 의미합니다.
(1은 소수가 아닙니다.)

## 제한 사항  
* n은 2이상 1000000이하의 자연수입니다


## 입출력 예  
  
|n|result|
|:---:|:---:|
|10|4|
|5|3|

  
## 입출력 예 설명
입출력 예 #1
1부터 10 사이의 소수는 [2,3,5,7] 4개가 존재하므로 4를 반환

입출력 예 #2
1부터 5 사이의 소수는 [2,3,5] 3개가 존재하므로 3를 반환



## 풀이
1. 에라토스테네스의 체 이용(2의배수 제거, 3의배수제거, 4의배수제거 ...)


## 푸는 과정에서의 오류
이중 for문에서 i*2 부분을 i*i로 코딩한 뒤 효율성테스트 및 10번 테스트케이스부터 에러가 났다.....


## 배운점




## 소스코드
~~~
class Solution {
  public int solution(int n) {
      int answer = 0;
	      
	      boolean[] isRight = new boolean[n+1];
	      for(int i=0;i<isRight.length;i++) {
              isRight[i] = true;
          }
            
	      isRight[0] = false;
	      isRight[1] = false;
	      
	      for(int i=2;i<isRight.length;i++) {
              if(isRight[i] == false) continue;
	          for(int ii=i*2;ii<isRight.length;ii+=i) {
	              isRight[ii] = false;
	          }
	      }
	      
	      for(int i=0;i<isRight.length;i++) {
	          if(isRight[i] == true) {
	              answer++;
	          }
	      }
	      
	      return answer;
  }
}
~~~
