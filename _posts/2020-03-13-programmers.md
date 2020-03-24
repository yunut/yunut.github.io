---
layout: post
title:  "[프로그래머스 알고리즘] 소수찾기"
date:   2020-03-13
excerpt: ""
tag:
- coding test 
comments: true
---

# 소수 찾기

## 문제 설명  
한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.  
  
각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.  

  

## 제한 사항  
* numbers는 길이 1 이상 7 이하인 문자열입니다.
* numbers는 0~9까지 숫자만으로 이루어져 있습니다.
* 013은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.


## 입출력 예  
  
|numbers|return|
|:---:|:---:|
|"17"|3|
|"011"|2|  


  
## 입출력 예 설명
[1, 7]으로는 소수 [7, 17, 71]를 만들 수 있습니다.  
[0, 1, 1]으로는 소수 [11, 101]를 만들 수 있습니다.  
11과 011은 같은 숫자로 취급합니다.  



## 풀이
1. 주어진 numbers 배열의 원소를 정렬해 나올수 있는 최대 값을 구한다.
2. 에라토스테네스의 체로 최댓값까지의 소수를 구한다.
3. 백트래킹 완전탐색으로 탐색한다.


## 푸는 과정에서의 오류
백트래킹의 대한 이해가 부족해서 오래 걸렸던 문제입니다. 배열을 인자로 넘겨주면 함수 호출이 끝날 시 배열이 초기화 되는것으로 이해하고 있었습니다. 타 홈페이지에서 백트래킹 알고리즘을 몇개 풀어보고 풀수 있었습니다.  
처음 문제를 풀었을때, 나올 수 있는 숫자의 최댓값을 구하지않고 탐색한 숫자를 소수를 찾는 함수에 넘겨주는 것으로 풀었을때 몇개의 테스트케이스에서 시간초과가 났습니다.



## 배운점
백트래킹의 대한 이해



## 소스코드
~~~
import java.util.*;
class Solution {
    private static HashSet<Integer> set;
    private static boolean[] primecheck;
    
    public int solution(String numbers) {
        int answer = 0;
        String max = "";
        set = new HashSet<>();
        String[] sot = numbers.split("");
        Arrays.sort(sot);
        for(String tmp : sot) {
            max = tmp + max;
        }
        primeCheck(Integer.parseInt(max));
        
        
        for(int i=0;i<numbers.length();i++) {
            boolean[] check = new boolean[numbers.length()];
            check[i] = true;
            dfs(numbers,Integer.toString(numbers.charAt(i) - '0'),check);    
        }
        
        answer = set.size();
        System.out.println(answer);
        return answer;
    }
    public static void dfs(String numbers,String position,boolean[] check) {
		 	
	 	if(position.charAt(0) == '0') return;
	 	
	 	if(primecheck[Integer.parseInt(position)] == false) set.add(Integer.parseInt(position));
	 	
	 	if(position.length() == numbers.length()) return;
        
        for(int i=0;i<numbers.length();i++) {
            if(check[i] == false) {
                position += numbers.charAt(i) - '0';
                check[i] = true;;
                dfs(numbers,position,check);
                check[i] = false;
                position = position.substring(0,position.length()-1);
                
            }
        }
	 }
	 
	 public static boolean primeCheck(int num) {
		 primecheck = new boolean[num+1];
		 
		 primecheck[0] = true;
		 primecheck[1] = true;
		 
		 for(int i=2;i<=num/2+1;i++) {
			 for(int ii=i*2;ii<=num;ii+=i) {
				 primecheck[ii] = true;
			 }
		 }
		 
		 return primecheck[num];
	 }
	

}
~~~
