---
layout: post
title:  "[프로그래머스 알고리즘] 시저 암호"
date:   2020-01-20
excerpt: ""
tag:
- coding test 
comments: true
---

# 시저 암호

## 문제 설명  

어떤 문장의 각 알파벳을 일정한 거리만큼 밀어서 다른 알파벳으로 바꾸는 암호화 방식을 시저 암호라고 합니다. 예를 들어 AB는 1만큼 밀면 BC가 되고, 3만큼 밀면 DE가 됩니다. z는 1만큼 밀면 a가 됩니다. 문자열 s와 거리 n을 입력받아 s를 n만큼 민 암호문을 만드는 함수, solution을 완성해 보세요.

## 제한 사항  
* 공백은 아무리 밀어도 공백입니다.
* s는 알파벳 소문자, 대문자, 공백으로만 이루어져 있습니다.
* s의 길이는 8000이하입니다.
* n은 1 이상, 25이하인 자연수입니다.



## 입출력 예  
  
|s|n|result|
|:---:|:---:|:---:|
|"AB"|1|"BC"|
|"z"|1|"a"|
|"a B z"|4|"e F d"|
  
## 입출력 예 설명




## 풀이
1. StringBulider 클래스 replace를 이용해 글자를 변경
2. ASCII코드 .대문자: 65~90 소문자:97~122 를 이용해 문자가 z -> a 혹은 Z->A로 갈때 -26을해서 첫 알파벳부터 시작하게 변경


## 푸는 과정에서의 오류
1. z->a 같이 첫 알파벳으로 이동할때 먼저 글자를 이동시킨뒤, -25를 하여 오류가 발생하였습니다. ASCII코드로 90 -> 65로 이동될시에 먼저 이동한다음에 처리되기에 처리방식이 91 -> -25를해서 66으로 이동되기에 -26으로 처리해주어야합니다.
2. Z->A 이동 시 대문자는 ASCII코드 65~90, 소문자는 97~122기에 Z를 25번 이동한다 할시에 소문자로 처리되는 오류가있었습니다. 이를 isLowerCase()로 구분해 처리하였습니다.


## 배운점
1. 문자열처리, isUpperCASE() 등



## 소스코드
~~~
class Solution {
  public String solution(String s, int n) {
      String answer = "";
      StringBuilder str = new StringBuilder();
      str.append(s);

      for(int i=0;i<str.length();i++) {
          char tmp = str.charAt(i);
          //소문자일때
          if((tmp != 32) && Character.isLowerCase(tmp)) {
              str.replace(i,i+1,Character.toString((char) (str.charAt(i) + n)));
              if(str.charAt(i) > 122) {
                  str.replace(i,i+1,Character.toString((char) (str.charAt(i) - 26)));
              }
          }


          //대문자일때
          if((tmp != 32) && Character.isUpperCase(tmp)) {
              str.replace(i,i+1,Character.toString((char) (str.charAt(i) + n)));
              if(str.charAt(i) > 90) {
                  str.replace(i,i+1,Character.toString((char) (str.charAt(i) - 26)));
              }
          }

      }

      for(int i=0;i<str.length();i++) {
          answer = str.toString();
      }
      return answer;
  }
}
~~~
