# Chapter 3. 함수 (Better way 19 ~ 22)

## 19. 함수가 여러 값을 반환하는 경우 절대로 네 값 이상을 언패킹하지 말라
```python
def get_stats(numbers):
    minimum = min(numbers)
    maximum = max(numbers)
    count = len(numbers)
    average = sum(numbers) / count
    sorted_numbers = sorted(numbers)
    middle = count // 2
    if count % 2 == 0:
        lower = sorted_numbers[middle - 1]
        upper = sorted_numbers[middle]
        median = (lower + upper) / 2
    else:
        median = sorted_numbers[middle]

    return minimum, maximum, average, median, count

minimum, maximum, average, median, count = get_stats(lengths)
```
위와 같은 코드에는 두 가지 문제가 있다.  
1. 모든 반환 값이 수이기 때문에 순서를 혼동하기 쉽다. 이런 실수는 나중에 알아내기 어려운 버그를 만들어내고, 반환 값이 많으면 실수하기도 아주 쉬워진다.  
2. 함수를 호출하는 부분과 반환 값을 언패킹하는 부분이 길고, 여러가지 방법으로 줄을 바꿀 수 있어서 가독성이 나빠진다.  
이런 문제를 피하려면 함수가 여러 값을 반환하거나 언패킹할 때 값이나 변수를 네 개 이상 사용하면 안 된다. 만약 많은 값을 언패킹해야 한다면 경량 클래스나 namedtuple을 사용하고 함수도 이런 값을 반환하게 만드는 것이 낫다.  

## 20. None을 반환하기보다는 예외를 발생시켜라
한 수를 다른 수로 나누는 도우미 함수를 작성한다고 하자. 0으로 나누는 경우 결과가 정해져 있지 않으므로 None을 반환하는 것이 자연스러워 보인다.
```python
def careful_divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return None

x, y = 1, 0
result = careful_divide(x, y)
if result is None:
    print('잘못된 입력')
```
만약 피제수가 0이면, 제수가 0이 아닌 한 함수는 0을 반환한다. 그런데 함수가 반환한 결과를 if문 등의 조건에서 평가할 때 0 값이 문제가 될 수 있다. None인지 검사하는 대신, 실수로 0 값을 False로 취급하는 검사를 실행할 수 있다.  

```python
def careful_divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return None

x, y = 0, 5
result = careful_divide(x, y)
if not result:
    print('잘못된 입력') # 이 코드가 실행되는데, 사실 이 코드가 실행되면 안된다!
```
이러한 실수를 줄이는 방법은 두 가지가 있다.  
첫 번째 방법은 반환 값을 2-튜플로 분리하는 것이다. 튜플의 첫 번째 부분은 연산이 성공인지 실패인지를 표시하고 두 번째 부분은 계산에 성공한 경우 실제 결과값을 저장한다.  
```python
def careful_divide(a, b):
    try:
        return True, a / b
    except ZeroDivisionError:
        return False, None

success, result = careful_divide(x, y)
if not success:
    print('잘못된 입력')
```
이 방법의 문제점은 호출하는 쪽에서 튜플의 첫 번째 부분을 쉽게 무시할 수 있다는 것이다.  
따라서 더 나은 두 번째 방법은 특별한 경우에 결코 None을 반환하지 않는 것이다. 대신 Exception을 호출한 쪽으로 발생시켜서 호출자가 이를 처리하게 한다.  
```python
def careful_divide(a, b):
    try:
        return a / b
    except ZeroDivisionError as e:
        raise ValueError('잘못된 입력')

x, y = 5, 2
try:
    result = careful_divide(x, y)
except ValueError:
    print('잘못된 입력')
else:
    print('결과는 %.1f 입니다' % result)
```
호출자는 더 이상 반환 값에 대한 조건문을 사용하지 않아도 된다. 대신에 반환 값이 항상 올바르다고 가정하고, try 문의 else 블록에서 이 값을 즉시 사용할 수 있다.