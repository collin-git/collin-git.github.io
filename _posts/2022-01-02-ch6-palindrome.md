# String Manipulation
## Coding Excercises & Lesson Learned

### Palindrome 문제
 - 주어진 문자열을 뒤집었을 때도 같은 말이 되는 단어, 또는 문장인지 확인하라. 
 - https://leetcode.com/problems/valid-palindrome/

#### 나의 풀이
##### 활용 함수 및 기법
 - isalnum() : 해당 문자가 문자이거나 숫자인지 확인하여 Boolean 리턴
 - lower() : 해당 문자를 소문자로 변환하여 리턴
 - Flow 
    + indexing을 통해 start_ind, end_ind가 만날 때 까지 움직이면서 하나씩 비교하기
    + 문자이거나 숫자인 경우가 나올때 까지 각 index 이동
    + 두 문자를 소문자로 변환하여 비교해서 같으면 다음으로 넘어가고, 다르면 종료하고 False 반환
    + 이 작업을 끝까지해서 모두 같은 걸로 확인되면 True 반환


```python
input_str1 = "A man, a plan, a canal: Panama"
input_str2 = "race a car"
```


```python
def isPalindrome(s: str) -> bool:
    start_ind = 0
    end_ind = len(s)-1
    result = True
    
    while start_ind <= end_ind:
        while not s[start_ind].isalnum():
            start_ind += 1
        while not s[end_ind].isalnum():
            end_ind -= 1

        if s[start_ind].lower()==s[end_ind].lower():
            if start_ind<end_ind:
                start_ind += 1
                end_ind -= 1
            else:
                break
        else:
            result =  False
            break
    return result
print(isPalindrome(input_str1))
print(isPalindrome(input_str2))
```

    True
    False
    

#### 교재 풀이 1. 리스트로 변환
##### 활용 함수 & 기법
 - isalnum() : 해당 문자가 문자이거나 숫자인지 확인하여 Boolean 리턴
 - lower() : 해당 문자를 소문자로 변환하여 리턴
 - pop() : list의 pop 기능(지정 index의 원소를 꺼내서 리턴하고 리스트에서는 삭제)
 - Flow 
    + 문자인 경우만 소문자로 변환해서 리스트에 순서대로 넣어줌
    + List 내에 문자가 두 개 이상인 경우에 첫번째 문자와 마지막 문자를 꺼내서 비교하고, 다르면 False 반환
    + False가 반환되지 않는 경우(끝까지 같은 경우) True 반환


```python
def isPalindrome(s: str) -> bool:
    strs = []
    for char in s:
        if char.isalnum():
            strs.append(char.lower())
    while len(strs) > 1:
        if strs.pop(0) != strs.pop():
            return False
    return True
print(isPalindrome(input_str1))
print(isPalindrome(input_str2))
```

    True
    False
    

#### 교재 풀이 2. 데크 자료형을 이용한 최적화
##### 활용 함수 & 기법
 - collections.deque : 속도 향상을 위한 데크 활용 --> pop(0)은 O(n)인 반면, popleft()는 O(1) 으로 시간 단축 가능
 - isalnum() : 해당 문자가 문자이거나 숫자인지 확인하여 Boolean 리턴
 - lower() : 해당 문자를 소문자로 변환하여 리턴
 - pop() : list의 pop 기능(지정 index의 원소를 꺼내서 리턴하고 리스트에서는 삭제)
 - popleft() : 가장 먼저 들어온 원소(왼쪽 끝)를 pop
 - Flow (풀이 1과 동일)
    + 문자인 경우만 소문자로 변환해서 리스트에 순서대로 넣어줌
    + List 내에 문자가 두 개 이상인 경우에 첫번째 문자와 마지막 문자를 꺼내서 비교하고, 다르면 False 반환
    + False가 반환되지 않는 경우(끝까지 같은 경우) True 반환


```python
import collections
def isPalindrome(s: str) -> bool:
    strs: Deque = collections.deque()
    for char in s:
        if char.isalnum():
            strs.append(char.lower())
    while len(strs) > 1:
        if strs.popleft() != strs.pop():
            return False
    return True
print(isPalindrome(input_str1))
print(isPalindrome(input_str2))
```

    True
    False
    

#### 교재 풀이 3. 슬라이싱 사용
##### 활용 함수 & 기법
 - regex : 정규 방정식 활용하여 문자 필터링
    + re.sub() : 특정 정규방정식을 만족하는 부분에 대해서 다른 문자열로 대체함
 - lower() : 해당 문자를 소문자로 변환하여 리턴
 - Flow
    + 문자열 전체를 소문자로 변환 후, 필요한 문자열(숫자 또는 문자)에 해당되지 않는 것들은 ''으로 대체하여 삭제
    + 처리된 문자열과 뒤집은 문자열을 직접 비교하여 True, False 반환


```python
import re
def isPalindrome(s: str) -> bool:
    s = s.lower()
    s = re.sub('[^a-z0-9]','',s)
    return s==s[::-1]
print(isPalindrome(input_str1))
print(isPalindrome(input_str2))
```

    True
    False
    
