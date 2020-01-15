---
layout: post
title:  "[프로그래머스 알고리즘] 가장 큰 수"
date:   2020-01-14
excerpt: ""
tag:
- coding test 
comments: true
---

# 가장 큰 수

## 문제 설명  

0 또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내 주세요.

예를 들어, 주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이중 가장 큰 수는 6210입니다.

0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요.

## 제한 사항  
* numbers의 길이는 1 이상 100,000 이하입니다.
* numbers의 원소는 0 이상 1,000 이하입니다.
* 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.

## 입출력 예  
  
|numbers|return|
|:---:|:---:|
|[6, 10, 2]|"6210"|
|[3, 30, 34, 5, 9]|"9534330"|

  
## 입출력 예 설명



## 풀이
* 기존의 정렬방식으로는 풀수 없는 문제, 정렬방식의 기준을 세워야 한다.
* Comparator 함수로 정렬 기준을 세워 정렬한다.


## 푸는 과정에서의 오류
1. Comparator 함수에 대해 이해가 명확히 되지 않아 정렬 기준을 세우는데 오류가 있었다.
2. 숫자가 0으로만 주어질때 return값이 "0"으로 나오는 코드를 만들지 않았다.


## 배운점
1. Comparator 함수로 각각의 원소를 기준에 맞게 비교할수 있다.
2. `return (o2+o1).compareTo(o1+o2)` 같은 compareTo 함수에 관해 명확하게 알지 못했는데 명확하게 알수 있었다. 예를들어 위에 코드는 o2+o1의 문자열을 합친것고 o1+o2 문자열을 합친 것에대해 비교해 양수, 음수 를 반환해 정렬 기준을 삼는다. 



## 소스코드
~~~
import java.util.*;
class Solution {
    public String solution(int[] numbers) {
       String answer = "";
	        
	        //합친 문자열을 비교하기위한 String 배열 선언
	        String[] number_tmp = new String[numbers.length];
	        
	        //int형 배열을 String배열에 저장
	        for(int i=0;i<numbers.length;i++) {
	            number_tmp[i] = String.valueOf(numbers[i]);
	        }
	        
	        
	        Arrays.sort(number_tmp, new Comparator<String>() {
	            @Override
	            public int compare(String o1, String o2) {
	                return (o2+o1).compareTo(o1+o2);
	            }
	        });
	        if(number_tmp[0].equals("0")) {
                answer += "0";
            } else {
                for(int i=0;i<number_tmp.length;i++) {
                    answer += number_tmp[i];
                }
            }
	        
	        return answer;
	    }
}
~~~
