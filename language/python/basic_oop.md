# 객체 지향 프로그래밍(OOP: Object-Oriented Programming)

### 객체 지향 프로그래밍

- 컴퓨터 프로그래밍의 패러다임(방법론) 중 하나
- 컴퓨터 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러 개의 독립된 단위, 즉 ‘객체'들의 모임으로 파악하고자 하는 것
- 각각의 객체는 메시지를 주고받고, 데이터를 처리할 수 있다.

→ 프로그램을 여러 개의 독립된 객체들과 그 객체 간의 상호작용으로 파악하는 프로그래밍 방법

### OOP의 장점 및 단점

장점

- 클래스 단위로 모듈화시켜 개발할 수 있으므로 많은 인원이 참여하는 대규모 소프트웨어 개발에 적합
- 필요한 부분만 수정하기 쉽게 때문에 프로그램의 유지보수가 쉬움

단점

- 설계시 많은 노력과 시간이 필요함
    - 다양한 객체들의 상호 작용 구조를 만들기 위해 많은 시간과 노력이 필요
- 실행 속도가 상대적으로 느림
    - 절차 지향 프로그래밍이 컴퓨터의 처리구조와 비슷해서 실행 속도가 빠름

### 클래스와 인스턴스

- 클래스로 만든 객체를 해당 클래스(타입)의 인스턴스라고 한다.
- 가수(클래스)의 인스턴스 아이유(인스턴스)에 대해
    - 아이유는 객체다(O)
    - 아이유는 인스턴스다(X)
    - 아이유는 가수의 인스턴스다(O)

### 파이썬은 모든 것이 객체이다.

파이썬의 모든 것에는 속성(Attribute)과 행동(Method)이 존재한다.

```python
[3, 2, 1].sort()
리스트.정렬()
객체.행동()
```

```python
'banana'.upper()
문자열.대문자로()
객체.행동()
```

<br>

## 속성

- 특정 데이터 타입/클래스의 객체들이 가지게 될 상태/데이터를 의미
- 클래스 변수 / 인스턴스 변수가 존재

```python
class Person:
    blood_color = 'red' # 클래스 변수
    popluation = 100 # 클래스 변수

    def __init__(self, name):
        self.name = name # 인스턴스 변수

person1 = Person('John')
print(person1.name) # John
```

### 인스턴스 변수

- 각 인스턴스들의 고유한 변수(속성)
- 생성자 메서드(\_\_init__)에서 self.<name>으로 정의
- 인스턴스가 생성된 이후 <instance>.<name>으로 접근 및 할당

### 클래스 변수

- 클래스 선언 내부에서 정의
- <classname>.<name>으로 접근 및 할당
- 클래스 변수 활용(사용자 수 계산하기)
    - 인스턴스가 생성될 때마다 클래스 변수가 늘어나도록 설정하면 됨
    
    ```python
    class Person:
        count = 0
        # 인스턴스 변수 설정
        def __init__(slef, name):
            self.name = name
            Person.count += 1
    
    person1 = Person('Jane')
    person2 = Person('John')
    
    print(Person.count) # 2
    ```
    
- 클래스 변수를 변경할 때는 항상 <class>.<class variable> 형식으로 변경

### 인스턴스와 클래스 간의 이름 공간(namespace)

- 클래스를 정의하면, 클래스와 해당하는 이름 공간 생성
- 인스턴스를 만들면, 인스턴스 객체가 생성되고 이름 공간 생성
- 인스턴스에서 특정 속성에 접근하면, 인스턴스-클래스 순으로 탐색

<br>

## 메서드

- 특정 데이터 타입/클래스의 객체에 공통적으로 적용 가능한 행위(함수)
- 인스턴스 메서드 / 클래스 메서드 / 정적 메서드

### 인스턴스 메서드

- 인스턴스 변수를 사용하거나, 인스턴스 변수에 값을 설정하는 메서드
- 클래스 내부에 정의되는 메서드의 기본
- 인스턴스 메서드는 호출시, 첫번째 인자로 인스턴스 자기자신(self)이 전달되게 설계됨
    - 첫번째 인자의 위치로 전달되는 이름이 굳이 ‘self’라는 단어일 필요는 없지만 컨벤션으로 self를 씀

