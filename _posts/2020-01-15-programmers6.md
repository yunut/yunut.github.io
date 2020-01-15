---
layout: post
title:  "[프로그래머스 알고리즘] 문자열 내 p와 y의 개수"
date:   2020-01-15
excerpt: ""
tag:
- coding test 
comments: true
---

# 문자열 내 p와 y의 개수

## 문제 설명  

대문자와 소문자가 섞여있는 문자열 s가 주어집니다. s에 'p'의 개수와 'y'의 개수를 비교해 같으면 True, 다르면 False를 return 하는 solution를 완성하세요. 'p', 'y' 모두 하나도 없는 경우는 항상 True를 리턴합니다. 단, 개수를 비교할 때 대문자와 소문자는 구별하지 않습니다.

예를 들어 s가 pPoooyY면 true를 return하고 Pyy라면 false를 return합니다.

## 제한 사항  
* 문자열 s의 길이 : 50 이하의 자연수
* 문자열 s는 알파벳으로만 이루어져 있습니다.


## 입출력 예  
  
|s|answer|
|:---:|:---:|:---:|
|pPoooyY|true|
|Pyy|1|false|

  
## 입출력 예 설명
입출력 예 #1
'p'의 개수 2개, 'y'의 개수 2개로 같으므로 true를 return 합니다.

입출력 예 #2
'p'의 개수 1개, 'y'의 개수 2개로 다르므로 false를 return 합니다.



## 풀이
* 대소문자가 섞여있으니 string 클래스의 toUpperCase() 메소드나 toLowerCase() 메소드를 활용해 하나로 통일한뒤 갯수를 세서 비교해주면 됩니다.


## 푸는 과정에서의 오류



## 배운점
1. Comparator 함수 복습



## 소스코드
~~~
class Solution {
    boolean solution(String s) {
        boolean answer;
        int p_count=0;
        int y_count=0;

        for(int i=0;i<s.length();i++) {
            if(s.toUpperCase().charAt(i) == 'P') {
                p_count++;
            } else if(s.toUpperCase().charAt(i) == 'Y') {
                y_count++;
            }
        }
        
        if(p_count == y_count) {
            answer = true;
        } else {
            answer = false;
        }

        return answer;
    }
}
~~~
