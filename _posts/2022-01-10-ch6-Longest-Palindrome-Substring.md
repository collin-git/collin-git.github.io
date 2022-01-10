# String Manipulation
## Coding Excercises & Lesson Learned

### Longest  Palindrome Substring 문제
 - 문자열 내의 가장 긴 팰린드롬 부분 문자열을 출력
 - https://leetcode.com/problems/longest-palindromic-substring/

#### 나의 풀이
##### 활용 함수 및 기법
 - indexing 만 활용
 - Flow 
    + palindrome의 유형을 크게 두가지로 지정 : Case 1. 중앙 기준점에서 두개가 동일해서 하나로 봐주고 앞뒤 검색하는 경우, Case 2. 중앙 기준점을 중심으로 앞뒤를 쭉 검색해 나가는 경우
    + 아래 반복 (start_ind = 1 ~ len(s)-1 까지)
        + Case 1 Flow
            - 기준점과 그 앞의 문자가 같으면,
                + 두 개 짜리 문자열이면 바로 문자열 자체를 반환
                + (prev_ind < 0) 시작점이면 뒤로만 검색해 나가면서 똑같으면 붙여나감 (다 같은 문자인 경우에 해당)
                + (post_ind>len(s)-1) 끝점이면 앞으로만 검색해 나가면서 똑같으면 붙여나감 (다 같은 문자인 경우에 해당)
                + 둘다 아니라면 prev_ind, post_ind를 통해 앞뒤로 한칸씩 이동하면서 똑같으면 붙여나감
        + Longest Update
        + Case 2 Flow
            - 하나를 중심으로 앞뒤로 쭉 검색해 나가면서 똑같으면 붙여나감
        + Longest Update
 - 비효율적인 검색 부분이 많이 있는 것 같아서(중복), 아쉽지만 모든 경우들을 고려해주기 위해서 노력했다.. 교재의 코드를 보면서 다시 공부할 필요가 있다.


```python
def longest_palindrome( s: str) -> str:
    longest_palin = s[0]
    for start_ind in range(1, len(s)):
        palin = s[start_ind]
        # same - two start_ind
        if s[start_ind]==s[start_ind-1]:
            palin = s[start_ind-1]+s[start_ind] 
            prev_ind, post_ind = start_ind-2, start_ind+1
            if prev_ind < 0 and post_ind > len(s)-1:
                longest_palin = palin
                return longest_palin
            elif prev_ind < 0:
                while s[start_ind]==s[post_ind]:
                    palin = palin + s[post_ind]
                    post_ind += 1
                    if post_ind > len(s)-1:
                        break
            elif post_ind > len(s)-1:
                while s[start_ind]==s[prev_ind]:
                    palin = s[prev_ind] + palin
                    prev_ind -= 1
                    if prev_ind < 0:
                        break
            else:
                while s[prev_ind]==s[post_ind]:
                    palin = s[prev_ind] + palin + s[post_ind]
                    prev_ind -= 1
                    post_ind += 1
                    if prev_ind < 0 or post_ind > len(s)-1:
                        break
        if len(palin)>len(longest_palin):
            longest_palin = palin
        
        # one start_ind
        palin = s[start_ind]
        prev_ind, post_ind = start_ind-1, start_ind+1
        if prev_ind >= 0 and post_ind <= len(s)-1:
            while s[prev_ind]==s[post_ind]:
                palin = s[prev_ind] + palin + s[post_ind]
                prev_ind -= 1
                post_ind += 1
                if prev_ind < 0 or post_ind > len(s)-1:
                    break
        if len(palin)>len(longest_palin):
            longest_palin = palin
        
    return longest_palin
        
        
```


```python
s = "babad"
longest_palindrome(s)
```




    'bab'



#### 교재 풀이 1. 중앙을 중심으로 확장하는 풀이
##### 활용 함수 & 기법
 - indexing 및 기본함수 max만 활용
 - Flow 
    + 전진하는 두 개의 인덱스(둘 또는 셋으로 이루어진)에서 팰린드롬 패턴을 발견
    + 발견되면 추가적으로 계속 찾아나가면서 패턴 발견
    + 처음부터 끝까지 전진해서 찾기 완료하고, 가장 긴 것을 반환


```python
def longest_palindrome( s: str) -> str:
    def expand(left:int, right:int) -> str:
        while left>=0 and right<len(s) and s[left]==s[right]:
            left -= 1
            right += 1
        return s[left+1:right]
    
    if len(s)<2 or s==s[::-1]:
        return s
    
    result = ''
    for i in range(len(s)-1):
        result = max(result, expand(i, i+1), expand(i, i+2), key=len)
    
    return result 
```


```python
s = "babad"
longest_palindrome(s)
```




    'bab'




```python

```
