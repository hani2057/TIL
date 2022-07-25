# 얕은 복사 vs. 깊은 복사

### 복사 방법

- 할당
- 얕은 복사
- 깊은 복사

### 할당(assignment)

- 대입 연산자 = 를 통한 복사는 해당 객체에 대한 객체 참조를 복사(주소를 복사)
    
    ```python
    original_list = [1, 2, 3]
    copy_list = original_list
    print(original_list, copy_list) # [1, 2, 3] [1, 2, 3]
    
    copy_list[0] = 'hello'
    print(original_list, copy_list) # ['hello', 2, 3] ['hello', 2, 3]
    ```
    

### 얕은 복사(shallow copy)

- Slice 연산자를 활용하여 같은 원소를 가진 리스트지만 연산된 결과를 복사 (다른 주소)
    
    ```python
    a = [1, 2, 3]
    b = a[:]
    print(a, b) # [1, 2, 3] [1, 2, 3]
    
    b[0] = 5
    print(a, b) # [1, 2, 3] [5, 2, 3]
    ```
    
- 복사하는 리스트의 원소가 주소를 참조하는 경우(2차원)에는 원본도 변하게 됨
    
    ```python
    a = [1, 2, ['a', 'b']]
    b = a[:]
    print(a, b) # [1, 2, ['a', 'b']]  [1, 2, ['a', 'b']]
    
    b[2][0] = 0
    print(a, b) # [1, 2, [0, 'b']] [1, 2, [0, ['b']]
    ```
    

### 깊은 복사(deep copy)

- copy 모듈의 `copy.deepcopy(sth)` 사용
    
    ```python
    import copy
    
    a = [1, 2, ['a', 'b']]
    b = copy.deepcopy(a)
    print(a, b) # [1, 2, ['a', 'b']] [1, 2, ['a', 'b']]
    
    b[2][0] = 0
    print(a, b) # [1, 2, ['a', 'b']] [1, 2, [0, 'b']]
    ```
