---
title: (Python) mutable/immuatble과 pass by sharing
date: 2023-03-26 22:00:00+0900
categories: [python, basic]
tags: [python, object, mutable, immutable, argument, pass by sharing]
render_with_liquid: false
---

python에서 모든 데이터는 객체입니다. 숫자, 배열, 심지어 함수도 객체입니다.

오늘은 이러한 파이썬의 data model과 대표적인 mutable/ immutable 객체에 대해 알아보겠습니다.



## id() / is 함수

모든 object는 `id`함수를 통해 구분할 수 있고, 같은 객체인지의 여부는 `is` 이항 연산자로 확인 가능합니다.

id는 객체의 주소값과 긴밀하게 연관되어 있어, id가 같은 두 데이터는 값이 일치하며 (가상 메모리 공간에) 저장된 위치도 같습니다.

만약 `A is B`가 `True`인 경우, 메모리영역을 추상화하여 그리면 아래와 같을 겁니다.
![A is B](assets/img/2024-03-26-python-mutable-immutable/A_IS_B.png)

주의할 점은, **값이 같은 것**과 **같은 객체** 인 것은 구분해야 한다는 것입니다.

1. 값이 같다 ↔️ `A==B`가 `True`
2. 같은 객체이다 ↔️ `A is B`가 True

```python
a = []
b = []
print(a == b) # True
print(a is b) # False
```
위의 코드를 보면 a와 b의 값은 같지만, 다른 주소에 각각 배열이 할당되어 있으므로 같은 객체가 아닙니다.

> 값은 쉽게 말해서 생김새라고 보면 될 것 같습니다!
{:.prompt-tip }


대입 연산시, 할당이 일어나는지 재할당이 일어나는지 ..?

## mutable object

- lists
- sets
- dictionaries

mutable 객체는 **id가 변함 없으면서, 값을 변경할 수 있는 객체**를 의미합니다.

주로 container(객체를 포함한 객체)는 mutable합니다.(tuple 제외)

### 생성/할당

mutable object는 생성시 항상 새 값(다른 위치에 새로운 값)을 만들어준다는 특징이 있습니다. 
```python
a = []
b = []
print(a == b) # True
print(a is b) # False
```
따라서, 위의 코드에서 `a is b`가 False가 나온 것입니다. `[ ]`로 새로운 빈 배열을 만들 때 항상 별도의 공간에서 새로운 배열을 만듭니다.

### 변경

연산에는 mutable object는 할당이 아닌 **변경**이 일어나게 됩니다.
```python
a = []
b = a

a.append(1)

print(b)
print(a==b)
print(a is b)
```
빈 배열을 a, b가 공유하도록 하고, a에 원소 1을 추가해봅시다. 

둘은 모두 배열의 위치를 가리키고, 배열은 mutable object이므로, b 역시 계속 같은 배열을 가리키게 됩니다.

따라서, 1이 출력되며 `a==b`, `a is b`모두 True입니다!

## immutable object

- numbers
- strings
- tuples
- frozen sets

immutable 객체는 **값을 변경할 수 없는 객체**를 의미합니다.

### 생성/할당

불변객체는 생성시 새 값을 만들어줄 수도 있고, **기존 객체를 사용할 수도 있**습니다.

```python
a = 1
b = 1

print(a == b)
print(a is b)
```
다음 코드의 결과는 어떻게 나올까요? mutable object와 다르게 immutable object는 할당시 기존의 값을 사용할 수 있습니다. 따라서, 모두 True입니다!

하지만, **항상 기존의 값을 사용하지는 않**고, 새로 생성하는 경우도 있습니다. 다음 코드를 보시죠.
```python
a = (1,2)
b = (1,2)

print(a == b)
print(a is b)
```
tuple `(1,2)`를 a와 b가 공유할까요?

상당수의 경우 그렇지만, **이를 보장하지는 않습니다**. (저도 놀랐습니다~!)

파이썬 공식문서에서도, 이렇게 표현하고 있습니다. 

