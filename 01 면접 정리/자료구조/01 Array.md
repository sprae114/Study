
# 코드없는 프로그래밍 문제
## 개념정리(STABLE VS UNSTABLE)
-   stable![](https://api.transno.com/v3/document_image/71aacb8e-8d84-4a0c-b922-046c3fc3375f-10826299.jpg)  
정렬 한 이후에도, 순서 유지 된다면 STABLE

-   unstable![](https://api.transno.com/v3/document_image/2a94aa3f-eff2-44ab-a54c-57fc7f0a26d5-10826299.jpg)
정렬 한 이후에도, 순서 유지가 안된다면 UNSTABLE
## 704. Binary Search 🛠
문제 링크   [Binary Search - LeetCode](https://leetcode.com/problems/binary-search/)

해결해야될 문제
- binary search를 구현해보자
	- 그림으로 이해하기![[KakaoTalk_20221014_222558937.jpg]]

틀린점
- 언제까지 pivot을 반복해야 될까?
	left < right가 아닌 left <= right why? left == rigth일때 값이 맞는지 확인해야지!!

## 283. Move Zeroes 🛠
문제 링크 [Move Zeroes - LeetCode](https://leetcode.com/problems/move-zeroes/submissions/817048571/)

해결해야될 문제
- 배열 그 상태로 유지하면서, 0만 뒤로 뺄까?
	- [del과 append를 이용해서 0만 뒤로 보내자](https://leetcode.com/problems/move-zeroes/submissions/817048571/)
	- [0이 아닌 숫자와 0인 숫자와 swap하자, 이후 0을 정렬해야함 🛠](https://leetcode.com/problems/move-zeroes/submissions/817106184/)
		- 이해하기![](https://api.transno.com/v3/document_image/6441c121-3a0a-4725-84b5-6c663b51c74e-10826299.jpg)
	- [0이 아니면 앞에서부터 넣고 인덱스 증가 🛠](https://leetcode.com/problems/move-zeroes/submissions/817112323/)

틀린점
- 0인 경우에는 인덱스 유지 + 조건처리 , 아닌 경우에는 인덱스 증가 시키면 되는거 아닌가?
	- [0,0,0,0,0,0]인 경우에는? 처리했으니깐 범위를 -1 줄이자
- 버블 정렬하면 되는 거 아닌가?
	- 숫자 인덱스가 0인덱스 보다 작으면 안됌(순서 달라짐)


## 724. Find Pivot Index
문제 링크   [Find Pivot Index - LeetCode](https://leetcode.com/problems/find-pivot-index/description/)

해결해야될 문제
- [pivot을 기준으로 오른쪽 왼쪽 합이 일정한지 알고싶어](https://leetcode.com/problems/find-pivot-index/submissions/817058994/)
	- 이해하기![](https://api.transno.com/v3/document_image/b09da716-2d37-4d8e-ac68-939414dda905-10826299.jpg)

## 209. Minimum Size Subarray Sum 🛠
문제 링크   [Minimum Size Subarray Sum - LeetCode](https://leetcode.com/problems/minimum-size-subarray-sum/)

해결해야될 문제
- 최소 길이로 부분 집합의 합이 target인 것을 알고 싶어.
	- [슬라이싱을 이용해서, 최소합의 부분 집합을 구해보자. 그 이후, 합을 비교하여 가장 짧은 부분 집합 길이 찾기 -> timover](https://leetcode.com/problems/minimum-size-subarray-sum/submissions/817166560/)
	- [이중 for문을 Q(n)으로 나타낼 수 있을까?(일정하게 증가하는 상태)🛠](https://leetcode.com/problems/minimum-size-subarray-sum/submissions/817172926/)
	
틀린점
- 시간초과
	- 이중 for문을 하면 중복해서 계산하는 경우가 많아짐. + two pointer 이해 안감
		- 직접 그려보면서 이해해보자![[Pasted image 20221011144543.jpg]]

## 56. Merge Intervals
문제 링크   [Merge Intervals - LeetCode](https://leetcode.com/problems/merge-intervals/description/)

해결해야될 문제
- 어떻게 겹치는 구간을 합칠 수 있을까?
	- [구간을 정렬을 이용해서 포함된 유무를 따져보자](https://leetcode.com/problems/merge-intervals/submissions/817216992/)
	- 구간을 리스트로 나타내보자



# 파이썬 알고리즘 인터뷰