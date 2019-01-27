chapter 2: Considering Algorithm Design
======================================================
파이썬 기본 빌트인 데이터 타입중에  list 와 tuple에 대해서 알아보자.


2.1 Sequence Overview
------------------------
list와 tuple의 차이점은 변경이 가능하냐이다.
list는 변경이 가능하고 tuple은 변경할 수 없다.

리스트는 다음과 같이 표현할 수 있다.

.. code-block:: python


    >>> edward = ['Edward Gumby', 42]
    >>> john = ['John Smith', 50]


2.2 Common Sequence Operators
-------------------------------
indexing, slicing, adding, multiplying, and checking for membership 등의 빌드인 연산자를 알아보자.

Indexing
~~~~~~~~~~
시퀀스 안에 있는 모든 요소는 숫자로 구성되어 있다.

.. code-block:: python

    >>> greeting = 'Hello'
    >>> greeting[0]
    'H'

마이너스 숫자는 마지막부터 시작해서 반대로 시작된다.

.. code-block:: python

    >>> greeting[-1]
    'o'

다음처럼 입력한 값을 직접 호출할 수 있다.


.. code-block:: python

    >>> fourth = input('Year: ')[3]
    Year: 2005
    >>> fourth
    '5'

Listing 2-1 Indexing Example

.. code-block:: python

    # Print out a date, given year, month, and day as numbers
    months = [
    'January',
    'February',
    'March',
    'April',
    'May',
    'June',
    'July',
    'August',
    'September',
    'October',
    'November',
    'December'
    ]
    # A list with one ending for each number from 1 to 31
    endings = ['st', 'nd', 'rd'] + 17 * ['th'] \
    + ['st', 'nd', 'rd'] + 7 * ['th'] \
    + ['st']
    year = input('Year: ')
    month = input('Month (1-12): ')
    day = input('Day (1-31): ')
    month_number = int(month)
    day_number = int(day)
    # Remember to subtract 1 from month and day to get a correct index
    month_name = months[month_number-1]
    ordinal = day + endings[day_number-1]
    print(month_name + ' ' + ordinal + ', ' + year)


Slicing
~~~~~~~~~~
다음처럼 두개로 나눌 수 있다.

.. code-block:: python

    >>> tag = '<a href="http://www.python.org">Python web site</a>'
    >>> tag[9:30]
    'http://www.python.org'
    >>> tag[32:-4]
    'Python web site'

슬라이싱은 본것과 같이 시퀀스에서 특정부분을 분리하는데 자주 쓰인다.
여기서 앞숫자와 뒷숫자가 중요하다.

.. code-block:: python

    >>> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    >>> numbers[3:6] [4, 5, 6]
    >>> numbers[0:1] [1]

처음숫자는 포함하고자하는 첫번째 엘리먼트 숫자이고 두번째는 자른후 첫번째 숫자이다.
첫번째 숫자는 0부터 시작한다.
첫번째는 inclusive 두번째는 exclusive 로 한계가 주어진다.


A Nifty Shortcut
~~~~~~~~~~~~~~~~~
다음에서 마지막 3개 숫자를 선택해 보자.


.. code-block:: python

    >>> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    >>> numbers[7:10]
    [8, 9, 10]
여기서 10번째 숫자는 11번째에 있게 되지만 존재하지 않는 숫자이다.
여기서 뒤에서부터 셈을 할때는 다음처럼 하면 된다.

.. code-block:: python

    >>> numbers[-3:-1]
    [8, 9]
여기서 마지막은 -1부터 시작한다.
따라서 다음처럼 할수 없다.

.. code-block:: python

    >>> numbers[-3:0]
    []

다음처럼 생략할수도 있다.

.. code-block:: python

    >>> numbers[-3:]
    [8, 9, 10]

    >>> numbers[:3]
    [1, 2, 3]

시퀀스 전부를 복사하고자 하면 다음처럼 하면 된다.

.. code-block:: python

    >>> numbers[:]
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]


Longer Steps
~~~~~~~~~~~~~~
슬라이싱할때 명시적 또는 암시적으로 시작점과 끝나는점을 표기할 수 있다.
또다른 파라미터로서 스텝길이(step lenght)가 있다. 디폴트로 1이다.
1이라는 것은 시작점에서 끝나는점까지 1개의 엘리먼트를 움직인다는 것이다.

