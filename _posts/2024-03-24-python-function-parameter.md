---
title: python function parameter과 관련한 특징들(전달방법 / packing & unpacking / *과 ** /  키워드 강제하기)
date: 2023-03-24 22:00:00+0900
categories:
  - Python
tags:
  - Python
render_with_liquid: false
---

오늘은 파이썬 함수에서 인자를 선언하는 몇가지 방법에 대해 알아보겠습니다.

또한, 자유도가 높은 파이썬에서 좋은 convention이 있을지 생각해봅시다.

# parameter

> 간단하게, 선언할 때의 변수는 `parameter`, 호출할 때는 `argument`라고 생각할 수 있습니다!
{: .prompt-tip }

`parameter`는 함수를 호출하며 전달한 `argument`를 받기 위해 **선언한 변수**입니다. 
python에서는 parameter를 선언하는 방법과 argument를 전달하는 방법이 몇가지가 있습니다.

1. 기본적인 사용법
	
	보통 parameter와 argument는 다음처럼 사용합니다.
	```python
	def sum(a, b):
		# a,b 는 parameter
		return a + b
		
	result = sum(1, 2) # 1,2는 argument
	print(result) #3
	```
2. default parameters

	위의 경우에는 sum 함수를 호출하기 위해 **2개의 인자를 모두 전달**해야 합니다.
	따라서, optional 한 parameter의 경우에는 다음과 같이 선언할 수 있습니다.
	```python
	def sum_or_plus1(a, b=1): # b=1로 기본값 선언
		return a + b
		
	print(sum_or_plus1(2)) # 3
	```
	주의해야할 점은, `default parameter` 혹은 `optional parameter` 는 **항상 일반 parameter 뒤에 선언해야 한다**는 것입니다. 
	
	호출시 뭐가 optional인지 애매해지기 때문에 `Non-default argument follows default argument` 오류가 발생할 것입니다.

	이렇게 호출시 **이름을 지정하지 않고, 순서에 맞게 매핑되도록 호출하는 인자**를 `positional arguments`라고도 부릅니다.	
	
3. keyword arguments
	
	그 외에도 호출하는 방법이 있는데요. 호출시 **이름을 keyword로 매핑해서 호출하는 인자를** `keyword arguments`라고 부릅니다. 
	```python
	def sum_or_plus1(a, b=1):
		return a + b
		
	print(sum_or_plus1(b=3, a=2)) # 5
	```
	positional 방법과 비교하여 <u>호출시의 장점이 존재</u>합니다.	
	1. 함수 호출시 변수의 **순서에 있어 자유**롭고 
	2. 변수명을 명시하므로 호출시 **혼동을 줄여줍**니다.
	또한 호출시, `sum_or_plus1(1,b=2)`처럼 `positional argument`와 `keyword argument`를 혼용하여 사용할 수도 있습니다.


>  positional arguments(순서에 맞게 호출) 는 **list와 유사**하고, keyword arguments는(파라미터명에 맞춰 호출) **dictionary와 유사**한 것 같습니다!
{: .prompt-tip }
# packing/unpacking 과 `*`, `**`

`unpacking`은 값을 나눠 재할당 하는 것입니다. js의 `destructuruing assignment`와 유사하다고 볼 수 있을 것 같습니다.

```python
first,second,third = [1,2,3]

print(type(first)) # <class 'int'>
print(second) # 2
```
괄호가 없는 경우, 기본적으로 `tuple`로 선언되므로 아래처럼 pythonic하게 `swap`을 구현할 수 있습니다.
```python
first,second = 1,2
first,second = second,first

print(first,second) # 2 1
```

이런 unpacking을 처리해야하는데, sequence의 갯수를 알 수 없는 경우에는 어떻게 받을 수 있을까요?
```python
first,second = 1,2,3
```
위의 코드는 `ValueError: too many values to unpack (expected 2)`가 나올 것입니다.

