chapter 4: Introducing Python for Algorithm Programming
===========================================================
리스트가 그룹값을 어떤 구조속에 넣고 숫자로 그 값을 참조할때 유용한것을 봤다.
이 장에서는 이름값으로 각 값을 참조할 수 있는 구조를 배우도록 하겠다.
이러한 타입 구조를 mapping 이라고 한다.
유일한 내장 매핑타입은 dictionary이다.
dictionary 값은 특별한 순서를 가지지 않는다. 그렇지만 숫자,스트링,튜플이 될 수도 있는 키에 의해서 저장된다.


4.1 Dictionary Uses
-------------------------
dictionary라는 이름이 이 구조의 목적에 대해서 알려줄 것이다. 보통 책은 처음부터 끝까지 읽게 되어 있다.
원한다면 특별한 페이지를 오픈해서 읽을수도 있다. 이것은 파이썬의 리스트와 같다.
반대로, dictionary는 특정한 키값을 빨리 찾을 수 있도록 되어 있다.
dictionary가 많은 경우에 유용한 경우가 많다. 다음은 파이썬 dictionary가 쓰이는  예이다.

• Representing the state of a game board, with each key being a tuple of coordinates
• Storing file modification times, with file names as keys
• A digital telephone/address book

다음 예를 보자.

.. code-block:: python

    >>> names = ['Alice', 'Beth', 'Cecil', 'Dee-Dee', 'Earl']

사람들의 전화번호를 저장하고 싶으면  다음처럼 처리했다.

.. code-block:: python

    >>> numbers = ['2341', '9102', '3158', '0142', '5551']

일단 리스트를 생성했으면 다음처럼 접근이 가능하다.

.. code-block:: python

    >>> numbers[names.index('Cecil')]

이렇게도 가능하지만 실제로 다음처럼 동작되길 원할것이다.

.. code-block:: python

    >>> phonebook['Cecil']



4.2 Creating and Using Dictionaries
--------------------------------------
dictionary는 다음처럼 표현된다.

.. code-block:: python

    phonebook = {'Alice': '2341', 'Beth': '9102', 'Cecil': '3258'}


dictionary는 key, value값으로 구성된다.예에서 이름은 key값이고 전화번호는 value값이 된다.
각 키는 :으로 value값으로 구분된다.
기본 dictionarys는 {}로 표현된다.

The dict Function
~~~~~~~~~~~~~~~~~~~~~~~~~~~
다른 mapping이나 key,value값을 갖는 것으로부터 dictionary를 생성하기 위해 dict 함수를 사용할 수 있다.

.. code-block:: python

    >>> items = [('name', 'Gumby'), ('age', 42)]
    >>> d = dict(items)

다음처럼 keyword 전달자로 생성할 수도 있다.

.. code-block:: python

    >>> d = dict(name='Gumby', age=42)
    >>> d
    {'age': 42, 'name': 'Gumby'}

Basic Dictionary Operations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
dictionary는 많은 면에서 sequence의 특성을 가지고 있다.

• len(d) returns the number of items (key-value pairs) in d.
• d[k] returns the value associated with the key k.
• d[k] = v associates the value v with the key k.
• del d[k] deletes the item with key k.
• k in d checks whether there is an item in d that has the key k.

dictionary에서 key membership 체크하는 것이 list에서 membership 체크하는것보다 효과적이다.
그 차이점은 데이터가 커질수록 더 크다.

첫번째로 dictionary에서 가장 강력한 것으로 key는 변경이 불가능하다.
두번째는 중요하다는 것이다.

.. code-block:: python

    >>> x = []
    >>> x[42] = 'Foobar'
    Traceback (most recent call last):
    File "<stdin>", line 1, in ?
    IndexError: list assignment index out of range
    >>> x = {}
    >>> x[42] = 'Foobar'
    >>> x
    {42: 'Foobar'}

첫번째 예제에서는 42번째 리스트에 값을 넣으려고 했지만 리스트가 없기때문에 에러를 발생한다.
만약에 가능하게 하려면 x=['test']*43 으로 초기화를 하고 x[43]을 하면 가능하다.

Listing 4-1. Dictionary Example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
다음 예를 보자.

.. code-block:: python

    people = {
    'Alice': {
    'phone': '2341',
    'addr': 'Foo drive 23'
    },
    'Beth': {
    'phone': '9102',
    'addr': 'Bar street 42'
    },
    'Cecil': {
    'phone': '3158',
    'addr': 'Baz avenue 90'
    }
    }

    labels = {
    'phone': 'phone number',
    'addr': 'address'
    }

    name = input('Name: ')

    request = input('Phone number (p) or address (a)? ')

    if request == 'p': key = 'phone'
    if request == 'a': key = 'addr'

