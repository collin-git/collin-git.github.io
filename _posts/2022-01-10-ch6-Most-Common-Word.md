# String Manipulation
## Coding Excercises & Lesson Learned

### Most Common Word 문제
 - 금지된 단어를 제외하고 가장 자주 등장하는 단어 리턴 (대소 구분 및 구두점 무시)
 - https://leetcode.com/problems/most-common-word/

#### 나의 풀이
##### 활용 함수 및 기법
 - isalpha() : 특정 문자가 알파벳인지 아닌지 판단
 - lower() : 소문자로 변환
 - collections.Counter() : 원소의 개수를 dict 형태로 리턴
 - re.split() : 특정 문자열을 delimeter로 하여 문자열 분리
 - pop() : dict에서는 특정 key에 해당하는 원소를 뽑아내고 삭제
 - Flow 
    + 문자열을 공백이나 쉽표를 기준으로 분리
    + 알파벳에 해당되는 문자만 소문자로 변환하여 모으기
    + Counter 함수를 활용하여 원소별 개수 확인
    + 가장 많이 나온 문자열에 해당하는 key 값을 반환하되,banned에 해당하는 것이 나오면 삭제
    + 이를 반복하여 다음으로 큰 것을 찾는 식으로 banned 제외하고 가장 많이 나온 원소를 찾음 


```python
import collections
import re
paragraph = "Bob hit a ball, the hit BALL flew far after it was hit."
banned = ["hit"]
```


```python
def most_common_word(paragraph: str, banned: []) -> str:
    # s_list = paragraph.split(' ')
    s_list = re.split(',| ',paragraph)
    al_list = []
    for s in s_list:
        temp_s = ''
        for c in s:
            if c.isalpha():
                temp_s += c.lower()
        if temp_s!='':
            al_list.append(temp_s)
    cnt = collections.Counter(al_list)
    while True:
        max_val = max(list(cnt.values()))
        max_key = list(cnt.keys())[list(cnt.values()).index(max_val)]
        if max_key in banned:
            cnt.pop(max_key)
        else:
            return max_key
```


```python
print(most_common_word(paragraph, banned))
```

    ball
    

#### 교재 풀이 1. 리스트 컴프리헨션, Counter 객체 사용
##### 활용 함수 & 기법
 - re.sub() : 정규방정식에 해당하는 문자를 다른 문자로 치환(Substitute)하는 함수
 - lower() : 소문자로 변환
 - split() : default는 공백으로 문자열을 분리
 - collections.defaultdict(int) : 비어있는 형태의 dict를 생성하는데, 값으로 integer 자료형이 들어옴을 명시(키 유무 확인 필요없이 즉시 count 수행할 수 있음)
 - max() : max에서 key를 지정하여 argmax 구현 가능 e.g., max(counts, key=counts.get)
 - collections.Counter() : 원소의 개수를 dict 형태로 리턴
 - most_common(n) : 가장 흔한 순으로 n 번째 값을 추출 e.g., [('ball', 2)]와 같이 추출됨
 - Flow 
    + paragraph 문자열을 정규방정식을 통해 word character에 속하지 않는 것들은 공백으로 치환
    + 치환된 문자열을 소문자로 변환하고 공백을 기준으로 분할하여 word를 만들고 이것이 banned에 포함되지 않는다면 모음
    + 모은 것들의 원소 카운트를 수행
    + 가장 자주 등장하는 단어의 첫 번째 인덱스 리턴


```python
paragraph = "Bob hit a ball, the hit BALL flew far after it was hit."
banned = ["hit"]

def most_common_word(paragraph: str, banned:[]) -> str:
    words = [word for word in re.sub(r'[^\w]',' ',paragraph).lower().split() if word not in banned]
    counts = collections.Counter(words)
    return counts.most_common(1)[0][0]
```


```python
print(most_common_word(paragraph, banned))
```

    ball
    