### 매직 매서드(스페셜 메서드)

- Double underscore(__)가 있는 메서드는 특수한 동작을 위해 만들어진 메서드임
- 던더 어쩌구로 부른다. 예) 던더 스트링(`__str__` )
- 특정 상황에 자동으로 불리는 메서드
    - 객체의 특수 조작 행위를 지정(함수, 연산자 등)
- 디버깅용으로 많이 사용함
- 예시
    
    ```python
    __str__(self), __len__(self), __repr__(self)
    __lt__(self, other), __le__(self, other), __eq__(self, other)
    __gt__(self, other), __ge__(self, other), __ne__(self, other)
    ```
    
    - `__str__` : 해당 객체의 출력 형태를 지정
        - 프린트 함수를 호출할 때, 자동으로 호출
        - 어떤 인스턴스를 출력하면 `__str__` 의 return값이 출력됨
    - `__gt__` : grater than
        
        ```python
        class Circle:
        
            def __init__(self, r):
                self.r = r
        
            def __str__(self):
                return f'[circle] radius: {self.r}'
        
            def __gt__(self, other):
                return self.r > other.r
        
        c1 = Circle(10)
        c2 = Circle(1)
        
        print(c1) # [circle] radius: 10
        print(c1 > c2) # True
        print(c1 < c2) # False
        ```
        

### 생성자(constructor) 메서드

- `__init__(self)`
- 인스턴스 객체가 생성될 때 자동으로 호출되는 메서드
- 인스턴스 변수들의 초기값을 설정 가능
    
    ```python
    class Person:
        def __init__(self):
            print('인스턴스가 생성되었습니다.')
    
    person1 = Person() # 인스턴스가 생성되었습니다.
    ```
    

### 소멸자(destructor) 메서드

- `__del__(self)`
- 인스턴스 객체가 소멸(파괴)되기 직전에 호출되는 메서드
    
    ```python
    class Person:
        def __def__(self):
            print('인스턴스가 사라졌습니다.')
    
    person1 = Person()
    del person1 # 인스턴스가 사라졌습니다.
    ```
    
<br>

## 데코레이터

- 함수를 어떤 함수로 꾸며서 새로운 기능을 부여
- @데코레이터(함수명) 형태로 함수 위에 작성
- 순서대로 적용되기 때문에 작성 순서가 중요
- 데코레이터 없이 함수 꾸미기
    
    ```python
    def hello():
        print("hello")
    
    # 데코레이팅 함수
    def add_print(original): # 파라미터로 함수를 받는다.
        def wrapper():       # 함수 내에서 새로운 함수 선언
            print("함수 시작") # 부가 기능 -> original 함수를 꾸민다.
            original()
            print("함수 끝")   # 부가 기능 -> original 함수를 꾸민다.
        return wrapper       # 함수를 return 한다.
    
    add_print(hello)()
    # 또는
    print_hello = add_print(hello)
    print_hello()
    ```
    
- 데코레이터 사용해서 함수 꾸미기
    
    ```python
    # 데코레이팅 함수
    def add_print(original): # 파라미터로 함수를 받는다.
        def wrapper():       # 함수 내에서 새로운 함수 선언
            print("함수 시작") # 부가 기능 -> original 함수를 꾸민다.
            original()
            print("함수 끝")  # 부가 기능 -> original 함수를 꾸민다.
        return wrapper      # 함수를 return 한다.
    
    @add_print
    def print_hello():
        print("hello")
    
    print_hello()
    ```
    
- 하나의 데코레이터를 여러 함수에 적용도 가능, 여러개의 데코레이터를 하나의 함수에 적용도 가능(이때 순서 중요)

<br>

## 클래스 메서드

