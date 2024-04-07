---
title: python namespace와 scope (lexical scope), nonlocal/global 키워드
date: 2023-04-08 00:00:00+0900
categories: [python, basic]
tags: [python, function, namespace, scope, lexical scope, nonlocal, local, gloabl]
render_with_liquid: false
---

오늘은 변수 간의 충돌을 방지하고, 예상치 못한 결과를 방지하기 위해 `namespace`와 `scope`에 대해 공부해봅시다.

## namespace

파이썬에서 **모든 것은 object**입니다. 함수도 객체이고 변수에 할당될 수 있으며, 이것을 `객체가 변수(혹은 변수명)에 bind되었다`라고 표현합니다.

이러한 binding은 선언되는 위치에 따라 `namespace`라는 자료형에 저장됩니다! namespace를 통하여 어떠한 이름이 어떤 객체에 바인딩되었는지를 확인할 수 있습니다. 다음 코드를 실행해봅시다.

```python
print(dir())
# ['__annotations__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__']
```
`dir()`함수는 현재 scope에서 접근할 수 있는 name을 리스트의 형태로 제공합니다. 즉, 현재 namespace의 key값들만 list로 반환합니다.

추가로, scope에 따라 접근할 수 있는 namespace를 출력해주는 함수 `locals`와 `globals`가 있습니다. globals()는 항상 전역 네임스페이스를 출력하고, locals()는 실행되는 위치의 네임스페이스, 즉 현재 접근 가능한 것들을 출력합니다.

```python
a = 1
print(locals())

def func1():
	x = 2
	print(locals())

func1()
```
2번 줄은 `{'__name__': '__main__', '__doc__': None, ... , '__cached__': None, 'a': 1}`가 출력이 되고,
6번줄에서는 `{'x': 2}`가 출력 됩니다.

2번 줄 위치에서는 a에 접근이 되지만, 6번 줄 위치에서는 x에만 접근할 수 있다는 것을 명확하게 알 수 있습니다!

 >`namespace`는 변수와 binding된 객체를 알 수 있는 dictionary의 형태로 제공 되며, 각 변수는 scope에 따라 접근이 가능합니다.
 {: .prompt-tip }

### if \_\_name\_\_ == '\_\_main\_\_': 

파이썬 함수를 실행하거나, 모듈로써 가져올 때 `if __name__ == '__main__':` 이 코드를 많이 봤을겁니다.  `__name__`이라는 변수는 독립적으로 실행되었을 경우에만 `'__main__'`으로 설정이 됩니다. 

test.py는 다음과 같습니다.
```python
import test1

print('test name: ', __name__)
```
같은 디렉토리에 test1.py를 다음처럼 작성해봅시다.
```python
print('test1 name: ', __name__)
```
이후, `python test.py`를 실행하면
```
test1 name : test
test name : __main__
```
이 출력됩니다.

즉, **파이썬 명령어로 실행하는 진입점이 되는 스코프에만 `'__main__'`이 할당**되고, 그게 아니면 `__name__`이라는 변수에는 호출한 모듈의 이름이 바인드됩니다.

위치에 따라 namespace가 다르기에 사용할 수 있는 변수가 다르다고 했는데요. 그럼 변수가 어디에 선언되어야 현재 실행하는 라인에서 접근이 가능할까요?
## python scope

`scope`는 **변수가 접근 가능한 영역**을 의미합니다. python에서 variable은 일반적으로 정의된 위치에 따라 scope가 결정되고, 해당 scope에서만 유효합니다.
![scope](assets/img/2024-04-08-python-namespace-scopes/scope.png)
위와 같이 4개의 스코프가 존재하며 각 스코프마다 네임스페이스를 갖습니다. 

중요한 점은, **내부 스코프에서는 바깥 스코프의 네임스페이스를 접근할 수 있지만 반대는 불가능하다는 점**입니다!!!

1. built-in scope
	```python
	print(sum)
	# <built-in function sum>
	print(id(sum))
	# 4295747248
	```
	sum 함수는 파이썬에 내장되어 있는 함수로 어디서든 접근이 가능합니다. 이는 `built-in` scope로 내장 함수/상수 등이 이에 해당합니다. 
	
	심지어 `print`, `True` 등 어디서든 파이썬 어디서든 사용한 것들은 built-in 객체라고 보면 됩니다.
	
2. global scope
	
	다음 코드는 오류 없이 실행될까요?
	```python
	a = 1
	def func():
		print(a)
	func()
	```
	a는 전역적으로 선언되었으므로, func 내에서 선언되지 않았음에도 접근이 가능합니다. 이는 `global` scope로, 같은 파일/모듈 단위에서 접근할 수 있습니다.
	