.. code-block:: python

    >>> numbers[0:10:1]
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

step size가 1이상이면  엘리먼트를 뛰어넘을 것이다.



.. code-block:: python

    >>> numbers[0:10:2]
    [1, 3, 5, 7, 9]
    numbers[3:6:3]
    [4]


다음처럼 step size만 표시해도 된다.

.. code-block:: python

    >>> numbers[::4]
    [1, 5, 9]

스텝 사이즈는 0이 될수 없다.
그러나 마이너스는 가능하다.그 의미는 오른쪽에서 왼쪽으로 엘리먼트를 뺀다는 것을 의미한다.


.. code-block:: python

    >>> numbers[8:3:-1]
    [9, 8, 7, 6, 5]
    >>> numbers[10:0:-2]
    [10, 8, 6, 4, 2]
    >>> numbers[0:10:-2]
    []
    >>> numbers[::-2]
    [10, 8, 6, 4, 2]
    >>> numbers[5::-2]
    [6, 4, 2]
    >>> numbers[:5:-2]
    [10, 8]

하나 고려해봐야 할것은 만약, step size가 마이너스라면 두번째 인덱스보다 큰 첫번째 인덱스를 가져야 한다.
하지만 파이썬은 양의 스텝사이즈는 왼쪽에서 오른쪽 음의 스텝사이즈는 오른쪽에서 왼쪽으로 계산한다.



Adding Sequences
~~~~~~~~~~~~~~~~~~
시퀀스는 플러스로 합칠 수 있다.

.. code-block:: python

    >>> [1, 2, 3] + [4, 5, 6]
    [1, 2, 3, 4, 5, 6]
    >>> 'Hello,' + 'world!'
    'Hello, world!'
    >>> [1, 2, 3] + 'world!'
    Traceback (innermost last):
    File "<pyshell>", line 1, in ?
    [1, 2, 3] + 'world!'
    TypeError: can only concatenate list (not "string") to list

상기 에러에서 알수 있듯이 list와 string은 합칠 수가 없다. 일반적으로 다른 타입은 합칠 수가 없다.

Multiplication
~~~~~~~~~~~~~~~~
숫자는 앞의 시퀀스를 반복한다는 의미이다.

.. code-block:: python


    >>> 'python' * 5
    'pythonpythonpythonpythonpython'
    >>> [42] * 10
    [42, 42, 42, 42, 42, 42, 42, 42, 42, 42]


None, Empty Lists, and Initialization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
빈 리스트는 []로 표기되며 초기화를 [0]*10  [42]*10 이런식으로 초기화를 할 수 있고
[None]*10 은 아무것도 없다는 의미이다.

.. code-block:: python

    >>> sequence = [None] * 10
    >>> sequence
    [None, None, None, None, None, None, None, None, None, None]


다음 프로그램을 실행해 보자.
입력한 글자를 가운데로 하고 네모난 박스를 그리는 프로그램이다.

.. code-block:: python

    # Prints a sentence in a centered "box" of correct width
    sentence = input("Sentence: ")
    screen_width = 80
    text_width = len(sentence)
    box_width = text_width + 6
    left_margin = (screen_width - box_width) // 2
    print()
    print(' ' * left_margin + '+' + '-' * (box_width-2) + '+')
    print(' ' * left_margin + '| ' + ' ' * text_width + ' |')
    print(' ' * left_margin + '| ' + sentence + ' |')
    print(' ' * left_margin + '| ' + ' ' * text_width + ' |')
    print(' ' * left_margin + '+' + '-' * (box_width-2) + '+')
    print()

Membership
~~~~~~~~~~~~~~
시퀀스에서 어떤값이 있는지 체크할때가 있다. 이럴때 우리는 연산자를 사용한다. 이러한 연산자는 곱하기,더하기와는 다른 비트연산자를 쓴다.
따라서 비트값에 따라 참값,거짓값을 반환하게 된다. 불린 연산자, 블린값이라고 한다.
불린에 대해서는 5장에서 자세히 다루도록 하겠다.

