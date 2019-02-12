chapter 19: stack
==================================



19.1 is consecutive
-------------------------
Given a stack, a function is_consecutive takes a stack as a parameter and that
returns whether or not the stack contains a sequence of consecutive integers
starting from the bottom of the stack (returning true if it does, returning
false if it does not).

For example:
bottom [3, 4, 5, 6, 7] top
Then the call of is_consecutive(s) should return true.
bottom [3, 4, 6, 7] top
Then the call of is_consecutive(s) should return false.
bottom [3, 2, 1] top
The function should return false due to reverse order.

Note: There are 2 solutions:
first_is_consecutive: it uses a single stack as auxiliary storage
second_is_consecutive: it uses a single queue as auxiliary storage

.. code-block:: python

    import collections

    def first_is_consecutive(stack):
        storage_stack = []
        for i in range(len(stack)):
            first_value = stack.pop()
            if len(stack) == 0:                # Case odd number of values in stack
                return True
            second_value = stack.pop()
            if first_value - second_value != 1: # Not consecutive
                return False
            stack.append(second_value)          # Backup second value
            storage_stack.append(first_value)

        # Back up stack from storage stack
        for i in range(len(storage_stack)):
            stack.append(storage_stack.pop())
        return True

    def second_is_consecutive(stack):
        q = collections.deque()
        for i in range(len(stack)):
            first_value = stack.pop()
            if len(stack) == 0:                # Case odd number of values in stack
                return True
            second_value = stack.pop()
            if first_value - second_value != 1: # Not consecutive
                return False
            stack.append(second_value)          # Backup second value
            q.append(first_value)

        # Back up stack from queue
        for i in range(len(q)):
            stack.append(q.pop())
        for i in range(len(stack)):
            q.append(stack.pop())
        for i in range(len(q)):
            stack.append(q.pop())

        return True


19.2 is sorted
------------------------
Given a stack, a function is_sorted accepts a stack as a parameter and returns
true if the elements in the stack occur in ascending increasing order from
bottom, and false otherwise. That is, the smallest element should be at bottom

For example:
bottom [6, 3, 5, 1, 2, 4] top
The function should return false
bottom [1, 2, 3, 4, 5, 6] top
The function should return true

.. code-block:: python

    def is_sorted(stack):
        storage_stack = []
        for i in range(len(stack)):
            if len(stack) == 0:
                break
            first_val = stack.pop()
            if len(stack) == 0:
                break
            second_val = stack.pop()
            if first_val < second_val:
                return False
            storage_stack.append(first_val)
            stack.append(second_val)

        # Backup stack
        for i in range(len(storage_stack)):
            stack.append(storage_stack.pop())

        return True


19.3 longest abs path
--------------------------


.. code-block:: python


    # def lengthLongestPath(input):
        # maxlen = 0
        # pathlen = {0: 0}
        # for line in input.splitlines():
            # print("---------------")
            # print("line:", line)
            # name = line.strip('\t')
            # print("name:", name)
            # depth = len(line) - len(name)
            # print("depth:", depth)
            # if '.' in name:
                # maxlen = max(maxlen, pathlen[depth] + len(name))
            # else:
                # pathlen[depth + 1] = pathlen[depth] + len(name) + 1
            # print("maxlen:", maxlen)
        # return maxlen

    # def lengthLongestPath(input):
        # paths = input.split("\n")
        # level = [0] * 10
        # maxLength = 0
        # for path in paths:
            # print("-------------")
            # levelIdx = path.rfind("\t")
            # print("Path: ", path)
            # print("path.rfind(\\t)", path.rfind("\t"))
            # print("levelIdx: ", levelIdx)
            # print("level: ", level)
            # level[levelIdx + 1] = level[levelIdx] + len(path) - levelIdx + 1
            # print("level: ", level)
            # if "." in path:
                # maxLength = max(maxLength, level[levelIdx+1] - 1)
                # print("maxlen: ", maxLength)
        # return maxLength

    def length_longest_path(input):
        """
        :type input: str
        :rtype: int
        """
        curr_len, max_len = 0, 0    # running length and max length
        stack = []    # keep track of the name length
        for s in input.split('\n'):
            print("---------")
            print("<path>:", s)
            depth = s.count('\t')    # the depth of current dir or file
            print("depth: ", depth)
            print("stack: ", stack)
            print("curlen: ", curr_len)
            while len(stack) > depth:    # go back to the correct depth
                curr_len -= stack.pop()
            stack.append(len(s.strip('\t'))+1)   # 1 is the length of '/'
            curr_len += stack[-1]    # increase current length
            print("stack: ", stack)
            print("curlen: ", curr_len)
            if '.' in s:    # update maxlen only when it is a file
                max_len = max(max_len, curr_len-1)    # -1 is to minus one '/'
        return max_len

    st= "dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdirectory1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext"
    st2 = "a\n\tb1\n\t\tf1.txt\n\taaaaa\n\t\tf2.txt"
    print("path:", st2)

    print("answer:", length_longest_path(st2))





