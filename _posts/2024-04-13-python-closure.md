---
title: (Python) closure (nested function, free variable, decorator)
date: 2023-04-13 00:00:00+0900
categories: [python, basic]
tags: [python, function, nested function, closure, free variable, decorator]
render_with_liquid: false
---

python scope에 대해 공부해보았는데, 사실은 *함수가 종료된다 해서 항상 변수가 소멸되지는 않습니다*. 오늘은 이와 관련한 `closure`에 대해 알아봅시다!

## nested function

파이썬에서는 함수 안에 함수를 정의할 수 있습니다. 파이썬에서 메인 함수를 별도로 작성해보았다면 이런 `nested function`(중첩함수)은 익숙할 것입니다.
```python
def main():
	cnt = 0
	def click():
		nonlocal cnt
		cnt += 1
	click()
	print(cnt)

if __name__ == "__main__":
	main()
```
`global`로 선언된 main함수는 `local`에 선언된 click함수를 호출합니다. click 함수에서 cnt 변수는 click의 로컬 영역이 아닌, `nonlocal` 영역을 참조하도록 선언하였으므로 외부의 cnt에 접근이 가능합니다! 

### 함수를 리턴하는 함수

파이썬에서는 함수도 일급객체(First Class Object)입니다. 말은 어렵지만, 변수처럼 사용할 수 있다는 것입니다.

알고리즘 문제를 풀 때 입력을 빠르게 처리하기 위해 `input = stdin.readline`으로 readline 함수를 input이라는 변수명으로 할당하는 경우도 이에 해당합니다!

또 다른 예시로는, 콜백 함수입니다! 함수를 인자로 넣어줄 수 있습니다. (`lambda` 등을 이용한 익명 함수로 자주 사용됨) 그리고, 함수를 반환하는 함수를 만들 수도 있습니다. 
```python
def outer():
	a = 1
	print('outer executed')
	def inner():
		print('inner executed')
		print(f'a = {a}')
	return inner

inner_func = outer() # outer가 실행 & inner가 inner_func에 할당
inner_func()
```
위의 실행 결과로는
```
outer exectued
inner executed
a = 1
```
이 출력이 됩니다. 당연한 것 같지만, scope만 배웠던 우리는 궁금증이 생길 수 있습니다.

**outer함수가 종료되었는데도 왜 inner에서 outer의 local scope에 선언된 a에 접근할 수 있는걸까요?**

### free variable