이런 경우, 개발자들은 의미없는 변수를 나타내는 `_`를 사용하여 3번째 인자를 받아올 수 있습니다.
```python
first,second,_ = 1,2,3
```
하지만, 더 많아지면 `*` operator를 이용할 수 있습니다.
```python
first,second,*others = 1,2,3,4,5,6
```
first는 1, second는 2, others는 [3,4,5,6]이 할당될 것입니다. 기본적으로 우항의 sequnce 객체의 한 원소씩 할당이 되며, `asterisk` operator로 선언된 변수가 있다면 **하나의 list로 묶어서(packing) 할당**이 됩니다.

반대로, 이를 `unpacking`할 때에도 사용할 수 있습니다. 위의 코드에서 이어서 작성해봅시다.
```python
print(others) # [3,4,5,6]
print(*others) # 3 4 5 6
```
`*`가 없다면 others라는 리스트를 출력하지만, `*others`로 print 함수 인자로 전달하면 3, 4, 5, 6이 각각 전달되어 출력되는 것입니다.

정리하면, 변수 앞의 `*`는 두가지 상황 모두 사용 가능합니다.

> - **packing : 변수 선언시 ➡️ (parameters) 를 하나로 묶어준다**
> - **unpacking: 변수 이용시 ➡️ (arguments)를 나눠준다**
{: .prompt-info }

print가 아닌 다른 함수에서도 적용해봅시다!
## `*`args : variadic parameters

parameter에서의 `*`는 곧 *여러 값을 하나의 sequence 타입의 변수로 묶어서(packing) 처리하겠다는 것을 의미*합니다. 아래의 코드를 보시죠.

```python
def print_and_sum_all(*args):
	print(args)
	return sum(args)
	
result = print_and_sum_all(1,2,3,4) # (1,2,3,4)
print(result) # 10
```
위의 코드를 실행시, args에는 1,2,3,4가 packing되어 list로 할당될 것입니다.

주의할 점으로는, `*`로 선언한 파라미터 이후의 값을 이용하기 위해서는 항상 `keyword argument`로 호출해야한다는 점입니다. (잘 생각해보면 당연합니다! packing의 범위를 명시하지 않았기 때문이죠)

```python
def print_and_sum_all(*args, extra_value):
	print(args)
	print('extra value:', extra_value)
	return sum(args) + extra_value

result = print_and_sum_all(1,2,3,4, extra_value=5)
print(result)
# (1, 2, 3, 4)
# extra value: 5
# 15
```
위의 코드를 보면, `extra_value`라는 파라미터에 값을 전달하기 위해서는 **무조건** keyword parameter로 넘겨줘야합니다! 
만약 `print_and_sum_all(1,2,3,4,5)`로 호출했다면, (1,2,3,4,5)까지 packing되어 args로 할당되고, extra_value는 값이 전달되지 않았으므로 에러가 발생할 것입니다.

`*args`와 같이 여러 positional argument를 packing하기 위해 사용되는 파라미터를 `variadic parameter`라고 한다고 합니다. `args`는 conventional한 표현이고, 상황에 맞게 사용하면 됩니다.

## `**` kwargs : keyword parameters

parameter에서 `**`는 *keyword arguments를 하나의 dictionary로 묶어서(packing) 처리하겠다는 것*을 의미합니다.

```python
def print_all(**kwargs):
	print(type(kwargs))
	print(kwargs)

print_all(a=1,b=2,c=3,extra=4)
# <class 'dict'>
# {'a': 1, 'b': 2, 'c': 3, 'extra': 4}
```
keyword arguments로 넘어온 값들을 하나의 dictionary로 packing함을 확인할 수 있습니다.!

또한, dictionary를 `unpacking`하여 함수에 전달할 때에도 사용됩니다. 이는 보통 다음과 같이 사용됩니다.

```python
def print_person_info(name, age):
	print(f'name : {name} / age : {age}')

me = {'age' : 25, 'name' : 'eastsea'}
print_person_info(**me)
```
`*`와 다르게 `**`는 **unpacking하여 print할 수 없는데**, 그 이유는 dictionary의 key가 함수의 parameter와 매핑되지 않기 때문이죠! (print 함수의 keyword parameter 에 name, age가 있다면 가능하겠죠..?)

