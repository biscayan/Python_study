# Chapter 1. 파이썬답게 생각하기 (Better way 1 ~ 5)

## 1. 사용 중인 파이썬의 버전을 알아두라
사용하고 있는 파이썬의 버전을 정확히 알고 싶으면 --version 플래그를 사용하라  
```
$ python --version
Python 3.7.6
```
내장 모듈인 sys로도 현재 실행 중인 파이썬 버전을 알아낼 수 있다.  
```python
import sys 

print(sys.version_info)
>>> sys.version_info(major=3, minor=7, micro=6, releaselevel='final', serial=0) 

print(sys.version)
>>> 3.7.6 | packaged by conda-forge | (default, Mar  5 2020, 14:47:50) [MSC v.1916 64 bit (AMD64)]
```

## 2. PEP 8 스타일 가이드를 따르라
파이썬 개선 제안 (Python Enhancement Proposal) #8, 또는 PEP 8은 파이썬 코드를 어떤 형식으로 작성할지 알려주는 스타일 가이드다.  
### 공백
파이썬에서 공백은 중요한 의미가 있다. 코드를 작성할 때 공백과 관련한 다음의 가이드라인을 따라야 한다.  
- 탭 대신 스페이스를 사용해 들여쓰기하라.  
- 문법적으로 중요한 들여쓰기에는 4칸의 스페이스를 사용하라.  
- 라인 길이는 79개 문자 이하여야 한다.  
- 긴 식을 다음 줄에 이어서 쓸 경우에는 일반적인 들여쓰기보다 4 스페이스를 더 들여써야 한다.  
- 파일 안에서 각 함수와 클래스 사이에는 빈 줄을 두 줄 넣어라.  
- 클래스 안에서 메소드와 메소드 사이에는 븐 줄을 한 줄 넣어라.  
- 딕셔너리에서 키와 콜론 사이에는 공백을 넣지 않고, 한 줄 안에 키와 값을 같이 넣는 경우에는 콜론 다음에 스페이스를 하나 넣는다.  
- 변수 대입에서 = 전후에는 스페이스를 하나씩만 넣는다.  
- 타입 표기를 덧붙이는 경우에는 변수 이름과 콜론 사이에 공백을 넣지 않도록 주의하고, 콜론과 타입 정보 사이에는 스페이스를 하나 넣어라.  
### 명명 규약
- 함수, 변수, 애트리뷰트는 lowercase_underscore처럼 소문자와 밑줄을 사용한다.  
- 보호돼야 하는 인스턴스 애트리뷰트는 일반적인 애트리뷰트 이름 규칙을 따르되, _leading_underscore처럼 밑줄로 시작한다.  
- 비공개(private) 인스턴스 애트리뷰트는 일반적인 애트리뷰트 이름 규칙을 따르되, __leading_underscore처럼 밑줄 두 개로 시작한다.  
- 클래스는 CapitalizedWord처럼 여러 단어를 이어 붙이되, 각 단어의 첫 글자를 대문자로 만든다.  
- 모듈 수준의 상수는 ALL_CAPS처럼 모든 글자를 대문자로 하고, 단어와 단어 사이를 밑줄로 연결한 형태를 사용한다.  
- 클래스에 들어 있는 인스턴스 메소드는 호출 대상 객체를 가리키는 첫 번째 인자의 이름으로 반드시 self를 사용해야 한다.  
- 클래스 메소드는 클래스를 가리키는 첫 번째 인자의 이름으로 반드시 cls를 사용해야 한다.  
### 식과 문
- 긍정적인 식을 부정하지 말고 (if not a is b) 부정을 내부에 넣어라 (if a is not b).  
- 빈 컨테이너나 시퀀스([] 또는 '' 등)를 검사할 때는 길이를 0과 비교 (if len(something) == 0)하지 말라. 빈 컨테이너나 시퀀스 값이 암뭄적으로 False로 취급된다는 사실을 활용해 'if not container'라는 조건문을 써라.  
- 비어 있지 않은 컨테이너나 시퀀스(```[1]```이나 'hi' 등)를 검사할 때도 길이가 0보다 큰지 비교하지 말라. 대신 if 컨테이너가 비어 있지 않은 경우 암묵적으로 True로 평가된다는 사실을 활용하라.  
- 한 줄짜리 if문이나 한 줄짜리 for, while 루프, 한 줄짜리 except 복함문을 사용하지 말라. 명확성을 위해 각 부분을 여러 줄에 나눠 배치하라.  
- 식을 한 줄 안에 다 쓸 수 없는 경우, 식을 괄호로 둘러싸고 줄바꿈과 들여쓰기를 추가해서 읽기 쉽게 만들라.  
- 여러 줄에 걸쳐 식을 쓸 대는 줄이 계속된다는 표시를 하는 \ 분자보다는 괄호를 사용하라.  
### 임포트
- import문을 항상 파일 맨 앞에 위치시켜라.  
- 모듈을 임포트할 때는 절대적인 이름을 사용하고, 현 모듈의 경로에 상대적인 이름은 사용하지 말라.  
- 반드시 상대적인 경로로 임포트해야 하는 경우에는 from . import foo처럼 명시적인 구문을 사용하라.  
- 임포트를 적을 때는 표준 라이브러리 모듈, 서드 파티 모듈, 사용자 정의 모듈 순서로 섹션을 나눠라. 각 섹션에는 알파벳 순서로 모듈을 임포트하라.  

