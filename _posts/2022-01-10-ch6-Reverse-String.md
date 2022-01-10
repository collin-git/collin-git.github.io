# String Manipulation
## Coding Excercises & Lesson Learned

### Reverse String 문제
 - 문자열을 뒤집는 함수 작성. 리스트 내부를 직접 조작하기. 
 - https://leetcode.com/problems/reverse-string/

#### 나의 풀이
##### 활용 함수 및 기법
 - 인덱스를 활용한 swap
 - Flow 
    + 앞뒤 인덱스로 swap하여 reverse


```python
input_str = ['h','e','l','l','o']
```


```python
def reverseString(s:[]) -> None:
    num = len(s)
    for i in range(num//2):
        temp_str = input_str[num-1-i]
        input_str[num-1-i] = input_str[i]
        input_str[i] = temp_str
```


```python
reverseString(input_str)
```


```python
input_str
```




    ['o', 'l', 'l', 'e', 'h']



#### 교재 풀이 1. 투 포인터를 이용한 스왑
##### 활용 함수 & 기법
 - 한 번에 양쪽의 포인터를 교환하는 방식으로 swap
 - Flow 
    + 왼쪽, 오른쪽 각각 인덱스를 움직여 가면서 한번에 두 개를 교환하는 방법 


```python
input_str = ['h','e','l','l','o']
def reverseString(s: []) -> None:
    left, right = 0, len(s)-1
    while left < right:
        s[left], s[right] = s[right], s[left]
        left += 1
        right -= 1
```


```python
reverseString(input_str)
```


```python
input_str
```




    ['o', 'l', 'l', 'e', 'h']



#### 교재 풀이 2. 파이썬다운 방식
##### 활용 함수 & 기법
 - reverse() 함수 활용
 - Flow
    + list 내장 기능인 reverse() 함수 활용
 - 이외에도 s[:] = s[::-1] 와 같이 변경하는 방법도 활용 가능함


```python
input_str = ['h','e','l','l','o']
def reverseString(s: []) -> None:
    s.reverse()
```


```python
reverseString(input_str)
```


```python
input_str
```




    ['o', 'l', 'l', 'e', 'h']




```python

```