.. code-block:: python

    >>> permissions = 'rw'
    >>> 'w' in permissions
    True
    >>> 'x' in permissions
    False
    >>> users = ['mlh', 'foo', 'bar']
    >>> input('Enter your user name: ') in users
    Enter your user name: mlh
    True
    >>> subject = '$$$ Get rich now!!! $$$'
    >>> '$$$' in subject
    True

다음 예제는 데이터베이스에 PIN 번호를 체크하는 것이다.

.. code-block:: python


# Check a user name and PIN code

    database = [
        ['albert',  '1234'],
        ['dilbert', '4242'],
        ['smith',   '7524'],
        ['jones',   '9843']
    ]

    username = input('User name: ')
    pin = input('PIN code: ')

    if [username, pin] in database: print('Access granted')

Length, Minimum, and Maximum
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
빌트인 함수인 len,min,max 는 아주 유용하다.len 함수는 시퀀스가 포함한 엘리먼트의 숫자를 리턴한다.
min,max는 각각 시퀀스의 가장 작은수,큰 엘리먼트를 리턴한다.

.. code-block:: python

    >>> numbers = [100, 34, 678]
    >>> len(numbers)
    3
    >>> max(numbers)
    678
    >>> min(numbers)
    34
    >>> max(2, 3)
    3
    >>> min(9, 3, 2, 5)
    2




2.3 Lists: Python's Workhorse
-------------------------------
이장에서는 list에 대해서 좀더 다루겠다.
list는 변경가능하다. 따라서 여러가지 예를 들어 보고 사용을 알아보자.

The list Function
~~~~~~~~~~~~~~~~~~~
list 함수를 이용해서 다음처럼 입력도 가능하다.

.. code-block:: python


    >>> list('Hello')
    ['H', 'e', 'l', 'l', 'o']

Basic List Operations
~~~~~~~~~~~~~~~~~~~~~~
list에도 indexing, slicing, concatenating,multiplying을 할 수 있다.
재미있는 점은 list는 변경이 가능하다는 것이다.
이 장에서는 list를 변경하는 예를 아이템 할당,삭제,슬라이스 할당등을 보도록 하겠다.

Changing Lists: Item Assignments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
다음 예를 보자.

.. code-block:: python


    x[1] = 2.
    >>> x = [1, 1, 1]
    >>> x[1] = 2
    >>> x
    [1, 2, 1]

Deleting Elements
~~~~~~~~~~~~~~~~~~

.. code-block:: python

    >>> names = ['Alice', 'Beth', 'Cecil', 'Dee-Dee', 'Earl']
    >>> del names[2]
    >>> names
    ['Alice', 'Beth', 'Dee-Dee', 'Earl']


del 구문은 4장에서 좀더 상세하게 다루도록 하겠다.

Assigning to Slices
~~~~~~~~~~~~~~~~~~~~

.. code-block:: python


    >>> name = list('Perl')
    >>> name
    ['P', 'e', 'r', 'l']
    >>> name[2:] = list('ar')
    >>> name
    ['P', 'e', 'a', 'r']


    >>> name = list('Perl')
    >>> name[1:] = list('ython')
    >>> name
    ['P', 'y', 't', 'h', 'o', 'n']

슬라이스 할당으로 다음처럼 처음 리스트를 교체하는것 없이 집어 넣을 수 있다.

.. code-block:: python

    >>> numbers = [1, 5]
    >>> numbers[1:1] = [2, 3, 4]
    >>> numbers
    [1, 2, 3, 4, 5]

다음처럼 빈 .list를 할당해서 지울수 도 있다.

.. code-block:: python

    >>> numbers
    [1, 2, 3, 4, 5]
    >>> numbers[1:4] = []
    >>> numbers
    [1, 5]

이 예는 del numbers[1:4] 구문도 똑같다.

List Methods
~~~~~~~~~~~~~~~
메쏘드는 함수이며 어떤 오브젝트에 밀접하게 연결되어 있다. (list,number,string)
다음처럼 쓰인다.

object.method(arguments)