어쨌든 선언시에는 dictionary로 packing하고, 호출시에는 key와 value로 unpacking하는 것은 맥락상 single asterisk와 동일합니다!

추가로 `**kwargs`도 다른 parameter와 혼용할 수 있지만, 선언시 일반 parameter 뒤에 위치해야 한다는 주의점이 있습니다.
```python
def print_person_info(id, **info):
	print(f'id: {id}, {info}')

print_person_info(1, name='eastsea', age=25)
```
위의 코드는 가능하지만, id parameter는 항상 keyword parameter 앞에 위치해야합니다!
그렇지 않다면 `arguments cannot follow var-keyword argument` 오류가 나옵니다...

### 복잡한 예제

오늘 내용을 정리해 볼만한 복잡한 예제를 생각해봅시다!

다음 코드들이 동작하는지 생각해보고, 동작한다면 어떤 결과가 나오는지 생각해봅시다.

```python
def smth(id, *info, extra):
	pass

smth(1, 2, 3, 4)
```

`*info` 뒤에 선언한 extra는 항상 keyword argument로만 호출 가능합니다. 따라서 오류 발생합니다.

아래처럼 default parameter로 한다면? 가능합니다.
```python
def smth(id, *info, extra=5):
	pass

smth(1, 2, 3, 4)
```

추가로, `*`와 `**`가 혼용된 예시도 살펴봅시다.

```python
def smth(id, *info, **extra):
	print(id)
	print(info)
	print(extra)

smth(1, 2, 3, 4)
```

id는 positional argument로 전달된 1, info는 그 이후의 positional arguments들을 묶은 2,3,4가 될 것이고, extra는 keyword argument로 전달된 것이 없으므로 빈 dictionary가 출력될 것입니다!

하나만 더 살펴보죠.
```python
def smth(id, *info, **extra):
	print(id)
	print(info)
	print(extra)

smth(a=1, b=2, id=3, 4, 5) # (1)
smth(4,5,6,a=1, b=2) # (2)
```

(1)
이것은 호출부가 잘못되었죠? positional arguments들은 keyword arguments보다 먼저 선언되어야 합니다. `positional argument follows keyword argument` 에러가 발생합니다.

(2)
id에는 4가 할당되고, [5,6]은 info로 패킹됩니다. {'a' :1, 'b' :2}로 extra에 패킹되어 할당될 것입니다.

너무 방법이 다양하고, 오히려 혼란을 일으킬 것 같죠? 맞습니다 ...

# keyword arguments로 강제하는 방법

fastapi 코드를 보다가 이런 표현을 발견하였습니다. [참고](https://github.com/Netflix/dispatch/blob/master/src/dispatch/service/service.py#L14-L16)
```python
def get(*, db_session, service_id: int) -> Optional[Service]:
    """Gets a service by id."""
    return db_session.query(Service).filter(Service.id == service_id).first()

...
service.get(db_session=db_session, search_filter_id=f.id)
```
위의 내용을 종합해보면, get 함수를 호출하기 위해 db_session, service_id가 `keyword` 형식으로 입력되어야 한다는 것을 알 수 있습니다! 

자유로운 python의 함수 호출 방법을 `keyword argument`로 통일하기 위한 좋은 방법이라고 생각합니다.
이렇게 선언한다면 호출시에는 optional한지 아닌지만 판단하면 됩니다. 추가로 `typing`을 이용한 parameter type hint를 제공한다면 편할 것 같습니다.


# 정리



>- parameter는 선언시, argument는 호출시 부르는 이름이다.
>
> - default(optional) parameter는 항상 필수값들 이후에 선언되어야 한다.
> 
> - positional argument 이후에 
>  
> - `*`과 `**`는 변수의 선언시와 호출시에 따라 packing/ unpacking의 역할을 수행한다.
> 
>- `*`는 tuple이나 list, `**`는 dictionary를/로 변환해준다.
>
>- `*`로 선언한 변수 이후에는 항상 keyword argument로만 호출이 가능하다. 이 특성을 이용하여 keyword argument로 호출을 강제할 수 있다.
{: .prompt-tip }


화이팅!
