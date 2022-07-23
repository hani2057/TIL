# 변수 및 자료형(Datatype)

### 각 변수의 값을 바꿔서 저장하기

- 방법1. 임시 변수 활용

```python
x, y = 10, 20

tmp = x
x = y
y = tmp
print(x, y) # 20 10
```

- 방법2. *Pythonic!*

```python
x, y = 10, 20

y, x = x, y # 실제로는 tuple로 처리됨
print(x, y) # 20 10
```

### 식별자

변수 이름 규칙

- 식별자의 이름은 영문 알파벳, 언더스코어(_), 숫자로 구성
- 첫 글자에 숫자가 올 수 없음
- 길이 제한이 없고, 대소문자를 구별
- 예약어(reserved words)는 사용할 수 없음

```python
import keyword
print(keyword.kwlist) 
# 출력 결과
['False', 'None', 'True', '__peg_parser__', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```

### 실수 연산시 주의할 점(부동 소수점)

- 컴퓨터는 2진수를 이용해서 10진수의 근사값을 표시하기 때문에, 실수 연산시 예상치 못한 결과가 나타날 수 있다.
    
    ```python
    print((3.2 - 3.1) == (1.2 - 1.1)) # False
    ```
    
- 해결방법
    - 1.임의의 작은 수 활용
    
    ```python
    print(abs(a - b) <= 1e-10) # True
    ```
    
    - 2.math 모듈 사용(python 3.5 이상)
    
    ```python
    import math
    print(math.isclose(a, b)) # True
    ```
    

### Escape sequnce

역슬래시(backslash) 뒤에 특정 문자가 와서 특수한 기능을 하는 문자 조합

- \n: 줄 바꿈
- \t: 탭
- \r: 캐리지 리턴
- \0: 널(Null)
- \\: \
- \’:단일인용부호
- \”:이중인용부호

### String Interpolation(문자열을 변수를 활용하여 만드는 법)

- %-formatting(예전에 쓰던 방식)
    
    ```python
    name = 'Kim'
    score = 4.5
    
    print('Hello, %s' % name)
    print('내 성적은 %d' % score)
    print('내 성적은 %f' %score)
    ```
    
- str.format()
    
    ```python
    print(’hello, {}! 성적은 {}’.format(name, score))
    ```
    
- f-strings (python 3.6+)
    
    ```python
    print(f’Hello, {name}! 성적은 {score}’)
    
    import datetime
    today = datetime.datetime.now()
    print(today)
    print(f’오늘은 {today:%y}년 {today:%m}월 {today:%d}일')
    
    pi = 3.141592
    print(f’원주율은 {pi:.3}. 반지름이 2일 때 원의 넓이는 {pi*2*2}’)
    ```
    
    - f-string 안에 변수, 문자, 코드 넣을 수 있고 포매팅도 할 수 있다.

### 할당 연산자(Assignment Operator): `=`

- 변수는 `=` 을 통해 할당(assign)된다.
- 해당 데이터 타입을 확인하기 위해서는 `type()` 을 활용한다.
- 해당 값의 메모리 주소를 확인하기 위해서는 `id()` 를 활용한다.
    - 파이썬에서 -5부터 256까지 정수의 id는 동일하다.
    
    ```jsx
    a = 3
    b = 3
    print (a is b) # True
    print (id(a) == id(b)) # True
    ```

### 비교 연산자

- <, <=, >, >=, ==, !=
- is: 객체 아이덴티티
- is not: 객체 아이덴티티가 아닌 경우
- `==`는 '값'이 같으면 True, `is`는 메모리 위치까지 같은지를 체크함

### 논리 연산자

- A and B
- A or B
- not
- Falsy 값
    - 0, 0.0, (), [], {}, None, ‘’(빈 문자열)
- 논리 연산자 우선순위
    - not, and, or 순으로 우선순위가 높음
- 논리 연산자의 단축 평가
    - and 연산은 첫번째 Falsy값을 반환하고, 모두 Truthy이면 마지막 값을 반환한다.
    - or 연산에서 첫번째 Truthy값을 반환하고, 모두 Falsy인 경우 마지막 값을 반환한다.

### 연산자 우선순위

0.()을 통한 grouping

1.Slicing

2.Indexing

3.제곱연산자 **

4.단한영산자 +, -

5.산술연산자 *, /, %

6.산술연산자 +, -

7.비교연산자 in, is

8.not

9.and

10.or

<br>

# 컨테이너

### 컨테이너의 분류

1. 시퀀스형
    1. 리스트 []: 여러 개의 값을 순서가 있는 구조로 저장하고 싶을 때 사용 (가변 자료형)
    2. 튜플 (): 여러 개의 값을 순서가 있는 구조로 저장하고 싶을 때 사용 (불변 자료형)
    3. 레인지(불변형)
2. 비시퀀스형
    1. 세트: 중복되는 요소가 없이, 순서에 상관없는 데이터들의 묶음(수학에서 집합 개념)
    2. 딕셔너리

## 시퀀스형

### List

- 리스트는 대괄호 [] 혹은 list()를 통해 생성한다.
- 어떠한 자료형도 저장할 수 있으며, 리스트 안에 리스트도 넣을 수 있다.
- 생선된 이후 내용 변경이 가능하다(가변 자료형)
- 순서가 있는 시퀀스로 인덱스를 통해 접근 가능하다.
    - 값에 대한 접근은 list_name[index]