메쏘드 호출은 함수 호출과 유사하다. 단지 메쏘드 앞에 오브젝트가 들어간다는 것 뿐이다.

append
~~~~~~~~~~~~~~
append 메쏘드는 list의 끝에  추가할때 사용된다.

.. code-block:: python

    >>> lst = [1, 2, 3]
    >>> lst.append(4)
    >>> lst
    [1, 2, 3, 4]

clear
~~~~~~~~~~~~~~
clear 메쏘드는 list를 비울때 사용된다.

.. code-block:: python

    >>> lst = [1, 2, 3]
    >>> lst.clear()
    >>> lst
    []

copy
~~~~~~~~~~~~~~
copy 메쏘드는 다음처럼 사용한다.

.. code-block:: python


    >>> a = [1, 2, 3]
    >>> b = a
    >>> b[1] = 4
    >>> a
    [1, 4, 3]

또는 함수를 이용해서 다음처럼 사용해도 된다.

    >>> a = [1, 2, 3]
    >>> b = a.copy()
    >>> b[1] = 4
    >>> a
    [1, 2, 3]

count
~~~~~~~~~~~~~~
count 메쏘드는 다음처럼 사용한다.

.. code-block:: python

    >>> ['to', 'be', 'or', 'not', 'to', 'be'].count('to')
    2
    >>> x = [[1, 2], 1, 1, [2, 1, [1, 2]]]
    >>> x.count(1)
    2
    >>> x.count([1, 2])
    1

extend
~~~~~~~~~~~~~~
extend 메쏘드는 다음처럼 사용한다.

.. code-block:: python

    >>> a = [1, 2, 3]
    >>> b = [4, 5, 6]
    >>> a.extend(b)

다음처럼도 가능하다. 하지만 다음의 경우는 새롭게 만드는것이 아니라 두개를 연결한것이다.

.. code-block:: python

    >>> a = [1, 2, 3]
    >>> b = [4, 5, 6]
    >>> a + b
    [1, 2, 3, 4, 5, 6]
    >>> a
    [1, 2, 3]

다음처럼 실행해도 동일한 값을 얻는다.

.. code-block:: python

    >>> a = a + b

 다음처럼 슬라이스로 표현을 해도 동일한 값을 얻는다.

.. code-block:: python

    >>> a = [1, 2, 3]
    >>> b = [4, 5, 6]
    >>> a[len(a):] = b
    >>> a
    [1, 2, 3, 4, 5, 6]

동일한 내용을 가지지만 가시성이 좀 안좋다.

index
~~~~~~~~~~~~
index는 리스트에서 해당값이 처음 발생되는 위치를 알 수 있다.

.. code-block:: python

    >>> knights = ['We', 'are', 'the', 'knights', 'who', 'say', 'ni']
    >>> knights.index('who')
    4
    >>> knights.index('herring')
    Traceback (innermost last):
    File "<pyshell>", line 1, in ?
    knights.index('herring')
    ValueError: list.index(x): x not in list

반대로 다음처럼 index값을 넣으면 해당 값이 표시가 된다.

.. code-block:: python

    >>> knights[4]
    'who'

insert
~~~~~~~~~~~~
insert 함수는 리스트에 오브젝트를 넣을때 사용된다.

.. code-block:: python

    >>> numbers = [1, 2, 3, 5, 6, 7]
    >>> numbers.insert(3, 'four')
    >>> numbers
    [1, 2, 3, 'four', 5, 6, 7]

다음처럼 slice를 이용해도 동일한 값을 얻는다.

.. code-block:: python

    >>> numbers = [1, 2, 3, 5, 6, 7]
    >>> numbers[3:3] = ['four']
    >>> numbers
    [1, 2, 3, 'four', 5, 6, 7]

pop
~~~~~~~~~~~~
pop는 list에서 (디폴트 마지막 ) 엘리먼트를 삭제하고 결과값을 리턴한다.

.. code-block:: python

    >>> x = [1, 2, 3]
    >>> x.pop()
    3
    >>> x
    [1, 2]
    >>> x.pop(0)
    1
    >>> x
    [2]

remove
~~~~~~~~~~~~
remove는 리스트에서 첫번째 발생되는 값을 지울때 사용된다.

