# 검색

- 저장되어 있는 자료 중에서 원하는 항목을 찾는 작업
- 목적하는 탐색 키를 가진 항목을 찾는 것
    - 탐색 키(search key): 자료를 구별하여 인식할 수 있는 키

### Sequential Search(순차 검색)

- 일렬로 되어 있는 자료를 순서대로 검색하는 방법
- 정렬되어 있지 않은 경우
    - 검색 과정
        - 첫 번째 원소부터 순서대로 검색 대상과 키 값이 같은 원소가 있는지 비교하며 찾는다.
        - 키 값이 동일한 원소를 찾으면 그 원소의 인덱스를 반환한다.
        - 자료구조의 마지막에 이를 때까지 검색 대상을 찾지 못하면 검색 실패
    - 찾고자 하는 원소의 순서에 따라 비교회수가 결정됨
    
    ```python
    # 구현 예(의사코드)
    def sequentialSearch(a, n, key):
        i <- 0
        while i < n and a[i] != key:
            i <- i + 1
        if i < n: return i
        else: return -1
    ```
    
- 정렬되어 있는 경우
    - 검색 과정
        - 자료가 오름차순으로 정렬된 상태라고 가정시,
        - 자료를 순차적으로 검색하면서 키 값을 비교하여, 원소의 키 값이 검색 대상의 키 값보다 크면 찾는 원소가 없다는 것이므로 더 이상 검색하지 않고 검색을 종료함
    - 정렬이 되어 있으므로 검색 실패를 반환하는 경우 평균 비교 횟수가 정렬되지 않았을 경우보다 약 반으로 줄어든다.
    
    ```python
    # 구현 예(의사코드)
    def sequentialSearch(a, n, key):
        i <- 0
        while i < n and a[i] < key:
            i <- i + 1
        if i < n and a[i] < key: return i
        else: return -1
    ```
    
- 시간 복잡도는 O(n)

### Binary Search(이진 검색)

- 자료의 가운데에 있는 항목의 키 값과 비교하여 다음 검색의 위치를 결정하고 검색을 계속 진행하는 방법
- 이진 검색을 하기 위해서는 자료가 정렬된 상태여야 한다.
- 검색 과정
    - 자료의 중앙에 있는 원소를 고른다.
    - 중앙 원소의 값과 찾고자 하는 목표 값을 비교한다.
    - 목표 값이 중앙 원소의 값보다 작으면 자료의 왼쪽 반에 대해서 새로 검색을 수행하고, 크다면 자료의 오른쪽 반에 대해서 새로 검색을 수행한다.
    - 찾고자 하는 값을 찾을 때까지 동일 과정을 반복한다.
- 이진 검색의 경우, 자료에 삽입이나 삭제가 발생했을 때 배열의 상태를 항상 정렬 상태로 유지하는 추가 작업이 필요하다.

```python
def binarySearch(a, N, key):
    start = 0
    end = N - 1
    while start <= end:
        middle = (start + end) // 2
        if a[middle] == key:
            return True
        elif a[middle] > key:
            end = middle - 1
        else:
            start = middle + 1
    return False
```

```python
# 재귀 함수 이용
def binarySearch(a, low, high, key):
    if low > high:
        return False
    else:
        middle = (low + high) // 2
        if key == a[middle]:
            return True
        elif key < a[middle]:
            return binarySearch(a, low, middle - 1, key)
        elif a[middle] < key:
            return binarySearch(a, middle + 1, high, key)
```