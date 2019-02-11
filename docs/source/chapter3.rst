chapter 3: Backtrack
====================================



3.1 add operators
------------------------------
Given a string that contains only digits 0-9 and a target value,
return all possibilities to add binary operators (not unary) +, -, or *
between the digits so they prevuate to the target value.

Examples:
"123", 6 -> ["1+2+3", "1*2*3"]
"232", 8 -> ["2*3+2", "2+3*2"]
"105", 5 -> ["1*0+5","10-5"]
"00", 0 -> ["0+0", "0-0", "0*0"]
"3456237490", 9191 -> []

.. code-block:: python


    def add_operators(num, target):
        """
        :type num: str
        :type target: int
        :rtype: List[str]
        """

        def dfs(res, path, num, target, pos, prev, multed):
            if pos == len(num):
                if target == prev:
                    res.append(path)
                return
            for i in range(pos, len(num)):
                if i != pos and num[pos] == '0':  # all digits have to be used
                    break
                cur = int(num[pos:i+1])
                if pos == 0:
                    dfs(res, path + str(cur), num, target, i+1, cur, cur)
                else:
                    dfs(res, path + "+" + str(cur), num, target,
                        i+1, prev + cur, cur)
                    dfs(res, path + "-" + str(cur), num, target,
                        i+1, prev - cur, -cur)
                    dfs(res, path + "*" + str(cur), num, target,
                        i+1, prev - multed + multed * cur, multed * cur)

        res = []
        if not num:
            return res
        dfs(res, "", num, target, 0, 0, 0)
        return res


3.2 anagram
------------------------------

.. code-block:: python

    def anagram(s1, s2):
        c1 = [0] * 26
        c2 = [0] * 26

        for c in s1:
            pos = ord(c)-ord('a')
            c1[pos] = c1[pos] + 1

        for c in s2:
            pos = ord(c)-ord('a')
            c2[pos] = c2[pos] + 1

        return c1 == c2



3.3 array sum combination
------------------------------
WAP to take one element from each of the array add it to the target sum.
Print all those three-element combinations.

/*
A = [1, 2, 3, 3]
B = [2, 3, 3, 4]
C = [2, 3, 3, 4]
target = 7
*/

Result:
[[1, 2, 4], [1, 3, 3], [1, 3, 3], [1, 3, 3], [1, 3, 3], [1, 4, 2],
 [2, 2, 3], [2, 2, 3], [2, 3, 2], [2, 3, 2], [3, 2, 2], [3, 2, 2]]


.. code-block:: python


    import itertools
    from functools import partial


    def array_sum_combinations(A, B, C, target):

        def over(constructed_sofar):
            sum = 0
            to_stop, reached_target = False, False
            for elem in constructed_sofar:
                sum += elem
            if sum >= target or len(constructed_sofar) >= 3:
                to_stop = True
                if sum == target and 3 == len(constructed_sofar):
                    reached_target = True
            return to_stop, reached_target

        def construct_candidates(constructed_sofar):
            array = A
            if 1 == len(constructed_sofar):
                array = B
            elif 2 == len(constructed_sofar):
                array = C
            return array

        def backtrack(constructed_sofar=[], res=[]):
            to_stop, reached_target = over(constructed_sofar)
            if to_stop:
                if reached_target:
                    res.append(constructed_sofar)
                return
            candidates = construct_candidates(constructed_sofar)

            for candidate in candidates:
                constructed_sofar.append(candidate)
                backtrack(constructed_sofar[:], res)
                constructed_sofar.pop()

        res = []
        backtrack([], res)
        return res


    def unique_array_sum_combinations(A, B, C, target):
        """
        1. Sort all the arrays - a,b,c. - This improves average time complexity.
        2. If c[i] < Sum, then look for Sum - c[i] in array a and b.
           When pair found, insert c[i], a[j] & b[k] into the result list.
           This can be done in O(n).
        3. Keep on doing the above procedure while going through complete c array.

        Complexity: O(n(m+p))
        """
        def check_sum(n, *nums):
            if sum(x for x in nums) == n:
                return (True, nums)
            else:
                return (False, nums)

        pro = itertools.product(A, B, C)
        func = partial(check_sum, target)
        sums = list(itertools.starmap(func, pro))

        res = set()
        for s in sums:
            if s[0] is True and s[1] not in res:
                res.add(s[1])

        return list(res)