- 클래스가 사용할 메서드
- @classmethod 데코레이터를 사용하여 정의
- 클래스 메서드는 호출시, 첫번째 인자로 클래스(cls)가 전달되게 설계됨
    
    ```python
    class Person:
        count = 0 # 클래스 변수
    
        def __init__(self, name):
            self.name = name
            Person.count += 1
    
        @classmethod
        def number_of_population(cls):
            print(f'인구수는 {cls.count}입니다.')
    
    person1 = Person('Jane')
    person2 = Person('John')
    print(Person.count)
    ```
- 클래스 메서드를 이용해서 인스턴스를 생성할 수도 있다.
    
    ```python
    class Person:
        @classmethod
        def create_instance(cls):
            ins = cls()
            return ins
    ```

<br>

## 스태틱 메서드(정적 메서드)

- 인스턴스 변수, 클래스 변수를 전혀 다루지 않는 메서드
    - 즉, 객체 상태나 클래스 상태를 수정할 수 없음
- @staticmethod 데코레이터를 사용하여 정의
- 속성을 다루지 않고 단지 기능(행동)만을 하는 메서드를 정의할 때 사용
- 일반 함수처럼 동작하지만, 클래스의 이름공간에 귀속됨 (주로 해당 클래스로 한정하는 용도로 사용)
    
    ```python
    class Person:
        count = 0
        
        def __init__(self, name):
            self.name = name
            Person.count += 1
    
        @staticmethod
        def check_rich(money): # 스태틱은 cls, self 사용 X
            return money > 10000
    
    person1 = Person('Jane')
    person2 = Person('John')
    print(Person.check_rich(100000)) # True 스태틱은 클래스로 접근 가능
    print(person1.check_rich(100000)) # True 스태틱은 인스턴스로 접근 가능
    ```

<br>
<br>

# OOP의 핵심 개념 4가지

- 추상화
- 상속
- 다형성
- 캡슐화

<br>

## 추상화

- 현실 세계를 프로그램 설계에 반영
- 복잡한 것은 숨기고, 필요한 것만 드러낸다
- 내부 구조를 몰라도 사용할 수 있도록 함

<br>

## 상속

- 두 클래스 사이 부모 - 자식 관계를 정립하는 것
- 클래스는 상속 가능하다
    - 모든 파이썬 클래스는 object를 상속받는다
    
    ```python
    class ChildClass(ParentClass):
        pass
    ```
    
- 하위 클래스는 상위 클래스에 정의된 속성, 행동, 관계 및 제약 조건을 모두 상속받음
- 부모클래스의 속성/메서드가 자식 클래스에 상속되므로 코드 재사용성이 높아짐
- 상속을 통한 메서드 재사용 예시
    
    ```python
    class Person:
        def __init__(self, name, age):
            self.name = name
            self.age = age
    
        def talk(self):
            print(f'반갑습니다. {self.name}입니다.')
    
    class Professor(Person):
        def __init__(self, name, age, department):
            self.name = name
            self.age = age
            self.department = department
    
    class Student(Person):
        def __init__(self, name, age, gpa):
            self.name = name
            self.age = age
            self.gpa = gpa
    
    p1 = Professor('박교수', 49, '컴퓨터공학과')
    s1 = Student('김학생', 20, 3.5)
    
    # 부모 Person 클래스의 talk 메서드를 활용
    p1.talk() # 반갑습니다. 박교수입니다.
    s1.talk() # 반갑습니다. 김학생입니다.
    ```
    

### 상속 관련 함수와 메서드

- isinstance(object, classinfo)
    - classinfo의 instance거나 subclass인 경우 True
    
    ```python
    class Person:
        pass
    
    class Professor(Person):
        pass
    
    class Student(Person):
        pass
    
    p1 = Professor()
    
    print(isinstance(p1, Person)) # True
    print(isinstance(p1, Professor)) # True
    print(isinstance(p1, Student)) # False
    ```
    
- issubclass(class, classinfo)
    - class가 classinfo의 subclass 이면 True
    - classinfo는 클래스 객체의 튜플일 수 있으며, classinfo의 모든 항목을 검사하여 하나라도 해당되면 True를 반환함
    
    ```python
    class Person:
        pass
    
    class Professor(Person):
        pass
    
    class Student(Person):
        pass
    
    p1 = Professor()
    
    print(issubclass(Professor, Person)) # True
    print(issubclass(Professor, (Person, Student))) # True
    print(issubclass(bool, int)) # True
    print(issubclass(float, int)) # False
    ```
    
