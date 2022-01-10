# String Manipulation
## Coding Excercises & Lesson Learned

### Reorder Log Files 문제
 - 로그를 재정렬하라.
    + There are two types of logs: 
         - Letter-logs: All words (except the identifier) consist of lowercase English letters.
         - Digit-logs: All words (except the identifier) consist of digits.
    + Reorder these logs so that:
         - The letter-logs come before all digit-logs.
         - The letter-logs are sorted lexicographically by their contents. If their contents are the same, then sort them lexicographically by their identifiers.
         - The digit-logs maintain their relative ordering.
    + Return the final order of the logs.
 - https://leetcode.com/problems/reorder-data-in-log-files/

#### 나의 풀이
##### 활용 함수 및 기법
 - split() : 문자열 분리해주는 함수
 - join() : 문자열을 합쳐주는 함수
 - isdigit() : 숫자인지 아닌지 확인해주는 함수
 - append() : 리스트에 원소를 추가하는 함수
 - insert() : 리스트의 특정 위치에 원소를 추가하는 함수
 - extend() : 리스트에 다른 리스트의 원소들을 추가하는 함수
 - Flow 
    + log의 문자열을 Identifier와 Contents으로 구분한다.
    + 구분된 contents가 숫자인지 아닌지 판단하여 숫자이면 digit_logs 리스트에 순서대로 모아준다.
    + 문자이면 letter_logs 리스트에 모아주는데, 이전에 모아져 있는 원소들과 비교해서 정렬하여 모아준다.
        - letter_logs 안에 있는 첫번째 원소부터 비교해서 마지막까지 확인함
        - 원소의 contents와 비교해서 먼저인지, 나중인지 판단하여 먼저이면 해당자리에 insert, 나중이면 같은지 같지 않은지 판단함
        - 같다면 identifier를 비교하여 해당자리에 순서를 정하고, 같지 않다면 넘어감
        - 모든 원소들과 다 비교했을 때도 이전 자리에 속하지 않는다면 letter_logs의 마지막에 넣어줌


```python
input_logs = ["dig1 8 1 5 1","let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"]
```


```python
def reorderLogFiles(logs: []) -> []:
    reordered_logs = []
    letter_logs = []
    digit_logs = []

    for log in logs:
        # check whether log is digit or letter 
        splitted_log = log.split(' ')
        identifier = splitted_log[0]
        contents = ''.join(splitted_log[1:])
        contents2 = ' '.join(splitted_log[1:])

        digit = True
        if not contents.isdigit():
            digit = False

        if digit:
            digit_logs.append(log)
        else:
            # reorder letter logs
            if len(letter_logs)==0:
                letter_logs.append(log)
            else:
                for i, letter_log in enumerate(letter_logs):
                    letter_contents = ' '.join(letter_log.split(' ')[1:])
                    if contents2 < letter_contents:
                        letter_logs.insert(i, log)
                        break
                    elif contents2 == letter_contents:
                        if identifier < letter_log.split(' ')[0]:
                            letter_logs.insert(i, log)
                            break
                        else:
                            letter_logs.insert(i+1, log)
                            break
                    elif i==len(letter_logs)-1:
                        letter_logs.append(log)
                        break

    reordered_logs.extend(letter_logs)
    reordered_logs.extend(digit_logs)
    return reordered_logs
```


```python
print(reorderLogFiles(input_logs))
```

    ['let1 art can', 'let3 art zero', 'let2 own kit dig', 'dig1 8 1 5 1', 'dig2 3 6']
    

#### 교재 풀이 1. 람다와 + 연산자를 이용
##### 활용 함수 & 기법
 - split() : 문자열 분리해주는 함수
 - append() : 리스트에 원소를 추가하는 함수
 - sort() : 리스트의 원소를 정렬해주는 함수
 - lambda 함수 : sort의 key를 지정해주기 위해 사용
 - Flow 
    + split한 두번째 원소로 숫자형인지 여부 확인
    + 숫자형이면 digits 리스트로, 아니면 letters 리스트로 append
    + letters를 정렬해주는데, key로 lambda함수 x.split()[1:], x.split()[0]를 지정해줘서 contents 부분 먼저 비교하고, 같으면 다음으로 identifier를 비교하는 것을 구현
    + 두 리스트 letters, digits를 연결하여 리턴


```python
input_logs = ["dig1 8 1 5 1","let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"]
def reorderLogFiles(logs: []) -> []:
    letters, digits = [], []
    for log in logs:
        if log.split()[1].isdigit():
            digits.append(log)
        else:
            letters.append(log)
    
    letters.sort(key=lambda x: (x.split()[1:], x.split()[0]))
    return letters + digits
```


```python
print(reorderLogFiles(input_logs))
```

    ['let1 art can', 'let3 art zero', 'let2 own kit dig', 'dig1 8 1 5 1', 'dig2 3 6']
    