3.4 combination sum
------------------------------
Given a set of candidate numbers (C) (without duplicates) and a target number
(T), find all unique combinations in C where the candidate numbers sums to T.

The same repeated number may be chosen from C unlimited number of times.

Note:
All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
For example, given candidate set [2, 3, 6, 7] and target 7,
A solution set is:
[
  [7],
  [2, 2, 3]
]



.. code-block:: python

    def combination_sum(candidates, target):

        def dfs(nums, target, index, path, res):
            if target < 0:
                return  # backtracking
            if target == 0:
                res.append(path)
                return
            for i in range(index, len(nums)):
                dfs(nums, target-nums[i], i, path+[nums[i]], res)

        res = []
        candidates.sort()
        dfs(candidates, target, 0, [], res)
        return res



3.5 factor combinations
------------------------------
Numbers can be regarded as product of its factors. For example,

8 = 2 x 2 x 2;
  = 2 x 4.
Write a function that takes an integer n
and return all possible combinations of its factors.

Note:
You may assume that n is always positive.
Factors should be greater than 1 and less than n.
Examples:
input: 1
output:
[]
input: 37
output:
[]
input: 12
output:
[
  [2, 6],
  [2, 2, 3],
  [3, 4]
]
input: 32
output:
[
  [2, 16],
  [2, 2, 8],
  [2, 2, 2, 4],
  [2, 2, 2, 2, 2],
  [2, 4, 4],
  [4, 8]
]


