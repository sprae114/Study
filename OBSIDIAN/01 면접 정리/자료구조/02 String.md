# 코드없는 프로그래밍
## 28. Find the Index of the First Occurrence in a String
문제 링크   [Find the Index of the First Occurrence in a String - LeetCode](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)

해결해야될 문제
- 어떻게 문자열 A에 문자열 B가 포함 인덱스의 위치는?
	- 단순 포함은 in을 쓰면 되는데 인덱스를 찾아야함.
	- [for문과 slicing을 이용해서 일치하는 지 확인하자](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/submissions/814186541/)

## 796. Rotate String
문제 링크   [Rotate String - LeetCode](https://leetcode.com/problems/rotate-string/submissions/814189392/)

해결해야될 문제
- 어떻게 문자열 A가 순환해서 문자열 B가 되는지 확인할까?
	- [리스트 만들어서 넣었다가 뺐다가 반복](https://leetcode.com/problems/rotate-string/submissions/818457671/)
	- [문자열 두개 붙여서 확인하기](https://leetcode.com/problems/rotate-string/submissions/814189392/)

## 125. Valid Palindrome
문제 링크   [Valid Palindrome - LeetCode](https://leetcode.com/problems/valid-palindrome/description/)

해결해야될 문제
- 어떻게 할린드롬인지 확인할까?
	- [뒤집어서 일치하는지 확인](https://leetcode.com/problems/valid-palindrome/submissions/814196751/)
	- [two point를 활용한 문자열 확인](https://leetcode.com/problems/valid-palindrome/submissions/523376536/)

## 415. Add Strings 
문제 링크   [Add Strings - LeetCode](https://leetcode.com/problems/add-strings/description/)

해결해야될 문제
- [어떻게 계산기 구현할까? 🛠](https://leetcode.com/problems/add-strings/submissions/814208032/)
- [어떻게 문자를 연산할까? 🛠](https://leetcode.com/problems/add-strings/submissions/814199424/)
- 서로 다른 자리수를 동일하게 맞추려면?
- 앞에서부터 시작이 아니고 뒤에서부터 시작이네
	- 뒤에부터 시작하거나 reverse를 이용하거나


틀린점
- (반례) 자릿수는 끝났는데 carry가 0이 아닌 경우
	- "9" + "0"인경우
## 49. Group Anagrams
문제 링크   [Group Anagrams - LeetCode](https://leetcode.com/problems/group-anagrams/)

해결해야될 문제
- 배열만 다른 문자열을 구별하는 방법은?
	- [정렬해서 동일한지 확인하기](https://leetcode.com/problems/group-anagrams/submissions/523844293/)
	- 알파벳 갯수 세기
## 3. Longest Substring Without Repeating Characters 🛠
문제 링크   [Longest Substring Without Repeating Characters - LeetCode](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

해결해야될 문제
- 문자열에서 반복하지 않는 가장 긴 구간 찾기
	-  [dict를 통해서 알파벳 해당 구간에 알파벳 갯수를 체크하자 🛠](https://leetcode.com/problems/longest-substring-without-repeating-characters/submissions/818470098/)


