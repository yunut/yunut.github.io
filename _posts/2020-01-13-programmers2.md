---
layout: post
title:  "[프로그래머스 알고리즘] 같은 숫자는 싫어"
date:   2020-01-13
excerpt: ""
tag:
- coding test 
comments: true
---

# 탑

## 문제 설명  

배열 arr가 주어집니다. 배열 arr의 각 원소는 숫자 0부터 9까지로 이루어져 있습니다. 이때, 배열 arr에서 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거하려고 합니다. 단, 제거된 후 남은 수들을 반환할 때는 배열 arr의 원소들의 순서를 유지해야 합니다. 예를 들면,  
* arr = [1, 1, 3, 3, 0, 1, 1] 이면 [1, 3, 0, 1] 을 return 합니다.
* arr = [4, 4, 4, 3, 3] 이면 [4, 3] 을 return 합니다.  
배열 arr에서 연속적으로 나타나는 숫자는 제거하고 남은 수들을 return 하는 solution 함수를 완성해 주세요.

## 제한 사항  
* 배열 arr의 크기 : 1,000,000 이하의 자연수
* 배열 arr의 원소의 크기 : 0보다 크거나 같고 9보다 작거나 같은 정수

## 입출력 예  
  
|arr|answer|
|:---:|:---:|
|[1,1,3,3,0,1,1]|[1,3,0,1]|
|[4,4,4,3,3]|[4,3]|

  
## 입출력 예 설명



## 풀이
* 첫 원소를 배열에 추가하고, 다른 숫자의 원소가 나올때마다 배열에 추가해준다. 


## 푸는 과정에서의 오류
~~~
static int[] Solution(int[] arr) {
		 ArrayList<Integer> answer_tmp = new ArrayList<>();
	        

	        for(int i=0;i<arr.length-1;i++) {
	            answer_tmp.add(arr[i]);
	            for(int ii=i+1;ii<arr.length;ii++) {
	                if(arr[i] == arr[ii]) {
	                    continue;
	                } else if(arr[i] != arr[ii]) {
	                    i=ii;
	                    break;
	                } 
	            }
	        }
	        
	        int[] answer = new int[answer_tmp.size()];
	        
	        for(int i=0;i<answer_tmp.size();i++) {
	            answer[i] = answer_tmp.get(i);
	        }

	        return answer;
		
	}

~~~
1. 처음 오류는 else if 구문에서 i를 바뀐 ii인덱스를 바꾸어 주는 과정에서 오류가 났다. 인덱스를 바꾸어주는건 좋앗지만, 그 후 for문에서 i가 증가가 되버려 잘못된 인덱스 참조가 일어난다.
2. 인덱스 참조를 바꿔줘도 기본 실행에서는 통과되지만 테스트케이스에서 오류가 많이 나기에 로직이 문제가 있다는 것을 생각해 로직을 바꿔, 반복문을 한번만 돌리고 숫자가 바뀔때만 배열에 추가하는 로직으로 변경하였다.



## 소스코드
~~~
import java.util.*;

public class Solution {
	public int[] solution(int []arr) {
        ArrayList<Integer> answer_tmp = new ArrayList<>();
	        int i=1;
	        int tmp;
	        tmp = arr[0];
	        answer_tmp.add(arr[0]);
	        while(i < arr.length) {
	        	if(tmp != arr[i]) {
	        		tmp = arr[i];
	        		answer_tmp.add(arr[i]);
	        	}
	        	i++;
	        }
	        
	        int[] answer = new int[answer_tmp.size()];
	        
	        for(i=0;i<answer_tmp.size();i++) {
	            answer[i] = answer_tmp.get(i);
	        }

	        return answer;
	}
}
~~~
