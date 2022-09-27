# 분할정복

- c의 8승은 c를 8번 곱해서 얻을 수도 있지만 c의 4승 두 개를 곱해서 얻을 수도 있다.(연산 횟수가 줄어든다)
- 아래는 c의 n승을 구하는 코드를 작성하는 과정
- 생각의 전개구조를 이런 식으로 만들어가는 연습을 계속 하자

```python
# 가장 큰 전체 구조를 잡는다
def recur_power(x, n):
    return 왼쪽 * 오른쪽

# 수도코드 작성
def recur_power(x, n):
    만약 n이 1이면
        숫자 그대로 내려줘
    만약 n이 홀수이면
        n//2 * n//2 * 한번더
    만약 n이 짝수이면
        n//2 * n//2
    return 왼쪽 * 오른쪽

# 코드로 변환
def recur_power(x, n):
    if n == 1:
        return x
    if n % 2:
        return recur_power(x, n//2) * recur_power(x, n//2) * x
    else:
        return recur_power(x, n//2) * recur_power(x, n//2)

# 목적(연산 횟수 감소)에 맞게 코드 정리
def recur_power(x, n):
    if n == 1:
        return x
    if n % 2:
        half = recur_power(x, n//2)
        return half * half * x
    else:
        half = recur_power(x, n//2)
        return half * half

# 이건 옵셔널. 추가 정리
def recur_power(x, n):
    if n == 1:
        return x

    half = recur_power(x, n//2)
    if n % 2:
        return half * half * x
    else:
        return half * half
```

참고.

```python
# 리스트, 딕셔너리 등을 이용해서 값을 저장하는 방식으로 사용할 수도 있다.
# 메모이제이션같은 방식임
# 동일한 방식으로 여러개 계산하고자 할 때 기존에 계산해둔 값을 이용할 수 있다.
def recur_power(x, n):
    if n == 1:
        return x

    lst[n//2] = recur_power(x, n//2)
    dic[n//2] = recur_power(x, n//2)
    if n % 2:
        return lst[n//2] * lst[n//2] * x
    else:
        return lst[n//2] * lst[n//2]
```
