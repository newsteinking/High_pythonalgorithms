chapter 20: Strings
========================================



20.1 add binary
-----------------------------
Given two binary strings,
return their sum (also a binary string).

For example,
a = "11"
b = "1"
Return "100".

.. code-block:: python

    def add_binary(a, b):
        s = ""
        c, i, j = 0, len(a)-1, len(b)-1
        zero = ord('0')
        while (i >= 0 or j >= 0 or c == 1):
            if (i >= 0):
                c += ord(a[i]) - zero
                i -= 1
            if (j >= 0):
                c += ord(b[j]) - zero
                j -= 1
            s = chr(c % 2 + zero) + s
            c //= 2

        return s


20.2 atbash cipher
-----------------------------
Atbash cipher is mapping the alphabet to it's reverse.
So if we take "a" as it is the first letter, we change it to the last - z.

Example:
Attack at dawn --> Zggzxp zg wzdm

Complexity: O(n)


.. code-block:: python

    def atbash(s):
        translated = ""
        for i in range(len(s)):
            n = ord(s[i])

            if s[i].isalpha():

                if s[i].isupper():
                    x = n - ord('A')
                    translated += chr(ord('Z') - x)

                if s[i].islower():
                    x = n - ord('a')
                    translated += chr(ord('z') - x)
            else:
                translated += s[i]
        return translated


20.3 breaking bad
-----------------------------
Given an api which returns an array of words and an array of symbols, display
the word with their matched symbol surrounded by square brackets.

If the word string matches more than one symbol, then choose the one with
longest length. (ex. 'Microsoft' matches 'i' and 'cro'):

Example:
Words array: ['Amazon', 'Microsoft', 'Google']
Symbols: ['i', 'Am', 'cro', 'Na', 'le', 'abc']

Output:
[Am]azon, Mi[cro]soft, Goog[le]

My solution(Wrong):
(I sorted the symbols array in descending order of length and ran loop over
words array to find a symbol match(using indexOf in javascript) which
worked. But I didn't make it through the interview, I am guessing my solution
was O(n^2) and they expected an efficient algorithm.

output:
['[Am]azon', 'Mi[cro]soft', 'Goog[le]', 'Amaz[o]n', 'Micr[o]s[o]ft', 'G[o][o]gle']


.. code-block:: python

    from functools import reduce


    def match_symbol(words, symbols):
        import re
        combined = []
        for s in symbols:
            for c in words:
                r = re.search(s, c)
                if r:
                    combined.append(re.sub(s, "[{}]".format(s), c))
        return combined

    def match_symbol_1(words, symbols):
        res = []
        # reversely sort the symbols according to their lengths.
        symbols = sorted(symbols, key=lambda _: len(_), reverse=True)
        for word in words:
            for symbol in symbols:
                word_replaced = ''
                # once match, append the `word_replaced` to res, process next word
                if word.find(symbol) != -1:
                    word_replaced = word.replace(symbol, '[' + symbol + ']')
                    res.append(word_replaced)
                    break
            # if this word matches no symbol, append it.
            if word_replaced == '':
                res.append(word)
        return res

    """
    Another approach is to use a Tree for the dictionary (the symbols), and then
    match brute force. The complexity will depend on the dictionary;
    if all are suffixes of the other, it will be n*m
    (where m is the size of the dictionary). For example, in Python:
    """


    class TreeNode:
        def __init__(self):
            self.c = dict()
            self.sym = None


    def bracket(words, symbols):
        root = TreeNode()
        for s in symbols:
            t = root
            for char in s:
                if char not in t.c:
                    t.c[char] = TreeNode()
                t = t.c[char]
            t.sym = s
        result = dict()
        for word in words:
            i = 0
            symlist = list()
            while i < len(word):
                j, t = i, root
                while j < len(word) and word[j] in t.c:
                    t = t.c[word[j]]
                    if t.sym is not None:
                        symlist.append((j + 1 - len(t.sym), j + 1, t.sym))
                    j += 1
                i += 1
            if len(symlist) > 0:
                sym = reduce(lambda x, y: x if x[1] - x[0] >= y[1] - y[0] else y,
                             symlist)
                result[word] = "{}[{}]{}".format(word[:sym[0]], sym[2],
                                                 word[sym[1]:])
        return tuple(word if word not in result else result[word] for word in words)


20.4 caesar cipher
-----------------------------
Julius Caesar protected his confidential information by encrypting it using a cipher.
Caesar's cipher shifts each letter by a number of letters. If the shift takes you
past the end of the alphabet, just rotate back to the front of the alphabet.
In the case of a rotation by 3, w, x, y and z would map to z, a, b and c.
Original alphabet:      abcdefghijklmnopqrstuvwxyz
Alphabet rotated +3:    defghijklmnopqrstuvwxyzabc



.. code-block:: python

    def caesar_cipher(s, k):
        result = ""
        for char in s:
            n = ord(char)
            if 64 < n < 91:
                n = ((n - 65 + k) % 26) + 65
            if 96 < n < 123:
                n = ((n - 97 + k) % 26) + 97
            result = result + chr(n)
        return result



20.5 contain string
-----------------------------
Implement strStr().

Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

Example 1:
Input: haystack = "hello", needle = "ll"
Output: 2

Example 2:
Input: haystack = "aaaaa", needle = "bba"
Output: -1
Reference: https://leetcode.com/problems/implement-strstr/description/


.. code-block:: python

    def contain_string(haystack, needle):
        if len(needle) == 0:
            return 0
        if len(needle) > len(haystack):
            return -1
        for i in range(len(haystack)):
            if len(haystack) - i < len(needle):
                return -1
            if haystack[i:i+len(needle)] == needle:
                return i
        return -1



20.6 count binary substring
-----------------------------
Give a string s, count the number of non-empty (contiguous) substrings that have
 the same number of 0's and 1's, and all the 0's and all the 1's in these substrings are grouped consecutively.

Substrings that occur multiple times are counted the number of times they occur.
Example 1:
Input: "00110011"
Output: 6
Explanation: There are 6 substrings that have equal number of consecutive 1's and 0's: "0011", "01", "1100", "10", "0011", and "01".

Notice that some of these substrings repeat and are counted the number of times they occur.

Also, "00110011" is not a valid substring because all the 0's (and 1's) are not grouped together.