## bytes와 str의 차이를 알아두라
파이썬에는 문자열 데이터의 시퀀스를 표현하는 두 가지 타입이 있다. 바로 bytes와 str이다.  
bytes타입의 인스턴스에는 부호가 없는 8바이트 데이터가 그대로 들어간다.  
```python
a = b'h\x65llo'
print(list(a))
print(a)

>>>
[104, 101, 108, 108, 111]
b'hello'
```
str 인스턴스에는 사람이 사용하는 언어의 문자를 표현하는 유니코드 코드 포인트가 들어있다.
```python
a = 'a\u0300 propos'
print(list(a))
print(a)

>>>
['a', '̀', ' ', 'p', 'r', 'o', 'p', 'o', 's']
à propos
```
중요한 사실은 str 인스턴스에는 직접 대응하는 이진 인코딩이 없고, bytes에는 직접 대응하는 텍스트 인코딩이 없다는 것이다.  
따라서 유니코드 데이터를 이진 데이터로 변환하려면 str의 encode 메소드를 호출해야 하고, 이진 데이터를 유니코드 데이터로 변환하려면 bytes의 decode 메소드를 호출해야 한다.  

이진 8비트 값과 유니코드 문자열을 파이썬에서 다룰 때 꼭 기억해야 할 두가지 문제점이 있다.  
첫 번째 문제점은 bytes와 str이 똑같이 작동하는 것처럼 보이지만 각각의 인스턴스는 서로 호환되지 않기 때문에 전달 중인 문자 시퀀스가 어떤 타입인지를 항상 잘 알고 있어야 한다는 것이다.  
연산자를 사용하여 bytes를 bytes에 더하거나 str을 str에 더할 수 있지만, str 인스턴스를 bytes 인스턴스에 더하거나 bytes 인스턴스를 str 인스턴스에 더할 수 없다.  
또한, 더하기 연산 뿐만 아니라 비교연산자도 사용을 할 수 없다.  
```python
print(b'one' + 'two')

>>>
Traceback (most recent call last):
  File "c:\git\upload\Effective_python\Chapter1\item3.py", line 34, in <module>
    print(b'one' + 'two')
TypeError: can't concat str to bytes
```
```python
print('one' + b'two')

>>>
Traceback (most recent call last):
  File "c:\git\upload\Effective_python\Chapter1\item3.py", line 34, in <module>
    print('one' + b'two')
TypeError: can only concatenate str (not "bytes") to str
```
두 번째 문제점은 open을 호출해 얻은 파일 핸들과 관련한 연산들이 디폴트로 유니코드 문자열을 요구하고 이진 바이트 문자열을 요구하지 않는 것이다.  
따라서 파일을 열 때, 텍스트 쓰기 모드('w')가 아닌 이진 쓰기 모드('wb')를, 그리고 텍스트 읽기 모드('r')가 아닌 이진 쓰기 모드('rb')를 사용해야 한다.  

