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