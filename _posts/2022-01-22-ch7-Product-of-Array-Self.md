# Array
## Coding Excercises & Lesson Learned

### Product of Array Self 문제
 - 주어진 배열에서 특정 index 위치의 원소를 제외한 나머지 값들의 곱이 오도록 리턴
 - 나누기를 쓰지 않고 O(n)안에 풀기
 - https://leetcode.com/problems/product-of-array-except-self/

#### 나의 풀이
##### 활용 함수 및 기법
 - 배열을 순서대로 누적 곱하고, 역순으로 누적 곱하고 각각의 원소로 원하는 배열 만들기 (--> O(n)으로 풀이 가능)
 - Flow 
    + 배열을 순서대로 누적 곱하고, 역순으로 누적 곱해서 각 temp1, temp2를 생성
    + 해당 원소의 이전과 이후의 누적 곱한 값을 곱해서 해당 원소를 제외한 모든 원소들의 곱을 구할 수 있다.


```python
def productExceptSelf(nums: []) -> []:
    output = []
    leng = len(nums)
    temp1 = []
    for n in nums:
        if len(temp1)==0:
            temp1.append(n)
        else:
            temp1.append(temp1[-1]*n)

    temp2 = []
    for i in range(leng-1,-1,-1):
        if len(temp2)==0:
            temp2.append(nums[i])
        else:
            temp2.append(temp2[-1]*nums[i])

    for i in range(leng):
        if i>=1 and i<leng-1:
            output.append(temp1[i-1]*temp2[leng-i-2])
        elif i==0:
            output.append(temp2[leng-i-2])
        else:
            output.append(temp1[i-1])
    return output
```


```python
nums = [1,2,3,4]
productExceptSelf(nums)
```




    [24, 12, 8, 6]




```python
nums = [-1,1,0,-3,3]
productExceptSelf(nums)
```




    [0, 0, 9, 0, 0]



#### 교재 풀이 1. 왼쪽 곱셈 결과에 오른쪽 값을 차례대로 곱셈 
##### 활용 함수 & 기법
 - 왼쪽 곱셈 결과에 오른쪽 마지막 값부터 순서대로 곱셈을 해서 리턴
 - Flow 
    + 1부터 nums 배열 안의 값들을 곱해나가면서 p를 업데이트하여 out의 원소로 넣어줌
    + out안의 원소들에 오른쪽 끝에서의 값들을 곱해나가면서 업데이트


```python
def productExceptSelf(nums: []) -> []:
    out = []
    p = 1
    for i in range(0, len(nums)):
        out.append(p)
        p = p*nums[i]
    p = 1
    for i in range(len(nums)-1, -1, -1):
        out[i] = out[i]*p
        p = p*nums[i]
    
    return out
```


```python
nums = [1,2,3,4]
productExceptSelf(nums)
```




    [24, 12, 8, 6]




```python
nums = [-1,1,0,-3,3]
productExceptSelf(nums)
```




    [0, 0, 9, 0, 0]