Example 2:
Input: "10101"
Output: 4
Explanation: There are 4 substrings: "10", "01", "10", "01" that have equal number of consecutive 1's and 0's.
Reference: https://leetcode.com/problems/count-binary-substrings/description/


.. code-block:: python

    def count_binary_substring(s):
        cur = 1
        pre = 0
        count = 0
        for i in range(1, len(s)):
            if s[i] != s[i - 1]:
                count = count + min(pre, cur)
                pre = cur
                cur = 1
            else:
                cur = cur + 1
        count = count + min(pre, cur)
        return count



20.7 decode string
-----------------------------
# Given an encoded string, return it's decoded string.

# The encoding rule is: k[encoded_string], where the encoded_string
# inside the square brackets is being repeated exactly k times.
# Note that k is guaranteed to be a positive integer.

# You may assume that the input string is always valid; No extra white spaces,
# square brackets are well-formed, etc.

# Furthermore, you may assume that the original data does not contain any
# digits and that digits are only for those repeat numbers, k.
# For example, there won't be input like 3a or 2[4].

# Examples:

# s = "3[a]2[bc]", return "aaabcbc".
# s = "3[a2[c]]", return "accaccacc".
# s = "2[abc]3[cd]ef", return "abcabccdcdcdef".


.. code-block:: python

    def decode_string(s):
        """
        :type s: str
        :rtype: str
        """
        stack = []; cur_num = 0; cur_string = ''
        for c in s:
            if c == '[':
                stack.append((cur_string, cur_num))
                cur_string = ''
                cur_num = 0
            elif c == ']':
                prev_string, num = stack.pop()
                cur_string = prev_string + num * cur_string
            elif c.isdigit():
                cur_num = cur_num*10 + int(c)
            else:
                cur_string += c
        return cur_string



20.8 delete reocurring
-----------------------------
QUESTION: Given a string as your input, delete any reoccurring
character, and return the new string.

This is a Google warmup interview question that was asked duirng phone screening
at my university.


.. code-block:: python

    # time complexity O(n)
    def delete_reoccurring_characters(string):
        seen_characters = set()
        output_string = ''
        for char in string:
            if char not in seen_characters:
                seen_characters.add(char)
                output_string += char
        return output_string


20.9 domain extractor
-----------------------------
Write a function that when given a URL as a string, parses out just the domain name and returns it as a string.

Examples:
domain_name("http://github.com/SaadBenn") == "github"
domain_name("http://www.zombie-bites.com") == "zombie-bites"
domain_name("https://www.cnet.com") == "cnet"

Note: The idea is not to use any built-in libraries such as re (regular expression) or urlparse except .split()
built-in function



.. code-block:: python

    # Non pythonic way
    def domain_name_1(url):
        #grab only the non http(s) part
        full_domain_name = url.split('//')[-1]
        #grab the actual one depending on the len of the list
        actual_domain = full_domain_name.split('.')

        # case when www is in the url
        if (len(actual_domain) > 2):
            return actual_domain[1]
        # case when www is not in the url
        return actual_domain[0]


    # pythonic one liner
    def domain_name_2(url):
        return url.split("//")[-1].split("www.")[-1].split(".")[0]



20.10 encode decode
-----------------------------
esign an algorithm to encode a list of strings to a string.
The encoded mystring is then sent over the network and is decoded
back to the original list of strings.


.. code-block:: python

    # Implement the encode and decode methods.

    def encode(strs):
        """Encodes a list of strings to a single string.
        :type strs: List[str]
        :rtype: str
        """
        res = ''
        for string in strs.split():
            res += str(len(string)) + ":" + string
        return res

    def decode(s):
        """Decodes a single string to a list of strings.
        :type s: str
        :rtype: List[str]
        """
        strs = []
        i = 0
        while i < len(s):
            index = s.find(":", i)
            size = int(s[i:index])
            strs.append(s[index+1: index+1+size])
            i = index+1+size
        return strs


20.11 first unique char
-----------------------------
Given a string, find the first non-repeating character in it and return it's
index. If it doesn't exist, return -1.

For example:
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.

Reference: https://leetcode.com/problems/first-unique-character-in-a-string/description/


