---
layout: post
title:  "[프로그래머스 알고리즘] 조이스틱"
date:   2020-01-16
excerpt: ""
tag:
- coding test 
comments: true
---

# 조이스틱

## 문제 설명  

조이스틱으로 알파벳 이름을 완성하세요. 맨 처음엔 A로만 이루어져 있습니다.
ex) 완성해야 하는 이름이 세 글자면 AAA, 네 글자면 AAAA

조이스틱을 각 방향으로 움직이면 아래와 같습니다.

▲ - 다음 알파벳  
▼ - 이전 알파벳 (A에서 아래쪽으로 이동하면 Z로)  
◀ - 커서를 왼쪽으로 이동 (첫 번째 위치에서 왼쪽으로 이동하면 마지막 문자에 커서)  
▶ - 커서를 오른쪽으로 이동   

예를 들어 아래의 방법으로 JAZ를 만들 수 있습니다.

- 첫 번째 위치에서 조이스틱을 위로 9번 조작하여 J를 완성합니다.
- 조이스틱을 왼쪽으로 1번 조작하여 커서를 마지막 문자 위치로 이동시킵니다.
- 마지막 위치에서 조이스틱을 아래로 1번 조작하여 Z를 완성합니다.
따라서 11번 이동시켜 "JAZ"를 만들 수 있고, 이때가 최소 이동입니다.  

만들고자 하는 이름 name이 매개변수로 주어질 때, 이름에 대해 조이스틱 조작 횟수의 최솟값을 return 하도록 solution 함수를 만드세요.

## 제한 사항  
* name은 알파벳 대문자로만 이루어져 있습니다.
* name의 길이는 1 이상 20 이하입니다.


## 입출력 예  
  
|name|return|
|:---:|:---:|:---:|
|JEROEN|56|
|JAN|23|

  
## 입출력 예 설명




## 풀이
1. A가 아닌 문자들의 조이스틱 이동 횟수를 먼저 더한다.
2. 1.조이스틱이 모두 좌->우로 이동한 경우  
2.조이스틱이 우->좌로 움직이는 경우
3.조이스틱이 양방향 모두로 움직이는 경우를 구해 이동횟수를 더한다.


## 푸는 과정에서의 오류
커서를 이동하는 과정에서 많은 오류가 발생하였다. 좌->우 우->좌 ->좌 우 같은 경우도 생각해야했기에 문제 출제의도는 탐욕법으로 푸는것이었지만, 탐욕법과는 별개의 알고리즘으로 풀어야 했다. 처음에는 임시 배열을 만들어서 A가있는칸은 1, 없는칸은 0으로 만들어 뒤로가는경우와 앞으로가는 경우를 비교해 1로만들고 다시 비교하는 알고리즘을 생각했는데, 오류가 많이 있었다.


## 배운점




## 소스코드
~~~
class Solution {
    public int solution(String name) {
        int answer = 0;
        char[] names = name.toCharArray();
        //65 'A' 90 'Z'

        //알파벳 이동에 필요한 수 계산
        for(int i=0;i<name.length();i++) {
            if(name.charAt(i) <= 'N') {
                answer += name.charAt(i) - 65;
            } else {
                answer += 91 - name.charAt(i) ;
            }
        }

        //커서 이동수 계산
        //제일 적게 움직이는 경우 좌->우 
        int min = name.length()-1;

        //A가 포함되어있을 경우 
        if(name.contains("A")) {
            int moveCnt=1;

            //우-> 좌로 이동하는 경우
            for(int i=1;i<names.length;i++) {
                if(names[i] != 'A') {
                    //총 길이에서 우로 이동하는 거리를 빼면 좌로 이동하는 수가 나온다.
                    moveCnt = names.length-i;
                    break;
                }
            }
            if(min > moveCnt) {
                min = moveCnt;
            }

            //좌,우 둘다 움직이는 경우
            int i=0;
            while( i<names.length) {
                int end=0;
                if( names[i] == 'A') {
                    end= i+1;
                    while(end < names.length && names[end] =='A') {
                        end++;
                    }

                    //모두 AAAA로 끝나는 경우
                    if(end == names.length) {
                        moveCnt = i-1;

                    //좌우로 움직여야 하는 상황의 경우 
                    } else {
                        //되돌아 가는 거리 + name의 마지막 인덱스 - 뒤로 움직이는 목표 인덱스
                        moveCnt = 1 + (i-1)*2 + names.length -1 -end;
                    }
                    if(moveCnt < min) {
                        min = moveCnt;
                    }

                    i = end+1;
                } else {
                    i++;
                }
            }
            if(min > moveCnt) {
                min = moveCnt;
            }
        }
        return answer+min;
    }
}
~~~
