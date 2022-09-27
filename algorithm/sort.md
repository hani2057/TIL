# 정렬

2개 이상의 자료를 특정 기준에 의해 오름차순(ascending) 혹은 내림차순(descending)으로 재배열하는 것

## Bubble sort

- 인접한 두 개의 원소를 비교하며 자리를 계속 교환하는 방식
- 정렬 과정
  - 첫 번째 원소부터 인접한 원소끼리 계속 자리를 교환하면서 맨 마지막 자리까지 이동한다.
  - 한 단계가 끝나면 가장 큰 원소가 마지막 자리로 정렬된다.
  - 교환하며 자리를 이동하는 모습이 물 위에 올라오는 거품 모양과 비슷하다고 하여 버블 정렬이라고 한다.
- 시간 복잡도는 O(n\*\*2)

## Counting sort

- 항목들의 순서를 결정하기 위해 집합에 각 항목이 몇 개씩 있는지 세는 작업을 하여, 선형 시간에 정렬하는 효율적인 알고리즘
- 제한 사항
  - 정수나 정수로 표현할 수 있는 자료에 대해서만 적용 가능 - 각 항목의 발생 회수를 기록하기 위해 정수 항목으로 인덱스되는 카운트들의 배열을 사용하기 때문
  - 카운트들을 위한 충분한 공간을 할당하려면 집합 내의 가장 큰 정수를 알아야 한다.
- 시간 복잡도는 O(n + k)
  - n은 리스트 길이, k는 정수의 최대값

## Selection sort

- 주어진 자료들 중 가장 작은 값의 원소부터 차례대로 선택하여 위치를 교환하는 방식(셀렉션 알고리즘을 전체 자료에 적용한 것)
- 정렬 과정
  - 주어진 리스트 중에서 최소값을 찾는다.
  - 그 값을 리스트의 맨 앞에 위치한 값과 교환한다.
  - 맨 처음 위치를 제외한 나머지 리스트를 대상으로 위의 과정을 반복한다.
- 시간 복잡도는 O(n\*\*2)
- Selection Algorithm
  - 저장되어 있는 자료로부터 k번째로 큰 혹은 작은 원소를 찾는 방법을 셀렉션 알고리즘이라고 한다.(최소값, 최대값 혹은 중간값 등에도 적용)
  - 선택 과정
    - 정렬 알고리즘을 이용하여 자료 정렬하기
    - 원하는 순서에 있는 원소 가져오기

## Merge sort

### 머지소트 시간복잡도: O(nlogn)

전체 머지소트 = 반을 나눈 것을 머지소트 + 합치는 과정

f(n) = 2*f(n/2) + n <br>
= 2(2f(n/(2^2)) + n/2) + n <br>
= 2^2 * f(n/(2^2)) + 2n <br>
= 2^k _ f(n/(2^k)) + kn <br>
k = log n <br>
= n + n _ log n <br>
O(nlogn) <br>

### 머지소트 코드베이스

```python
def merge_sort(lst):
    if len(lst) == 1:
        return lst

    middle = len(lst)//2

    left = lst[:middle]
    right = lst[middle:]

    left = merge_sort(left)
    right = merge_sort(right)

    return merge(left, right)

def merge(left, right):

    result = []

    while left or right:

        if left and right:
            if left[0] <= right[0]:
                result.append(left.pop(0))
            else:
                result.append(right.pop(0))
        elif left:
            result.extend(left)
            break
        elif right:
            result.extend(right)
            break

    return result

# 참고로 pop(0)의 시간복잡도(O(n)) 때문에 머지소트는 linked list에서 좋다고 함
```

## 퀵소트(Quick sort)

### 퀵소트 코드베이스

```python
def quick_sort(lst, l, r):

    if l < r:
        pivot = partition(lst, l, r) # 사실상 이게 소팅 과정(pivot을 하나 정해서 걔를 소팅하는 과정)
        quick_sort(lst, l, pivot-1)
        quick_sort(lst, pivot+1, r)

def hoare_partition(lst, l, r):
    pivot = lst[l]
    i = l
    j = r
    while i <= j:

        while i <= j and lst[i] <= pivot:
        # while not (lst[i] > pivot):
            i += 1
        while i <= j and lst[j] >= pivot:
            j -= 1
        if i < j :
            lst[i], lst[j] = lst[j], lst[i]

    lst[l], lst[j] = lst[j], lst[l]


    return j

def lomuto_partition(lst,l,r):
    pivot = lst[r]
    i = l-1
    for j in range(l, r):
        if lst[j] <= pivot:
            i += 1
            lst[j], lst[i] = lst[i], lst[j]
    lst[r], lst[i+1] = lst[i+1], lst[r]
    return i+1

# 아래는 파이써닉한 방법

left = [x for x in lst if x < pivot]
right = [x for x in lst if x > pivot]

result = left + [pivot] + right
```