.. code-block:: python

    def first_unique_char(s):
        """
        :type s: str
        :rtype: int
        """
        if (len(s) == 1):
            return 0
        ban = []
        for i in range(len(s)):
            if all(s[i] != s[k] for k in range(i + 1, len(s))) == True and s[i] not in ban:
                return i
            else:
                ban.append(s[i])
        return -1


20.12 fizz buzz
-----------------------------
Wtite a function that returns an array containing the numbers from 1 to N,
where N is the parametered value. N will never be less than 1.

Replace certain values however if any of the following conditions are met:

If the value is a multiple of 3: use the value 'Fizz' instead
If the value is a multiple of 5: use the value 'Buzz' instead
If the value is a multiple of 3 & 5: use the value 'FizzBuzz' instead
"""

"""
There is no fancy algorithm to solve fizz buzz.

Iterate from 1 through n
Use the mod operator to determine if the current iteration is divisible by:
3 and 5 -> 'FizzBuzz'
3 -> 'Fizz'
5 -> 'Buzz'
else -> string of current iteration
return the results
Complexity:

Time: O(n)
Space: O(n)


.. code-block:: python

    def fizzbuzz(n):

        # Validate the input
        if n < 1:
            raise ValueError('n cannot be less than one')
        if n is None:
            raise TypeError('n cannot be None')

        result = []

        for i in range(1, n+1):
            if i%3 == 0 and i%5 == 0:
                result.append('FizzBuzz')
            elif i%3 == 0:
                result.append('Fizz')
            elif i%5 == 0:
                result.append('Buzz')
            else:
                result.append(i)
        return result

    # Alternative solution
    def fizzbuzz_with_helper_func(n):
        return [fb(m) for m in range(1,n+1)]

    def fb(m):
        r = (m % 3 == 0) * "Fizz" + (m % 5 == 0) * "Buzz"
        return r if r != "" else m



20.13 group anagrams
-----------------------------
Given an array of strings, group anagrams together.

For example, given: ["eat", "tea", "tan", "ate", "nat", "bat"],
Return:

[
  ["ate", "eat","tea"],
  ["nat","tan"],
  ["bat"]
]


.. code-block:: python

    def group_anagrams(strs):
        d = {}
        ans = []
        k = 0
        for str in strs:
            sstr = ''.join(sorted(str))
            if sstr not in d:
                d[sstr] = k
                k += 1
                ans.append([])
                ans[-1].append(str)
            else:
                ans[d[sstr]].append(str)
        return ans



20.14 int to roman
-----------------------------
Given an integer, convert it to a roman numeral.
Input is guaranteed to be within the range from 1 to 3999.


