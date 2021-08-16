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

## 21. 변수 영역과 클로저의 상호작용 방식을 이해하라
클로저는 자신이 정의된 영역 밖의 변수를 참조하는 함수를 의미한다.  
파이썬에서 함수가 일급 시민 객체이다. 일급 시민 객체라는 말은 이를 직접 가리킬 수 있고, 변수에 대입하거나 다른 함수에 인자로 전달할 수 있으며, 식이나 if문에서 함수를 비교하거나 함수에서 반환하는 것 등이 가능하다는 것을 의미한다.  
```python
def sort_priority2(numbers, group):
    found = False  # 영역: 'sort_priority2'
    def helper(x):
        if x in group:
            found = True  # 영역: 'helper' -- 좋지 않음!
            return (0, x)
        return (1, x)
    numbers.sort(key=helper)
    return found

numbers = [8, 3, 1, 2, 5, 4, 7, 6]
group = {2, 3, 5, 7}
found = sort_priority2(numbers, group)
print('발견:', found)
print(numbers)

>>>
발견 : False
[2,3,5,7,1,4,6,8]
```
위의 예시에서 숫자가 group에 있지만 False를 리턴한다. found 변수는 helper 도우미 함수의 영역에서 True로 대입된다. 따라서 helper 함수의 클로저 안에서 이 대입문은 helper 영역 안에 새로운 변수를 정의하는 것으로 취급되지, sort_priority2 안에서 기존 변수에 값을 대입하는 것으로 취급되지는 않는다. 이러한 문제를 영역 지정 버그 (scoping bug)라고도 한다.  
파이썬에는 클로저 밖으로 데이터를 끌어내는 특별한 구문이 있다. nonlocal문이 지정된 변수에 대해서는 앞에서 설명한 영역 결정 규칙에 따라 대인될 변수의 영역이 결정된다.  
```python
def sort_priority2(numbers, group):
    found = False
    def helper(x):
        nonlocal found       # 추가함
        if x in group:
            found = True
            return (0, x)
        return (1, x)
    numbers.sort(key=helper)
    return found
```
nonlocal문은 대입할 데이터가 클로저 밖에 있어서 다른 영역에 속한다는 사실을 분명히 알려준다. nonlocal문은 변수 대입 시 모듈 영역(전역 영역)을 사용해야 한다고 지정하는 global문을 보완해준다.  
하지만 전역 변수를 사용하는 여러 안티 패턴의 경우와 마친가지로, 간단한 함수 외에는 어떤 경우라도 nonlocal을 사용하지 말라고 경고하고 싶다. 특히 함수가 길고 nonlocal문이 지정한 변수와 대입이 이뤄지는 위치의 거리가 멀면 함수 동작을 이해하기 어려워진다.  
nonlocal을 사용하는 방식이 복잡해지면 도우미 함수로 상태를 감싸는 편이 더 낫다.  
```python
class Sorter:
    def __init__(self, group):
        self.group = group
        self.found = False

    def __call__(self, x):
        if x in self.group:
            self.found = True
            return (0, x)
        return (1, x)

sorter = Sorter(group)
numbers.sort(key=sorter)
assert sorter.found is True
```

## 22. 변수 위치 인자를 사용해 시각적인 잡음을 줄여라
위치 인자를 가변적으로 받을 수 있으면 함수 호출이 더 깔끔해지고 시각적 잡음도 줄어든다. 이런 위치 인자를 가변 인자나 스타 인자라고도 한다.  
```python
def log(message, values):
    if not values:
        print(message)
    else:
        values_str = ', '.join(str(x) for x in values)
        print(f'{message}: {values_str}')

log('내 숫자는 ', [1, 2])
log('안녕 ', [])

>>>
내 숫자는: 1, 2
안녕
```
로그에 남길 값이 없을 때도 빈 리스트를 넘겨야 한다면 귀찮을 뿐 아니라 코드 잡음도 많다. 이럴 때 두번째 인자를 완전히 생략하면 좋을 것이다.
```python
def log(message, *values):  # 달라진 유일한 부분
    if not values:
        print(message)
    else:
        values_str = ', '.join(str(x) for x in values)
        print(f'{message}: {values_str}')

log('내 숫자는 ', [1, 2])
log('안녕 ')  # 훨씬 좋다

>>>
내 숫자는: 1, 2
안녕
```
파이썬에서 마지막 위치 인자 이름 앞에 *를 붙이면 인자가 선택사항이 된다.  

하지만 가변적인 위치 인자를 받는 데는 두 가지 문제점이 있다.  
첫 번째 문제점은 이런 선택적인 위치 인자가 함수에 전달되기 전에 항상 튜플로 변환된다는 것이다. 이는 함수를 호출하는 쪽에서 제너레이터 앞에 *연산자를 사용하면 제너레이터의 모든 원소를 얻기 위해 반복한다는 뜻이다. 따라서 이로 인해 메모리를 아주 많이 소비하거나 프로그램이 중단돼버릴 수 있다.  
*args를 받는 함수는 인자 목록에서 가변적인 부분에 들어가는 인자의 개수가 처리하기 좋을 정도로 충분히 작다는 사실을 이미 알고 있는 경우에 가장 적합하다.  
*args의 두 번째 문제는 함수에 새로운 위치 인자를 추가하면 해당 함수를 호출하는 모든 코드를 변경해야만 한다는 것이다. 이미 가변 인자가 존재하는 함수 인자 목록의 앞부분에 위치 인자를 추가하려고 시도하면, 기존 호출코드를 변경하지 않는 경우 호출하는 코드가 미묘하게 깨질 수도 있다.  
이런 문제의 가능성을 완전히 없애려면 *args를 받아들이는 함수를 확장할 때는 키워드 기반의 인자만 사용해야 한다. 