19.4 ordered stack
------------------------
#The stack remains always ordered such that the highest value is at the top and the lowest at the bottom

.. code-block:: python

    class OrderedStack:
         def __init__(self):
             self.items = []

         def is_empty(self):
             return self.items == []

         def push_t(self, item):
             self.items.append(item)

         def push(self, item): #push method to maintain order when pushing new elements
             temp_stack = OrderedStack()
             if self.is_empty() or item > self.peek():
                 self.push_t(item)
             else:
                 while item < self.peek() and not self.is_empty():
                     temp_stack.push_t(self.pop())
                 self.push_t(item)
                 while not temp_stack.is_empty():
                     self.push_t(temp_stack.pop())

         def pop(self):
             if self.is_empty():
                 raise IndexError("Stack is empty")
             return self.items.pop()

         def peek(self):
             return self.items[len(self.items) - 1]

         def size(self):
             return len(self.items)



19.5 remove min
-------------------
Given a stack, a function remove_min accepts a stack as a parameter
and removes the smallest value from the stack.

For example:
bottom [2, 8, 3, -6, 7, 3] top
After remove_min(stack):
bottom [2, 8, 3, 7, 3] top


.. code-block:: python


    def remove_min(stack):
        storage_stack = []
        if len(stack) == 0:  # Stack is empty
            return stack
        # Find the smallest value
        min = stack.pop()
        stack.append(min)
        for i in range(len(stack)):
            val = stack.pop()
            if val <= min:
                min = val
            storage_stack.append(val)
        # Back up stack and remove min value
        for i in range(len(storage_stack)):
            val = storage_stack.pop()
            if val != min:
                stack.append(val)
        return stack


19.6 simplify path
-------------------
Given an absolute path for a file (Unix-style), simplify it.

For example,
path = "/home/", => "/home"
path = "/a/./b/../../c/", => "/c"

* Did you consider the case where path = "/../"?
    In this case, you should return "/".
* Another corner case is the path might contain multiple slashes '/' together, such as "/home//foo/".
    In this case, you should ignore redundant slashes and return "/home/foo".

.. code-block:: python

    def simplify_path(path):
        """
        :type path: str
        :rtype: str
        """
        skip = {'..', '.', ''}
        stack = []
        paths = path.split('/')
        for tok in paths:
            if tok == '..':
                if stack:
                    stack.pop()
            elif tok not in skip:
                stack.append(tok)
        return '/' + '/'.join(stack)

19.7 stack
-------------------
Stack Abstract Data Type (ADT)
Stack() creates a new stack that is empty.
   It needs no parameters and returns an empty stack.
push(item) adds a new item to the top of the stack.
   It needs the item and returns nothing.
pop() removes the top item from the stack.
   It needs no parameters and returns the item. The stack is modified.
peek() returns the top item from the stack but does not remove it.
   It needs no parameters. The stack is not modified.
isEmpty() tests to see whether the stack is empty.
   It needs no parameters and returns a boolean value.
size() returns the number of items on the stack.
   It needs no parameters and returns an integer.

.. code-block:: python

    from abc import ABCMeta, abstractmethod
    class AbstractStack(metaclass=ABCMeta):
        """Abstract Class for Stacks."""
        def __init__(self):
            self._top = -1

        def __len__(self):
            return self._top + 1

        def __str__(self):
            result = " ".join(map(str, self))
            return 'Top-> ' + result

        def is_empty(self):
            return self._top == -1

        @abstractmethod
        def __iter__(self):
            pass

        @abstractmethod
        def push(self, value):
            pass

        @abstractmethod
        def pop(self):
            pass

        @abstractmethod
        def peek(self):
            pass


    class ArrayStack(AbstractStack):
        def __init__(self, size=10):
            """
            Initialize python List with size of 10 or user given input.
            Python List type is a dynamic array, so we have to restrict its
            dynamic nature to make it work like a static array.
            """
            super().__init__()
            self._array = [None] * size

        def __iter__(self):
            probe = self._top
            while True:
                if probe == -1:
                    return
                yield self._array[probe]
                probe -= 1

        def push(self, value):
            self._top += 1
            if self._top == len(self._array):
                self._expand()
            self._array[self._top] = value

        def pop(self):
            if self.is_empty():
                raise IndexError("stack is empty")
            value = self._array[self._top]
            self._top -= 1
            return value

        def peek(self):
            """returns the current top element of the stack."""
            if self.is_empty():
                raise IndexError("stack is empty")
            return self._array[self._top]

        def _expand(self):
            """
             expands size of the array.
             Time Complexity: O(n)
            """
            self._array += [None] * len(self._array)  # double the size of the array


    class StackNode:
        """Represents a single stack node."""
        def __init__(self, value):
            self.value = value
            self.next = None


    class LinkedListStack(AbstractStack):

        def __init__(self):
            super().__init__()
            self.head = None

        def __iter__(self):
            probe = self.head
            while True:
                if probe is None:
                    return
                yield probe.value
                probe = probe.next

        def push(self, value):
            node = StackNode(value)
            node.next = self.head
            self.head = node
            self._top += 1

        def pop(self):
            if self.is_empty():
                raise IndexError("Stack is empty")
            value = self.head.value
            self.head = self.head.next
            self._top -= 1
            return value

        def peek(self):
            if self.is_empty():
                raise IndexError("Stack is empty")
            return self.head.value


