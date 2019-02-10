chapter 2: Bit Manipulation
=======================================

ref : https://github.com/anirudh-janga/py-bit-manipulation/tree/master/code

2.1 Bitmask
------------------------


.. code-block:: python

    Set of all bit wise operations


    class BitOperations(object):
        '''
        Initiaze the bit operations object
        '''

        def __init__(self, number):
            self._number = number

        def is_bit_on(self, bit_num):
            return self._number & (1 << bit_num)

        def set_bit(self, bit_num):
            return self._number | (1 << bit_num)

        def clear_bit(self, bit_num):
            return self._number & ~(1 << bit_num)

        def flip_bit(self, bit_num):
            return self._number ^ (1 << bit_num)

        def clear_last_set_bit(self):
            return self._number & (self._number-1)




2.2 Count Number Sets Bits
----------------------------------


.. code-block:: python




    class BitOperations(object):

        def __init__(self, number):

            #validation
            if number > 255:
                raise TypeError('For now only works for 8 bit numbers')

            self._num = number

            #build a look up table
            bit_table = [0]*256
            for index in range(256):
                bit_table[index] = (index&1) + bit_table[index>>1]
            self.bit_table = bit_table

        def count_set_bits_1(self):

            #worst case - O(n)
            num_set_bits = 0
            for bit_num in range(8):
                if self._num & (1<<bit_num):
                    num_set_bits += 1

            return num_set_bits

        def count_set_bits_2(self):

            number = self._num

            #worst case - O(n)
            num_set_bits = 0
            while number:
                number &= (number-1)
                num_set_bits += 1

            return num_set_bits

        def count_set_bits_3(self):

            #worst case - O(1)
            return self.bit_table[self._num & 0xff]

        def get_parity(self):
            num_bits = self.count_set_bits_3()
            return 'even' if num_bits%2==0 else 'odd'

2.3  Next Power of 2
----------------------------------


.. code-block:: python



    Implementations for counting a number n such as n > given number where n = 2^n


    class NextPow2(object):

        def __init__(self, number):
            self._num = number

        def next_pow_2(self):
            n = self._num
            n |= n>>1
            n |= n>>2
            n |= n>>4
            n |= n>>8
            n |= n>>16
            return n+1

2.4  Rotate Bits
----------------------------------


.. code-block:: python

    class BitOperations(object):

        def __init__(self, number):
            self._num = number

        def rotate_right(self, num_times):
            n = self._num
            return (n >> num_times) | (n <<(64 - num_times))

        def rotate_left(self, num_times):
            n = self._num
            return (n << num_times) | (n >> (64 - num_times))


2.5  test 
----------------------------------


.. code-block:: python


    test
    this is test

    