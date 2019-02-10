chapter 1: Sorting
=======================================


1.1 Bubble Sort
---------------------------------


.. image:: https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Bubblesort-edited-color.svg/220px-Bubblesort-edited-color.svg.png
   :target: https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Bubblesort-edited-color.svg/220px-Bubblesort-edited-color.svg.png
   :alt: alt text


**Bubble sort**\ , sometimes referred to as *sinking sort*\ , is a simple sorting algorithm that repeatedly steps through the list to be sorted, compares each pair of adjacent items and swaps them if they are in the wrong order. The pass through the list is repeated until no swaps are needed, which indicates that the list is sorted.

**Properties**


* Worst case performance    O(n\ :raw-html-m2r:`<sup>2</sup>`\ )
* Best case performance O(n)
* Average case performance  O(n\ :raw-html-m2r:`<sup>2</sup>`\ )

Source: `Wikipedia <https://en.wikipedia.org/wiki/Bubble_sort>`_
####################################################################

View the algorithm in `action <https://www.toptal.com/developers/sorting-algorithms/bubble-sort>`_
######################################################################################################
Bubble Sort Example
~~~~~~~~~~~~~~~~~~~~~~

Step-by-step example
Take an array of numbers " 5 1 4 2 8", and sort the array from lowest number to greatest number using bubble sort. In each step, elements written in bold are being compared. Three passes will be required.

First Pass
( 5 1 4 2 8 ) → ( 1 5 4 2 8 ), Here, algorithm compares the first two elements, and swaps since 5 > 1.
( 1 5 4 2 8 ) → ( 1 4 5 2 8 ), Swap since 5 > 4
( 1 4 5 2 8 ) → ( 1 4 2 5 8 ), Swap since 5 > 2
( 1 4 2 5 8 ) → ( 1 4 2 5 8 ), Now, since these elements are already in order (8 > 5), algorithm does not swap them.
Second Pass
( 1 4 2 5 8 ) → ( 1 4 2 5 8 )
( 1 4 2 5 8 ) → ( 1 2 4 5 8 ), Swap since 4 > 2
( 1 2 4 5 8 ) → ( 1 2 4 5 8 )
( 1 2 4 5 8 ) → ( 1 2 4 5 8 )
Now, the array is already sorted, but the algorithm does not know if it is completed. The algorithm needs one whole pass without any swap to know it is sorted.

Third Pass
( 1 2 4 5 8 ) → ( 1 2 4 5 8 )
( 1 2 4 5 8 ) → ( 1 2 4 5 8 )
( 1 2 4 5 8 ) → ( 1 2 4 5 8 )
( 1 2 4 5 8 ) → ( 1 2 4 5 8 )


.. code-block:: python


    def bubble_sort(collection):
        """Pure implementation of bubble sort algorithm in Python

        :param collection: some mutable ordered collection with heterogeneous
        comparable items inside
        :return: the same collection ordered by ascending

        Examples:
        >>> bubble_sort([0, 5, 3, 2, 2])
        [0, 2, 2, 3, 5]

        >>> bubble_sort([])
        []

        >>> bubble_sort([-2, -5, -45])
        [-45, -5, -2]

        >>> bubble_sort([-23,0,6,-4,34])
        [-23,-4,0,6,34]
        """
        length = len(collection)
        for i in range(length-1):
            swapped = False
            for j in range(length-1-i):
                if collection[j] > collection[j+1]:
                    swapped = True
                    collection[j], collection[j+1] = collection[j+1], collection[j]
            if not swapped: break  # Stop iteration if the collection is sorted.
        return collection


    if __name__ == '__main__':
        #===========================================================================
        # try:
        #     raw_input          # Python 2
        # except NameError:
        #     raw_input = input  # Python 3
        #===========================================================================
        user_input = input('Enter numbers separated by a comma:').strip()
        unsorted = [int(item) for item in user_input.split(',')]
        print(*bubble_sort(unsorted), sep=',')

Bubble Sort Animation
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    import random
    import pygame
    from pygame.locals import *

    scr_size = (width,height) = (900,600)
    FPS = 20
    screen = pygame.display.set_mode(scr_size)
    clock = pygame.time.Clock()
    black = (0,0,0)
    white = (255,255,255)

    pygame.display.set_caption('Bubble Sort')

    def generatearray(lowerlimit,upperlimit,length):
        arr = []
        for i in range(0,length):
            arr.append(2*i)

            #arr.append(random.randrange(lowerlimit,upperlimit))

        random.shuffle(arr)
        return arr
    #    arr = []
    #    for i in range(0,length):
    #        arr.append(random.randrange(lowerlimit,upperlimit))
    #
    #    return arr


    class sort():
        def __init__(self,arr):
            self.arr = arr
            self.n = len(arr)
            self.i = 1
            self.image = pygame.Surface((width - width/5,height - height/5))
            self.rect = self.image.get_rect()
            self.rect.left = width/10
            self.rect.top = height/10
            self.width_per_bar = self.rect.width / self.n - 2

        def update(self):
            if self.i < self.n:
                self.image.fill(black)
                #################Sorting Algorithm here#############################
                for j in range(0,self.n - self.i):
                    if self.arr[j] > self.arr[j+1]:
                        self.arr[j],self.arr[j+1] = self.arr[j+1],self.arr[j]
                self.i += 1
                ####################################################################
                l = 0
                for k in range(0,int(self.rect.width),int(self.width_per_bar + 2)):
                    bar = pygame.Surface((self.width_per_bar,self.arr[l]))
                    bar_rect = bar.get_rect()
                    bar.fill(white)
                    bar_rect.bottom = self.rect.height
                    bar_rect.left = k

                    self.image.blit(bar,bar_rect)
                    l += 1

            else:
                pass


        def draw(self):
            screen.blit(self.image,self.rect)


    def main():
        arr = generatearray(1,height - height/5 - 10,240)
        bubble_sort = sort(arr)
        while True:
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    quit()
                if event.type == pygame.KEYDOWN:
                    pass
                if event.type == pygame.KEYUP:
                    pass
            bubble_sort.update()
            screen.fill(black)
            print(bubble_sort.arr)
            bubble_sort.draw()
            pygame.display.update()
            clock.tick(FPS)

    main()


1.2 Selection Sort
---------------------------------



1.3 Insertion Sort
---------------------------------



1.4 Merge Sort
---------------------------------



1.5 Random Quick Sort
---------------------------------


1.6 Counting Sort
---------------------------------


1.7 Randix Sort
---------------------------------




1.5 The Interactive Interpreter
---------------------------------