String Formatting with Dictionaries
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
다음에서 phonebook이란 dictionary를 사용해 보자.여기서는 format_map이란 함수를 사용했다.


.. code-block:: python

    >>> phonebook={'Beth': '9102', 'Alice': '2341', 'Cecil': '3258'}
    >>> "Cecil's phone number is {Cecil}.".format_map(phonebook)
    "Cecil's phone number is 3258."

dictionary를 사용하는것처럼  모든 주어진 키값이 dictionary에서 발견되는한 변환 특별자를 가질 지 모르겠다.
이러한 것은 HTML을 사용하는 template 시스템에서 매우 유용하다.

.. code-block:: python


    >>> template = '''<html>
    ... <head><title>{title}</title></head>
    ... <body>
    ... <h1>{title}</h1>
    ... <p>{text}</p>
    ... </body>'''
    >>> data = {'title': 'My Home Page', 'text': 'Welcome to my home page!'}
    >>> print(template.format_map(data))
    <html>
    <head><title>My Home Page</title></head>
    <body>
    <h1>My Home Page</h1>
    <p>Welcome to my home page!</p>
    </body>

Dictionary Methods
~~~~~~~~~~~~~~~~~~~~~~~
다른 빌트인 타입처럼 dictionary도 많은 메쏘드를 가지고 있다.
여기서는 많이 쓰이는 메쏘드를 중심으로 서술하겠다.

clear
~~~~~~
이 메쏘드는 dictionary에서 모든 아이템을 지우는 역할을 한다.

.. code-block:: python

    >>> d = {}
    >>> d['name'] = 'Gumby'
    >>> d['age'] = 42
    >>> d
    {'age': 42, 'name': 'Gumby'}
    >>> returned_value = d.clear()
    >>> d
    {}
    >>> print(returned_value)
    None

다음 시나리오를 보자.

.. code-block:: python

    >>> x = {}
    >>> y = x
    >>> x['key'] = 'value'
    >>> y
    {'key': 'value'}
    >>> x = {}
    >>> x = {}
    {'key': 'value'}

    >>> x = {}
    >>> y = x
    >>> x['key'] = 'value'
    >>> y
    {'key': 'value'}
    >>> x.clear()
    >>> y
    {}

copy
~~~~~~
이 메쏘드는 key,value값을 갖는 새로운 dictionary를 만드는 역할을 한다.

.. code-block:: python


    >>> x = {'username': 'admin', 'machines': ['foo', 'bar', 'baz']}
    >>> y = x.copy()
    >>> y['username'] = 'mlh'
    >>> y['machines'].remove('bar')
    >>> y
    {'username': 'mlh', 'machines': ['foo', 'baz']}
    >>> x
    {'username': 'admin', 'machines': ['foo', 'baz']}

복사는 원래 dictionary를 변경하지 않기때문에 원래값을 변경하지 않고 복사해서 쓸때 자주 쓰인다.
만약 원래값도 변경이 가능한경우에는 deepcopy를 사용한다.

.. code-block:: python

    >>> from copy import deepcopy
    >>> d = {}
    >>> d['names'] = ['Alfred', 'Bertrand']
    >>> c = d.copy()
    >>> dc = deepcopy(d)
    >>> d['names'].append('Clive')
    >>> c
    {'names': ['Alfred', 'Bertrand', 'Clive']}
    >>> dc
    {'names': ['Alfred', 'Bertrand']}


fromkeys
~~~~~~~~~~
fromkeys 메쏘드는 주어진 key값으로 새로운 dictionary를 생성한다.디폴트 value값은 None이다.

.. code-block:: python

    >>> {}.fromkeys(['name', 'age'])
    {'age': None, 'name': None}

다음처럼 값을 넣을수 있다.

.. code-block:: python

    >>> dict.fromkeys(['name', 'age'])
    {'age': None, 'name': None}

    >>> dict.fromkeys(['name', 'age'], '(unknown)')
    {'age': '(unknown)', 'name': '(unknown)'}



get
~~~~~~
get 메쏘드는 dictionary item들을 접근할때 쓰인다. dictionary에 없는 item을 접근할때 오류를 발생한다.

.. code-block:: python

    >>> d = {}
    >>> print(d['name'])

    >>> print(d.get('name'))
    None

위 두번째 예처럼 get를 쓰면 디폴트 None이란 값을 발생한다.
만약 키가 있다면 get은 보통 dictionary 찾기기능을 갖는다.

.. code-block:: python

    >>> d['name'] = 'Eric'
    >>> d.get('name')
    'Eric'


