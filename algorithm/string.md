# 컴퓨터에서 문자의 표현

### 비트맵

- 글자 모양 그대로 비트맵으로 저장(메모리 낭비가 심함)

### ASCII

- 1967년 미국에서 만든 문자 인코딩 표준
- 7bit 인코딩으로 128문자 표현
- 33개의 출력 불가능한 제어 문자들과 공백을 비롯한 95개의 출력 가능한 문자들로 이루어짐
- 확장 아스키: 1byte 내의 8bit를 모두 사용하여 추가적인 문자를 표현

### 유니코드

- 다국어 처리를 위해 마련한 표준
- Character Set으로 분류됨
    - UCS-2(Universal Character Set 2)
    - UCS-4(Universal Cahracter Set 4)
- 바이트 순서에 대해 표준화하지 못해서 외부 인코딩이 필요함

### 유니코드 인코딩 (UTF: Unicode Transformation Format)

- UTF-8 (in web)
    - MIN: 8bit, MAX: 32bit (1Byte * 4)
    - 내부 규칙을 통해 4byte중 필요한 byte만 가변적으로 사용해서 문자를 저장 → 메모리 관리에 용이
    - 첫 bit 빼고 뒤 7 bit에 대해 아스키 코드를 그대로 사용
- UTF-16 (in windows, java)
    - MIN: 16bit, MAX: 32bit (2Byte * 2)
- UTF-32 (in unix)
    - MIN: 32bit, MAX: 32bit (4Byte * 1)

### big-endian, little-endian

- 용어 출처는 걸리버여행기에서…
- 16진수(2**4) 2개 → 1byte
- 16진수 2개를 한 묶음으로 4묶음 → 4byte(유니코드 기준단위)
- ABCD 라는 4개의 캐릭터 묶음을 전달할 때,
    - A, B, C, D 순서(byte order)로 전달하는 것이 big-endian
    - D, C, B, A 순으로 전달하는 것이 little-endian
- 예를 들어 network는 little-endian 방식을 사용하여 데이터 순서 유지를 위해 별도로 처리해주는 과정이 있다.

# 문자열

String Class로 생성된 str에 대해 클래스 변수에 문자열의 길이, hash값 등이 저장되어 있다.

→ 실제로 len(str) 등의 메서드 사용시 그때마다 개수를 계산해서 주는 게 아니라 클래스 변수에 이미 저장되어 있는 값을 꺼내주는 것.

### 문자열 뒤집기

```python
s = 'Reverse this strings'
#1
s = s[::-1]
#2
s = list(s)
s.reverse()

#3
ans = ''
for char in s:
    ans = ans + char

#4
s = list(s)
for idx in range(len(s)-1, -1, -1):

```

### 문자를 숫자로, 숫자를 문자로

```python
def atoi(str):
    i = 0
    for char in str:
        i = 10 * i + ord(i) - ord('0')
    return i

def itoa(int):
    if int == 0:
        return '0'

    is_positive = True
    if int < 0:
        is_positive = False
        int = -int

    a = ''
    while int:
        int = int // 10
        reminder = int % 10
        a = chr(reminder + ord('0')) + a

    if is_positive:
        return a
    else:
        return '-' + a
```

## 문자열 비교 - 패턴 매칭

### Brute Force

- 본문 문자열을 처음부터 끝까지 차례대로 순회하면서 패턴 내의 문자들을 일일이 비교하는 방식으로 동작
    
    ```python
    text = 'this is a book'
    pattern = 'is'
    
    def func(text, pattern):
        for i in range(len(text) - len(pattern) + 1):
            for j in range(len(pattern)):
                if text[i + j] != patter[j]:
                    break
            else:
                return i
        return -1
    ```
    
    ```python
    text = 'this is a book'
    pattern = 'is'
    
    def func(text, pattern):
        i = 0
        j = 0
        while j < len(pattern) and i < len(text):
            if text[i] != patten[j]
                i = i - j
                j = 0
            i += 1
        if j == len(pattern):
            return i - len(pattern)
        else:
            return -1
    ```
    
    ```python
    text = 'this is a book'
    pattern = 'is'
    
    # pythonic!
    def func(text, pattern):
        m = len(pattern)
        for in in range(len(text) - len(pattern) + 1):
            if text[i:i+m] == pattern:
                return i
        return -1
    ```
    
- 시간 복잡도는 O(nm)
    - n = len(text), m = len(pattern)

### KMP

- 패턴에 대해 prefix list(중계리스트)를 만들어 패턴 내에서 동일하게 반복되는 문자열 정보를 저장해서 shift할 수 있는 범위를 알 수 있도록 한다.
    
    ```python
    abcdabcefacabd
    00001230010120
    '''
    만약 text가 
    abcdabedfabdfb.... 이런 식이면
    6번째 인덱스의 값이 달라서 다시 찾아야 함 
    이때 해당 인덱스의 prefix list에서 값이 3이기 때문에
    4번째, 5번째 인덱스가 같다는 정보를 알 수 있으므로
    1번째 인덱스로 옮겨서 비교를 시작하는 게 아니라
    4번째 인덱스로 옮겨서 6번째 인덱스부터 다음 비교를 시작한다.
    '''
    ```
    
- 패턴 내부에 반복되는 문자열이 있을 때 사용

### Boyer-Moore(보이어-무어)

- (패턴의 길이만큼 중) 오른쪽에서 왼쪽으로 비교
- 대부분의 상용 소프트웨어에서 채택하고 있는 알고리즘
- text의 인덱스 i는 skip 배열의 인덱스(또는 딕셔너리의 값)만큼씩 움직임
- pattern의 인덱스 j는 항상 pattern의 뒤에서부터 앞으로 움직임