- super()
    - 자식클래스에서 부모클래스를 사용하고 싶은 경우
    
    ```python
    class Person:
        def __init__(self, name, age):
            self.name = name
            self.age = age
    
    class Student(Person):
        def __init__(self, name, age, student_id):
            super().__init__(name, age)
            self.student_id = student_id
    ```
    

### 상속 정리

- 파이썬의 모든 클래스는 object로부터 상속됨
- 부모 클래스의 모든 요소(속성, 메서드)가 상속됨
- super()를 통해 부모 클래스의 요소를 호출할 수 있음
- 메서드 오버라이딩을 통해 자식 클래스에서 재정의 가능함
- 상속관계에서의 이름 공간은 인스턴스, 자식 클래스, 부모 클래스 순으로 탐색

### 다중 상속

- 두 개 이상의 클래스를 상속받는 경우
- 상속받은 모든 클래스의 요소를 활용 가능함
- 중복된 속성이나 메서드가 있는 경우 상속 순서에 의해 결정됨
- 참고. 다중 상속 기능이 없는 언어도 있음(파이썬 특징 중 하나)
    
    ```python
    class Person:
        def __init__(self, name):
            self.name = name
    
    class Mom(Person):
        gene = 'XX'
    
    class Dad(Person):
        gene = 'XY'
    
    class FirstChild(Dad, Mom):
        pass
    
    class SecondDhild(Mom, Dad):
        pass
    
    baby1 = FirstChild('아가')
    baby2 = SecondChild('아가')
    
    print(baby1.gene) # XY
    print(baby2.gene) # XX
    ```
    
- mro 메서드(Method Resolution Order)
    - 해당 인스턴스의 클래스가 어떤 부모 클래스를 가지는지 확인하는 메서드
    - 기존의 ‘인스턴스 → 클래스’ 순으로 이름 공간을 탐색하는 과정에서, 상속 관계에 있으면 ‘인스턴스 → 자식 클래스 → 부모 클래스’로 확장
    
    ```python
    print(FirstChild.mro())
    # [<class '__main__.FirstChild'>, <class '__main__.Dad'>, <class '__main__.Mom'>, <class '__main__.Person'>, <class 'object'>]
    print(SecondChild.mro())
    # [<class '__main__.SecondChild'>, <class '__main__.Mom'>, <class '__main__.Dad'>, <class '__main__.Person'>, <class 'object'>]
    ```
    
<br>

## 다형성(Polymorphism)

- ‘여러 모양'을 뜻하는 그리스어
- 동일한 메서드가 클래스에 따라 다르게 행동할 수 있음을 의미
- 즉, 서로 다른 클래스에 속해 있는 객체들이 동일한 메시지에 대해 다른 방식으로 응답할 수 있음

### 메서드 오버라이딩

- 상속받은 메서드를 재정의
    - 상속받은 클래스에서 같은 이름의 메서드로 덮어씀
    - 부모 클래스의 메서드를 싱행시키고 싶은 경우 super() 활용
- 클래스 상속시, 부모 클래스에서 정의한 메서드를 자식 클래스에서 변경
- 부모 클래스의 메서드 이름과 기본 기능은 그대로 사용하지만 특정 기능을 바꾸고 싶을 때 사용
    
    ```python
    class Person:
        def __init__(self, name):
            self.name = name
    
        def talk(self):
            print(f'반갑습니다. {self.name}입니다.')
    
    class Professor(Person):
        def talk(self):
            print(f'{self.name}일세.')
    
    class Student(Person):
        def talk(self):
            super().talk()
            print(f'저는 학생입니다.')
    
    p1 = Professor('김교수')
    p1.talk() # 김교수일세.
    
    s1 = Student('이학생')
    s1.talk() 
    # 반갑습니다. 이학생입니다.
    # 저는 학생입니다.
    ```
    

