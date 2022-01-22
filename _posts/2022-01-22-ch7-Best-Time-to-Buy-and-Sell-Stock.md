# Array
## Coding Excercises & Lesson Learned

### Best Time to Buy and Sell Stock 문제
 - 한 번의 사고 팔기를 했을 때의 최대 이익 산출
 - https://leetcode.com/problems/best-time-to-buy-and-sell-stock/

#### 나의 풀이
##### 활용 함수 및 기법
 - 뒤에서 부터 해당 인덱스까지의 max값을 미리 저장해두고 이것을 이용해 풀이
 - Flow 
    + 뒤에서 부터 오면서 max값을 이어붙인 배열 생성(max_now)
    + 첫번째~마지막-1까지의 각 값에서 buy했다고 가정하고, 이후의 값들의 max값에서 sell했다고 가정해서 이 차이와 max_profit 중 큰 것으로 max_profit 업데이트 


```python
def maxProfit(prices: []) -> int:
    max_profit = 0

    max_now = [] 
    for i in range(len(prices)-1, -1, -1):
        if i ==len(prices)-1:
            max_now.append(prices[i])
        else:
            max_now.append(max(max_now[-1],prices[i]))

    for i, p in enumerate(prices):
        if i < len(prices)-1:
            max_profit = max(max_profit, max_now[len(prices)-2-i]-prices[i])

    return max_profit
```


```python
prices = [7,1,5,3,6,4]
maxProfit(prices)
```




    5



#### 교재 풀이 1. 브루트 포스로 계산
##### 활용 함수 & 기법
 - 가능한 경우의 수 모두 계산
 - leete code Time-out으로 Fail!
 - Flow 
    + 첫번째 사서 뒤에 팔때 모든 경우의 수에서의 이익을 계산해서 최대값 산출


```python
def maxProfit(prices: []) -> int:
    max_price = 0

    for i, price in enumerate(prices):
        for j in range(i, len(prices)):
            max_price = max(prices[j] - price, max_price)

    return max_price
```


```python
prices = [7,1,5,3,6,4]
maxProfit(prices)
```




    5



#### 교재 풀이 2. 저점과 현재 값과의 차이 계산
##### 활용 함수 & 기법
 - 저점을 계속 경신해 나가면서 현재값과의 차이로 profit을 계산해서 업데이트
 - sys.maxsize : 시스템 상의 가장 큰 수 (초기화시 사용)
 - Flow 
    + profit을 0으로, min_price를 sys.maxsize로 초기화
    + price를 처음부터 이동해가면서 min_price를 업데이트해 나감
    + 현 price와 min_price의 차이를 계산해서 profit과 비교해서 큰 것으로 업데이트


```python
import sys
def maxProfit(prices: []) -> int:
    profit = 0
    min_price = sys.maxsize

    for price in prices:
        min_price = min(min_price, price)
        profit = max(profit, price-min_price)
    return profit
```


```python
prices = [7,1,5,3,6,4]
maxProfit(prices)
```




    5


