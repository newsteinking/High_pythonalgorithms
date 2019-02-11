chapter 17: search
=============================



17.1 binary search
--------------------------------------------
#
# Binary search works for a sorted array.
# Note: The code logic is written for an array sorted in
#       increasing order.
# T(n): O(log n)
#

.. code-block:: python

    def binary_search(array, query):
        lo, hi = 0, len(array) - 1
        while lo <= hi:
            mid = (hi + lo) // 2
            val = array[mid]
            if val == query:
                return mid
            elif val < query:
                lo = mid + 1
            else:
                hi = mid - 1
        return None

    def binary_search_recur(array, low, high, val):
        if low > high:       # error case
            return -1
        mid = (low + high) // 2
        if val < array[mid]:
            return binary_search_recur(array, low, mid - 1, val)
        elif val > array[mid]:
            return binary_search_recur(array, mid + 1, high, val)
        else:
            return mid




17.2 find min rotate
-------------------------------------------------
Suppose an array sorted in ascending order is rotated at some pivot unknown
to you beforehand. (i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element. The complexity must be O(logN)

You may assume no duplicate exists in the array.


.. code-block:: python

    def find_min_rotate(array):
        low = 0
        high = len(array) - 1
        while low < high:
            mid = (low + high) // 2
            if array[mid] > array[high]:
                low = mid + 1
            else:
                high = mid

        return array[low]

    def find_min_rotate_recur(array, low, high):
        mid = (low + high) // 2
        if mid == low:
            return array[low]
        elif array[mid] > array[high]:
            return find_min_rotate_recur(array, mid + 1, high)
        else:
            return find_min_rotate_recur(array, low, mid)


17.3 firt occurrence
-------------------------------------------------
#
# Find first occurance of a number in a sorted array (increasing order)
# Approach- Binary Search
# T(n)- O(log n)
#


.. code-block:: python

    def first_occurrence(array, query):
        lo, hi = 0, len(array) - 1
        while lo <= hi:
            mid = (lo + hi) // 2
            #print("lo: ", lo, " hi: ", hi, " mid: ", mid)
            if lo == hi:
                break
            if array[mid] < query:
                lo = mid + 1
            else:
                hi = mid
        if array[lo] == query:
          return lo




17.4 jump search
-------------------------------------------------



.. code-block:: python

    import math

    def jump_search(arr,target):
        """Jump Search
            Worst-case Complexity: O(√n) (root(n))
            All items in list must be sorted like binary search

            Find block that contains target value and search it linearly in that block
            It returns a first target value in array

            reference: https://en.wikipedia.org/wiki/Jump_search

        """
        n = len(arr)
        block_size = int(math.sqrt(n))
        block_prev = 0
        block= block_size

        # return -1 means that array doesn't contain taget value
        # find block that contains target value

        if arr[n - 1] < target:
            return -1
        while block <= n and arr[block - 1] < target:
            block_prev = block
            block += block_size

        # find target value in block

        while arr[block_prev] < target :
            block_prev += 1
            if block_prev == min(block, n) :
                return -1

        # if there is target value in array, return it

        if arr[block_prev] == target :
            return block_prev
        else :
            return -1



17.5 last occurrence
-------------------------------------------------
#
# Find last occurance of a number in a sorted array (increasing order)
# Approach- Binary Search
# T(n)- O(log n)
#


.. code-block:: python


    def last_occurrence(array, query):
        lo, hi = 0, len(array) - 1
        while lo <= hi:
            mid = (hi + lo) // 2
            if (array[mid] == query and mid == len(array)-1) or \
               (array[mid] == query and array[mid+1] > query):
                return mid
            elif (array[mid] <= query):
                lo = mid + 1
            else:
                hi = mid - 1


17.6 linear search
-------------------------------------------------

#
# Linear search works in any array.
#
# T(n): O(n)
#

.. code-block:: python

    def linear_search(array, query):
        for i in range(len(array)):
            if array[i] == query:
                return i

        return -1


17.7 next greatest letter
-------------------------------------------------

Given a list of sorted characters letters containing only lowercase letters,
and given a target letter target, find the smallest element in the list that
is larger than the given target.

Letters also wrap around. For example, if the target is target = 'z' and
letters = ['a', 'b'], the answer is 'a'.

Input:
letters = ["c", "f", "j"]
target = "a"
Output: "c"

Input:
letters = ["c", "f", "j"]
target = "c"
Output: "f"

Input:
letters = ["c", "f", "j"]
target = "d"
Output: "f"

Reference: https://leetcode.com/problems/find-smallest-letter-greater-than-target/description/

.. code-block:: python


    import bisect

    """
    Using bisect libarary
    """
    def next_greatest_letter(letters, target):
        index = bisect.bisect(letters, target)
        return letters[index % len(letters)]

    """
    Using binary search: complexity O(logN)
    """
    def next_greatest_letter_v1(letters, target):
        if letters[0] > target:
            return letters[0]
        if letters[len(letters) - 1] <= target:
            return letters[0]
        left, right = 0, len(letters) - 1
        while left <= right:
            mid = left + (right - left) // 2
            if  letters[mid] > target:
                right = mid - 1
            else:
                left = mid + 1
        return letters[left]

    """
    Brute force: complexity O(N)
    """
    def next_greatest_letter_v2(letters, target):
        for index in letters:
            if index > target:
                return index
        return letters[0]


17.8 search insert
-------------------------------------------------
Given a sorted array and a target value, return the index if the target is
found. If not, return the index where it would be if it were inserted in order.

For example:
[1,3,5,6], 5 -> 2
[1,3,5,6], 2 -> 1
[1,3,5,6], 7 -> 4
[1,3,5,6], 0 -> 0


.. code-block:: python

    def search_insert(array, val):
        low = 0
        high = len(array) - 1
        while low <=  high:
            mid = low + (high - low) // 2
            if val > array[mid]:
                low = mid + 1
            else:
                high = mid - 1
        return low



17.9 search range
-------------------------------------------------
Given an array of integers nums sorted in ascending order, find the starting
and ending position of a given target value. If the target is not found in the
array, return [-1, -1].

For example:
Input: nums = [5,7,7,8,8,8,10], target = 8
Output: [3,5]
Input: nums = [5,7,7,8,8,8,10], target = 11
Output: [-1,-1]


.. code-block:: python

    def search_range(nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        low = 0
        high = len(nums) - 1
        while low <= high:
            mid = low + (high - low) // 2
            if target < nums[mid]:
                high = mid - 1
            elif target > nums[mid]:
                low = mid + 1
            else:
                break

        for j in range(len(nums) - 1, -1, -1):
            if nums[j] == target:
                return [mid, j]

        return [-1, -1]



17.10 search rotate
-------------------------------------------------
Search in Rotated Sorted Array
Suppose an array sorted in ascending order is rotated at some pivot unknown
to you beforehand. (i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).

You are given a target value to search. If found in the array return its index,
otherwise return -1.

Your algorithm's runtime complexity must be in the order of O(log n).
---------------------------------------------------------------------------------
Explanation algorithm:

In classic binary search, we compare val with the midpoint to figure out if
val belongs on the low or the high side. The complication here is that the
array is rotated and may have an inflection point. Consider, for example:

Array1: [10, 15, 20, 0, 5]
Array2: [50, 5, 20, 30, 40]

Note that both arrays have a midpoint of 20, but 5 appears on the left side of
one and on the right side of the other. Therefore, comparing val with the
midpoint is insufficient.

However, if we look a bit deeper, we can see that one half of the array must be
ordered normally(increasing order). We can therefore look at the normally ordered
half to determine whether we should search the low or hight side.

For example, if we are searching for 5 in Array1, we can look at the left element (10)
and middle element (20). Since 10 < 20, the left half must be ordered normally. And, since 5
is not between those, we know that we must search the right half

In array2, we can see that since 50 > 20, the right half must be ordered normally. We turn to
the middle 20, and right 40 element to check if 5 would fall between them. The value 5 would not
Therefore, we search the left half.

There are 2 possible solution: iterative and recursion.
Recursion helps you understand better the above algorithm explanation


.. code-block:: python


    def search_rotate(array, val):
        low, high = 0, len(array) - 1
        while low <= high:
            mid = (low + high) // 2
            if val == array[mid]:
                return mid

            if array[low] <= array[mid]:
                if array[low] <= val <= array[mid]:
                    high = mid - 1
                else:
                    low = mid + 1
            else:
                if array[mid] <= val <= array[high]:
                    low = mid + 1
                else:
                    high = mid - 1

        return -1

    # Recursion technique
    def search_rotate_recur(array, low, high, val):
        if low >= high:
            return -1
        mid = (low + high) // 2
        if val == array[mid]:       # found element
            return mid
        if array[low] <= array[mid]:
            if array[low] <= val <= array[mid]:
                return search_rotate_recur(array, low, mid - 1, val)    # Search left
            else:
                return search_rotate_recur(array, mid + 1, high, val)   # Search right
        else:
            if array[mid] <= val <= array[high]:
                return search_rotate_recur(array, mid + 1, high, val)   # Search right
            else:
                return search_rotate_recur(array, low, mid - 1, val)    # Search left


17.11 two sum
-------------------------------------------------
Given an array of integers that is already sorted in ascending order, find two
numbers such that they add up to a specific target number. The function two_sum
should return indices of the two numbers such that they add up to the target,
where index1 must be less than index2. Please note that your returned answers
(both index1 and index2) are not zero-based.
You may assume that each input would have exactly one solution and you
may not use the same element twice.

Input: numbers = [2, 7, 11, 15], target=9
Output: index1 = 1, index2 = 2

Solution:
two_sum: using binary search
two_sum1: using dictionary as a hash table
two_sum2: using two pointers


.. code-block:: python

    # Using binary search technique
    def two_sum(numbers, target):
        for i in range(len(numbers)):
            second_val = target - numbers[i]
            low, high = i+1, len(numbers)-1
            while low <= high:
                mid = low + (high - low) // 2
                if second_val == numbers[mid]:
                    return [i + 1, mid + 1]
                elif second_val > numbers[mid]:
                    low = mid + 1
                else:
                    high = mid - 1

    # Using dictionary as a hash table
    def two_sum1(numbers, target):
        dic = {}
        for i, num in enumerate(numbers):
            if target - num in dic:
                return [dic[target - num] + 1, i + 1]
            dic[num] = i

    # Using two pointers
    def two_sum2(numbers, target):
        p1 = 0                      # pointer 1 holds from left of array numbers
        p2 = len(numbers) - 1       # pointer 2 holds from right of array numbers
        while p1 < p2:
            s = numbers[p1] + numbers[p2]
            if s == target:
                return [p1 + 1, p2 + 1]
            elif s > target:
                p2 = p2 - 1
            else:
                p1 = p1 + 1


