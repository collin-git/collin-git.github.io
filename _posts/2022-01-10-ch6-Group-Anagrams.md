# String Manipulation
## Coding Excercises & Lesson Learned

### Group Anagrams 문제
 - 문자열 배열을 받아서 애너그램 단위로 그룹핑하라.(애너그램은 문자열을 재배열해서 다른 뜻을 가진 단어로 바꾸는 것) 
 - https://leetcode.com/problems/group-anagrams/

#### 나의 풀이
##### 활용 함수 및 기법
 - indexing으로 list내의 원소를 모으는 부분을 처리함
 - collections.Counter() 및 append 정도의 기본 함수만 사용
 - Flow 
    + 문자를 하나씩 쪼개어 리스트 형태로 만들고, 이를 Counter 하여 문자 개수를 카운트하여 저장
    + 실제 anagram 단어를 모은 anagram_list를 생성하고, 하나씩 카운트에 대한 비교를 수행하기 위해 anagram_cnt_list를 모아나감
    + 카운트가 똑같은 부분에 anagram에 해당하는 문자를 이어붙여서 anagram 배열 완성


```python
import collections
input_strs = ["eat","tea","tan","ate","nat","bat"]
```


```python
def group_anagrams(strs: []) -> [[]]:
    cnt_list = []
    for s in strs: #eat
        s_list = []
        for ss in s: # [e, a, t]
            s_list.append(ss)
        cnt_list.append(collections.Counter(s_list)) #[{'e':1, 'a':1, 't':1}, ...]

    anagram_list = []
    anagram_cnt_list = []
    for i, cnt in enumerate(cnt_list):
        if len(anagram_list)==0:
            anagram_list.append([strs[i]])
            anagram_cnt_list.append([cnt])
        else:
            new_one = True
            for j, anagram_cnt in enumerate(anagram_cnt_list):
                if cnt == anagram_cnt[0]:
                    anagram_list[j].append(strs[i])
                    anagram_cnt_list[j].append(cnt)
                    new_one = False
                    break
            if new_one:
                anagram_list.append([strs[i]])
                anagram_cnt_list.append([cnt])
    return anagram_list   
```


```python
group_anagrams(input_strs)
```




    [['eat', 'tea', 'ate'], ['tan', 'nat'], ['bat']]



#### 교재 풀이 1. 정렬하여 딕셔너리에 추가
##### 활용 함수 & 기법
 - sorted(), append, collections.defaultdict() 사용 
 - Flow 
    + 정렬한 것을 key로, 정렬하기 전 원래 단어를 value로 하여 딕셔너리에 원소 추가
    + values 부분 리턴


```python
input_strs = ["eat","tea","tan","ate","nat","bat"]
def group_anagrams(strs: []) -> [[]]:
    anagrams = collections.defaultdict(list)
    for word in strs:
        anagrams[''.join(sorted(word))].append(word)
    return list(anagrams.values())
```


```python
group_anagrams(input_strs)
```




    [['eat', 'tea', 'ate'], ['tan', 'nat'], ['bat']]


