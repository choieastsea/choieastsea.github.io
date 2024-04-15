---
title: (Python) decorator (구현 방법, closure과의 비교, functools.wraps())
date: 2023-04-16 00:00:00+0900
categories: [python, basic]
tags: [python, function, decorator, closure, functools]
render_with_liquid: false
---
오늘은 파이썬에서 함수 혹은 클래스를 꾸며주는 역할을 해주는 decorator에 대해 알아보도록 합시다!
## decorator란?

decorator는 원본 객체(혹은 함수)를 직접 건드리지 않고, 해당 객체를 포함하면서 새로운 특징을 추가하여 제공하는 기법입니다. 

*본래의 핵심 기능을 유지하며 동시에(원본 객체의 수정없이) 추가 기능을 덧붙이는 것처럼 구현할 수 있는 것이 핵심*입니다.
## python decorator

`python decorator`는 함수 내부를 건드리지 않고, 이를 확장한 기능을 제공하는 새로운 함수라고 보면 됩니다.

보통 이를 구현하기 위해 `wrapper` 함수를 만들어줍니다. 함수를 실행하기 전에 항상 `start!`를 출력해주는 기능을 데코레이터로 구현해봅시다.

```python
def print_strt(func):
	def wrapper(*args, **kwargs):
		print('start!')
		func(*args, **kwargs)
	return wrapper

def plus(a, b):
	print(a+b)
	return a+b
  
print('before decorating')
plus(1, 2) # 3

print('after decorating')
plus = print_strt(plus) # decorating by composition
plus(1, 2) # start!\n3
```

15번줄에서 `print_strt`라는 고차함수는 plus 함수를 argument로 받고, 기존 plus함수에서 앞에 출력 로직이 포함된 **꾸며진 wrapper 함수**를 리턴합니다. 따라서 우리는 *꾸며진(기능이 추가된) plus 함수*를 호출할 수 있게 됩니다!

따라서, 12번줄에서 실행된 plus는 단지 3만을 출력하지만, 16번줄에서 실행된 plus는 `start!`를 출력하고 3을 출력합니다.

이러한 데코레이터의 특징은 **고차 함수를 통하여 기존 함수의 로직을 변경하지 않고도 관점 중심의 로직을 추가할 수 있다는 점**이 있습니다.

> 디자인 패턴중, decorator pattern은 이에 대한 일반적인 경우(이므로 구분할 필요가 있다!)라고 보는게 좋을 것 같습니다.
>
> python decorator는 함수를 꾸며주기 위해 사용되고, `decorator pattern`은 동적으로 객체에 기능을 더해주기 위해 사용됩니다.
 {: .prompt-tip }

### 구현 방법 (1) composition

이 정도 공식(?)만 알아둬도 좋을 것 같습니다.
1. 함수를 인자로 받습니다
2. wrapper를 리턴합니다 (저는 wrapper라고 표현하지만 어쨌든 내부함수를 리턴합니다)
	wrapper는 인자로 받은 함수에서 필요한 인자를 받습니다.
3. wrapper함수에서는 인자로 받은 함수를 실행하고, **그 전후로 데코레이터 로직이 들어갈 수 있습니다!!**
	인자로 받은 함수의 실행 결과 리턴값을 이용할 수도 있고, 아닐수도 있습니다.

결국은 wrapper 함수에서 고차함수에서 인자로 받은 함수를 실행하며 추가적인 행동을 수행하는 것이 핵심입니다. *수학에서의 합성함수와 비슷하다고 볼 수 있겠습니다*만, **실행 함수의 값을 항상 다음 함수의 인자로 활용하지는 않아도 된다는 점에서 구분**됩니다.

> wrapper 함수는 conventional하게 사용하는 것이므로 필수는 아니지만, 저의 경우 데코레이팅이 일어나는 내부 함수를 wrapper 함수라고 부르겠습니다.!
 {: .prompt-warning}

### 구현 방법 (2) @(at sign)

항상 함수를 합성하여 데코레이터 패턴을 구현하는 것은 귀찮습니다. 특히 합성이 늘어날수록 복잡해집니다. 

따라서, 파이썬에는 *@(at sign)을 이용하여 이를 간소화할 수 있도록 제공*합니다. 꾸며줄 함수의 선언부 위에, `@데코레이터의 이름`의 꼴을 추가해주는 것입니다. 

앞의 코드를 간단하게 바꿔봅시다!
```python
@print_strt
def plus(a, b):
	print(a+b)
	return a+b

plus(1,2) # start!\n3
```
훨씬 간단해지지 않았나요??