19.8 stutter
-------------------
Given a stack, stutter takes a stack as a parameter and  replaces every value
in the stack with two occurrences of that value.

For example, suppose the stack stores these values:
bottom [3, 7, 1, 14, 9] top
Then the stack should store these values after the method terminates:
bottom [3, 3, 7, 7, 1, 1, 14, 14, 9, 9] top

Note: There are 2 solutions:
first_stutter: it uses a single stack as auxiliary storage
second_stutter: it uses a single queue as auxiliary storage

.. code-block:: python

    import collections

    def first_stutter(stack):
        storage_stack = []
        for i in range(len(stack)):
            storage_stack.append(stack.pop())
        for i in range(len(storage_stack)):
            val = storage_stack.pop()
            stack.append(val)
            stack.append(val)

        return stack

    def second_stutter(stack):
        q = collections.deque()
        # Put all values into queue from stack
        for i in range(len(stack)):
            q.append(stack.pop())
        # Put values back into stack from queue
        for i in range(len(q)):
            stack.append(q.pop())
        # Now, stack is reverse, put all values into queue from stack
        for i in range(len(stack)):
            q.append(stack.pop())
        # Put 2 times value into stack from queue
        for i in range(len(q)):
            val = q.pop()
            stack.append(val)
            stack.append(val)

        return stack


19.9 swithch pairs
----------------------
Given a stack, switch_pairs function takes a stack as a parameter and that
switches successive pairs of numbers starting at the bottom of the stack.

For example, if the stack initially stores these values:
bottom [3, 8, 17, 9, 1, 10] top
Your function should switch the first pair (3, 8), the second pair (17, 9), ...:
bottom [8, 3, 9, 17, 10, 1] top

if there are an odd number of values in the stack, the value at the top of the
stack is not moved: For example:
bottom [3, 8, 17, 9, 1] top
It would again switch pairs of values, but the value at the top of the stack (1)
would not be moved
bottom [8, 3, 9, 17, 1] top

Note: There are 2 solutions:
first_switch_pairs: it uses a single stack as auxiliary storage
second_switch_pairs: it uses a single queue as auxiliary storage

.. code-block:: python

    import collections

    def first_switch_pairs(stack):
        storage_stack = []
        for i in range(len(stack)):
            storage_stack.append(stack.pop())
        for i in range(len(storage_stack)):
            if len(storage_stack) == 0:
                break
            first = storage_stack.pop()
            if len(storage_stack) == 0:    # case: odd number of values in stack
                stack.append(first)
                break
            second = storage_stack.pop()
            stack.append(second)
            stack.append(first)
        return stack

    def second_switch_pairs(stack):
        q = collections.deque()
        # Put all values into queue from stack
        for i in range(len(stack)):
            q.append(stack.pop())
        # Put values back into stack from queue
        for i in range(len(q)):
            stack.append(q.pop())
        # Now, stack is reverse, put all values into queue from stack
        for i in range(len(stack)):
            q.append(stack.pop())
        # Swap pairs by appending the 2nd value before appending 1st value
        for i in range(len(q)):
            if len(q) == 0:
                break
            first = q.pop()
            if len(q) == 0:                 # case: odd number of values in stack
                stack.append(first)
                break
            second = q.pop()
            stack.append(second)
            stack.append(first)

        return stack

19.10 valid parenthesis
-----------------------------
Given a string containing just the characters
'(', ')', '{', '}', '[' and ']',
determine if the input string is valid.

The brackets must close in the correct order,
"()" and "()[]{}" are all valid but "(]" and "([)]" are not.

.. code-block:: python


    def is_valid(s: str) -> bool:
        stack = []
        dic = {")": "(",
               "}": "{",
               "]": "["}
        for char in s:
            if char in dic.values():
                stack.append(char)
            elif char in dic:
                if not stack or dic[char] != stack.pop():
                    return False
        return not stack