## C 스타일 형식 문자열을 str.format과 쓰기보다는 f-문자열을 통한 인터폴레이션을 사용하라
형식화는 미리 정의된 문자열에 데이터 값을 끼워 넣어서 사람이 보기 좋은 문자열로 저장하는 과정이다.  
파이썬에서 문자열을 형식화하는 가장 일반적인 방법은 % 형식화 연산자를 사용하는 것이다.  
예를 들어 읽기 어려울 이진 값이나 십육진 값을 십진수로 표시하기 위해 %를 다음과 같이 사용할 수 있다.  
```python
a = 0b10111011
b = 0xc5f
print('이진수: %d, 십육진수: %d' % (a, b))

>>>
이진수: 187, 십육진수: 3167
```
형식 지정자의 문법은 C언어의 printf 함수에서 비롯됐으며, 파이썬에 이식됐다. 따라서 파이썬에서도 %s, %x, %f 등 C언어의 printf에서 사용할 수 있는 대부분의 형식 지정자를 사용할 수 있다.  
하지만 파이썬에서 C 스타일 형식 문자열을 사용하는 데는 네 가지 문제점이 있다.  
(1) 첫 번째 문제점은 형식화 식에서 오른쪽에 있는 tuple 내 데이터 값의 순서를 바꾸거나 값의 타입을 바꾸면 타입 변환이 불가능하므로 오류가 발생할 수 있다.  
이런 오류를 피하려면 % 연산자의 좌우가 서로 잘 맞는지 계속 검사해야 한다.  
```python
key = 'my_var'
value = 1.234
reordered_tuple = '%-10s = %.2f' % (value, key)  # TypeError

>>>
Traceback (most recent call last):
  File "c:\git\upload\Effective_python\Chapter1\item4.py", line 15, in <module>
    reordered_tuple = '%-10s = %.2f' % (value, key)  # TypeError
TypeError: must be real number, not str
```
(2) 두 번째 문제점은 형식화를 하기 전에 값을 살짝 변셩해야 한다면 식을 읽기가 매우 어려워진다는 것이다.  
(3) 세 번째 문제점은 형식화 문자열에서 같은 값을 여러 번 사용하고 싶다면 튜플에서 같은 값을 여러 번 반복해야 한다는 것이다.  
```python
template = '%s는 음식을 좋아해. %s가 요리하는 모습을 봐요.'
name = '철수'
formatted = template % (name, name)
print(formatted)

>>>
철수는 음식을 좋아해. 철수가 요리하는 모습을 봐요.
```
이런 문제를 해결하기 위해 % 연산자에 튜플 대신 딕셔너리를 사용해 형식화하는 기능이 추가됐다.  
```python
name = '철수'
template = '%(name)s는 음식을 좋아해. %(name)s가 요리하는 모습을 봐요.'
after = template % {'name': name} # 딕셔너리
print(after)

>>>
철수는 음식을 좋아해. 철수가 요리하는 모습을 봐요.
```
(4) 하지만 형식화 식에 딕셔너리를 사용하면 문장이 번잡스러워진다. 이것이 네 번째 문제점이다. 각 키를 최소 두 번(한 번은 형식 지정자에, 다른 한 번은 딕셔너리 키에) 반복하게 된다.  
가독성을 해치는 문자를 제외하더라도 이런 불필요한 중복으로 인해 딕셔너리를 사용하는 형식화 식이 너무 길어진다.  