또 다른 예시로 중첩 데코레이팅에 적용해 봅시다. 데코레이터 선언부는 동일하므로, 생략하였습니다.
```python
def func(arg1, arg2, ...):
    pass
func = dec2(dec1(func))
```
위의 복잡한(읽히지 않는) 코드는 아래 처럼 바꿔줄 수 있습니다!
```python
@dec2
@dec1
def func(arg1, arg2, ...):
    pass
```

참고로, *중첩 데코레이팅의 경우* 순서는 다음과 같습니다.
- @로 표현한 경우에는 함수 선언부로부터 **가까운 순서부터** 래핑되어 함수를 꾸며줍니다. 
- 함수의 합성(composition)으로 표현한 경우에는 (당연히) **안쪽부터** 함수를 꾸며줍니다.

이처럼 **@을 이용하면 더욱 직관적이고 유연하게 표현이 가능**합니다.

추가로, 데코레이터도 함수이므로 `데코레이터를 위한 arguments`를 받을 수도 있습니다.
```python
@decomaker(argA, argB, ...)
def func(arg1, arg2, ...):
    pass

func = decomaker(argA, argB, ...)(func)
```
복잡해질수록 composition으로 데코레이팅을 하는 것은 직관적이지 않죠? **@를 이용하여 데코레이터를 편하게 이용**합시다. 

물론, 데코레이터를 **선언하는 것**은 위의 공식을 보고 익숙해지도록 합시다.

## closure와 다른가요?

지난 시간에는 `closure`에 대해 알아보았는데요. [링크](https://choieastsea.github.io/posts/python-closure/)

`decorator`와 `closure`는 연관되어있지만, 구분되는 개념입니다! 

- closure 입장에서 decorator를 보면, **free variable이 함수**이고, closure에서는 이를 실행하고 꾸며줍니다.
- decorator 입장에서 보면, **wrapper 함수는 곧 closure입니다**.

decorator는 free variable이 함수인 closure (= wrapper 함수)에서 추가적인 동작이 수행되는 것이라고 보면 되겠죠?

## class decorator

python에서는 `dataclass`를 다음과 같이 설정합니다.
```python
from dataclasses import dataclass

@dataclass
class Person:
	name : str
	age : int
```
오호라... class를 꾸며주는 decorator도 구현하는 방법도 간단하게만 알아봅시다! 

class도 변수처럼 활용이 가능하므로, 인자로 class를 받아서 **꾸며진 새로운 class를 리턴**하면 됩니다.

그 과정에서 원본 class의 `__init__`메서드를 `setattr`등을 이용해 꾸며진 init 함수로 오버라이드하여 구현하는 것 같습니다.

> 데코레이터는 함수 뿐만 아니라 메서드, 클래스, 제네레이터 등에도 적용이 가능합니다.
>
> 데코레이터를 구현하는 방법도 함수를 이용하지 않고, call을 이용한 class로도 구현 가능하다고 합니다.!
 {: .prompt-tip }
## functools.wraps() 로 센스있는 데코레이터 사용

데코레이터를 포함하여 고차함수(`high order function`)관련된 기능이 있는 `functools` 라이브러리가 있습니다. 간단하게 `functools.wraps()`를 이용하여 데코레이터를 풍성하게 할 수 있습니다.

단지, wrapper 함수앞에 한줄만 추가하면 됩니다.
```python
def print_strt(func):
	@functools.wraps(func)
	def wrapper(*args, **kwargs):
		print('start!')
		func(*args, **kwargs)
	return wrapper
```
특징으로는 꾸며질 함수의 <u>docstring이 유지가 된다</u>는 점이 있습니다. 논리적인 동작보다도, **데코레이터의 본분을 지킬 수 있도록 도와주는 데코레이터**라고 볼 수 있겠습니다! 

또한, wrapper 함수 위에 해당 데코레이터가 있다면 해당 함수는 데코레이터라는 것을 알려주는 컨벤션으로도 활용이 가능합니다.

## 정리

데코레이터는 `AOP`에서 자주 활용되는 개념으로 로그를 찍거나, 소요 시간을 측정하거나, 파라미터를 검사하거나, 캐싱을 하거나 등 비즈니스 로직 이외에 로직을 수행하는데 유용하게 사용됩니다.

사용법 이외에도 선언법을 익히고, 활용할 수 있으면 좋을 것 같습니다.

## 참고

[PEP 318 – Decorators for Functions and Methods](https://peps.python.org/pep-0318/)

[PEP 3129 – Class Decorators](https://peps.python.org/pep-3129/)


