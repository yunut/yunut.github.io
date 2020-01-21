---
layout: post
title:  "[프로그래머스 알고리즘] 이상한 문자 만들기"
date:   2020-01-22
excerpt: ""
tag:
- coding test 
comments: true
---

# 이상한 문자 만들기

## 문제 설명  

문자열 s는 한 개 이상의 단어로 구성되어 있습니다. 각 단어는 하나 이상의 공백문자로 구분되어 있습니다. 각 단어의 짝수번째 알파벳은 대문자로, 홀수번째 알파벳은 소문자로 바꾼 문자열을 리턴하는 함수, solution을 완성하세요.

## 제한 사항  
* 문자열 전체의 짝/홀수 인덱스가 아니라, 단어(공백을 기준)별로 짝/홀수 인덱스를 판단해야합니다.
* 첫 번째 글자는 0번째 인덱스로 보아 짝수번째 알파벳으로 처리해야 합니다.



## 입출력 예  
  
|s|return|
|:---:|:---:|:---:|
|"try hello world"|"TrY HeLlO WoRlD"|

  
## 입출력 예 설명
try hello world는 세 단어 try, hello, world로 구성되어 있습니다. 각 단어의 짝수번째 문자를 대문자로, 홀수번째 문자를 소문자로 바꾸면 TrY, HeLlO, WoRlD입니다. 따라서 TrY HeLlO WoRlD 를 리턴합니다.



## 풀이
1. 문자열을 공백 기준으로 split하여 각각의 문자열 짝수 홀수 인덱스에 따라 대문자 소문자로 나눈다.


## 푸는 과정에서의 오류
1. 처음에 알파벳상 인덱스인줄 착각하여, ASCII코드상의 홀수 짝수로 코드를 작성하였으나 실패...
2. 문제를 이해하고 split 메소드로 문자열을 잘랐으나 몇몇의 테스트 케이스에서 오류가 났다. 이 경우 테스트케이스에 중복된 공백이 존재하기에 발생하는걸로 생각해 split메소드 limit에 -1을 추가하였다.


## 배운점
split(문자열,리미트) 메소드에 limit 인수에 -1을 넣으면 공백도 포함해 적용된다는 점.



## 소스코드
~~~
class Solution {
  public String solution(String s) {
      String answer = "";
      String[] tmp = s.split("\\s", -1);
      
      for(int i=0;i<tmp.length;i++) {
          for(int ii=0;ii<tmp[i].length();ii++) {
              if(ii % 2 == 0) {
                  answer += Character.toString(tmp[i].charAt(ii)).toUpperCase();
              } else {
                  answer += Character.toString(tmp[i].charAt(ii)).toLowerCase();
              }
          }
          if(i != tmp.length-1 ){
              answer += " ";
          }
      }
      return answer;
  }
}
~~~
