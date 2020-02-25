---
layout: post
title:  "[프로그래머스 알고리즘] 2 X n 타일링"
date:   2020-02-24
excerpt: ""
tag:
- coding test 
comments: true
---

# 2 X n 타일링

## 문제 설명  
가로 길이가 2이고 세로의 길이가 1인 직사각형모양의 타일이 있습니다. 이 직사각형 타일을 이용하여 세로의 길이가 2이고 가로의 길이가 n인 바닥을 가득 채우려고 합니다. 타일을 채울 때는 다음과 같이 2가지 방법이 있습니다.

타일을 가로로 배치 하는 경우
타일을 세로로 배치 하는 경우
예를들어서 n이 7인 직사각형은 다음과 같이 채울 수 있습니다.  
![program](/photo/codingTest/TwoMulNTile.PNG)  

직사각형의 가로의 길이 n이 매개변수로 주어질 때, 이 직사각형을 채우는 방법의 수를 return 하는 solution 함수를 완성해주세요.
  

## 제한 사항  
* 가로의 길이 n은 60,000이하의 자연수 입니다.
* 경우의 수가 많아 질 수 있으므로, 경우의 수를 1,000,000,007으로 나눈 나머지를 return해주세요.


## 입출력 예  
  
|n|result|
|:---:|:---:|
|4|5|

  
## 입출력 예 설명




## 풀이
1. 처음 풀이를 생각할때는 완전탐색을 생각하여 dfs로 구현하였습니다. 하지만 가로의길이가 60,000이기때문에 dfs완전탐색을하면 시간초과 오류가 나왔습니다. 그래서 가로 길이가 1,2,3,4,5 일때는 모두 구해보니 피보나치 수 경우가 나와 피보나치 풀이로 문제를 해결하였습니다.

* 길이가 1 늘어났을경우, 직전 경우에서 한가지 방법 + 2번전 경우에서 한가지 방법이 추가되어 피보나치가 맞습니다.



## 푸는 과정에서의 오류
1. 가로의 길이가 60,000이기때문에 dfs로 구현하면 시간초과 오류가 난다.



## 배운점




## 소스코드
~~~
class Solution {
    
    int answer;
    public int solution(int n) {
        answer = 0;
        int[] num = new int[n+1];
        num[1] = 1;
        num[2] = 2;
        
        for(int i=3;i<=n;i++) {
            num[i] = (num[i-1]+num[i-2]) % 1000000007;
        }
        answer = num[n];
        return answer % 1000000007;
    }
    
    /* 처음 풀이할때의 dfs 풀이*/
    public void dfs(int n) {
        if(n == 0) {
            answer++;
            return;
        }
        if(n < 0) {
            return;
        }
        
        dfs(n-1);
        dfs(n-2);
    }
}
~~~