.. code-block:: python

    def int_to_roman(num):
        """
        :type num: int
        :rtype: str
        """
        m = ["", "M", "MM", "MMM"];
        c = ["", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"];
        x = ["", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"];
        i = ["", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"];
        return m[num//1000] + c[(num%1000)//100] + x[(num%100)//10] + i[num%10];


20.15 is palindrome
-----------------------------
Given a string, determine if it is a palindrome,
considering only alphanumeric characters and ignoring cases.
For example,
"A man, a plan, a canal: Panama" is a palindrome.
"race a car" is not a palindrome.
Note:
Have you consider that the string might be empty?
This is a good question to ask during an interview.
For the purpose of this problem,
we define empty string as valid palindrome.


.. code-block:: python

    from string import ascii_letters


    def is_palindrome(s):
        """
        :type s: str
        :rtype: bool
        """
        i = 0
        j = len(s)-1
        while i < j:
            while i < j and not s[i].isalnum():
                i += 1
            while i < j and not s[j].isalnum():
                j -= 1
            if s[i].lower() != s[j].lower():
                return False
            i, j = i+1, j-1
        return True

    """
    Here is a bunch of other variations of is_palindrome function.

    Variation 1:
    Find the reverse of the string and compare it with the original string

    Variation 2:
    Loop from the start to length/2 and check the first character and last character
    and so on... for instance s[0] compared with s[n-1], s[1] == s[n-2]...

    Variation 3:
    Using stack idea.

    Note: We are assuming that we are just checking a one word string. To check if a complete sentence
    """
    def remove_punctuation(s):
        """
        Remove punctuation, case sensitivity and spaces
        """
        return "".join(i.lower() for i in s if i in ascii_letters)

    # Variation 1
    def string_reverse(s):
        return s[::-1]

    def is_palindrome_reverse(s):
        s = remove_punctuation(s)

        # can also get rid of the string_reverse function and just do this return s == s[::-1] in one line.
        if (s == string_reverse(s)):
            return True
        return False


    # Variation 2
    def is_palindrome_two_pointer(s):
        s = remove_punctuation(s)

        for i in range(0, len(s)//2):
            if (s[i] != s[len(s) - i - 1]):
                return False
        return True


    # Variation 3
    def is_palindrome_stack(s):
        stack = []
        s = remove_punctuation(s)

        for i in range(len(s)//2, len(s)):
            stack.append(s[i])
        for i in range(0, len(s)//2):
            if s[i] != stack.pop():
                return False
        return True


20.16 is rotated
-----------------------------
Given two strings s1 and s2, determine if s2 is a rotated version of s1.
For example,
is_rotated("hello", "llohe") returns True
is_rotated("hello", "helol") returns False

accepts two strings
returns bool
Reference: https://leetcode.com/problems/rotate-string/description/


.. code-block:: python

    def is_rotated(s1, s2):
        if len(s1) == len(s2):
            return s2 in s1 + s1
        else:
            return False

    """
    Another solution: brutal force
    Complexity: O(N^2)
    """
    def is_rotated_v1(s1, s2):
        if len(s1) != len(s2):
            return False
        if len(s1) == 0:
            return True

        for c in range(len(s1)):
            if all(s1[(c + i) % len(s1)] == s2[i] for i in range(len(s1))):
                return True
        return False



20.17 judge circle
-----------------------------
Initially, there is a Robot at position (0, 0). Given a sequence of its moves,
judge if this robot makes a circle, which means it moves back to the original place.

The move sequence is represented by a string. And each move is represent by a
character. The valid robot moves are R (Right), L (Left), U (Up) and D (down).
The output should be true or false representing whether the robot makes a circle.

Example 1:
Input: "UD"
Output: true
Example 2:
Input: "LL"
Output: false


.. code-block:: python

    def judge_circle(moves):
        dict_moves = {
            'U' : 0,
            'D' : 0,
            'R' : 0,
            'L' : 0
        }
        for char in moves:
            dict_moves[char] = dict_moves[char] + 1
        return dict_moves['L'] == dict_moves['R'] and dict_moves['U'] == dict_moves['D']


20.18 license number
-----------------------------

.. code-block:: python

    def license_number(key, k):
        res, alnum = [], []
        for char in key:
            if char != "-":
                alnum.append(char)
        for i, char in enumerate(reversed(alnum)):
            res.append(char)
            if (i+1) % k == 0 and i != len(alnum)-1:
                res.append("-")
        return "".join(res[::-1])



20.19 longest common prefix
-----------------------------
Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

Example 1:
Input: ["flower","flow","flight"]
Output: "fl"

Example 2:
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.

Reference: https://leetcode.com/problems/longest-common-prefix/description/


.. code-block:: python


    First solution: Horizontal scanning

    def common_prefix(s1, s2):
        "Return prefix common of 2 strings"
        if not s1 or not s2:
            return ""
        k = 0
        while s1[k] == s2[k]:
            k = k + 1
            if k >= len(s1) or k >= len(s2):
                return s1[0:k]
        return s1[0:k]

    def longest_common_prefix_v1(strs):
        if not strs:
            return ""
        result = strs[0]
        for i in range(len(strs)):
            result = common_prefix(result, strs[i])
        return result


    Second solution: Vertical scanning

    def longest_common_prefix_v2(strs):
        if not strs:
            return ""
        for i in range(len(strs[0])):
            for string in strs[1:]:
                if i == len(string) or string[i] != strs[0][i]:
                    return strs[0][0:i]
        return strs[0]


    Third solution: Divide and Conquer

    def longest_common_prefix_v3(strs):
        if not strs:
            return ""
        return longest_common(strs, 0, len(strs) -1)

    def longest_common(strs, left, right):
        if left == right:
            return strs[left]
        mid = (left + right) // 2
        lcp_left = longest_common(strs, left, mid)
        lcp_right = longest_common(strs, mid + 1, right)
        return common_prefix(lcp_left, lcp_right)


20.20 make sentence
-----------------------------
For a given string and dictionary, how many sentences can you make from the
string, such that all the words are contained in the dictionary.

eg: for given string -> "appletablet"
"apple", "tablet"
"applet", "able", "t"
"apple", "table", "t"
"app", "let", "able", "t"

"applet", {app, let, apple, t, applet} => 3
"thing", {"thing"} -> 1


.. code-block:: python

    count = 0


    def make_sentence(str_piece, dictionaries):
        global count
        if len(str_piece) == 0:
            return True
        for i in range(0, len(str_piece)):
            prefix, suffix = str_piece[0:i], str_piece[i:]
            if prefix in dictionaries:
                if suffix in dictionaries or make_sentence(suffix, dictionaries):
                    count += 1
        return True


20.21 merge string checker
-----------------------------
At a job interview, you are challenged to write an algorithm to check if a
given string, s, can be formed from two other strings, part1 and part2.
The restriction is that the characters in part1 and part2 are in the same
order as in s. The interviewer gives you the following example and tells
you to figure out the rest from the given test cases.
'codewars' is a merge from 'cdw' and 'oears':
s:  c o d e w a r s   = codewars
part1:  c   d   w         = cdw
part2:    o   e   a r s   = oears


.. code-block:: python

    # Recursive Solution
    def is_merge_recursive(s, part1, part2):
        if not part1:
            return s == part2
        if not part2:
            return s == part1
        if not s:
            return part1 + part2 == ''
        if s[0] == part1[0] and is_merge_recursive(s[1:], part1[1:], part2):
            return True
        if s[0] == part2[0] and is_merge_recursive(s[1:], part1, part2[1:]):
            return True
        return False


    # An iterative approach
    def is_merge_iterative(s, part1, part2):
        tuple_list = [(s, part1, part2)]
        while tuple_list:
            string, p1, p2 = tuple_list.pop()
            if string:
                if p1 and string[0] == p1[0]:
                    tuple_list.append((string[1:], p1[1:], p2))
                if p2 and string[0] == p2[0]:
                    tuple_list.append((string[1:], p1, p2[1:]))
            else:
                if not p1 and not p2:
                    return True
        return False



20.22 min distance
-----------------------------
Given two words word1 and word2, find the minimum number of steps required to
make word1 and word2 the same, where in each step you can delete one character
in either string.

For example:
Input: "sea", "eat"
Output: 2
Explanation: You need one step to make "sea" to "ea" and another step to make "eat" to "ea".

Reference: https://leetcode.com/problems/delete-operation-for-two-strings/description/


.. code-block:: python

    def min_distance(word1, word2):
        return len(word1) + len(word2) - 2 * lcs(word1, word2, len(word1), len(word2))

    def lcs(s1, s2, i, j):
        """
        The length of longest common subsequence among the two given strings s1 and s2
        """
        if i == 0 or j == 0:
            return 0
        elif s1[i - 1] == s2[j - 1]:
            return 1 + lcs(s1, s2, i - 1, j - 1)
        else:
            return max(lcs(s1, s2, i - 1, j), lcs(s1, s2, i, j - 1))

    # TODO: Using dynamic programming



20.23 multiply strings
-----------------------------
Given two non-negative integers num1 and num2 represented as strings,
return the product of num1 and num2.

Note:

The length of both num1 and num2 is < 110.
Both num1 and num2 contains only digits 0-9.
Both num1 and num2 does not contain any leading zero.
You must not use any built-in BigInteger library or convert
the inputs to integer directly.


.. code-block:: python

    def multiply(num1:"str", num2:"str")->"str":
        carry = 1
        interm = []
        zero = ord('0')
        i_pos = 1
        for i in reversed(num1):
            j_pos = 1
            add = 0
            for j in reversed(num2):
                mult = (ord(i)-zero) * (ord(j)-zero) * j_pos * i_pos
                j_pos *= 10
                add += mult
            i_pos *= 10
            interm.append(add)
        return str(sum(interm))


    if __name__ == "__main__":
        print(multiply("1", "23"))
        print(multiply("23", "23"))
        print(multiply("100", "23"))
        print(multiply("100", "10000"))



20.24 one edit distance
-----------------------------
Given two strings S and T, determine if they are both one edit distance apart.


.. code-block:: python

    def is_one_edit(s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        if len(s) > len(t):
            return is_one_edit(t, s)
        if len(t) - len(s) > 1 or t == s:
            return False
        for i in range(len(s)):
            if s[i] != t[i]:
                return s[i+1:] == t[i+1:] or s[i:] == t[i+1:]
        return True


    def is_one_edit2(s, t):
        l1, l2 = len(s), len(t)
        if l1 > l2:
            return is_one_edit2(t, s)
        if len(t) - len(s) > 1 or t == s:
            return False
        for i in range(len(s)):
            if s[i] != t[i]:
                if l1 == l2:
                    s = s[:i]+t[i]+s[i+1:]  # modify
                else:
                    s = s[:i]+t[i]+s[i:]  # insertion
                break
        return s == t or s == t[:-1]



20.25 rabin karp
-----------------------------
# Following program is the python implementation of
# Rabin Karp Algorithm


.. code-block:: python

    class RollingHash:
        def __init__(self, text, size_word):
            self.text = text
            self.hash = 0
            self.size_word = size_word

            for i in range(0, size_word):
                #ord maps the character to a number
                #subtract out the ASCII value of "a" to start the indexing at zero
                self.hash += (ord(self.text[i]) - ord("a")+1)*(26**(size_word - i -1))

            #start index of current window
            self.window_start = 0
            #end of index window
            self.window_end = size_word

        def move_window(self):
            if self.window_end <= len(self.text) - 1:
                #remove left letter from hash value
                self.hash -= (ord(self.text[self.window_start]) - ord("a")+1)*26**(self.size_word-1)
                self.hash *= 26
                self.hash += ord(self.text[self.window_end])- ord("a")+1
                self.window_start += 1
                self.window_end += 1

        def window_text(self):
            return self.text[self.window_start:self.window_end]

    def rabin_karp(word, text):
        if word == "" or text == "":
            return None
        if len(word) > len(text):
            return None

        rolling_hash = RollingHash(text, len(word))
        word_hash = RollingHash(word, len(word))
        #word_hash.move_window()

        for i in range(len(text) - len(word) + 1):
            if rolling_hash.hash == word_hash.hash:
                if rolling_hash.window_text() == word:
                    return i
            rolling_hash.move_window()
        return None




20.26 repeat string
-----------------------------
Given two strings A and B, find the minimum number of times A has to be repeated such that B is a substring of it. If no such solution, return -1.

For example, with A = "abcd" and B = "cdabcdab".

Return 3, because by repeating A three times (“abcdabcdabcd”), B is a substring of it; and B is not a substring of A repeated two times ("abcdabcd").

Note:
The length of A and B will be between 1 and 10000.

Reference: https://leetcode.com/problems/repeated-string-match/description/


.. code-block:: python

    def repeat_string(A, B):
        count = 1
        tmp = A
        max_count = (len(B) / len(A)) + 1
        while not(B in tmp):
            tmp = tmp + A
            if (count > max_count):
                count = -1
                break
            count = count + 1

        return count


20.27 repeat substring
-----------------------------
Given a non-empty string check if it can be constructed by taking
a substring of it and appending multiple copies of the substring together.

For example:
Input: "abab"
Output: True
Explanation: It's the substring "ab" twice.

Input: "aba"
Output: False

Input: "abcabcabcabc"
Output: True
Explanation: It's the substring "abc" four times.

Reference: https://leetcode.com/problems/repeated-substring-pattern/description/


.. code-block:: python

    def repeat_substring(s):
        """
        :type s: str
        :rtype: bool
        """
        str = (s + s)[1:-1]
        return s in str


20.28 reverse string
-----------------------------

.. code-block:: python

    def recursive(s):
        l = len(s)
        if l < 2:
            return s
        return recursive(s[l//2:]) + recursive(s[:l//2])

    def iterative(s):
        r = list(s)
        i, j = 0, len(s) - 1
        while i < j:
            r[i], r[j] = r[j], r[i]
            i += 1
            j -= 1
        return "".join(r)

    def pythonic(s):
        r = list(reversed(s))
        return "".join(r)

    def ultra_pythonic(s):
        return s[::-1]

20.29 reverse vowel
-----------------------------

.. code-block:: python

    def reverse_vowel(s):
        vowels = "AEIOUaeiou"
        i, j = 0, len(s)-1
        s = list(s)
        while i < j:
            while i < j and s[i] not in vowels:
                i += 1
            while i < j and s[j] not in vowels:
                j -= 1
            s[i], s[j] = s[j], s[i]
            i, j = i + 1, j - 1
        return "".join(s)



20.30 reverse words
-----------------------------

.. code-block:: python

    def reverse(array, i, j):
        while i < j:
            array[i], array[j] = array[j], array[i]
            i += 1
            j -= 1


    def reverse_words(string):
        arr = string.strip().split()  # arr is list of words
        n = len(arr)
        reverse(arr, 0, n-1)

        return " ".join(arr)


    if __name__ == "__main__":
        test = "I am keon kim and I like pizza"
        print(test)
        print(reverse_words(test))



20.31 roman to int
-----------------------------
Given a roman numeral, convert it to an integer.
Input is guaranteed to be within the range from 1 to 3999.


.. code-block:: python

    def roman_to_int(s:"str")->"int":
        number = 0
        roman = {'M':1000, 'D':500, 'C': 100, 'L':50, 'X':10, 'V':5, 'I':1}
        for i in range(len(s)-1):
            if roman[s[i]] < roman[s[i+1]]:
                number -= roman[s[i]]
            else:
                number += roman[s[i]]
        return number + roman[s[-1]]


    if __name__ == "__main__":
        roman = "DCXXI"
        print(roman_to_int(roman))



20.32 rotate
-----------------------------
Given a strings s and int k, return a string that rotates k times

For example,
rotate("hello", 2) return "llohe"
rotate("hello", 5) return "hello"
rotate("hello", 6) return "elloh"
rotate("hello", 7) return "llohe"

accepts two strings
returns bool


.. code-block:: python

    def rotate(s, k):
        double_s = s + s
        if k <= len(s):
            return double_s[k:k + len(s)]
        else:
            return double_s[k-len(s):k]


20.33 strip url params
-----------------------------
Write a function that does the following:
Removes any duplicate query string parameters from the url
Removes any query string parameters specified within the 2nd argument (optional array)

An example:
www.saadbenn.com?a=1&b=2&a=2') // returns 'www.saadbenn.com?a=1&b=2'


.. code-block:: python

    from collections import defaultdict
    import urllib
    import urllib.parse

    # Here is a very non-pythonic grotesque solution
    def strip_url_params1(url, params_to_strip=None):

        if not params_to_strip:
            params_to_strip = []
        if url:
            result = '' # final result to be returned
            tokens = url.split('?')
            domain = tokens[0]
            query_string = tokens[-1]
            result += domain
            # add the '?' to our result if it is in the url
            if len(tokens) > 1:
                result += '?'
            if not query_string:
                return url
            else:
                # logic for removing duplicate query strings
                # build up the list by splitting the query_string using digits
                key_value_string = []
                string = ''
                for char in query_string:
                    if char.isdigit():
                        key_value_string.append(string + char)
                        string = ''
                    else:
                        string += char
                dict = defaultdict(int)
                # logic for checking whether we should add the string to our result
                for i in key_value_string:
                    _token = i.split('=')
                    if _token[0]:
                        length = len(_token[0])
                        if length == 1:
                            if _token and (not(_token[0] in dict)):
                                if params_to_strip:
                                    if _token[0] != params_to_strip[0]:
                                        dict[_token[0]] = _token[1]
                                        result = result + _token[0] + '=' + _token[1]
                                else:
                                    if not _token[0] in dict:
                                        dict[_token[0]] = _token[1]
                                        result = result + _token[0] + '=' + _token[1]
                        else:
                            check = _token[0]
                            letter = check[1]
                            if _token and (not(letter in dict)):
                                if params_to_strip:
                                    if letter != params_to_strip[0]:
                                        dict[letter] = _token[1]
                                        result = result + _token[0] + '=' + _token[1]
                                else:
                                    if not letter in dict:
                                        dict[letter] = _token[1]
                                        result = result + _token[0] + '=' + _token[1]
        return result

    # A very friendly pythonic solution (easy to follow)
    def strip_url_params2(url, param_to_strip=[]):
        if '?' not in url:
            return url

        queries = (url.split('?')[1]).split('&')
        queries_obj = [query[0] for query in queries]
        for i in range(len(queries_obj) - 1, 0, -1):
            if queries_obj[i] in param_to_strip or queries_obj[i] in queries_obj[0:i]:
                queries.pop(i)

        return url.split('?')[0] + '?' + '&'.join(queries)


    # Here is my friend's solution using python's builtin libraries
    def strip_url_params3(url, strip=None):
        if not strip: strip = []

        parse = urllib.parse.urlparse(url)
        query = urllib.parse.parse_qs(parse.query)

        query = {k: v[0] for k, v in query.items() if k not in strip}
        query = urllib.parse.urlencode(query)
        new = parse._replace(query=query)

        return new.geturl()


20.34 strong password
-----------------------------
The signup page required her to input a name and a password. However, the password
must be strong. The website considers a password to be strong if it satisfies the following criteria:

1) Its length is at least 6.
2) It contains at least one digit.
3) It contains at least one lowercase English character.
4) It contains at least one uppercase English character.
5) It contains at least one special character. The special characters are: !@#$%^&*()-+
She typed a random string of length  in the password field but wasn't sure if it was strong.
Given the string she typed, can you find the minimum number of characters she must add to make her password strong?

Note: Here's the set of types of characters in a form you can paste in your solution:
numbers = "0123456789"
lower_case = "abcdefghijklmnopqrstuvwxyz"
upper_case = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
special_characters = "!@#$%^&*()-+"

Input Format
The first line contains an integer  denoting the length of the string.
The second line contains a string consisting of  characters, the password
typed by Louise. Each character is either a lowercase/uppercase English alphabet, a digit, or a special character.

Sample Input 1: strong_password(3,"Ab1")
Output: 3 (Because She can make the password strong by adding  characters,for example, $hk, turning the password into Ab1$hk which is strong.
2 characters aren't enough since the length must be at least 6.)

Sample Output 2: strong_password(11,"#Algorithms")
Output: 1 (Because the password isn't strong, but she can make it strong by adding a single digit.)


.. code-block:: python

    def strong_password(n, password):
        count_error = 0
        # Return the minimum number of characters to make the password strong
        if any(i.isdigit() for i in password) == False:
            count_error = count_error + 1
        if any(i.islower() for i in password) == False:
            count_error = count_error + 1
        if any(i.isupper() for i in password) == False:
            count_error = count_error + 1
        if any(i in '!@#$%^&*()-+' for i in password) == False:
            count_error = count_error + 1
        return max(count_error, 6 - n)



20.35 text justification
-----------------------------
Given an array of words and a width maxWidth, format the text such that each line
has exactly maxWidth characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as
you can in each line. Pad extra spaces ' ' when necessary so that each line has
exactly maxWidth characters.

Extra spaces between words should be distributed as evenly as possible. If the
number of spaces on a line do not divide evenly between words, the empty slots
on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left justified and no extra space is
inserted between words.

Note:
A word is defined as a character sequence consisting of non-space characters only.
Each word's length is guaranteed to be greater than 0 and not exceed maxWidth.
The input array words contains at least one word.

Example:
Input:
words = ["What","must","be","acknowledgment","shall","be"]
maxWidth = 16
Output:
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]


.. code-block:: python

    def text_justification(words, max_width):
        '''
        :type words: list
        :type max_width: int
        :rtype: list
        '''
        ret = []  # return value
        row_len = 0  # current length of strs in a row
        row_words = []  # current words in a row
        index = 0  # the index of current word in words
        is_first_word = True  # is current word the first in a row
        while index < len(words):
            while row_len <= max_width and index < len(words):
                if len(words[index]) > max_width:
                    raise ValueError("there exists word whose length is larger than max_width")
                tmp = row_len
                row_words.append(words[index])
                tmp += len(words[index])
                if not is_first_word:
                    tmp += 1  # except for the first word, each word should have at least a ' ' before it.
                if tmp > max_width:
                    row_words.pop()
                    break
                row_len = tmp
                index += 1
                is_first_word = False
            # here we have already got a row of str , then we should supplement enough ' ' to make sure the length is max_width.
            row = ""
            # if the row is the last
            if index == len(words):
                for word in row_words:
                    row += (word + ' ')
                row = row[:-1]
                row += ' ' * (max_width - len(row))
            # not the last row and more than one word
            elif len(row_words) != 1:
                space_num = max_width - row_len
                space_num_of_each_interval = space_num // (len(row_words) - 1)
                space_num_rest = space_num - space_num_of_each_interval * (len(row_words) - 1)
                for j in range(len(row_words)):
                    row += row_words[j]
                    if j != len(row_words) - 1:
                        row += ' ' * (1 + space_num_of_each_interval)
                    if space_num_rest > 0:
                        row += ' '
                        space_num_rest -= 1
            # row with only one word
            else:
                row += row_words[0]
                row += ' ' * (max_width - len(row))
            ret.append(row)
            # after a row , reset those value
            row_len = 0
            row_words = []
            is_first_word = True
        return ret



20.36 unique morse
-----------------------------
International Morse Code defines a standard encoding where each letter is mapped to
a series of dots and dashes, as follows: "a" maps to ".-", "b" maps to "-...", "c"
maps to "-.-.", and so on.

For convenience, the full table for the 26 letters of the English alphabet is given below:
        'a':".-",
        'b':"-...",
        'c':"-.-.",
        'd': "-..",
        'e':".",
        'f':"..-.",
        'g':"--.",
        'h':"....",
        'i':"..",
        'j':".---",
        'k':"-.-",
        'l':".-..",
        'm':"--",
        'n':"-.",
        'o':"---",
        'p':".--.",
        'q':"--.-",
        'r':".-.",
        's':"...",
        't':"-",
        'u':"..-",
        'v':"...-",
        'w':".--",
        'x':"-..-",
        'y':"-.--",
        'z':"--.."

Now, given a list of words, each word can be written as a concatenation of the
Morse code of each letter. For example, "cab" can be written as "-.-.-....-",
(which is the concatenation "-.-." + "-..." + ".-"). We'll call such a
concatenation, the transformation of a word.

Return the number of different transformations among all words we have.
Example:
Input: words = ["gin", "zen", "gig", "msg"]
Output: 2
Explanation:
The transformation of each word is:
"gin" -> "--...-."
"zen" -> "--...-."
"gig" -> "--...--."
"msg" -> "--...--."

There are 2 different transformations, "--...-." and "--...--.".


.. code-block:: python

    morse_code = {
        'a':".-",
        'b':"-...",
        'c':"-.-.",
        'd': "-..",
        'e':".",
        'f':"..-.",
        'g':"--.",
        'h':"....",
        'i':"..",
        'j':".---",
        'k':"-.-",
        'l':".-..",
        'm':"--",
        'n':"-.",
        'o':"---",
        'p':".--.",
        'q':"--.-",
        'r':".-.",
        's':"...",
        't':"-",
        'u':"..-",
        'v':"...-",
        'w':".--",
        'x':"-..-",
        'y':"-.--",
        'z':"--.."
    }
    def convert_morse_word(word):
        morse_word = ""
        word = word.lower()
        for char in word:
            morse_word = morse_word + morse_code[char]
        return morse_word

    def unique_morse(words):
        unique_morse_word = []
        for word in words:
            morse_word = convert_morse_word(word)
            if morse_word not in unique_morse_word:
                unique_morse_word.append(morse_word)
        return len(unique_morse_word)



20.37 validate coordinates
-----------------------------
Create a function that will validate if given parameters are valid geographical coordinates.
Valid coordinates look like the following: "23.32353342, -32.543534534". The return value should be either true or false.
Latitude (which is first float) can be between 0 and 90, positive or negative. Longitude (which is second float) can be between 0 and 180, positive or negative.
Coordinates can only contain digits, or one of the following symbols (including space after comma) -, .
There should be no space between the minus "-" sign and the digit after it.

Here are some valid coordinates:
-23, 25
43.91343345, 143
4, -3

And some invalid ones:
23.234, - 23.4234
N23.43345, E32.6457
6.325624, 43.34345.345
0, 1,2


.. code-block:: python

    # I'll be adding my attempt as well as my friend's solution (took us ~ 1 hour)

    # my attempt
    import re
    def is_valid_coordinates_0(coordinates):
        for char in coordinates:
            if not (char.isdigit() or char in ['-', '.', ',', ' ']):
                return False
        l = coordinates.split(", ")
        if len(l) != 2:
            return False
        try:
            latitude = float(l[0])
            longitude = float(l[1])
        except:
            return False
        return -90 <= latitude <= 90 and -180 <= longitude <= 180

    # friends solutions
    def is_valid_coordinates_1(coordinates):
        try:
            lat, lng = [abs(float(c)) for c in coordinates.split(',') if 'e' not in c]
        except ValueError:
            return False

        return lat <= 90 and lng <= 180

    # using regular expression
    def is_valid_coordinates_regular_expression(coordinates):
        return bool(re.match("-?(\d|[1-8]\d|90)\.?\d*, -?(\d|[1-9]\d|1[0-7]\d|180)\.?\d*$", coordinates))


20.38 word squares
-----------------------------
# Given a set of words (without duplicates),
# find all word squares you can build from them.

# A sequence of words forms a valid word square
# if the kth row and column read the exact same string,
# where 0 ≤ k < max(numRows, numColumns).

# For example, the word sequence ["ball","area","lead","lady"] forms
# a word square because each word reads the same both horizontally
# and vertically.

# b a l l
# a r e a
# l e a d
# l a d y
# Note:
# There are at least 1 and at most 1000 words.
# All words will have the exact same length.
# Word length is at least 1 and at most 5.
# Each word contains only lowercase English alphabet a-z.

# Example 1:

# Input:
# ["area","lead","wall","lady","ball"]

# Output:
# [
  # [ "wall",
    # "area",
    # "lead",
    # "lady"
  # ],
  # [ "ball",
    # "area",
    # "lead",
    # "lady"
  # ]
# ]

# Explanation:
# The output consists of two word squares. The order of output does not matter
# (just the order of words in each word square matters).

.. code-block:: python

    import collections

    def word_squares(words):
        n = len(words[0])
        fulls = collections.defaultdict(list)
        for word in words:
            for i in range(n):
                fulls[word[:i]].append(word)

        def build(square):
            if len(square) == n:
                squares.append(square)
                return
            prefix = ""
            for k in range(len(square)):
                prefix += square[k][len(square)]
            for word in fulls[prefix]:
                build(square + [word])
        squares = []
        for word in words:
            build([word])
        return squares