### 오버로딩

- 동일한 이름의 함수를 parameter의 개수를 다르게 하여 여러번 정의하는 방법
    - 인자 개수에 따라 다른 동작을 할 필요가 있을 때 사용
- 파이썬 문법에는 없음(가변인자 활용하여 해당 개념 구현 가능)

<br>

## 캡슐화

- 객체의 일부 구현 내용에 대해 외부로부터의 직접적인 액세스를 차단
- 파이썬에서 암묵적으로 존재하지만, 언어적으로는 존재하지 않음

### 접근제어자 종류

- Public Access Modifier
- Protected Access Modifier
- Private Access Modifier

### Public Member

- 언더바 없이 시작하는 메서드나 속성
- 어디서나 호출이 가능, 하위 클래스 override 허용
- 일반적으로 작성되는 메서드와 속성의 대다수를 차지

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

p1 = Person('John', 30)

# Person 클래스의 인스턴스인 p1은 name 과 age 모두 접근 가능함
print(p1.name) # John
print(p1.age) # 30
```

### Protected Member

- 언더바 1개로 시작하는 메서드나 속성
- 암묵적 규칙에 의해 부모 클래스 내부와 자식 클래스에서만 호출 가능
- 하위 클래스 override 허용

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self._age = age

    def get_age(self):
        return self._age

# 인스턴스를 만들고 get_age 메서드를 통해 호출할 수 있다
p1 = Person('John', 30)
print(p1.get_age()) # 30

# _age에 직접 접근하여도 확인 가능(파이썬에서는 암묵적으로 활용하기 때문)
print(p1._age) # 30
```

### Private Member

- 언더바 2개로 시작하는 메서드나 속성
- 본 클래스 내부에서만 사용이 가능
- 하위 클래스 상속 및 호출 불가능 (오류)
- 외부 호출 불가능 (오류)

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.__age = age

    def get_age(self):
        return self.__age

# 인스턴스를 만들고 get_age 메서드를 통해 호출할 수 있다
p1 = Person('John', 30)
print(p1.get_age()) # 30

# __age에 직접 접근이 불가능
print(p1.__age) # AttributeError: 'Person' object has no attribute '__age'
```

- 참고: `print(dir(instance))` 로 확인해보면 사용할 수 있는 메서드 종류가 나오는데, public/protected/private 각각에 대해 확인해보면 어떻게 되는지 확인해볼 수 있다.
    
    ```python
    class Person:
        def __init__(self, age):
            self.__age = age
    
    p1 = Person(20)
    print(p1.__age) # Error
    print(p1._Person__age) # 20 
    ```
    
### getter 메서드와 setter 메서드

- 변수에 접근할 수 있는 메서드를 별도로 생성
- getter 메서드: 변수의 값을 읽는 메서드
    - @property 데코레이터 사용
- setter 메서드: 변수의 값을 설정하는 성격의 메서드
    - @변수.setter 사용
- @property 및 @변수.setter 데코레이터 사용시, getter, setter 모두 함수로 정의되어도 마치 변수처럼 사용할 수 있도록 내부적으로 작성되어 있다. 아래의 _age 예시 참조.

```python
class Person:
    def __init__(self, age):
        self._age = age # Protected member 로 설정함

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, new_age):
        if new_age <= 19:
            raise ValueError('Too young')
            return
        self._age = new_age

# 인스턴스를 만들어서 나이에 접근하면 정상적으로 출력됨
# @property 데코레이터가 p1.age()로 호출하지 않아도 변수처럼 사용할 수 있도록 해줌
p1 = Person(20)
print(p1.age) # 20 

# 인스턴스의 나이를 다른 값으로 바꿔도 정상적으로 반영됨
# @age.setter 데코레이터가 p1.age()로 호출하지 않아도 변수처럼 사용할 수 있도록 해줌
p1.age = 30
print(p1.age) # 30

# setter 함수의 조건문 때문에 나이를 19 이하의 값으로 변경하면 오류가 발생함
p1.age = 10
print(p1.age) # ValueError: Too young
```
