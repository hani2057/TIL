### 조건 표현식(Conditional Expression)

- `true인 경우 값 if 조건 else false인 경우 값`
- eg. `value = num if num >= 0 else -num`
- 일반적으로 조건에 따라 값을 정할 때 활용
- 삼항 연산자(Ternary Operator)로 부르기도 함

### for문

- `for 변수명 in iterable:`
- for문은 시퀀스(string, tuple, list, range)를 포함한 순회 가능한 객체(iterable)의 요소를 모두 순회
- 처음부터 끝까지 모두 순회하므로 별도의 종료 조건이 필요하지 않음
- iterable
    - 순회할 수 있는 자료형(string, list, dict, tuple, range, set 등)
    - 순회형 함수(range, enumerate)
- Dictionary를 순회할 때는 기본적으로 key를 순회하며, key를 통해 값을 활용
    - 추가 메서드를 활용한 딕셔너리 순회
        - dict.keys(): key로 구성된 결과
        - dict.values(): value로 구성된 결과
        - dict.items(): (key, value)의 튜플로 구성된 결과
- enumerate 순회
    - enumerate(iterable, start=0)
    - enumerate(): (index, value) 형태의 튜플로 구성된 열거형(enumerate) 객체를 반환
    - start를 지정하면 해당 값부터 순차적으로 증가
- List Comprehension
    - 표현식과 제어문을 통해 특정한 값을 가진 리스트를 간결하게 생성하는 방법
    - [code for 변수 in iterable]
        
        ```python
        # 1~3의 세제곱 리스트 만들기
        cubic_list = [number ** 3 for number in range(1, 4)]
        ```
        
    - [code for 변수 in ierable if 조건식]
- Dictionary Comprehension
    - 표현식과 제어문을 통해 특정한 값을 가진 딕셔너리를 간결하게 생성하는 방법
    - {key: value for 변수 in iterable}
        
        ```python
        # 1~3의 세제곱 딕셔너리 만들기
        cubic_dict = {number: number ** 3 for number in range(1, 4)}
        ```
        
    - {key: value for 변수 in iterable if 조건식}

### 반복문 제어

- break: 반복문을 종료
- continue: continue 이후의 코드 블록은 수행하지 않고, 다음 반복을 수행
- for-else: 끝까지 반복문을 실행한 이후에 else문 실행
    - break를 통해 중간에 종료되는 경우 else문은 실행되지 않음
- pass: 아무것도 하지 않음(문법적으로 필요하지만, 할 일이 없을 때 사용)
    - 반복문이 아니어도 사용 가능