3. local scope
	```python
	a = 1
	def func():
		a = 2 
		print(a) # 1
	func()
	print(a) # 2
	```
	다음은 순차적으로 2,1이 출력될 것입니다!
	
	1번 위치에서 a는 local scope에 선언된 namespace를 바라볼 것이고, 이는 2입니다.
	하지만 2번 위치에서 a는 global scope에 있는 1입니다.
	
	1번에서 사용한 a처럼 특정 범위에서만 유효한 scope를 `local scope`라고 합니다. 

### lookup mechanism

위의 3번째 예시처럼, 실제 코드에서는 *변수명이 겹치는 경우*가 많이 발생합니다. 이때 파이썬 런타임은 어떤 변수에 접근할지 어떻게 결정할까요?

앞서 말했듯, **작은 스코프에서는 상위 스코프의 네임스페이스에서 접근할 수 있습**니다.

이는 mutable object에서도 마찬가지입니다.
```python
list1 = []

def func1():
	list1.append(1)

func1()
print(list1)
```
func1의 로컬 스코프에서는 global scope를 갖는 list1에 접근이 가능합니다.

"위의 local scope의 예시에서는 `a=2`를 했는데 왜 반영이 되지 않느냐?" 하면 [immutable object의 특징](https://choieastsea.github.io/posts/python-mutable-immutable/)을 다시 보면 좋습니다. (a=2는 값을 변경한 것이 아니라, 초기화한 것이기 때문입니다)

```python
def func1():
	a = 1
func1()
print(a)
```
위의 코드는 오류가 나올 것입니다.

a는 func1의 로컬스코프에서 선언되었으므로, global namespace에는 a에 대한 정보가 없기 때문입니다.

>현재 스코프의 네임스페이스부터 확인하고, 해당 변수명이 존재한다면 더 이상 확인하지 않습니다! 
>
>만약 존재하지 않는다면, 상위 스코프를 차례대로 확인합니다.
 {: .prompt-warning}

## lexical scope?!

위의 예시만으로 설명되지 않는 부분들이 존재합니다.
```python
def func():
	print(x)
	x = 2
func()
print(x)
```
해당 코드는 `UnboundLocalError: cannot access local variable 'x' where it is not associated with a value` 라는 오류가 발생합니다. 하지만, 2번 줄을 지우고 실행하면, `NameError: name 'x' is not defined`가 발생합니다.

python에서 스코프는 함수가 선언되는 시점에 결정됩니다. 이를 `lexical scope`라고 부릅니다. 따라서, func가 실행되기(4번 줄) 이전 선언(**1번 줄)에서** x가 bound되지 않은 상태로 초기화되었는데 2번줄에서 출력하려하니 해당 오류가 발생하는 것입니다.

> 함수가 선언되는 시점에 scope를 생성되고, 지역변수는 unbound된 상태입니다.
 {: .prompt-tip }

그럼 다음 코드의 결과를 예상해볼 수 있겠죠?
```python
x = 1
def func():
	print(x)
x = 2
func()
print(x)
```
위의 코드와 마찬가지로 `UnboundLocalError`가 발생할 것입니다.

그치만, 3번 라인에서 global scope의 1을 출력하고 싶은 경우, 즉 안쪽 scope에서 같은 이름으로 선언되었지만, 바깥 scope의 변수에 접근하고 싶은 경우에는 어떻게 할까요?

아래의 `global`, `nonlocal` 키워드를 알아두면 좋습니다!
## global / nonlocal keyword

클릭이 한번씩 일어날 때마다 count를 1씩 증가하는 함수를 만들고 싶습니다.
```python
cnt = 0

def click():
	cnt += 1

for _ in range(10):
	click()

print(cnt)
```
하지만, 위의 코드도 역시 `UnboundLocalError`가 발생합니다. 이유는 4번 줄에서 unbound상태인 local 변수 cnt에 할당을 하려하기 때문이죠. cnt라는 이름은 같지만 shadowing mechanism에 의해 항상 `local namespace`로 접근하게 됩니다.

click의 예시처럼 내부 namespace를 항상 사용하지 않고, 특정 상황에서 외부 namespace를 사용하고 싶을 때가 있는데요. `global`과 `nonlocal` keyword 입니다!
```python
cnt = 0

def click():
	global cnt # 1
	cnt += 1 

for _ in range(10):
	click()

print(cnt)
```
이제야 10이 잘 나옵니다! 1번 위치에 cnt를 global namespace에서 가져오도록 명시하였기에 shadowing되지 않고 외부 변수에 접근할 수 있습니다.

`nonlocal`의 경우에는 global이 아닌 바로 바깥 namespace에서 가져올 때 사용합니다.

> 내부에서 외부 namespace에 접근하고 싶지만, 변수 이름이 같아 내부 namespace밖에 사용하지 못하는 경우, `global`과 `nonlocal` 키워드를 사용합니다.
  {: .prompt-tip }


## 참고

https://www.youtube.com/watch?v=WYZrLtFNDVI

https://www.pythontutorial.net/advanced-python/python-variable-scopes/

