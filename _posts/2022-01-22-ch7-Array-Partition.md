# Array
## Coding Excercises & Lesson Learned

### Array Partition 문제
 - 주어진 배열로 n개의 pair을 만들어서 각 pair값들의 최소값을 모두 더해서 만들 수 있는 가장 큰 sum값을 구하라.
 - https://leetcode.com/problems/array-partition-i/

#### 나의 풀이
##### 활용 함수 및 기법
 - 정렬하고 index = 0, 2, 4, ..., 2n 까지 수를 더한 것이 결과적으로 min(pair) 값의 sum가 동일함 
 - Flow 
    + 정렬하고 index = 0, 2, 4, ..., 2n 까지 수를 더함


```python
def arrayPairSum(nums: []) -> int:
    sum_max = 0 
    nums.sort()

    for i in range(0,len(nums),2):
        sum_max += nums[i]

    return sum_max
```


```python
nums = [1,2,3,4]
arrayPairSum(nums)
```




    4



#### 교재 풀이 1. 오름차순 풀이
##### 활용 함수 & 기법
 - 정렬하고 앞에서 부터 두개씩 짝지어서 min을 구하고 더함
 - Flow 
    + 정렬하고 숫자를 pair에 넣기
    + pair가 두개가 되면 min값을 도하고 새로운 pair를 만들고
    + 이 것을 반복


```python
def arrayPairSum(nums: []) -> int:
    sum = 0
    pair = []
    nums.sort()

    for n in nums:
        pair.append(n)
        if len(pair)==2:
            sum += min(pair)
            pair=[]
    return sum
```


```python
nums = [1,2,3,4]
arrayPairSum(nums)
```




    4



#### 교재 풀이 2. 짝수 번째 값 계산
##### 활용 함수 & 기법
 - 나의 풀이와 동일. but, 전체 순회하여 나의 풀이보다는 느림(나의 풀이는 for 반복문에서 2개씩 건너뜀)


```python
def arrayPairSum(nums: []) -> int:
    sum = 0
    nums.sort()

    for i, n in enumerate(nums):
        if i % 2 ==0:
            sum += n

    return sum
```


```python
nums = [1,2,3,4]
arrayPairSum(nums)
```




    4



#### 교재 풀이 3. 파이썬다운 방식
##### 활용 함수 & 기법
 - 슬라이싱을 활용한 풀이


```python
def arrayPairSum(nums: []) -> int:
    return sum(sorted(nums)[::2])
```


```python
nums = [1,2,3,4]
arrayPairSum(nums)
```




    4