.. code-block:: python

    # Iterative:
    def get_factors(n):
        todo, combis = [(n, 2, [])], []
        while todo:
            n, i, combi = todo.pop()
            while i * i <= n:
                if n % i == 0:
                    combis.append(combi + [i, n//i])
                    todo.append((n//i, i, combi+[i]))
                i += 1
        return combis


    # Recursive:
    def recursive_get_factors(n):

        def factor(n, i, combi, combis):
            while i * i <= n:
                if n % i == 0:
                    combis.append(combi + [i, n//i]),
                    factor(n//i, i, combi+[i], combis)
                i += 1
            return combis

        return factor(n, 2, [], [])



3.6 find words
------------------------------
Given a matrix of words and a list of words to search,
return a list of words that exists in the board
This is Word Search II on LeetCode

board = [
         ['o','a','a','n'],
         ['e','t','a','e'],
         ['i','h','k','r'],
         ['i','f','l','v']
         ]

words = ["oath","pea","eat","rain"]


.. code-block:: python

    def find_words(board, words):

        def backtrack(board, i, j, trie, pre, used, result):
            '''
            backtrack tries to build each words from
            the board and return all words found

            @param: board, the passed in board of characters
            @param: i, the row index
            @param: j, the column index
            @param: trie, a trie of the passed in words
            @param: pre, a buffer of currently build string that differs
                    by recursion stack
            @param: used, a replica of the board except in booleans
                    to state whether a character has been used
            @param: result, the resulting set that contains all words found

            @return: list of words found
            '''

            if '#' in trie:
                result.add(pre)

            if i < 0 or i >= len(board) or j < 0 or j >= len(board[0]):
                return

            if not used[i][j] and board[i][j] in trie:
                used[i][j] = True
                backtrack(board, i+1, j, trie[board[i][j]],
                          pre+board[i][j], used, result)
                backtrack(board, i, j+1, trie[board[i][j]],
                          pre+board[i][j], used, result)
                backtrack(board, i-1, j, trie[board[i][j]],
                          pre+board[i][j], used, result)
                backtrack(board, i, j-1, trie[board[i][j]],
                          pre+board[i][j], used, result)
                used[i][j] = False

        # make a trie structure that is essentially dictionaries of dictionaries
        # that map each character to a potential next character
        trie = {}
        for word in words:
            curr_trie = trie
            for char in word:
                if char not in curr_trie:
                    curr_trie[char] = {}
                curr_trie = curr_trie[char]
            curr_trie['#'] = '#'

        # result is a set of found words since we do not want repeats
        result = set()
        used = [[False]*len(board[0]) for _ in range(len(board))]

        for i in range(len(board)):
            for j in range(len(board[0])):
                backtrack(board, i, j, trie, '', used, result)
        return list(result)


3.7 generate abbreviations
------------------------------
given input word, return the list of abbreviations.
ex)
word => [1ord, w1rd, wo1d, w2d, 3d, w3 ... etc]


.. code-block:: python

    def generate_abbreviations(word):

        def backtrack(result, word, pos, count, cur):
            if pos == len(word):
                if count > 0:
                    cur += str(count)
                result.append(cur)
                return

            if count > 0:  # add the current word
                backtrack(result, word, pos+1, 0, cur+str(count)+word[pos])
            else:
                backtrack(result, word, pos+1, 0, cur+word[pos])
            # skip the current word
            backtrack(result, word, pos+1, count+1, cur)

        result = []
        backtrack(result, word, 0, 0, "")
        return result


3.8 generate parenthesis
------------------------------
Given n pairs of parentheses, write a function to generate
all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]


.. code-block:: python

    def generate_parenthesis_v1(n):
        def add_pair(res, s, left, right):
            if left == 0 and right == 0:
                res.append(s)
                return
            if right > 0:
                add_pair(res, s + ")", left, right - 1)
            if left > 0:
                add_pair(res, s + "(", left - 1, right + 1)

        res = []
        add_pair(res, "", n, 0)
        return res


    def generate_parenthesis_v2(n):
        def add_pair(res, s, left, right):
            if left == 0 and right == 0:
                res.append(s)
            if left > 0:
                add_pair(res, s + "(", left - 1, right)
            if right > 0 and left < right:
                add_pair(res, s + ")", left, right - 1)

        res = []
        add_pair(res, "", n, n)
        return res



3.9 letter combination
------------------------------
Given a digit string, return all possible letter
combinations that the number could represent.

Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].


.. code-block:: python

    def letter_combinations(digits):
        if digits == "":
            return []
        kmaps = {
            "2": "abc",
            "3": "def",
            "4": "ghi",
            "5": "jkl",
            "6": "mno",
            "7": "pqrs",
            "8": "tuv",
            "9": "wxyz"
        }
        ans = [""]
        for num in digits:
            tmp = []
            for an in ans:
                for char in kmaps[num]:
                    tmp.append(an + char)
            ans = tmp
        return ans



3.10 palindrome partioning
------------------------------
It looks like you need to be looking not for all palindromic substrings,
but rather for all the ways you can divide the input string
up into palindromic substrings.
(There's always at least one way,
since one-character substrings are always palindromes.)



.. code-block:: python

    def palindromic_substrings(s):
        if not s:
            return [[]]
        results = []
        for i in range(len(s), 0, -1):
            sub = s[:i]
            if sub == sub[::-1]:
                for rest in palindromic_substrings(s[i:]):
                    results.append([sub] + rest)
        return results

There's two loops.
The outer loop checks each length of initial substring
(in descending length order) to see if it is a palindrome.
If so, it recurses on the rest of the string and loops over the returned
values, adding the initial substring to
each item before adding it to the results.


.. code-block:: python

    def palindromic_substrings_iter(s):
        """
        A slightly more Pythonic approach with a recursive generator
        """
        if not s:
            yield []
            return
        for i in range(len(s), 0, -1):
            sub = s[:i]
            if sub == sub[::-1]:
                for rest in palindromic_substrings_iter(s[i:]):
                    yield [sub] + rest



3.11 pattern match
------------------------------
Given a pattern and a string str,
find if str follows the same pattern.

Here follow means a full match, such that there is a bijection between
a letter in pattern and a non-empty substring in str.

Examples:
pattern = "abab", str = "redblueredblue" should return true.
pattern = "aaaa", str = "asdasdasdasd" should return true.
pattern = "aabb", str = "xyzabcxzyabc" should return false.
Notes:
You may assume both pattern and str contains only lowercase letters.


.. code-block:: python

    def pattern_match(pattern, string):
        """
        :type pattern: str
        :type string: str
        :rtype: bool
        """
        def backtrack(pattern, string, dic):

            if len(pattern) == 0 and len(string) > 0:
                return False

            if len(pattern) == len(string) == 0:
                return True

            for end in range(1, len(string)-len(pattern)+2):
                if pattern[0] not in dic and string[:end] not in dic.values():
                    dic[pattern[0]] = string[:end]
                    if backtrack(pattern[1:], string[end:], dic):
                        return True
                    del dic[pattern[0]]
                elif pattern[0] in dic and dic[pattern[0]] == string[:end]:
                    if backtrack(pattern[1:], string[end:], dic):
                        return True
            return False

        return backtrack(pattern, string, {})


3.12 Permute Unique
------------------------------
Given a collection of numbers that might contain duplicates,
return all possible unique permutations.

For example,
[1,1,2] have the following unique permutations:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]

.. code-block:: python

    def permute_unique(nums):
        perms = [[]]
        for n in nums:
            new_perms = []
            for l in perms:
                for i in range(len(l)+1):
                    new_perms.append(l[:i]+[n]+l[i:])
                    if i < len(l) and l[i] == n:
                        break  # handles duplication
            perms = new_perms
        return perms



3.13 Permute
------------------------------
Given a collection of distinct numbers, return all possible permutations.

For example,
[1,2,3] have the following permutations:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

.. code-block:: python

    def permute(elements):
        """
            returns a list with the permuations.
        """
        if len(elements) <= 1:
            return elements
        else:
            tmp = []
            for perm in permute(elements[1:]):
                for i in range(len(elements)):
                    tmp.append(perm[:i] + elements[0:1] + perm[i:])
            return tmp


    def permute_iter(elements):
        """
            iterator: returns a perumation by each call.
        """
        if len(elements) <= 1:
            yield elements
        else:
            for perm in permute_iter(elements[1:]):
                for i in range(len(elements)):
                    yield perm[:i] + elements[0:1] + perm[i:]


    # DFS Version
    def permute_recursive(nums):
        def dfs(res, nums, path):
            if not nums:
                res.append(path)
            for i in range(len(nums)):
                print(nums[:i]+nums[i+1:])
                dfs(res, nums[:i]+nums[i+1:], path+[nums[i]])

        res = []
        dfs(res, nums, [])
        return res


3.14 subsets unique
------------------------------
Given a collection of integers that might contain duplicates, nums,
return all possible subsets.

Note: The solution set must not contain duplicate subsets.

For example,
If nums = [1,2,2], a solution is:

[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]

.. code-block:: python

    def subsets_unique(nums):

        def backtrack(res, nums, stack, pos):
            if pos == len(nums):
                res.add(tuple(stack))
            else:
                # take
                stack.append(nums[pos])
                backtrack(res, nums, stack, pos+1)
                stack.pop()

                # don't take
                backtrack(res, nums, stack, pos+1)

        res = set()
        backtrack(res, nums, [], 0)
        return list(res)


3.15 subsets
------------------------------

.. code-block:: python


    Given a set of distinct integers, nums, return all possible subsets.

    Note: The solution set must not contain duplicate subsets.

    For example,
    If nums = [1,2,3], a solution is:

    [
      [3],
      [1],
      [2],
      [1,2,3],
      [1,3],
      [2,3],
      [1,2],
      []
    ]