위의 예시에서 변수 a는 외부 함수에서 선언되었지만, *외부 함수가 종료된 이후에도 사라지지 않고 남아있습니다*. 이처럼 해당 스코프에서 사용되지만 선언되지는 않은(즉 외부에서 선언된) 변수를 `free variable`이라고 합니다! [공식문서](https://docs.python.org/3/reference/executionmodel.html#binding-of-names)

이를 기반으로 위의 상황을 이해해봅시다.

`outer()`시, `inner` 함수는 실행되지 않고 선언되어 리턴된 상태입니다. 따라서, `inner_func()`를 통해 inner 함수를 실행하면 free variable인 a를 접근하는 것이죠.

inner 함수 입장에서 nonlocal 영역에 선언된 변수를 사용한다면 이는 free variable이라고 볼 수 있습니다.

## closure

클로져는 외부 스코프의 변수를 하나 이상 참조하고 있는 내부 함수입니다.

쉽게 표현하면 `free variable`을 포함한 내부 함수라고도 할 수 있습니다! 위의 예시에서 `inner`함수는 클로져입니다.

free variable은 **외부 함수가 종료된 이후에도 남아있어, closure가 종료되어야 메모리에서 삭제되는 특징**을 갖고 있습니다.

### `__closure__` 속성

python function에는 `__closure__`라는 property를 통해 이를 확인할 수 있습니다. [공식문서](https://docs.python.org/3/reference/datamodel.html#special-read-only-attributes)

위의 예시에서 closure 속성을 확인해봅시다! `hex(id(variable))`은 변수의 (가상)메모리 위치를 16진수로 표현하기 위해 사용합니다.

```python
def outer():
	a = 1
	print('outer executed')
	print(f'address of a : {hex(id(a))}')

	def inner():
		print('inner executed')
		print(f'a = {a}')
	return inner

inner_func = outer()
print(inner_func.__closure__)
inner_func()
```
다음과 같이 출력됩니다.
```shell
outer executed
address of a : 0x1013d8e20
(<cell at 0x1006dfc10: int object at 0x1013d8e20>,)
inner executed
a = 1
```
이를 그려보면 다음과 같습니다.
![scope](assets/img/2024-04-13-python-closure/closure.png)
free variable를 참조하는 `cell`을 두고, cell들의 튜플로 closure property를 관리함을 알 수 있습니다! 클로저에 포함된 변수는 내부 함수까지 완전히 종료되어야 메모리에서 해제될 것이므로, 별도의 속성으로 관리되는 듯 합니다.

추가로, `__code__.co_freevars`라는 속성으로 해당 함수의 free variables tuple을 확인해볼 수 있습니다.

위의 예제에서는 `print(inner_func.__code__.co_freevars)`의 결과는 `(a,)`로 나오는 것을 확인할 수 있습니다!

> 내부함수에서 외부에서 선언된 변수에 접근하게 하기 위해 closure를 도입했다고도 볼 수 있습니다!
{: .prompt-tip }

## 응용 

이러한 클로저의 특성을 이해하는 것도 중요하지만, 어떻게 활용될 수 있는지도 아는 것이 중요합니다.
### instance variable

**free variable은 closure가 종료될 때까지 남아있다는 속성**을 이용하여 인스턴수 변수처럼 활용이 가능합니다! 다음은 간단한 계산기를 closure로 구현한 코드입니다.

```python
def calculator(x):

	def add(y):
		nonlocal x
		x += y
		return x
		
	def multiply(y):
		nonlocal x
		x *= y
		return x
		
return add, multiply

# (1+2+3)*4
add, multiply = calculator(1)
print(add(2))
print(add(3))
print(multiply(4))
```
calculator 실행시 x값이 초기화되고, add, multiply 함수가 리턴됩니다. free variable인 x는 add와 multiply를 포함한 closure를 통해 접근이 가능해집니다! 따라서 `(1+2+3)*4`의 결과인 24가 출력되는 것을 확인할 수 있습니다.

이는 마치 class를 선언하고, method를 통해 인스턴스 변수에 접근하고 변경하는 것과 유사합니다!
```python
class calculator:
	def __init__(self, x):
		self._x = x
	
	def add(self, y):
		self._x += y
		return self._x
	
	def multiply(self, y):
		self._x *= y
		return self._x

calc = calculator(1)
print(calc.add(2))
print(calc.add(3))
print(calc.multiply(4))
```
> 이처럼 (외부 함수가 호출된 이후에도 closure에 의해 접근 가능한) free variable을 이용하여 global variable의 충돌을 방지할 수 있는 옵션으로도 사용하 수 있습니다!
{: .prompt-tip }

### decorator

위의 예시는 자주 활용되지는 않는 것 같습니다. 다만, 중첩함수(`nested function`)와 이를 이용한 `decorator`는 파이썬에서 굉장히 자주 사용됩니다!

예를 들어, 어떤 상태(state)가 변하고, 그 상태에 따라 함수가 실행되거나 실행되지 않도록 하는 decorator를 만들어봅시다. (데코레이터는 쉽게 말해 함수를 인자로 받는 함수입니다!)
```python
def execute_if_state(state):
	def decorator(func):
		def wrapper(*args, **kwargs):
			if state:
				return func(*args, **kwargs)
			else:
				print('state is bad')
				pass
	return wrapper
return decorator

@execute_if_state(True) # 해당 함수는 실행될 것
def func1():
	print('func1')

@execute_if_state(False) # 해당 함수는 실행되지 않을 것
def func2():
	print('func2')

func1() # func1
func2() # state is bad
```
`execute_if_state` 함수는 state라는 외부 스코프에 접근하는 decorator function을 리턴합니다. closure를 통해 wrapper에서 state에 접근할 수 있게 되고, state는 선언부에서 *execute_if_state가 종료된 이후에도 남아있어 함수 실행에 영향을 줄* 수 있게 됩니다!!

데코레이터 자체에 대한 내용은 다음에 다뤄보도록 하겠습니다. 오늘은 외부 스코프에 선언된 `free variable`이 `closure`에 의해 남아있으며 접근 가능하다는 점에 초점을 맞추면 될 것 같습니다.
## 참고

https://www.pythontutorial.net/advanced-python/python-closures/

https://www.youtube.com/watch?v=tNSOaA1z6Uo