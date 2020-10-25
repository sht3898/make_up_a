# 알고리즘 문제

* [프로그래머스 문제 정리](./Programmers)
* [백준 문제 정리](./BOJ)
* [나동빈 강의](./nadongbin)
* [Java 문제 풀이](./Java)
* [SSAFY에서 풀었던 문제 정리](./SSAFY)
* [기타 문제 정리](./ETC)



# 주요 개념 정리

## lambda

> lambda 인자: 표현식

* 예시

* 두 수를 더하는 함수

  ```python
  def hap(x, y):
      return x + y
  ```

* 람다로 표현하면

  ```python
  (lambda x, y: x+y)(10, 20)
  ```

* 활용 예시

  ```python
  result = list(map(lambda x:x[1]+10, mylist))
  ```

  ```python
  plus_ten = lambda x:x+10
  plus_ten(1)	# 11
  ```
  
  

## map

> map(함수, 리스트)

* 예시

  ```python
  list(map(lambda x: x**2, range(5)))
  [0, 1, 4, 9, 16]
  ```




## enumerate

> 인덱스와 원소 값을 동시에 반환

* 예시

  ```python
  for idx, num in enumerate(numbers):
      print(idx, num)
  ```




## heapq

> 이진  트리 기반의 최소 힙 자료구조
>
> 데이터를 정렬된 상태로 저장하기 위해서 사용
>
> PriorityQueue 와 유사
>
> [참고](https://www.daleseo.com/python-heapq/)

min heap 내의 모든 원소(k)는 항상 자식 원소들(2k+1, 2k+2) 보다 크기가 작거나 같도록 원소가 추가되고 삭제



* 모듈 임포트

  ```python
  import heapq
  ```

* 최소 힙 생성

  ```python
  heap = []
  ```

* 힙에 원소 추가

  ```python
  heapq.heappush(heap, 4)
  heapq.heappush(heap, 1)
  heapq.heappush(heap, 7)
  heapq.heappush(heap, 3)
  
  # [1, 3, 4, 7]
  ```

  가장 작은 원소가 인덱스 0에 위치

* 원소 삭제

  ```python
  heapq.heappop(heap)
  ```

* 최소값 삭제하지 않고 얻기

  ```python
  heap[0]
  ```

  인덱스 0에 가장 작은 원소가 있다고 해서, 인덱스 1에 두번째 작은 소, 인덱스 3에 세번째 작은 원소가 있다는 보장이 없음. 힙은 함수를 호출하여 원소를 삭제할 때마다 이진 트리의 재배치를 통해 매번 새로운 최소값을 인덱스 0에 위치시키기 때문

  따라서 두번째로 작은 원소를 얻으려면 반드시 heappop()을 통해 가장 작은 원소를 삭제 후에 heap[0]을 통해 새로운 최소값에 접근해야 함

* 기존 리스트를 힙으로 변환

  ```python
  heap = [4, 1, 7, 3, 8, 5]
  heapq.heapify(heap)
  print(heap)
  
  [1, 3, 5, 4, 8, 7]
  ```



## 정렬

```python
phone_book.sort(key=lambda x:len(x))
phone_book.sort(key=len)
sorted_phone_book = sorted(phone_book, key=lambda x:len(x))
sorted_phone_book = sorted(phone_book, key=len)
```

이런 식으로 sort나 sorted 안에 key 함수를 사용한다면 자신이 원하는 조건에 맞게 정렬할 수 있음



## nonlocal vs global

다음과 같이 안쪽 함수 B에서 바깥쪽 함수 A의 지역 변수 x를 변경해봅니다.

```python
def A():
    x = 10        # A의 지역 변수 x
    def B():
        x = 20    # x에 20 할당
 
    B()
    print(x)      # A의 지역 변수 x 출력
 
A()
```

실행 결과

```python
10
```

실행을 해보면 20이 나와야 할 것 같은데 10이 나왔습니다. 왜냐하면 겉으로 보기에는 바깥쪽 함수 A의 지역 변수 x를 변경하는 것 같지만, 실제로는 안쪽 함수 B에서 이름이 같은 지역 변수 x를 새로 만들게 됩니다. 즉, 파이썬에서는 함수에서 변수를 만들면 항상 현재 함수의 지역 변수가 됩니다.

```python
def A():
    x = 10        # A의 지역 변수 x
    def B():
        x = 20    # B의 지역 변수 x를 새로 만듦
```

현재 함수의 바깥쪽에 있는 지역 변수의 값을 변경하려면 nonlocal 키워드를 사용해야 합니다. 다음과 같이 함수 안에서 nonlocal에 지역 변수의 이름을 지정해줍니다.

- **nonlocal** **지역변수**

```python
def A():
    x = 10        # A의 지역 변수 x
    def B():
        nonlocal x    # 현재 함수의 바깥쪽에 있는 지역 변수 사용
        x = 20        # A의 지역 변수 x에 20 할당
 
    B()
    print(x)      # A의 지역 변수 x 출력
 
A()
```

실행 결과

```python
20
```

이제 함수 B에서 함수 A의 지역 변수 x를 변경할 수 있습니다. 즉, nonlocal은 현재 함수의 지역 변수가 아니라는 뜻이며 바깥쪽 함수의 지역 변수를 사용합니다.

### nonlocal이 변수를 찾는 순서

nonlocal은 현재 함수의 바깥쪽에 있는 지역 변수를 찾을 때 가장 가까운 함수부터 먼저 찾음

```
def A():
    x = 10
    y = 100
    def B():
        x = 20
        def C():
            nonlocal x
            nonlocal y
            x = x + 30
            y = y + 300
            print(x)
            print(y)
        C()
    B()
 
A()
```

실행 결과

```
50
400
```

### global

함수가 몇 단계든 상관없이 global 키워드를 사용하면 무조건 전역 변수를 사용




## [문자열 포매팅](https://wikidocs.net/13#format)

문자열에서 또 하나 알아야 할 것으로는 문자열 포매팅(Formatting)이 있다. 이것을 공부하기 전에 다음과 같은 문자열을 출력하는 프로그램을 작성했다고 가정해 보자.

**"현재 온도는 18도입니다."**

시간이 지나서 20도가 되면 다음 문장을 출력한다.

**"현재 온도는 20도입니다"**

위 두 문자열은 모두 같은데 20이라는 숫자와 18이라는 숫자만 다르다. 이렇게 문자열 안의 특정한 값을 바꿔야 할 경우가 있을 때 이것을 가능하게 해주는 것이 바로 문자열 포매팅 기법이다.

쉽게 말해 문자열 포매팅이란 문자열 안에 어떤 값을 삽입하는 방법이다. 다음 예를 직접 실행해 보면서 그 사용법을 알아보자.

### 문자열 포매팅 따라 하기

**1. 숫자 바로 대입**

```
>>> "I eat %d apples." % 3
'I eat 3 apples.'
```

위 예제의 결괏값을 보면 알겠지만 위 예제는 문자열 안에 정수 3을 삽입하는 방법을 보여 준다. 문자열 안에서 숫자를 넣고 싶은 자리에 %d 문자를 넣어 주고, 삽입할 숫자 3은 가장 뒤에 있는 % 문자 다음에 써 넣었다. 여기에서 %d는 문자열 포맷 코드라고 부른다.

**2. 문자열 바로 대입**

문자열 안에 꼭 숫자만 넣으라는 법은 없다. 이번에는 숫자 대신 문자열을 넣어 보자.

```
>>> "I eat %s apples." % "five"
'I eat five apples.'
```

위 예제에서는 문자열 안에 또 다른 문자열을 삽입하기 위해 앞에서 사용한 문자열 포맷 코드 %d가 아닌 %s를 썼다. 어쩌면 눈치 빠른 독자는 벌써 유추하였을 것이다. 숫자를 넣기 위해서는 %d를 써야 하고, 문자열을 넣기 위해서는 %s를 써야 한다는 사실을 말이다.

> ※ 문자열을 대입할 때는 앞에서 배운 것처럼 큰따옴표나 작은따옴표를 반드시 써주어야 한다.

**3. 숫자 값을 나타내는 변수로 대입**

```
>>> number = 3
>>> "I eat %d apples." % number
'I eat 3 apples.'
```

1번처럼 숫자를 바로 대입하나 위 예제처럼 숫자 값을 나타내는 변수를 대입하나 결과는 같다.

**4. 2개 이상의 값 넣기**

그렇다면 문자열 안에 1개가 아닌 여러 개의 값을 넣고 싶을 땐 어떻게 해야 할까?

```
>>> number = 10
>>> day = "three"
>>> "I ate %d apples. so I was sick for %s days." % (number, day)
'I ate 10 apples. so I was sick for three days.'
```

위 예문처럼 2개 이상의 값을 넣으려면 마지막 % 다음 괄호 안에 콤마(,)로 구분하여 각각의 값을 넣어 주면 된다.

### 문자열 포맷 코드

문자열 포매팅 예제에서는 대입해 넣는 자료형으로 정수와 문자열을 사용했으나 이 외에도 다양한 것을 대입할 수 있다. 문자열 포맷 코드로는 다음과 같은 것이 있다.

| 코드 | 설명                      |
| :--- | :------------------------ |
| %s   | 문자열(String)            |
| %c   | 문자 1개(character)       |
| %d   | 정수(Integer)             |
| %f   | 부동소수(floating-point)  |
| %o   | 8진수                     |
| %x   | 16진수                    |
| %%   | Literal % (문자 `%` 자체) |

여기에서 재미있는 것은 %s 포맷 코드인데, 이 코드는 어떤 형태의 값이든 변환해 넣을 수 있다. 무슨 말인지 예를 통해 확인해 보자.

```
>>> "I have %s apples" % 3
'I have 3 apples'
>>> "rate is %s" % 3.234
'rate is 3.234'
```

3을 문자열 안에 삽입하려면 %d를 사용하고, 3.234를 삽입하려면 %f를 사용해야 한다. 하지만 %s를 사용하면 이런 것을 생각하지 않아도 된다. 왜냐하면 %s는 자동으로 % 뒤에 있는 값을 문자열로 바꾸기 때문이다.





**[포매팅 연산자 %d와 %를 같이 쓸 때는 %%를 쓴다]**

```
>>> "Error is %d%." % 98
```

위 예문의 결괏값으로 당연히 "Error is 98%."가 출력될 것이라고 예상하겠지만 파이썬은 값이 올바르지 않다는 값 오류(Value Error) 메시지를 보여 준다.

```
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
ValueError: incomplete format
```

이유는 문자열 포맷 코드인 %d와 %가 같은 문자열 안에 존재하는 경우, %를 나타내려면 반드시 %%로 써야 하는 법칙이 있기 때문이다. 이 점은 꼭 기억해 두어야 한다. 하지만 문자열 안에 %d 같은 포매팅 연산자가 없으면 %는 홀로 쓰여도 상관이 없다.

따라서 위 예를 제대로 실행하려면 다음과 같이 해야 한다.

```
>>> "Error is %d%%." % 98
'Error is 98%.'
```





### 포맷 코드와 숫자 함께 사용하기

위에서 보았듯이 %d, %s 등의 포맷 코드는 문자열 안에 어떤 값을 삽입하기 위해 사용한다. 하지만 포맷 코드를 숫자와 함께 사용하면 더 유용하게 사용할 수 있다. 다음 예를 보고 따라해 보자.

**1. 정렬과 공백**

```
>>> "%10s" % "hi"
'        hi'
```

앞의 예문에서 `%10s`는 전체 길이가 10개인 문자열 공간에서 대입되는 값을 오른쪽으로 정렬하고 그 앞의 나머지는 공백으로 남겨 두라는 의미이다.

그렇다면 반대쪽인 왼쪽 정렬은 `%-10s`가 될 것이다. 확인해 보자.

```
>>> "%-10sjane." % 'hi'
'hi        jane.'
```

hi를 왼쪽으로 정렬하고 나머지는 공백으로 채웠음을 볼 수 있다.

**2. 소수점 표현하기**

```
>>> "%0.4f" % 3.42134234
'3.4213'
```

3.42134234를 소수점 네 번째 자리까지만 나타내고 싶은 경우에는 위와 같이 사용한다. 즉 여기서 '.'의 의미는 소수점 포인트를 말하고 그 뒤의 숫자 4는 소수점 뒤에 나올 숫자의 개수를 말한다. 다음 예를 살펴보자.

```
>>> "%10.4f" % 3.42134234
'    3.4213'
```

위 예는 숫자 3.42134234를 소수점 네 번째 자리까지만 표시하고 전체 길이가 10개인 문자열 공간에서 오른쪽으로 정렬하는 예를 보여 준다.

### format 함수를 사용한 포매팅

문자열의 format 함수를 사용하면 좀 더 발전된 스타일로 문자열 포맷을 지정할 수 있다. 앞에서 살펴본 문자열 포매팅 예제를 format 함수를 사용해서 바꾸면 다음과 같다.

**숫자 바로 대입하기**

```
>>> "I eat {0} apples".format(3)
'I eat 3 apples'
```

"I eat {0} apples" 문자열 중 {0} 부분이 숫자 3으로 바뀌었다.

**문자열 바로 대입하기**

```
>>> "I eat {0} apples".format("five")
'I eat five apples'
```

문자열의 {0} 항목이 five라는 문자열로 바뀌었다.

**숫자 값을 가진 변수로 대입하기**

```
>>> number = 3
>>> "I eat {0} apples".format(number)
'I eat 3 apples'
```

문자열의 {0} 항목이 number 변수 값인 3으로 바뀌었다.

**2개 이상의 값 넣기**

```
>>> number = 10
>>> day = "three"
>>> "I ate {0} apples. so I was sick for {1} days.".format(number, day)
'I ate 10 apples. so I was sick for three days.'
```

2개 이상의 값을 넣을 경우 문자열의 {0}, {1}과 같은 인덱스 항목이 format 함수의 입력값으로 순서에 맞게 바뀐다. 즉 위 예에서 {0}은 format 함수의 첫 번째 입력값인 number로 바뀌고 {1}은 format 함수의 두 번째 입력값인 day로 바뀐다.

**이름으로 넣기**

```
>>> "I ate {number} apples. so I was sick for {day} days.".format(number=10, day=3)
'I ate 10 apples. so I was sick for 3 days.'
```

위 예에서 볼 수 있듯이 {0}, {1}과 같은 인덱스 항목 대신 더 편리한 {name} 형태를 사용하는 방법도 있다. {name} 형태를 사용할 경우 format 함수에는 반드시 name=value 와 같은 형태의 입력값이 있어야만 한다. 위 예는 문자열의 {number}, {day}가 format 함수의 입력값인 number=10, day=3 값으로 각각 바뀌는 것을 보여 주고 있다.

**인덱스와 이름을 혼용해서 넣기**

```
>>> "I ate {0} apples. so I was sick for {day} days.".format(10, day=3)
'I ate 10 apples. so I was sick for 3 days.'
```

위와 같이 인덱스 항목과 name=value 형태를 혼용하는 것도 가능하다.

**왼쪽 정렬**

```
>>> "{0:<10}".format("hi")
'hi        '
```

`:<10` 표현식을 사용하면 치환되는 문자열을 왼쪽으로 정렬하고 문자열의 총 자릿수를 10으로 맞출 수 있다.

**오른쪽 정렬**

```
>>> "{0:>10}".format("hi")
'        hi'
```

오른쪽 정렬은 `:<` 대신 `:>`을 사용하면 된다. 화살표 방향을 생각하면 어느 쪽으로 정렬되는지 바로 알 수 있을 것이다.

**가운데 정렬**

```
>>> "{0:^10}".format("hi")
'    hi    '
```

`:^` 기호를 사용하면 가운데 정렬도 가능하다.

**공백 채우기**

```
>>> "{0:=^10}".format("hi")
'====hi===='
>>> "{0:!<10}".format("hi")
'hi!!!!!!!!'
```

정렬할 때 공백 문자 대신에 지정한 문자 값으로 채워 넣는 것도 가능하다. 채워 넣을 문자 값은 정렬 문자 `<, >, ^` 바로 앞에 넣어야 한다. 위 예에서 첫 번째 예제는 가운데(`^`)로 정렬하고 빈 공간을 `=` 문자로 채웠고, 두 번째 예제는 왼쪽(`<`)으로 정렬하고 빈 공간을 `!` 문자로 채웠다.

**소수점 표현하기**

```
>>> y = 3.42134234
>>> "{0:0.4f}".format(y)
'3.4213'
```

위 예는 format 함수를 사용해 소수점을 4자리까지만 표현하는 방법을 보여 준다. 앞에서 살펴보았던 표현식 0.4f를 그대로 사용한 것을 알 수 있다.

```
>>> "{0:10.4f}".format(y)
'    3.4213'
```

위와 같이 자릿수를 10으로 맞출 수도 있다. 역시 앞에서 살펴본 10.4f의 표현식을 그대로 사용한 것을 알 수 있다.

**`{` 또는 `}` 문자 표현하기**

```
>>> "{{ and }}".format()
'{ and }'
```

format 함수를 사용해 문자열 포매팅을 할 경우 `{ }`와 같은 중괄호(brace) 문자를 포매팅 문자가 아닌 문자 그대로 사용하고 싶은 경우에는 위 예의 `{{ }}`처럼 2개를 연속해서 사용하면 된다.

### f 문자열 포매팅

파이썬 3.6 버전부터는 f 문자열 포매팅 기능을 사용할 수 있다. 파이썬 3.6 미만 버전에서는 사용할 수 없는 기능이므로 주의해야 한다.

다음과 같이 문자열 앞에 f 접두사를 붙이면 f 문자열 포매팅 기능을 사용할 수 있다.

```
>>> name = '홍길동'
>>> age = 30
>>> f'나의 이름은 {name}입니다. 나이는 {age}입니다.'
'나의 이름은 홍길동입니다. 나이는 30입니다.'
```

f 문자열 포매팅은 위와 같이 name, age와 같은 변수 값을 생성한 후에 그 값을 참조할 수 있다. 또한 f 문자열 포매팅은 표현식을 지원하기 때문에 다음과 같은 것도 가능하다.

> ※ 표현식이란 문자열 안에서 변수와 +, -와 같은 수식을 함께 사용하는 것을 말한다.

```
>>> age = 30
>>> f'나는 내년이면 {age+1}살이 된다.'
'나는 내년이면 31살이 된다.'
```

딕셔너리는 f 문자열 포매팅에서 다음과 같이 사용할 수 있다.

> ※ 딕셔너리는 Key와 Value라는 것을 한 쌍으로 갖는 자료형이다. 02-5에서 자세히 알아본다.

```
>>> d = {'name':'홍길동', 'age':30}
>>> f'나의 이름은 {d["name"]}입니다. 나이는 {d["age"]}입니다.'
'나의 이름은 홍길동입니다. 나이는 30입니다.'
```

정렬은 다음과 같이 할 수 있다.

```
>>> f'{"hi":<10}'  # 왼쪽 정렬
'hi        '
>>> f'{"hi":>10}'  # 오른쪽 정렬
'        hi'
>>> f'{"hi":^10}'  # 가운데 정렬
'    hi    '
```

공백 채우기는 다음과 같이 할 수 있다.

```
>>> f'{"hi":=^10}'  # 가운데 정렬하고 '=' 문자로 공백 채우기
'====hi===='
>>> f'{"hi":!<10}'  # 왼쪽 정렬하고 '!' 문자로 공백 채우기
'hi!!!!!!!!'
```

소수점은 다음과 같이 표현할 수 있다.

```
>>> y = 3.42134234
>>> f'{y:0.4f}'  # 소수점 4자리까지만 표현
'3.4213'
>>> f'{y:10.4f}'  # 소수점 4자리까지 표현하고 총 자리수를 10으로 맞춤
'    3.4213'
```

f 문자열에서 `{ }` 문자를 표시하려면 다음과 같이 두 개를 동시에 사용해야 한다.

```
>>> f'{{ and }}'
'{ and }'
```

지금까지는 문자열을 가지고 할 수 있는 기본적인 것에 대해 알아보았다. 이제부터는 문자열을 좀 더 자유자재로 다루기 위해 공부해야 할 것을 설명할 것이다. 지쳤다면 잠시 책을 접고 휴식을 취하자.





