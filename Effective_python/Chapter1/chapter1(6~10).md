# Chapter 1. 파이썬답게 생각하기 (Better way 6 ~ 10)

## 6. 인덱스를 사용하는 대신 대입을 사용해 데이터를 언패킹하라
언패킹을 사용하는 것인 인덱스를 사용하는 것보다 시각적인 잡음이 적다.
```python
### 인덱스
item = ('호밧엿','약과')
first = item[0]
second = item[1]

### 언패킹
item = ('호밧엿','약과')
fisrt, second = item
```
```python
snacks = [('베이컨', 350), ('도넛', 240), ('머핀', 190)]

### 인덱스
for i in range(len(snacks)):
    item = snacks[i]
    name = item[0]
    calories = item[1]
    print(f'#{i+1}: {name} 은 {calories} 칼로리입니다.')

### 언패킹
for rank, (name, calories) in enumerate(snacks, 1):
    print(f'#{rank}: {name} 은 {calories} 칼로리입니다.')
```

## 7. range보다는 enumerate를 사용하라
리스트를 이터레이션하면서 리스트의 몇 번째 원소를 처리 중인지 알아야 한다면 range보다는 enumerate가 유용하다.  
enumerate는 루프 인덱스와 이터레이터의 다음 값으로 이뤄진 쌍을 넘겨준다.
```python
for i in range(len(flavor_list)):
    flavor = flavor_list[i]
    print(f'{i + 1}: {flavor}')

for i, flavor in enumerate(flavor_list):
    print(f'{i + 1}: {flavor}')

>>>
1: 바닐라
2: 초콜릿
3: 피칸
4: 딸기
```
enumerate의 두번째 파라미터를 이용하여 몇 번부터 수를 세기 시작할지 지정할수도 있다. 다폴트 값은 0임
```python
for i, flavor in enumerate(flavor_list, 1):
    print(f'{i}: {flavor}')

>>>
1: 바닐라
2: 초콜릿
3: 피칸
4: 딸기
```

## 8. 여러 이터레이터에 대해 나란히 루프를 수행하려면 zip을 사용하라
zip은 둘 이상의 이터레이터를 지연 계산 제너테이터를 사용해 묶어준다. zip은 자신이 감싼 이터레이터 원소를 하나씩 소비한다.
```python
names = ['Cecilia', '남궁민수', '毛泽东', 'Rosalind']
counts = [1,2,3]

for name, count in zip(names, counts):
    print(f'{name}: {count}')

>>>
Cecilia: 1
남궁민수: 2
毛泽东: 3
```
zip은 입력 이터레이터의 길이가 서로 다를 때는 문제가 발생한다. zip은 자신이 감싼 이터레이터 중 어느 하나가 끝날 때까지 튜플을 내놓는다.  
따라서 출력은 가장 짧은 입력의 길이와 같다. 길이가 긴 이터레이터의 내용을 모두 보고 싶으면 zip_longest를 사용해야 한다.
```python
import itertools

names = ['Cecilia', '남궁민수', '毛泽东', 'Rosalind']
counts = [1,2,3]

for name, count in itertools.zip_longest(names, counts):
    print(f'{name}: {count}')

>>>
Cecilia: 1
남궁민수: 2
毛泽东: 3
Rosalind: None
```

## 9. for나 while 루프 뒤에 else 블록을 사용하지 말라
파이썬 루프는 대부분의 다른 프로그래밍 언어가 제공하지 않는 기능을 제공한다. 파이썬에서는 루프 블록 바로 다음에 else블록을 추가할 수 있다.  
if/else에서 else는 앞의 블록이 실행되지 않으면 이 블록을 실행하라는 뜻이지만, 여기서는 앞의 작업을 모두 수행하고 else구문이 실행된다.  
```python
for i in range(3):
    print('Loop', i)
else:
    print('Else block!')

>>>
Loop 0
Loop 1
Loop 2
Else block!
```
만약 루프 안에서 break문을 사용하면 else블록은 실행되지 않는다. 
```python
for i in range(3):
    print('Loop', i)
    if i == 1:
        break
else:
    print('Else block!')

>>>
Loop 0
Loop 1
```
for else 구문을 사용함으로써 얻을 수 있는 표현력보다, 미래에 이 코드를 이해하려는 사람들이 느끼게 될 부담감이 더 크다.  
파이썬에서 루프와 같은 간단한 구성 요소는 그 자체로 의미가 명확해야 한다. 따라서 루프 뒤에 else블록을 사용하는 것은 좋지 않다.

## 10. 대입식을 사용해 반복을 피하라
대입식은 영어로 assignment expression이며 왈러스 연산자라고도 부른다. 이 대입식은 코드 중복 문제를 해결하고자 파이썬 3.8에서 새롭게 도입되었다.  
일반 대입문은 a = b라고 쓰며 a 이퀄 b라고 읽지만, 왈러스 연산자는 a := b라고 쓰며, a 왈러스 b라고 읽는다.  
대입식은 대입문이 쓰일 수 없는 위치에서 변수에 값을 대입할 수 있으므로 유용하다.  
```python
fresh_fruit = {
    '사과': 10,
    '바나나': 8,
    '레몬': 5,
}

def make_lemonade(count):
    n = 1
    print(f'레몬 {count*n} 개로 레모네이드 {count//n} 개를 만듭니다.')
    fresh_fruit['레몬'] -= (count * n)
    print(f'레몬이 {fresh_fruit["레몬"]} 개 남았습니다.')

def out_of_stock():
    print(f'제료가 부족합니다. 재료를 보충해 주세요.')

fresh_fruit['레몬'] = 0 

if count := fresh_fruit.get('lemon', 0):
    make_lemonade(count)
else:
    out_of_stock()
```
이렇게 하면 count가 if문의 첫 번째 블록에서만 의미가 있다는 점이 명확히 보이게 된다.
파이썬에는 do/while 루프가 없는데, 왈러스 연산자를 사용하면 while루프에서 무한 루프-중간에서 끝내기 관용어를 사용하지 않고 do/while루프문처럼 구현을 할 수 있다.  
```python
import random

fresh_fruit = {
    '사과': 10,
    '바나나': 8,
    '레몬': 5,
}

def pick_fruit():
    if random.randint(1,10) > 2:   # 80% 확률로 새 과일 보충
        return {
            '사과': random.randint(0,10),
            '바나나': random.randint(0,10),
            '레몬': random.randint(0,10),
        }
    else:
        return None

bottles = []

while fresh_fruit := pick_fruit():
    for fruit, count in fresh_fruit.items():
        batch = make_juice(fruit, count)
        bottles.extend(batch)
```