chapter 16: queues
===============================


16.1 max sliding window
-------------------------------
Given an array and a number k
Find the max elements of each of its sub-arrays of length k.

Keep indexes of good candidates in deque d.
The indexes in d are from the current window, they're increasing,
and their corresponding nums are decreasing.
Then the first deque element is the index of the largest window value.

For each index i:

1. Pop (from the end) indexes of smaller elements (they'll be useless).
2. Append the current index.
3. Pop (from the front) the index i - k, if it's still in the deque
   (it falls out of the window).
4. If our window has reached size k,
   append the current window maximum to the output.


.. code-block:: python

    import collections
    def max_sliding_window(arr, k):
        qi = collections.deque()  # queue storing indexes of elements
        result = []
        for i, n in enumerate(arr):
            while qi and arr[qi[-1]] < n:
                qi.pop()
            qi.append(i)
            if qi[0] == i - k:
                qi.popleft()
            if i >= k - 1:
                result.append(arr[qi[0]])
        return result

16.2 moving average
-------------------------------

.. code-block:: python

    from __future__ import division
    from collections import deque


    class MovingAverage(object):
        def __init__(self, size):
            """
            Initialize your data structure here.
            :type size: int
            """
            self.queue = deque(maxlen=size)

        def next(self, val):
            """
            :type val: int
            :rtype: float
            """
            self.queue.append(val)
            return sum(self.queue) / len(self.queue)


    # Given a stream of integers and a window size,
    # calculate the moving average of all integers in the sliding window.
    if __name__ == '__main__':
        m = MovingAverage(3)
        assert m.next(1) == 1
        assert m.next(10) == (1 + 10) / 2
        assert m.next(3) == (1 + 10 + 3) / 3
        assert m.next(5) == (10 + 3 + 5) / 3


16.3 priority queue
-------------------------------
Implementation of priority queue using linear array.
Insertion - O(n)
Extract min/max Node - O(1)


.. code-block:: python

    import itertools


    class PriorityQueueNode:
        def __init__(self, data, priority):
            self.data = data
            self.priority = priority

        def __repr__(self):
            return "{}: {}".format(self.data, self.priority)


    class PriorityQueue:
        def __init__(self, items=None, priorities=None):
            """Create a priority queue with items (list or iterable).
            If items is not passed, create empty priority queue."""
            self.priority_queue_list = []
            if items is None:
                return
            if priorities is None:
                priorities = itertools.repeat(None)
            for item, priority in zip(items, priorities):
                self.push(item, priority=priority)

        def __repr__(self):
            return "PriorityQueue({!r})".format(self.priority_queue_list)

        def size(self):
            """Return size of the priority queue.
            """
            return len(self.priority_queue_list)

        def push(self, item, priority=None):
            """Push the item in the priority queue.
            if priority is not given, priority is set to the value of item.
            """
            priority = item if priority is None else priority
            node = PriorityQueueNode(item, priority)
            for index, current in enumerate(self.priority_queue_list):
                if current.priority < node.priority:
                    self.priority_queue_list.insert(index, node)
                    return
            # when traversed complete queue
            self.priority_queue_list.append(node)

        def pop(self):
            """Remove and return the item with the lowest priority.
            """
            # remove and return the first node from the queue
            return self.priority_queue_list.pop().data



16.4 queue
-------------------------------
Queue Abstract Data Type (ADT)
* Queue() creates a new queue that is empty.
  It needs no parameters and returns an empty queue.
* enqueue(item) adds a new item to the rear of the queue.
  It needs the item and returns nothing.
* dequeue() removes the front item from the queue.
  It needs no parameters and returns the item. The queue is modified.
* isEmpty() tests to see whether the queue is empty.
  It needs no parameters and returns a boolean value.
* size() returns the number of items in the queue.
  It needs no parameters and returns an integer.
* peek() returns the front element of the queue.


.. code-block:: python

    from abc import ABCMeta, abstractmethod
    class AbstractQueue(metaclass=ABCMeta):

        def __init__(self):
            self._size = 0

        def __len__(self):
            return self._size

        def is_empty(self):
            return self._size == 0

        @abstractmethod
        def enqueue(self, value):
            pass

        @abstractmethod
        def dequeue(self):
            pass

        @abstractmethod
        def peek(self):
            pass

        @abstractmethod
        def __iter__(self):
            pass


    class ArrayQueue(AbstractQueue):

        def __init__(self, capacity=10):
            """
            Initialize python List with capacity of 10 or user given input.
            Python List type is a dynamic array, so we have to restrict its
            dynamic nature to make it work like a static array.
            """
            super().__init__()
            self._array = [None] * capacity
            self._front = 0
            self._rear = 0

        def __iter__(self):
            probe = self._front
            while True:
                if probe == self._rear:
                    return
                yield self._array[probe]
                probe += 1

        def enqueue(self, value):
            if self._rear == len(self._array):
                self._expand()
            self._array[self._rear] = value
            self._rear += 1
            self._size += 1

        def dequeue(self):
            if self.is_empty():
                raise IndexError("Queue is empty")
            value = self._array[self._front]
            self._array[self._front] = None
            self._front += 1
            self._size -= 1
            return value

        def peek(self):
            """returns the front element of queue."""
            if self.is_empty():
                raise IndexError("Queue is empty")
            return self._array[self._front]

        def _expand(self):
            """expands size of the array.
             Time Complexity: O(n)
            """
            self._array += [None] * len(self._array)


    class QueueNode:
        def __init__(self, value):
            self.value = value
            self.next = None

    class LinkedListQueue(AbstractQueue):

        def __init__(self):
            super().__init__()
            self._front = None
            self._rear = None

        def __iter__(self):
            probe = self._front
            while True:
                if probe is None:
                    return
                yield probe.value
                probe = probe.next

        def enqueue(self, value):
            node = QueueNode(value)
            if self._front is None:
                self._front = node
                self._rear = node
            else:
                self._rear.next = node
                self._rear = node
            self._size += 1

        def dequeue(self):
            if self.is_empty():
                raise IndexError("Queue is empty")
            value = self._front.value
            if self._front is self._rear:
                self._front = None
                self._rear = None
            else:
                self._front = self._front.next
            self._size -= 1
            return value

        def peek(self):
            """returns the front element of queue."""
            if self.is_empty():
                raise IndexError("Queue is empty")
            return self._front.value



16.5 reconstruct queue
-------------------------------
# Suppose you have a random list of people standing in a queue.
# Each person is described by a pair of integers (h, k),
# where h is the height of the person and k is the number of people
# in front of this person who have a height greater than or equal to h.
# Write an algorithm to reconstruct the queue.

# Note:
# The number of people is less than 1,100.

# Example

# Input:
# [[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

# Output:
# [[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]



.. code-block:: python

    def reconstruct_queue(people):
        """
        :type people: List[List[int]]
        :rtype: List[List[int]]
        """
        queue = []
        people.sort(key=lambda x: (-x[0], x[1]))
        for h, k in people:
            queue.insert(k, [h, k])
        return queue


16.6 zigzag iterator
-------------------------------

.. code-block:: python


    class ZigZagIterator:
        def __init__(self, v1, v2):
            """
            Initialize your data structure here.
            :type v1: List[int]
            :type v2: List[int]
            """
            self.queue=[_ for _ in (v1,v2) if _]
            print(self.queue)

        def next(self):
            """
            :rtype: int
            """
            v=self.queue.pop(0)
            ret=v.pop(0)
            if v: self.queue.append(v)
            return ret

        def has_next(self):
            """
            :rtype: bool
            """
            if self.queue: return True
            return False

    l1 = [1, 2]
    l2 = [3, 4, 5, 6]
    it = ZigZagIterator(l1, l2)
    while it.has_next():
        print(it.next())