### Tuple

- 튜플은 소괄호 () 혹은 tuple()을 통해 생성한다.
- 튜플은 수정 불가능한(immutable) 시퀀스이다.
- 순서가 있는 시퀀스로 인덱스를 통해 접근 가능하다.
    - 값에 대한 접근은 tuple_name[index]
- 단일 항목의 경우:
    - 하나의 항목으로 구성된 튜플은 생성 시 값 뒤에 쉼표를 붙여야 한다.
    - tuple_a = (1,)
        - (1)처럼 쓰면 문자열로 인식됨
- 복수 항목의 경우:
    - 마지막 항목에 붙은 쉼표는 없어도 되지만, 넣는 것을 권장한다.(Trailing comma)
    - tuple_b = (1, 2, 3,)
- 튜플 대입(Tuple assignment)
    - 우변의 값을 좌변의 변수에 한 번에 할당하는 과정
    - 튜플은 일반적으로 파이썬 내부에서 활용한다.

### Range

- 숫자의 시퀀스를 나타내기 위해 사용한다.
- 기본형: range(n)
    - 0부터 n-1 까지의 숫자의 시퀀스
- 범위 지정: range(n, m)
    - n부터 m-1 까지의 숫자의 시퀀스
- 범위 및 스텝 지정: range(n, m, s)
    - n부터 m-1까지 s만큼 증가시키며 숫자의 시퀀스
    - s에 음수가 들어가면 역순으로 시퀀스를 돈다.
        - `print(list(range(6, 1, -1))) # [6, 5, 4, 3, 2]`
        - `print(list(range(6, 1, 1))) # []`

### 슬라이싱 연산자

- 시퀀스를 특정 단위로 슬라이싱
- 인덱스와 콜론을 사용하여 시퀀스의 특정 부분만 잘라낼 수 있음
- sequence[n:m:k]
    - n부터 m-1까지 k 간격으로 슬라이싱한다.
    - n 자리가 비어있으면 0(처음)부터 슬라이싱한다.
    - m 자리가 비어있으면 끝까지 슬라이싱한다.

## 비시퀀스형

### Set

- 셋은 중괄호 {} 혹은 set()을 통해 생성한다.
    - 빈 Set을 만들기 위해서는 set()을 반드시 활용해야 한다. (빈 중괄호 {}는 Dictionary임)
- 수학에서의 집합을 표현한 컨테이너
    - 집합 연산이 가능(합집합, 차집합, 교집합)
    - 여집합을 표현하는 연산자는 별도로 존재하지 않는다.
- 데이터의 중복을 허용하지 않기 때문에 중복되는 원소가 있다면 하나만 저장된다.
- 순서가 없기 때문에 인덱스를 이용한 접근이 불가능하다.
- 담고 있는 요소를 삽입, 변경, 삭제 가능(가변 자료형)
- 셋을 활용하면 다른 컨테이너에서 중복된 값을 쉽게 제거할 수 있다.
    - 단, 이후 순서가 무시되므로 순서가 중요한 경우 사용하지 말 것
- 셋(Set) 연산자
    - | : 합집합
    - & : 교집합
    - - : 차집합
    - ^ : 대칭차집합
        - 합집합에서 교집합을 차집합
    - 여집합 연산자는 없음  

### Dictionary

- 딕셔너리는 중괄호 {} 혹은 dict()을 통해 생성한다.
- key는 변경 불가능한 데이터(immutable)만 활용 가능
    - string, integer, float, boolean, tuple, range
- value는 어떠한 형태든 상관없다.
- key를 통해 value에 접근한다.
- dictionary에서 for를 활용하는 4가지 방법
    ``` python
    # 0. dictionary 순회 (key 활용)
    for key in dict:
        print(key)
        print(dict[key])

    # 1. `.keys()` 활용
    for key in dict.keys():
        print(key)
        print(dict[key])

    # 2. `.values()` 활용
    # 이 경우 key는 출력할 수 없음
    for val in dict.values():
        print(val)

    # 3. `.items()` 활용
    for key, val in dict.items():
        print(key, val)
    ```

## 형 변환 (Typecasting)

- Python에서 데이터 형태는 서로 변환할 수 있다.
- 암시적 형 변환(Implicit): 사용자가 의도하지 않고, 파이썬 내부적으로 자료형을 변환하는 경우
    - bool
    - Numeric type(int, float)
    
    ```python
    print(True + 3) # 4
    print(3 + 5.0) # 8.0
    ```
    
- 명시적 형 변환(Explicit): 사용자가 특정 함수를 활용하여 의도적으로 자료형을 변환하는 경우
    - int
        - str, float → int
        - 단, 형식에 맞는 문자열만 정수로 변환 가능
        
        ```python
        print('3' + 4) # TypeError: can only concatenate srt (not "int") to str
        print(int('3') + 4) # 7, 명시적 타입 변환이 필요함
        ```
        
        - input()함수의 결과는 문자열로 저장되기 때문에 `int(input())` 형식을 많이 사용하게 됨
    - float
        - str(참고), int → float
        - 단, 형식에 맞는 문자열만 float로 변환 가능
        
        ```python
        print(float('3')) # 3.0
        print(float('3/4') + 5.3) # ValueError 발생, float 형식이 아닌 문자열은 타입 변환할 수 없음
        ```
        
    - str
        - int, float, list, tuple, dict → str
