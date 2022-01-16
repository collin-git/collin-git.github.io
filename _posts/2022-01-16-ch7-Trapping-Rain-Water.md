# Array
## Coding Excercises & Lesson Learned

### Trapping Rain Water 문제
 - 각각의 블럭 높이를 입력받아서 얼마나 많은 물이 저장되는지 계산
 - https://leetcode.com/problems/trapping-rain-water/

#### 나의 풀이
##### 활용 함수 및 기법
 - array 형태로 각 좌표에 블럭이 있다 없다를 0, 1로 표현하여 1 사이에 있는 0인 부분들의 수를 더하는 식으로 풀이
 - 문제는... 이 array를 만드는 과정에서 2차원 배열을 height의 개수 * height의 최대값 만큼 선언을 하고 연산을 하는 식으로 풀이를 해서, leetcode에서 memory limit을 넘어서게 된다... --> 해당 접근 방법은 Fail!
 - Flow 
    + height의 개수 * height의 최대값인 0값 배열을 선언
    + 1인 것들을 인덱스를 구하고 이 수가 연달아서(1차이)로 나오면 해당 인덱스 사이에 중간에 0인 값이 없으므로 넘어가고, 2이상 차이가 나면 그 수를 찾아서 0인 개수를 다 더하는 식으로 구현


```python
def trap(height: []) -> int:
    max_h = max(height)
    min_h = min(height)
    len_h = len(height)
    arr_h = []
    for h in height:
        temp_h = [1] * h
        temp_h.extend([0]*(max_h-h))
        arr_h.append(temp_h)

    water = 0
    for i in range(min_h, max_h):
        block_ind = []
        for j in range(len_h):
            if arr_h[j][i]==1:
                block_ind.append(j)    
        for k in range(len(block_ind)-1):
            if block_ind[k]+1 != block_ind[k+1]:
                water += block_ind[k+1]-block_ind[k]-1
    return water
```


```python
height = [0,1,0,2,1,0,1,3,2,1,2,1]
trap(height)
```




    6



#### 교재 풀이 1. 투 포인터를 최대로 이동
##### 활용 함수 & 기법
 - 투 포인터를 이용해서 왼쪽, 오른쪽에서 각각 상대 max값이 더 높은 방향으로 움직여 나가면서 차이가 나는 값들을 더한다.
 - Flow 
    + 왼쪽 끝, 오른쪽 끝에서 포인터를 하나씩 두고 max값이 더 큰 쪽을 향하여 한칸씩 다가간다.
    + 다가가면서 max값과 현재 값의 차이만큼 volume값에 더해준다.


```python
def trap(height: []) -> int:
    if not height:
        return 0

    volume = 0
    left, right = 0, len(height)-1
    left_max, right_max = height[left], height[right]

    while left < right:
        left_max, right_max = max(height[left], left_max), max(height[right], right_max)
        if left_max <= right_max:
            volume += left_max - height[left]
            left += 1
        else:
            volume += right_max - height[right]
            right -= 1
    return volume
```


```python
height = [0,1,0,2,1,0,1,3,2,1,2,1]
trap(height)
```




    6



#### 교재 풀이 2. 스택 쌓기
##### 활용 함수 & 기법
 - height의 인덱스를 스택에 쌓아 나가면서 현재의 높이가 이전 높이보다 높을 때, 스택의 마지막에 들어온 것을 pop 꺼내고 차이만큼 계산해서 더해준다.... -> 하나씩 따라가면서 stack 변화를 트랙킹했을 때 어느정도 이해되지만, 직관적으로 이해하기가 굉장히 어렵다.
 - Flow 
    + 왼쪽부터 하나씩 인덱스를 스택에 쌓아나감
    + 그 중에 stack에 쌓여있는 이전 인덱스가 있으며, 현재의 height값이 stack에 마지막으로 쌓인 height 값보다 크면 아래의 작업을 반복함
        - stack에 쌓여있는 마지막 인덱스를 꺼낸다. (이전 인덱스를 의미)
        - stack에 남은 것이 없다면 그냥 다음으로 넘어감 (앞에 막아설 블록이 없기 때문에 의미없는 경우가 됨)
        - 현재 스택의 마지막에 쌓여있는 것과 지금의 인덱스의 차이 만큼을 distance로 표현
        - 현재의 height와 이전에 스택의 마지막에 쌓여있는 height중 작은 것에서 바로 이전의 height 만큼 빼서 waters로 표현
        - 이 둘을 곱하여 volume에 더해줌.


```python
def trap(height: []) -> int:
    stack = []
    volume = 0

    for i in range(len(height)):
        while stack and height[i] > height[stack[-1]]:
            top = stack.pop()
            if not len(stack):
                break

            distance = i - stack[-1] - 1
            waters = min(height[i], height[stack[-1]]) - height[top]

            volume += distance * waters
        stack.append(i)
    return volume
```


```python
height = [0,1,0,2,1,0,1,3,2,1,2,1]
trap(height)
```




    6