### 내장 함수 format과 str.format
파이썬 3부터는 %를 사용하는 오래된 C스타일 형식화 문자열보다 더 표현력이 좋은 고급 문자열 형식화 기능이 도입됐다.  
str타입에 새로 추가된 format 메소드를 호출하면 여러 값에 대해 한꺼번에 이 기능을 적용할 수 있다.  
그리고 %d 같은 C 스타일 형식화 지정자를 사용하는 대신 위치 지정자 {}를 사용할 수 있다.  
```python
key = 'my_var'
value = 1.234

formatted = '{} = {}'.format(key, value)
print(formatted)

>>>
my_var = 1.234
```
각 위치 지정자에는 콜론 뒤에 형식 지정자를 붙여 넣어 문자열에 값을 넣을 때 어떤 형식으로 변환할지 정할 수 있다.  
```python
key = 'my_var'
value = 1.234

formatted = '{:<10} = {:.2f}'.format(key, value)
print(formatted)

>>>
my_var     = 1.23
```
위치 지정자 중괄호에 위치 인덱스, 즉 format 메소드에 전달된 인자의 순서를 표현하는 위치 인덱스를 전달할 수도 있다.  
이렇게 하면 format에 넘기는 인자의 순서를 바꾸지 않아도 형식화 문자열을 사용해 형식화한 값을 출력 순서를 변경할 수 있다.  
또한 형식화 문자열 안에서 같은 위치 인덱스를 여러 번 사용할 수도 있으므로 format에 넘기는 인자에 값을 여러 번 반복할 필요도 없다.  
```python
key = 'my_var'
value = 1.234

formatted = '{1} = {0}'.format(key, value)
print(formatted)

>>>
1.234 = my_var
```
```python
name = '철수'
formatted = '{0}는 음식을 좋아해. {0}가 요리하는 모습을 봐요.'.format(name)
print(formatted)

>>>
철수는 음식을 좋아해. 철수가 요리하는 모습을 봐요.
```

### 인터폴레이션을 통한 형식 문자열
파이썬 3.6부터는 인터폴레이션을 통한 문자열, 즉 f-문자열이 도입됐다. 이 새로운 언어 문법에는 형식 문자열 앞에 f문자를 붙여야 한다.  
f-문자열은 형식 문자열의 표현력을 극대화하고, 형식화 문자열에서 키와 값을 불필요하게 중복 지정해야 하는 경우를 없애준다.  
```python
key = 'my_var'
value = 1.234

formatted = f'{key} = {value}'
print(formatted)

>>>
my_var = 1.234
```
앞에서 살펴본 format 함수의 형식 지정자 안에서 콜론 뒤에 사용할 수 있는 내장 미니 언어를 f-문자열에서도 사용할 수 있다.  
```python
formatted = f'{key!r:<10} = {value:.2f}'
print(formatted)

>>>
'my_var'   = 1.23
```
f-문자열을 사용한 형식화는 C 스타일 형식화 문자열에 % 연산자를 사용하는 경우나 str.format 메소드를 사용하는 경우보다 항상 더 짧다.
```python
f_string = f'{key:<10} = {value:.2f}'

c_tuple  = '%-10s = %.2f' % (key, value)

str_args = '{:<10} = {:.2f}'.format(key, value)

str_kw   = '{key:<10} = {value:.2f}'.format(key=key, value=value)
                                            
c_dict   = '%(key)-10s = %(value).2f' % {'key': key, 'value': value}
```
f-문자열이 제공하는 표현력, 간결성, 명확성을 고려할 때 파이썬 프로그래머가 사용할 수 있는 형식화 옵션 중에서 f-문자열이 최고다.  
값을 문자열로 형식화해야 하는 상황을 만나게 되면 다른 대안 대신 f-문자열을 택하라.

## 5. 복잡한 식을 쓰는 대신 도우미 함수를 작성하라
어떠한 로직을 반복 적용하려면 도우미 함수를 작성하는 것이 좋다
```python
### found에 요소가 있으면 그 값을 리턴하고 만약 요소가 없다면 기본값인 0을 리턴
def get_first_int(values, key, default=0):
    found = values.get(key, [''])
    if found[0]:
        return int(found[0])
    return default
```