> Types affect almost all aspects of object behavior. 
>	
> Even the importance of object identity is affected in some sense: for immutable types, operations that compute new values may actually return a reference to any existing object with the same type and value, while for mutable objects this is not allowed. 
>	
> E.g., after `a = 1; b = 1`, `a` and `b` may or may not refer to the same object with the value one, depending on the implementation, but after `c =[]; d = []`, `c` and `d` are guaranteed to refer to two different, unique, newly created empty lists. (Note that `c = d = []` assigns the same object to both `c` and `d`.)
{:.prompt-warning}

결론을 내리자면, **생성/할당시 immutable object는 각각 다른 주소값이 보장되지만, mutable object는 보장되지 않는다** 입니다.

### 변경

mutable object처럼 공유된 값을 변경하는 코드를 작성해봅시다.
```python
a = 1
b = a

print(id(a))
a += 1
print(id(a))

print(a == b)
print(a is b)
```
a의 값이 변경되는 것처럼 보이지만, 값 뿐만 아니라 **재할당**이 일어난 것입니다! a = (a + 1)을 수행한 순간 a가 갖고 있던 주소값이 변경 됩니다.

따라서, `a == b`, `a is b` 모두 False 입니다!


## python 평가전략 (pass by sharing)

`call by value`, `call by reference`라는 표현을 들어보았는데요. 

이러한 특징을 `평가전략(Evaluation Strategy)` 이라고 하고, 함수 내부의 argument 대입시, 일어나는 객체 전달 방식으로 이를 구분합니다.

python에서는 `pass by sharing` 이라는 전략을 사용합니다. (용어보다는 느낌을 아는 것이 중요할 것 같습니다) 

### immutable - 1 증가시키기

1을 더해주는 함수를 구현하였습니다. x는 어떻게 출력이 될까요?
```python
def plus(a):
	a += 1

x = 1
plus(x)

print(x)
```
x는 그대로 1이 출력이 됩니다... 이 결과로 유추하면 인자로 전달한 a의 값만 변경하였으므로 x에는 영향을 주지 않았다고 볼 수 있습니다.

mutable object도 한번 함수 내에서 변경해보겠습니다.

### mutable - 배열에 값 추가하기

```python
def add_1(a):
	a.append(1)

x = [0]
add_1(x)
print(x)
```

이전의 논리대로면 x가 아닌 a에 추가한 것이므로, x 에는 영향을 주지 않아야할 것입니다.

하지만 `x: [0, 1]`가 출력됩니다. mutable과 immutable이 좀 다른 것 같네요.

argument는 변수에 영향을 줄 수 있는 걸까요? 없는걸까요?

### pass by sharing

[python tutor](https://pythontutor.com)라는 사이트에서 런타임에서의 메모리 구조를 확인할 수 있습니다.

위의 코드들을 실행하고, 함수 호출 직후의 메모리 상태는 다음과 같습니다.

1. immutable object
	![immutable](assets/img/2024-03-26-python-mutable-immutable/immutable.png)


	plus 함수에 argument 1이 전달되었는데, x를 가리키는 것이 아니라 a에 독자적인 1이 할당되었습니다.
	
	따라서, **a를 지지고 볶아도 x에는 영향을 미치지 않습니다**.

2. mutable object
	![mutable](assets/img/2024-03-26-python-mutable-immutable/mutable.png)
	add_1 함수에 argument로 `[0]`배열이 전달되었는데, x와 같은 배열을 가리키고 있습니다.
	
	따라서, **a에 값을 변경하면 x에도 영향을 미치게 됩니다**.


function context에 존재하는 변수 scope가 생겼을 뿐이지, mutable / immutable의 특성을 그대로 갖게 됨을 확인할 수 있습니다.

정리하면 다음과 같습니다.

> mutable object : 생성시 new object 생성 보장 / 변경시 주소 변경되지 않음.
> 
> immutable object : 생성시 기존 object 사용할 수도 / 변경은 없고 사실상 재할당 되어 주소가 변경됨.
{:.prompt-tip}

화이팅!

## 참고

[python data model](https://docs.python.org/3/reference/datamodel.html)

[is operator in mutable / immutable](https://stackoverflow.com/questions/63207389/is-the-is-python-operator-reliable-to-test-reference-equality-of-mutable-objec)

[메모리 구조 시각화 사이트](https://pythontutor.com)