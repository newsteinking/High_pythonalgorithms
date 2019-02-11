chapter 8: Dynamic Programming
====================================


8.1 buy sell stock
-----------------------------
Say you have an array for which the ith element
is the price of a given stock on day i.

If you were only permitted to complete at most one transaction
(ie, buy one and sell one share of the stock),
design an algorithm to find the maximum profit.

Example 1:
Input: [7, 1, 5, 3, 6, 4]
Output: 5

max. difference = 6-1 = 5
(not 7-1 = 6, as selling price needs to be larger than buying price)
Example 2:
Input: [7, 6, 4, 3, 1]
Output: 0

In this case, no transaction is done, i.e. max profit = 0.


.. code-block:: python

    # O(n^2) time
    def max_profit_naive(prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        max_so_far = 0
        for i in range(0, len(prices) - 1):
            for j in range(i + 1, len(prices)):
                max_so_far = max(max_so_far, prices[j] - prices[i])
        return max_so_far


    # O(n) time
    def max_profit_optimized(prices):
        """
        input: [7, 1, 5, 3, 6, 4]
        diff : [X, -6, 4, -2, 3, -2]
        :type prices: List[int]
        :rtype: int
        """
        cur_max, max_so_far = 0, 0
        for i in range(1, len(prices)):
            cur_max = max(0, cur_max + prices[i] - prices[i-1])
            max_so_far = max(max_so_far, cur_max)
        return max_so_far




8.2 climbing stairs
-----------------------------
You are climbing a stair case.
It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps.
In how many distinct ways can you climb to the top?

Note: Given n will be a positive integer.


.. code-block:: python

    # O(n) space

    def climb_stairs(n):
        """
        :type n: int
        :rtype: int
        """
        arr = [1, 1]
        for i in range(2, n+1):
            arr.append(arr[-1] + arr[-2])
        return arr[-1]


    # the above function can be optimized as:
    # O(1) space

    def climb_stairs_optimized(n):
        a = b = 1
        for _ in range(n):
            a, b = b, a + b
        return a




8.3 coin change
-----------------------------
Problem
Given a value N, if we want to make change for N cents, and we have infinite supply of each of
S = { S1, S2, .. , Sm} valued //coins, how many ways can we make the change?
The order of coins doesn’t matter.
For example, for N = 4 and S = [1, 2, 3], there are four solutions:
[1, 1, 1, 1], [1, 1, 2], [2, 2], [1, 3].
So output should be 4.

For N = 10 and S = [2, 5, 3, 6], there are five solutions:
[2, 2, 2, 2, 2], [2, 2, 3, 3], [2, 2, 6], [2, 3, 5] and [5, 5].
So the output should be 5.

.. code-block:: python

    def count(s, n):
        # We need n+1 rows as the table is consturcted in bottom up
        # manner using the base case 0 value case (n = 0)
        m = len(s)
        table = [[0 for x in range(m)] for x in range(n+1)]

        # Fill the enteries for 0 value case (n = 0)
        for i in range(m):
            table[0][i] = 1

        # Fill rest of the table enteries in bottom up manner
        for i in range(1, n+1):
            for j in range(m):
                # Count of solutions including S[j]
                x = table[i - s[j]][j] if i-s[j] >= 0 else 0

                # Count of solutions excluding S[j]
                y = table[i][j-1] if j >= 1 else 0

                # total count
                table[i][j] = x + y

        return table[n][m-1]


    if __name__ == '__main__':

        coins = [1, 2, 3]
        n = 4
        assert count(coins, n) == 4

        coins = [2, 5, 3, 6]
        n = 10
        assert count(coins, n) == 5




8.4 combination sum
-----------------------------
Given an integer array with all positive numbers and no duplicates,
find the number of possible combinations that
add up to a positive integer target.

Example:

nums = [1, 2, 3]
target = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is 7.
Follow up:
What if negative numbers are allowed in the given array?
How does it change the problem?
What limitation we need to add to the question to allow negative numbers?


.. code-block:: python

    dp = None


    def helper_topdown(nums, target):
        global dp
        if dp[target] != -1:
            return dp[target]
        res = 0
        for i in range(0, len(nums)):
            if target >= nums[i]:
                res += helper_topdown(nums, target - nums[i])
        dp[target] = res
        return res


    def combination_sum_topdown(nums, target):
        global dp
        dp = [-1] * (target + 1)
        dp[0] = 1
        return helper_topdown(nums, target)


    # EDIT: The above solution is top-down. How about a bottom-up one?
    def combination_sum_bottom_up(nums, target):
        comb = [0] * (target + 1)
        comb[0] = 1
        for i in range(0, len(comb)):
            for j in range(len(nums)):
                if i - nums[j] >= 0:
                    comb[i] += comb[i - nums[j]]
        return comb[target]


    combination_sum_topdown([1, 2, 3], 4)
    print(dp[4])

    print(combination_sum_bottom_up([1, 2, 3], 4))


8.5 edit distance
-----------------------------
The edit distance between two words is the minimum number of letter insertions,
letter deletions, and letter substitutions required to transform one word into another.

For example, the edit distance between FOOD and MONEY is at most four:

FOOD -> MOOD -> MOND -> MONED -> MONEY

Given two words A and B, find the minimum number of operations required to transform one string into the other.
In other words, find the edit distance between A and B.

Thought process:

Let edit(i, j) denote the edit distance between the prefixes A[1..i] and B[1..j].

Then, the function satifies the following recurrence:

edit(i, j) = i if j = 0
             j if i = 0
             min(edit(i-1, j) + 1,
                 edit(i, j-1), + 1,
                 edit(i-1, j-1) + cost) otherwise

There are two base cases, both of which occur when one string is empty and the other is not.
1. To convert an empty string A into a string B of length n, perform n insertions.
2. To convert a string A of length m into an empty string B, perform m deletions.

Here, the cost is 1 if a substitution is required,
or 0 if both chars in words A and B are the same at indexes i and j, respectively.

To find the edit distance between two words A and B,
we need to find edit(m, n), where m is the length of A and n is the length of B.

.. code-block:: python

    def edit_distance(A, B):
        # Time: O(m*n)
        # Space: O(m*n)

        m, n = len(A) + 1, len(B) + 1

        edit = [[0 for _ in range(n)] for _ in range(m)]

        for i in range(1, m):
            edit[i][0] = i

        for j in range(1, n):
            edit[0][j] = j

        for i in range(1, m):
            for j in range(1, n):
                cost = 0 if A[i - 1] == B[j - 1] else 1
                edit[i][j] = min(edit[i - 1][j] + 1, edit[i][j - 1] + 1, edit[i - 1][j - 1] + cost)

        return edit[-1][-1]  # this is the same as edit[m][n]



8.6 egg drop
-----------------------------
# A Dynamic Programming based Python Program for the Egg Dropping Puzzle
INT_MAX = 32767

# Function to get minimum number of trials needed in worst
# case with n eggs and k floors


.. code-block:: python

    def egg_drop(n, k):
        # A 2D table where entery eggFloor[i][j] will represent minimum
        # number of trials needed for i eggs and j floors.
        egg_floor = [[0 for x in range(k+1)] for x in range(n+1)]

        # We need one trial for one floor and0 trials for 0 floors
        for i in range(1, n+1):
            egg_floor[i][1] = 1
            egg_floor[i][0] = 0

        # We always need j trials for one egg and j floors.
        for j in range(1, k+1):
            egg_floor[1][j] = j

        # Fill rest of the entries in table using optimal substructure
        # property
        for i in range(2, n+1):
            for j in range(2, k+1):
                egg_floor[i][j] = INT_MAX
                for x in range(1, j+1):
                    res = 1 + max(egg_floor[i-1][x-1], egg_floor[i][j-x])
                    if res < egg_floor[i][j]:
                        egg_floor[i][j] = res

        # eggFloor[n][k] holds the result
        return egg_floor[n][k]



8.7 fib
-----------------------------

.. code-block:: python

    def fib_recursive(n):
        """[summary]
        Computes the n-th fibonacci number recursive.
        Problem: This implementation is very slow.
        approximate O(2^n)

        Arguments:
            n {[int]} -- [description]

        Returns:
            [int] -- [description]
        """

        # precondition
        assert n >= 0, 'n must be a positive integer'

        if n <= 1:
            return n
        else:
            return fib_recursive(n-1) + fib_recursive(n-2)

    # print(fib_recursive(35)) # => 9227465 (slow)

    def fib_list(n):
        """[summary]
        This algorithm computes the n-th fibbonacci number
        very quick. approximate O(n)
        The algorithm use dynamic programming.

        Arguments:
            n {[int]} -- [description]

        Returns:
            [int] -- [description]
        """

        # precondition
        assert n >= 0, 'n must be a positive integer'

        list_results = [0, 1]
        for i in range(2, n+1):
            list_results.append(list_results[i-1] + list_results[i-2])
        return list_results[n]

    # print(fib_list(100)) # => 354224848179261915075

    def fib_iter(n):
        """[summary]
        Works iterative approximate O(n)

        Arguments:
            n {[int]} -- [description]

        Returns:
            [int] -- [description]
        """

        # precondition
        assert n >= 0, 'n must be positive integer'

        fib_1 = 0
        fib_2 = 1
        sum = 0
        if n <= 1:
            return n
        for i in range(n-1):
            sum = fib_1 + fib_2
            fib_1 = fib_2
            fib_2 = sum
        return sum

    # => 354224848179261915075
    # print(fib_iter(100))




8.8 hosoya triangle
-----------------------------
Hosoya triangle (originally Fibonacci triangle) is a triangular arrangement
of numbers, where if you take any number it is the sum of 2 numbers above.
First line is always 1, and second line is always {1     1}.

This printHosoya function takes argument n which is the height of the triangle
(number of lines).

For example:
printHosoya( 6 ) would return:
1
1 1
2 1 2
3 2 2 3
5 3 4 3 5
8 5 6 6 5 8

The complexity is O(n^3).


.. code-block:: python

    def hosoya(n, m):
        if ((n == 0 and m == 0) or (n == 1 and m == 0) or
            (n == 1 and m == 1) or (n == 2 and m == 1)):
            return 1
        if n > m:
            return hosoya(n - 1, m) + hosoya(n - 2, m)
        elif m == n:
            return hosoya(n - 1, m - 1) + hosoya(n - 2, m - 2)
        else:
            return 0

    def print_hosoya(n):
        for i in range(n):
            for j in range(i + 1):
                print(hosoya(i, j) , end = " ")
            print ("\n", end = "")

    def hosoya_testing(n):
        x = []
        for i in range(n):
            for j in range(i + 1):
                x.append(hosoya(i, j))
        return x




8.9 house robber
-----------------------------
You are a professional robber planning to rob houses along a street.
Each house has a certain amount of money stashed,
the only constraint stopping you from robbing each of them
is that adjacent houses have security system connected and
it will automatically contact the police if two adjacent houses
were broken into on the same night.

Given a list of non-negative integers representing the amount of money
of each house, determine the maximum amount of money you
can rob tonight without alerting the police.


.. code-block:: python

    def house_robber(houses):
        last, now = 0, 0
        for house in houses:
            tmp = now
            now = max(last + house, now)
            last = tmp
        return now

    houses = [1, 2, 16, 3, 15, 3, 12, 1]

    print(house_robber(houses))



8.10 job scheduling
-----------------------------

.. code-block:: python

    # Python program for weighted job scheduling using Dynamic
    # Programming and Binary Search

    # Class to represent a job
    class Job:
        def __init__(self, start, finish, profit):
            self.start  = start
            self.finish = finish
            self.profit  = profit


    # A Binary Search based function to find the latest job
    # (before current job) that doesn't conflict with current
    # job.  "index" is index of the current job.  This function
    # returns -1 if all jobs before index conflict with it.
    # The array jobs[] is sorted in increasing order of finish
    # time.
    def binary_search(job, start_index):

        # Initialize 'lo' and 'hi' for Binary Search
        lo = 0
        hi = start_index - 1

        # Perform binary Search iteratively
        while lo <= hi:
            mid = (lo + hi) // 2
            if job[mid].finish <= job[start_index].start:
                if job[mid + 1].finish <= job[start_index].start:
                    lo = mid + 1
                else:
                    return mid
            else:
                hi = mid - 1
        return -1

    # The main function that returns the maximum possible
    # profit from given array of jobs
    def schedule(job):

        # Sort jobs according to finish time
        job = sorted(job, key = lambda j: j.finish)

        # Create an array to store solutions of subproblems.  table[i]
        # stores the profit for jobs till arr[i] (including arr[i])
        n = len(job)
        table = [0 for _ in range(n)]

        table[0] = job[0].profit

        # Fill entries in table[] using recursive property
        for i in range(1, n):

            # Find profit including the current job
            incl_prof = job[i].profit
            l = binary_search(job, i)
            if (l != -1):
                incl_prof += table[l]

            # Store maximum of including and excluding
            table[i] = max(incl_prof, table[i - 1])

        return table[n-1]



8.11 knapsack
-----------------------------
Given the capacity of the knapsack and items specified by weights and values,
return the maximum summarized value of the items that can be fit in the
knapsack.

Example:
capacity = 5, items(value, weight) = [(60, 5), (50, 3), (70, 4), (30, 2)]
result = 80 (items valued 50 and 30 can both be fit in the knapsack)

The time complexity is O(n * m) and the space complexity is O(m), where n is
the total number of items and m is the knapsack's capacity.


.. code-block:: python

    class Item(object):

        def __init__(self, value, weight):
            self.value = value
            self.weight = weight


    def get_maximum_value(items, capacity):
        dp = [0] * (capacity + 1)
        for item in items:
            dp_tmp = [total_value for total_value in dp]
            for current_weight in range(capacity + 1):
                total_weight = current_weight + item.weight
                if total_weight <= capacity:
                    dp_tmp[total_weight] = max(dp_tmp[total_weight],
                                               dp[current_weight] + item.value)
            dp = dp_tmp
        return max(dp)


    print(get_maximum_value([Item(60, 10), Item(100, 20), Item(120, 30)],
                            50))
    print(get_maximum_value([Item(60, 5), Item(50, 3), Item(70, 4), Item(30, 2)],
                            5))



8.12 longest increasing
-----------------------------

.. code-block:: python

    def longest_increasing_subsequence(sequence):
        """
        Dynamic Programming Algorithm for
        counting the length of longest increasing subsequence
        type sequence: List[int]
        """
        length = len(sequence)
        counts = [1 for _ in range(length)]
        for i in range(1, length):
            for j in range(0, i):
                if sequence[i] > sequence[j]:
                    counts[i] = max(counts[i], counts[j] + 1)
                    print(counts)
        return max(counts)


    sequence = [1, 101, 10, 2, 3, 100, 4, 6, 2]
    print("sequence: ", sequence)
    print("output: ", longest_increasing_subsequence(sequence))
    print("answer: ", 5)





8.13 matrix chain order
-----------------------------
Dynamic Programming
Implementation of matrix Chain Multiplication
Time Complexity: O(n^3)
Space Complexity: O(n^2)


.. code-block:: python

    INF = float("inf")

    def matrix_chain_order(array):
        n=len(array)
        matrix = [[0 for x in range(n)] for x in range(n)]
        sol = [[0 for x in range(n)] for x in range(n)]
        for chain_length in range(2,n):
            for a in range(1,n-chain_length+1):
                b = a+chain_length-1

                matrix[a][b] = INF
                for c in range(a, b):
                    cost = matrix[a][c] + matrix[c+1][b] + array[a-1]*array[c]*array[b]
                    if cost < matrix[a][b]:
                        matrix[a][b] = cost
                        sol[a][b] = c
        return matrix , sol
    #Print order of matrix with Ai as matrix

    def print_optimal_solution(optimal_solution,i,j):
        if i==j:
            print("A" + str(i),end = " ")
        else:
            print("(",end = " ")
            print_optimal_solution(optimal_solution,i,optimal_solution[i][j])
            print_optimal_solution(optimal_solution,optimal_solution[i][j]+1,j)
            print(")",end = " ")

    def main():
        array=[30,35,15,5,10,20,25]
        n=len(array)
        #Size of matrix created from above array will be
        # 30*35 35*15 15*5 5*10 10*20 20*25
        matrix , optimal_solution = matrix_chain_order(array)

        print("No. of Operation required: "+str((matrix[1][n-1])))
        print_optimal_solution(optimal_solution,1,n-1)
    if __name__ == '__main__':
        main()




8.14 max product subarray
-----------------------------
Find the contiguous subarray within an array
(containing at least one number) which has the largest product.

For example, given the array [2,3,-2,4],
the contiguous subarray [2,3] has the largest product = 6.


.. code-block:: python

    from functools import reduce


    def max_product(nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        lmin = lmax = gmax = nums[0]
        for i in range(len(nums)):
            t1 = nums[i] * lmax
            t2 = nums[i] * lmin
            lmax = max(max(t1, t2), nums[i])
            lmin = min(min(t1, t2), nums[i])
            gmax = max(gmax, lmax)


Another approach that would print max product and the subarray

Examples:
subarray_with_max_product([2,3,6,-1,-1,9,5])
    #=> max_product_so_far: 45, [-1, -1, 9, 5]
subarray_with_max_product([-2,-3,6,0,-7,-5])
    #=> max_product_so_far: 36, [-2, -3, 6]
subarray_with_max_product([-4,-3,-2,-1])
    #=> max_product_so_far: 24, [-4, -3, -2, -1]
subarray_with_max_product([-3,0,1])
    #=> max_product_so_far: 1, [1]

.. code-block:: python


    def subarray_with_max_product(arr):
        ''' arr is list of positive/negative numbers '''
        l = len(arr)
        product_so_far = max_product_end = 1
        max_start_i = 0
        so_far_start_i = so_far_end_i = 0
        all_negative_flag = True

        for i in range(l):
            max_product_end *= arr[i]
            if arr[i] > 0:
                all_negative_flag = False

            if max_product_end <= 0:
                max_product_end = arr[i]
                max_start_i = i

            if product_so_far <= max_product_end:
                product_so_far = max_product_end
                so_far_end_i = i
                so_far_start_i = max_start_i

        if all_negative_flag:
            print("max_product_so_far: %s, %s" %
                  (reduce(lambda x, y: x * y, arr), arr))
        else:
            print("max_product_so_far: %s, %s" %
                  (product_so_far, arr[so_far_start_i:so_far_end_i + 1]))




8.15 max subarray
-----------------------------

.. code-block:: python

    def max_subarray(array):
        max_so_far = max_now = array[0]
        for i in range(1, len(array)):
            max_now = max(array[i], max_now + array[i])
            max_so_far = max(max_so_far, max_now)
        return max_so_far

    a = [1, 2, -3, 4, 5, -7, 23]
    print(a)
    print(max_subarray(a))


8.16 min cost path
-----------------------------
author @goswami-rahul

To find minimum cost path
from station 0 to station N-1,
where cost of moving from ith station to jth station is given as:

Matrix of size (N x N)
where Matrix[i][j] denotes the cost of moving from
station i --> station j   for i < j

NOTE that values where Matrix[i][j] and i > j does not
mean anything, and hence represented by -1 or INF

For the input below (cost matrix),
Minimum cost is obtained as from  { 0 --> 1 --> 3}
                                  = cost[0][1] + cost[1][3] = 65
the Output will be:

The Minimum cost to reach station 4 is 65

Time Complexity: O(n^2)
Space Complexity: O(n)


.. code-block:: python

    INF = float("inf")

    def min_cost(cost):

        n = len(cost)
        # dist[i] stores minimum cost from 0 --> i.
        dist = [INF] * n

        dist[0] = 0   # cost from 0 --> 0 is zero.

        for i in range(n):
            for j in range(i+1,n):
                dist[j] = min(dist[j], dist[i] + cost[i][j])

        return dist[n-1]

    if __name__ == '__main__':

        cost = [ [ 0, 15, 80, 90],         # cost[i][j] is the cost of
                 [-1,  0, 40, 50],         # going from i --> j
                 [-1, -1,  0, 70],
                 [-1, -1, -1,  0] ]        # cost[i][j] = -1 for i > j
        total_len = len(cost)

        mcost = min_cost(cost)
        assert mcost == 65

        print("The Minimum cost to reach station %d is %d" % (total_len, mcost))




8.17 num decodings
-----------------------------
A message containing letters from A-Z is being
encoded to numbers using the following mapping:

'A' -> 1
'B' -> 2
...
'Z' -> 26
Given an encoded message containing digits,
determine the total number of ways to decode it.

For example,
Given encoded message "12",
it could be decoded as "AB" (1 2) or "L" (12).

The number of ways decoding "12" is 2.


.. code-block:: python

    def num_decodings(s):
        """
        :type s: str
        :rtype: int
        """
        if not s or s[0] == "0":
            return 0
        wo_last, wo_last_two = 1, 1
        for i in range(1, len(s)):
            x = wo_last if s[i] != "0" else 0
            y = wo_last_two if int(s[i-1:i+1]) < 27 and s[i-1] != "0" else 0
            wo_last_two = wo_last
            wo_last = x+y
        return wo_last


    def num_decodings2(s):
        if not s or s.startswith('0'):
            return 0
        stack = [1, 1]
        for i in range(1, len(s)):
            if s[i] == '0':
                if s[i-1] == '0' or s[i-1] > '2':
                    # only '10', '20' is valid
                    return 0
                stack.append(stack[-2])
            elif 9 < int(s[i-1:i+1]) < 27:
                # '01 - 09' is not allowed
                stack.append(stack[-2]+stack[-1])
            else:
                # other case '01, 09, 27'
                stack.append(stack[-1])
        return stack[-1]



8.18 regex matching
-----------------------------
Implement regular expression matching with support for '.' and '*'.

'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true


.. code-block:: python

    import unittest

    class Solution(object):
        def is_match(self, s, p):
            m, n = len(s) + 1, len(p) + 1
            matches = [[False] * n  for _ in range(m)]

            # Match empty string with empty pattern
            matches[0][0] = True

            # Match empty string with .*
            for i, element in enumerate(p[1:], 2):
                matches[0][i] = matches[0][i - 2] and element == '*'

            for i, ss in enumerate(s, 1):
                for j, pp in enumerate(p, 1):
                    if pp != '*':
                        # The previous character has matched and the current one
                        # has to be matched. Two possible matches: the same or .
                        matches[i][j] = matches[i - 1][j - 1] and \
                                        (ss == pp or pp == '.')
                    else:
                        # Horizontal look up [j - 2].
                        # Not use the character before *.
                        matches[i][j] |= matches[i][j - 2]

                        # Vertical look up [i - 1].
                        # Use at least one character before *.
                        #   p a b *
                        # s 1 0 0 0
                        # a 0 1 0 1
                        # b 0 0 1 1
                        # b 0 0 0 ?
                        if ss == p[j - 2] or p[j - 2] == '.':
                            matches[i][j] |= matches[i - 1][j]

            return matches[-1][-1]

    class TestSolution(unittest.TestCase):
        def test_none_0(self):
            s = ""
            p = ""
            self.assertTrue(Solution().isMatch(s, p))

        def test_none_1(self):
            s = ""
            p = "a"
            self.assertFalse(Solution().isMatch(s, p))

        def test_no_symbol_equal(self):
            s = "abcd"
            p = "abcd"
            self.assertTrue(Solution().isMatch(s, p))

        def test_no_symbol_not_equal_0(self):
            s = "abcd"
            p = "efgh"
            self.assertFalse(Solution().isMatch(s, p))

        def test_no_symbol_not_equal_1(self):
            s = "ab"
            p = "abb"
            self.assertFalse(Solution().isMatch(s, p))

        def test_symbol_0(self):
            s = ""
            p = "a*"
            self.assertTrue(Solution().isMatch(s, p))

        def test_symbol_1(self):
            s = "a"
            p = "ab*"
            self.assertTrue(Solution().isMatch(s, p))

        def test_symbol_2(self):
            # E.g.
            #   s a b b
            # p 1 0 0 0
            # a 0 1 0 0
            # b 0 0 1 0
            # * 0 1 1 1
            s = "abb"
            p = "ab*"
            self.assertTrue(Solution().isMatch(s, p))


    if __name__ == "__main__":
        unittest.main()




8.19 rod cut
-----------------------------
# A Dynamic Programming solution for Rod cutting problem


.. code-block:: python

    INT_MIN = -32767

    # Returns the best obtainable price for a rod of length n and
    # price[] as prices of different pieces
    def cut_rod(price):
        n = len(price)
        val = [0]*(n+1)

        # Build the table val[] in bottom up manner and return
        # the last entry from the table
        for i in range(1, n+1):
            max_val = INT_MIN
            for j in range(i):
                 max_val = max(max_val, price[j] + val[i-j-1])
            val[i] = max_val

        return val[n]

    # Driver program to test above functions
    arr = [1, 5, 8, 9, 10, 17, 17, 20]
    print("Maximum Obtainable Value is " + str(cut_rod(arr)))

    # This code is contributed by Bhavya Jain




8.20 word break
-----------------------------
Given a non-empty string s and a dictionary wordDict
containing a list of non-empty words,
determine if s can be segmented into a space-separated
sequence of one or more dictionary words.
You may assume the dictionary does not contain duplicate words.

For example, given
s = "leetcode",
dict = ["leet", "code"].

Return true because "leetcode" can be segmented as "leet code".

s = abc word_dict = ["a","bc"]
True False False False


.. code-block:: python


    # TC: O(N^2)  SC: O(N)
    def word_break(s, word_dict):
        """
        :type s: str
        :type word_dict: Set[str]
        :rtype: bool
        """
        dp = [False] * (len(s)+1)
        dp[0] = True
        for i in range(1, len(s)+1):
            for j in range(0, i):
                if dp[j] and s[j:i] in word_dict:
                    dp[i] = True
                    break
        return dp[-1]


    if __name__ == "__main__":
        s = "keonkim"
        dic = ["keon", "kim"]

        print(word_break(s, dic))
