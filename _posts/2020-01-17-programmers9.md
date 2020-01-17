---
layout: post
title:  "[프로그래머스 알고리즘] 스킬트리"
date:   2020-01-17
excerpt: ""
tag:
- coding test 
comments: true
---

# 스킬트리

## 문제 설명  

선행 스킬이란 어떤 스킬을 배우기 전에 먼저 배워야 하는 스킬을 뜻합니다.

예를 들어 선행 스킬 순서가 스파크 → 라이트닝 볼트 → 썬더일때, 썬더를 배우려면 먼저 라이트닝 볼트를 배워야 하고, 라이트닝 볼트를 배우려면 먼저 스파크를 배워야 합니다.

위 순서에 없는 다른 스킬(힐링 등)은 순서에 상관없이 배울 수 있습니다. 따라서 스파크 → 힐링 → 라이트닝 볼트 → 썬더와 같은 스킬트리는 가능하지만, 썬더 → 스파크나 라이트닝 볼트 → 스파크 → 힐링 → 썬더와 같은 스킬트리는 불가능합니다.

선행 스킬 순서 skill과 유저들이 만든 스킬트리1를 담은 배열 skill_trees가 매개변수로 주어질 때, 가능한 스킬트리 개수를 return 하는 solution 함수를 작성해주세요.

## 제한 사항  
* 스킬은 알파벳 대문자로 표기하며, 모든 문자열은 알파벳 대문자로만 이루어져 있습니다.
* 스킬 순서와 스킬트리는 문자열로 표기합니다.
예를 들어, C → B → D 라면 CBD로 표기합니다
* 선행 스킬 순서 skill의 길이는 1 이상 26 이하이며, 스킬은 중복해 주어지지 않습니다.
* skill_trees는 길이 1 이상 20 이하인 배열입니다.
* skill_trees의 원소는 스킬을 나타내는 문자열입니다.
skill_trees의 원소는 길이가 2 이상 26 이하인 문자열이며, 스킬이 중복해 주어지지 않습니다.
입출력 예


## 입출력 예  
  
|skill|skill_trees|return|
|:---:|:---:|:---:|
|"CBD"|"BACDE","CBADF","AECB","BDA"|2|
  
## 입출력 예 설명
BACDE: B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트립니다.
CBADF: 가능한 스킬트리입니다.
AECB: 가능한 스킬트리입니다.
BDA: B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트리입니다.



## 풀이
1. skill_trees 스트링 배열의 각각의 해당되는 문자열을 하나씩 자른다.
2. 자른 문자가 skill에 포함되지않았으면 다음 문자로 진행한다.
3. skill에 포함되어있으면 count변수를 활용해 전에 지정된 문자의 스킬을 포함했는지 확인한다.


## 푸는 과정에서의 오류
자른 문자가 skill에 포함되어있을때 어떻게 처리할지 고민했습니다. 처음에는 isRight 불리언 배열을 만들어 순서대로 처리했는지 확인하는방법을 생각했으나, 인덱스 처리해서 애를먹어 count변수를 활용해 올바른 순서대로 문자열이 배열되어있는지 확인한뒤, 배열되지않았으면 답에 포함하지 않는 방법으로 처리했습니다.


## 배운점
1. skill.contains() 부분에서 charAt으로 변환한것은 파라메터 오류가 났습니다. 확인해보니 contains는 String 자료형의 파라메터가 필요로해 charAt부분을 Character.toString()으로 감싸주어 String으로 형변환 하였습니다.



## 소스코드
~~~
class Solution {
    public int solution(String skill, String[] skill_trees) {
        int answer = 0;
        
        for(int i=0;i<skill_trees.length;i++) {
            Boolean isRight = true;
            int count = 0;
            for(int ii=0;ii<skill_trees[i].length();ii++) {
                if(!skill.contains(Character.toString(skill_trees[i].charAt(ii)))) {
                    continue;
                } else {
                    if(skill.charAt(count) == skill_trees[i].charAt(ii)) {
                        count++;
                    } else {
                        isRight = false;
                        break;
                    }
                }
            }
            if(isRight == true) {
                answer++;
            }
        }
        
        return answer;
    }
}
~~~