.. code-block:: python

    >>> x = ['to', 'be', 'or', 'not', 'to', 'be']
    >>> x.remove('be')
    >>> x
    ['to', 'or', 'not', 'to', 'be']
    >>> x.remove('bee')
    Traceback (innermost last):
    File "<pyshell>", line 1, in ?
    x.remove('bee')
    ValueError: list.remove(x): x not in list

remove는 리턴값이 없다.



reverse
~~~~~~~~~~~~
reverse는 list를 반대로 정렬할때 쓰인다.

.. code-block:: python

    >>> x = [1, 2, 3]
    >>> x.reverse()
    >>> x
    [3, 2, 1]


sort
~~~~~~~~~~~~
sort는 리스트 정렬을 위해 사용된다.

.. code-block:: python

    >>> x = [4, 6, 2, 1, 7, 9]
    >>> x.sort()
    >>> x
    [1, 2, 4, 6, 7, 9]

다음처럼 sort에 대한 리턴값은 없기때문에 다음처럼 하면 아무값이 없게 된다.

.. code-block:: python

    >>> x = [4, 6, 2, 1, 7, 9]
    >>> y = x.sort() # Don't do this!
    >>> print(y)
    None

    >>> x = [4, 6, 2, 1, 7, 9]
    >>> y = x.copy()
    >>> y.sort()
    >>> x
    [4, 6, 2, 1, 7, 9]
    >>> y
    [1, 2, 4, 6, 7, 9]

다음처럼 sorted 함수를 사용하여 다음처럼 동일하게 사용할 수 있다.

.. code-block:: python

    >>> x = [4, 6, 2, 1, 7, 9]
    >>> y = sorted(x)
    >>> x
    [4, 6, 2, 1, 7, 9]
    >>> y
    [1, 2, 4, 6, 7, 9]

Advanced Sorting
~~~~~~~~~~~~~~~~~~~
sort 함수는 2개의 옵션을 가지고 있다. key와 reverse 이다.

.. code-block:: python


    >>> x = ['aardvark', 'abalone', 'acme', 'add', 'aerate']
    >>> x.sort(key=len)
    >>> x
    ['add', 'acme', 'aerate', 'abalone', 'aardvark']

다음처럼 reverse 옵셥으로 True,False로 처리하면 된다.

.. code-block:: python

    >>> x = [4, 6, 2, 1, 7, 9]
    >>> x.sort(reverse=True)
    >>> x
    [9, 7, 6, 4, 2, 1]


2.4 Tuples:Immutable Sequence
-------------------------------
Tuples는 list와 동일하다. 다만 틀린점은 tuples는 변경이 불가능하다는 것이다.

.. code-block:: python

    >>> 1, 2, 3
    (1, 2, 3)

    >>> (1, 2, 3)
    (1, 2, 3)

    >>> ()
    ()

단일한 값을 갖는 tuple은 다음처럼 만들면 된다.다음처럼 콤마를 넣어주면 된다.

.. code-block:: python

    >>> 42
    42
    >>> 42,
    (42,)
    >>> (42,)
    (42,)

다음 2가지 예에서 처음은 tuple이 아니지만 두번째는 tuple이다.

.. code-block:: python

    >>> 3 * (40 + 2)
    126
    >>> 3 * (40 + 2,)
    (42, 42, 42)

다음처럼 list일경우는 tuple로 자동으로 변환된다.


.. code-block:: python

    >>> tuple([1, 2, 3])
    (1, 2, 3)
    >>> tuple('abc')
    ('a', 'b', 'c')
    >>> tuple((1, 2, 3))
    (1, 2, 3)

생성하는것이외에 쓰는것이 그리 복잡하지 않다.

.. code-block:: python

    >>> x = 1, 2, 3
    >>> x[1]
    2
    >>> x[0:2]
    (1, 2)






2.5 A Quick Summary
----------------------

이 장에서는 다음을 배웠다.

Sequences: A sequence is a data structure in which the elements are numbered
(starting with zero).


Membership: Whether a value can be found in a sequence (or other container)


Methods: Some of the built-in types (such as lists and strings but not tuples)

.. image:: ./img/chapter2-1.png
