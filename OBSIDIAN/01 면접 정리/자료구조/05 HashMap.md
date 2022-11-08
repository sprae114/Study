# 코드없는 프로그래밍
## 1. Two Sum
문제 링크   [Two Sum - LeetCode](https://leetcode.com/problems/two-sum/submissions/814313010/)

해결해야될 문제
- Q(n)의 시간 복잡도로, 두개의 합이 target인 것을 구할까?
	- [dict에 숫자/인덱스를 이용해서 담은 후, for문으로 조회해보자](https://leetcode.com/problems/two-sum/submissions/814313010/)

틀린점
- [Python - 딕셔너리에 key가 있는지 확인](https://codechacha.com/ko/python-check-key-exists-in-dict/)
- 다음과 같은 문제점이 생기지만, 정상적인 경우만 테스트 나온다고함
-  6 - 3 = 3 처럼 3이 두개 필요한 경우
- 같은 숫자가 여러개 있는 경우


## 387. First Unique Character in a String
문제 링크   [First Unique Character in a String - LeetCode](https://leetcode.com/problems/first-unique-character-in-a-string/description/)

해결해야될 문제
- [어떻게 반복하지 않은 문자중에서 가장 먼저 나온 문자 찾을까](https://leetcode.com/problems/first-unique-character-in-a-string/submissions/814329940/)
	- dict에 알파벳을 넣은후, key값을 통해서 인덱스 비교해보자
	- 이후 find를 이용해서 위치 찾기

## 205. Isomorphic Strings 🛠
문제 링크   [Isomorphic Strings - LeetCode](https://leetcode.com/problems/isomorphic-strings/)

해결해야될 문제
- 현재 알파벳에 대응되는 다른 알파벳 있나 살펴보기
	- [dict를 통해 매칭되는 알파벳이 있는지 살펴보자 🛠](https://leetcode.com/problems/isomorphic-strings/submissions/814349554/)

틀린점
- 단방향이 아니라 양방향을 해야된다

## 242. Valid Anagram
문제 링크   https://leetcode.com/problems/valid-anagram/submissions/814663111/

해결해야될 문제
- 어떻게 해당 알파벳의 갯수가 동일한 것을 확인할 수 있을까?
	- [정렬하면 같으니깐 정렬하자](https://leetcode.com/problems/valid-anagram/submissions/814664273/)
	- [알파벳과 갯수를 모두 셀 수 있는 counter함수를 사용하자](https://leetcode.com/problems/valid-anagram/submissions/814663111/)

## 290. Word Pattern
문제 링크   [Word Pattern - LeetCode](https://leetcode.com/problems/word-pattern/submissions/814673544/)

해결해야될 문제
- [문자열 A,B의 패턴이 맞는지 동일한지 확인해보자](https://leetcode.com/problems/word-pattern/submissions/814673544/)

틀린점
- 변수 지칭 문제(s 와 s_list를 헷갈려함)


## 692. Top K Frequent Words
문제 링크   [Top K Frequent Words - LeetCode](https://leetcode.com/problems/top-k-frequent-words/description/)

해결해야될 문제
- [빈도수가 높은 순서대로 갯수를 세보자. 동일한 빈도수면 알파벳 순서대로](https://leetcode.com/problems/top-k-frequent-words/submissions/814768117/)

## 347. Top K Frequent Elements
문제 링크   [Top K Frequent Elements - LeetCode](https://leetcode.com/problems/top-k-frequent-elements/description/)

해결해야될 문제
- [빈도수가 높은 숫자를 K만큼 뽑아보자](https://leetcode.com/problems/top-k-frequent-elements/submissions/814763227/)

틀린점
- ZIP 함수

```
# zip(*iterable) : 동일한 개수로 이루어진 자료형을 묶어 줌 
a = ['one', 'two', 'three'] 
b = ['a', 'b', 'c'] 
list(zip(a, b)) 
>>> [('one', 'a'), ('two', 'b'), ('three', 'c')]
```

```
# 두 리스트의 요소 끄집어내기 
# zip 함수 사용한 방법 
for val1, val2 in zip(a, b): 
print(val1, val2) 

>>> one a 
>>> two b 
>>> three c
```

```
# 전치행렬 생성 
a = [[1, 2, 3], [4, 5, 6], [7, 8, 9]] 
b = list(zip(*a)) 
print(b) 

>>> [(1, 4, 7), (2, 5, 8), (3, 6, 9)]
```




## 560. Subarray Sum Equals K 🛠
문제 링크   [Subarray Sum Equals K - LeetCode](https://leetcode.com/problems/subarray-sum-equals-k/solutions/)

해결해야될 문제
- subarray을 직접 구하지 않고 합을 이용하는 방법
	- 누적합을 이용해서 리스트를 만들고, dict가 있는지 조회

틀린점
- 자기 자신을 지칭하는 문제
- 구간이 하나가 아니고 둘인 경우에도 한개만 세는 문제점


## 525. Contiguous Array
문제 링크   [Contiguous Array - LeetCode](https://leetcode.com/problems/contiguous-array/)

해결해야될 문제
- 어떻게 문자열 A에서 동일한 개수의 연속적인 부분 집합을 찾을까?
	- [기존 누적합 아이디어를 활용해서, 같은 숫자의 길이 중 최대 길이를 구해보자](https://leetcode.com/problems/contiguous-array/submissions/818677876/)

## 939. Minimum Area Rectangle 🛠
문제 링크   [Minimum Area Rectangle - LeetCode](https://leetcode.com/problems/minimum-area-rectangle/)

해결해야될 문제
- 좌표중에서 만들 수 있는 가장 작은 직사각형을 만드는 방법은?
	- [(x, y)가 둘다 다른 좌표를 두개 선택한 후, 서로 다른 두 좌표 (x,y)를 바꾼게 dict에 있는지 확인하자 -> timeover](https://leetcode.com/problems/minimum-area-rectangle/submissions/816506605/)
	- [(x, y)가 둘다 다른 좌표를 두개 선택한 후, 서로 다른 두 좌표 (x,y)를 바꾼게 set에 있는지 확인하자 🛠](https://leetcode.com/problems/minimum-area-rectangle/submissions/818699013/)


