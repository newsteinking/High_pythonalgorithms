chapter 3: Linked List
====================================

ref: https://github.com/MyHackInfo/Algorithm-In-Python

3.1 About Array
------------------------------

.. code-block:: python

    Arrays: Data Structure
         ** A collection of elemements/values each identifued by an array index or key!!!
           -Index start at Zero
           -Because of the indexes:random access is possible
    Advantages:
           -Arrays are data Structures in order to store items of the same type
           -Dynamic array:when the size of the array is changing daynamically
           -Applications:lookup tables/hashtables, heaps
           -We can use random access Because of the keys:getitem(int index) will return the
            value with the given key very fast // O(1)
           -Very easy to implement and to use
           -Very fast dat Structure
           -We should use array in Applications when we want to add items over and over again
            and we want to take items with given indexes!!! ~ It will be fast
    Disadvantages:
            -We have to know the size of the array at complie-time: so it is not so
                dynamic data Structure
            -If it is full: we have to create a bigger array and have to copy the values one by one
                // reconsturcting an array is O(N) operation
            -It is not able to store items with different types
    Multidimensional Arrays: It can prove to be very important in mathematical related computations(matrixes)
            -number[][] two dimensional Arrays
            -First parameter: row index
            -Second parameter:column index

3.2 Array Operations
------------------------------

.. code-block:: python

    '''
        # create Arrays
        # Random indexing --> O(1) get items if we kwon the index !!!
        # Array index value change
        # Print all Array elements one by one
        # Using slice[] operator acces array elements
        # O(N) Search running time
    '''

    # create Arrays
    number=[12,34,567,8,9.6,8,4,56,34]

    # Random indexing --> O(1) get items if we kwon the index !!!
    print('1-access array using index value')
    print(number[0])
    print("index 2,5,1,0:",number[2],number[5],number[1],number[0])


    #Array index value change
    print('\n2-Array index value change')
    number[1]='String' #In python array support any data-type
    print(number[1])


    #Print all Array elements one by one
    print('\n3-Print all Array elements one by one')
    for num in number:
        print(num)


    #Using slice[] operator acces array elements
    print('\n4-Using slice[] operator acces array elements')
    print(number[:])
    print(number[:3])
    print(number[:-2])


    # O(N) Search running time
    print('\n5-O(N) Search running time')
    numbers=[23,56,87,345,67,89]
    maximum=numbers[0]
    for num in numbers:
        if num > maximum:
            maximum = num

    print(maximum)


3.3 Linked Lists
------------------------------

.. code-block:: python

    ''' ######## Linked Lists #############
    @Linked Lists@:-> Linked lists are composed of nodes and references/pointers pointing
                       form one node to the other!!!
                    -> The last reference is pointing to a NULL!
                    -> Each node is composed of a data and a reference/link to the
                         next node in the sequence
                    -> Simple and very common data structure!!
                    -> They can be used to implement several other common data
                        types: stacks,queues
                    -> Simple linked lists by themselves do not allow random access to
                        the data // so we can not use indexes .. getltem(int index)!!!
                    -> Many basic operations such as obtaining the last node of the list or
                        finding a node that contains a given data or locating the place where a
                        a new node should be inserted -- require sequential scanning of most or
                        all of the list emlememts!
    @Advantages@:->
            <>Linked list are dynamic data strutures(arrays are not!!).
            <>It can allocate the needed memory in run-time.
            <>Very efficient if we want to manipulate the first elememets.
            <>Easy implementation.
            <>Can store items with different sizes: an array assumes every element
                to be exactly the same.
            <>It's easier for a linked list to grow organically.An array;s size needs to
                be known ahead of time,or re-created when it needs to grow.
    @Disadvantages@:->
            <>Waste memory because of the references
            <>Nodes in a linked list must be read in order in order form the beginning as
                linked lists have sequential access.
            <>Difficulties arise in linked lists when it comes to reverse traversing.
                single lined lists are extremely difficult to navigate backwards.
            <>Solution: doubly lined lists-> easier to read,but memory is wasted in
                allocating space for a back pointer
    @Single Node@:->
                 @->contains data->integer,double or custom object
                 @->contains a regerence pointing to the next node in the linked list
        class Node{
                data
                Node nextNode
                ...
            }



3.4 Linked Lists Vs Array
------------------------------

.. code-block:: python

    ####### Linked List VS Arrays ##############

    ###On The Base        ## Linked List  ## Arrays    ###Best##
    #1-Search              | O(N)          | O(1)       Arrays
    #2-Insert at the Start | O(1)          | O(N)       Linked list
    #3-Insert at the End   | O(N)          | O(1)       Arrays
    #4-Waste Space         | O(N)          | 0          Arrays




    print('####### Linked List VS Arrays ##############')
    print('###On The Base        ## Linked List  ## Arrays    ###Best##')
    print('#1-Search              | O(N)          | O(1)       Arrays')
    print('#2-Insert at the Start | O(1)          | O(N)       Linked list')
    print('#3-Insert at the End   | O(N)          | O(1)       Arrays')
    print('#4-Waste Space         | O(N)          | 0          Arrays')



3.4 Linked Lists Vs Array
------------------------------


.. code-block:: python


    class Node(object):

        def _init_(self,data):
            self.data=data
            self.nextNode=None


    class LinkedList(object):

        def __init__(self):
            self.head=None
            self.size=0


        # O(1) !!!
        def insertStart(self,data):

            self.size=self.size + 1
            newNode=Node(data)

            if not self.head:
                self.head=newNode
            else:
                newNode.nextNode=self.head
                self.head=newNode

        def remove(self,data):

            if self.head is None:
                return
            self.size=self.size -1

            currentNode=self.head
            previousNode=Node

            while currentNode.data !=data:
                previousNode=currentNode
                currentNode=currentNode.nextNode
            if previousNode is None:
                self.head=currentNode.nextNode
            else:
                previousNode.nextNode=currentNode.nextNode



        #O(1)
        def size1(self):
            return self.size

        #O(N)
        def size2(self):
            actualNode=self.head
            size=0

            while actualNode is not None:
                size+=1
                actualNode=actualNode.nextNode

                return size


        def insertEnd(self,data):

            self.size=self.size +1
            newNode=Node(data)
            actualNode=self.head

            while actualNode.nextNode is not None:
                actualNode=actualNode.nextNode

            actualNode.nextNode=newNode

        def traverseList(self):
            actualNode=self.head

            while actualNode is not None:
                print("%d " % actualNode.data)
                actualNode=actualNode.nextNode


    linkedlist=LinkedList()

    linkedlist.insertStart(12)
    linkedlist.insertStart(122)
    linkedlist.insertEnd(31)

    linkedlist.traverseList()
