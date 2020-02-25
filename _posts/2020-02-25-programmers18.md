---
layout: post
title:  "[프로그래머스 알고리즘] 정수 삼각형"
date:   2020-02-25
excerpt: ""
tag:
- coding test 
comments: true
---

# 2 X n 타일링

## 문제 설명  
![program](/photo/codingTest/TriangleInt.PNG)  
위와 같은 삼각형의 꼭대기에서 바닥까지 이어지는 경로 중, 거쳐간 숫자의 합이 가장 큰 경우를 찾아보려고 합니다. 아래 칸으로 이동할 때는 대각선 방향으로 한 칸 오른쪽 또는 왼쪽으로만 이동 가능합니다. 예를 들어 3에서는 그 아래칸의 8 또는 1로만 이동이 가능합니다.

삼각형의 정보가 담긴 배열 triangle이 매개변수로 주어질 때, 거쳐간 숫자의 최댓값을 return 하도록 solution 함수를 완성하세요.
  

## 제한 사항  
* 삼각형의 높이는 1 이상 500 이하입니다.
* 삼각형을 이루고 있는 숫자는 0 이상 9,999 이하의 정수입니다.


## 입출력 예  
  
|triangle|result|
|:---:|:---:|
|[[7], [3, 8], [8, 1, 0], [2, 7, 4, 4], [4, 5, 2, 6, 5]]|30|

  
## 입출력 예 설명




## 풀이
1. 지난번에 풀어보았던 땅따먹기 문제와 동일합니다. 밑에서부터 더했을때 가장 큰 경우가나오는 수를 배열에 저장하며 거슬러 올라가면 맨 꼭대기에는 가장 큰 수가 남습니다.



## 푸는 과정에서의 오류




## 배운점




## 소스코드
~~~
class Solution {
    public int solution(int[][] triangle) {
        int answer = 0;
        int i=triangle.length-2;
        while(i>=0) {
            for(int ii=0;ii<triangle[i].length;ii++) {
                if(triangle[i][ii] + triangle[i+1][ii] >= triangle[i][ii] + triangle[i+1][ii+1]) triangle[i][ii] += triangle[i+1][ii];
                else triangle[i][ii] += triangle[i+1][ii+1]; 
            }
            i--;
        }
        answer = triangle[0][0];
        return answer;
    }
}
~~~