Listing 4-2. Dictionary Method Example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    people={
        'alice':{'phone':12233,'addr':'seoul'},
        'Beth' :{'phone':233443,'addr':'busan'},
        'Ceceil':{'phone':3333,'addr':'chungju'}


        }

    labels = {
    'phone': 'phone number',
    'addr': 'address'
    }
    name = input('Name: ')
    # Are we looking for a phone number or an address?
    request = input('Phone number (p) or address (a)? ')
    # Use the correct key:
    key = request # In case the request is neither 'p' nor 'a'
    if request == 'p': key = 'phone'
    if request == 'a': key = 'addr'
    # Use get to provide default values:
    person = people.get(name, {})
    label = labels.get(key, key)
    result = person.get(key, 'not available')
    print("{}'s {} is {}.".format(name, label, result))




items
~~~~~~
item 메쏘드는 모든 아이템이 (key,value)형태의 item 리스트로서 dictionary의 모든 item을 반환한다.

.. code-block:: python

    >>> d = {'title': 'Python Web Site', 'url': 'http://www.python.org', 'spam': 0}
    >>> d.items()

리턴값은 dictionary view 형태로 표현된다. dictionary view는 반복에 많이 쓰인다.부가적으로 length 나 멤버쉽을 체크할때 사용한다.


.. code-block:: python


    >>> it = d.items()
    >>> len(it)
    3
    >>> ('spam', 0) in it
    True

view가 유용한 것은 그것은 어떠한 것도 복사하지 않는다는 것이다.그것은 항상 이전것의 dictionary를 반영한다.

.. code-block:: python

    >>> d['spam'] = 1
    >>> ('spam', 0) in it
    False
    >>> d['spam'] = 0
    >>> ('spam', 0) in it
    True

어찌됐건 모든 item들을 list에 복사하고자 하면 다음처럼 할수 있다.

.. code-block:: python

    >>> list(d.items())
    [('spam', 0), ('title', 'Python Web Site'), ('url', 'http://www.python.org')]



key
~~~~~~
key 메쏘드는 dictionary에 있는 key들의 dictionary view를 리턴한다.



pop
~~~~~~
pop은 주어진 key값에 해당하는 key-value값을 삭제할때 사용한다.

.. code-block:: python

    >>> d = {'x': 1, 'y': 2}
    >>> d.pop('x')
    1
    >>> d
    {'y': 2}


popitem
~~~~~~~~~
popitem 메쏘드는 list에서 마지막 인자를 표현하는 list.pop가 유사하다. list.pop 과 다른점은 dictionary는 마지막 item을 갖고 있지
않기때문에 popitem은 임의의 item을 pop한다.

.. code-block:: python

    >>> d = {'url': 'http://www.python.org', 'spam': 0, 'title': 'Python Web Site'}
    >>> d.popitem()
    ('url', 'http://www.python.org')
    >>> d
    {'spam': 0, 'title': 'Python Web Site'}

dictionary는 list 타입이 아니기때문에 append 메쏘드가 없다.



setdefault
~~~~~~~~~~~~
setdefault 메쏘드는 주어진 key값에  상응하는 값을 얻는 get가 유사하다. get과 틀린점은 dictionary에 없는 값을 주어진 키값에
따라 설정할 수 있다.


.. code-block:: python

    >>> d = {}
    >>> d.setdefault('name', 'N/A')
    'N/A'
    >>> d
    {'name': 'N/A'}
    >>> d['name'] = 'Gumby'
    >>> d.setdefault('name', 'N/A')
    'Gumby'
    >>> d
    {'name': 'Gumby'}


update
~~~~~~~~~~~
update 메쏘드는 다른 아이템들을 가진 dictionary로 갱신할때 쓴다.

.. code-block:: python

    >>> d = {
    ... 'title': 'Python Web Site',
    ... 'url': 'http://www.python.org',
    ... 'changed': 'Mar 14 22:09:15 MET 2016'
    ... }
    >>> x = {'title': 'Python Language Website'}
    >>> d.update(x)
    >>> d
    {'url': 'http://www.python.org', 'changed':
    'Mar 14 22:09:15 MET 2016', 'title': 'Python Language Website'}


values
~~~~~~~~
values값은 dictionary에서 value값을 dictionary view로 리턴하는 것이다.

.. code-block:: python


    >>> d = {}
    >>> d[1] = 1
    >>> d[2] = 2
    >>> d[3] = 3
    >>> d[4] = 1
    >>> d.values()
    dict_values([1, 2, 3, 1])




4.3 A Quick Summary
----------------------
이 장에서는 다음을 배웠다.

Mappings

String formatting with dictionaries

Dictionary methods

새로운 함수
~~~~~~~~~~~~~

dict(seq) Creates dictionary from (key, value) pairs (or a mapping or keyword arguments)

