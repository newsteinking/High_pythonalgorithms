chapter 13: Maths
=======================================



13.1 base conversion
-----------------------------
Integer base conversion algorithm

int2base(5, 2) return '101'.
base2int('F', 16) return 15.



.. code-block:: python

    import string

    def int_to_base(n, base):
        """
            :type n: int
            :type base: int
            :rtype: str
        """
        is_negative = False
        if n == 0:
            return '0'
        elif n < 0:
            is_negative = True
            n *= -1
        digit = string.digits + string.ascii_uppercase
        res = ''
        while n > 0:
            res += digit[n % base]
            n //= base
        if is_negative:
            return '-' + res[::-1]
        else:
            return res[::-1]


    def base_to_int(s, base):
        """
            Note : You can use int() built-in function instread of this.
            :type s: str
            :type base: int
            :rtype: int
        """

        digit = {}
        for i,c in enumerate(string.digits + string.ascii_uppercase):
            digit[c] = i
        multiplier = 1
        res = 0
        for c in s[::-1]:
            res += digit[c] * multiplier
            multiplier *= base
        return res


13.2 combination
-----------------------------


.. code-block:: python

    def combination(n, r):
        """This function calculates nCr."""
        if n == r or r == 0:
            return 1
        else:
            return combination(n-1, r-1) + combination(n-1, r)

    def combination_memo(n, r):
        """This function calculates nCr using memoization method."""
        memo = {}
        def recur(n, r):
            if n == r or r == 0:
                return 1
            if (n, r) not in memo:
                memo[(n, r)] = recur(n - 1, r - 1) + recur(n - 1, r)
            return memo[(n, r)]
        return recur(n, r)

13.3 decimal to binary ip
-----------------------------
Given an ip address in dotted-decimal representation, determine the
binary representation. For example,
decimal_to_binary(255.0.0.5) returns 11111111.00000000.00000000.00000101
accepts string
returns string



.. code-block:: python

    def decimal_to_binary_util(val):
        bits = [128, 64, 32, 16, 8, 4, 2, 1]
        val = int(val)
        binary_rep = ''
        for bit in bits:
            if val >= bit:
                binary_rep += str(1)
                val -= bit
            else:
                binary_rep += str(0)

        return binary_rep

    def decimal_to_binary_ip(ip):
        values = ip.split('.')
        binary_list = []
        for val in values:
            binary_list.append(decimal_to_binary_util(val))
        return '.'.join(binary_list)



13.4 euler totient
-----------------------------
Euler's totient function, also known as phi-function ϕ(n),
counts the number of integers between 1 and n inclusive,
which are coprime to n.
(Two numbers are coprime if their greatest common divisor (GCD) equals 1).


.. code-block:: python


    def euler_totient(n):
        """Euler's totient function or Phi function.
        Time Complexity: O(sqrt(n))."""
        result = n;
        for i in range(2, int(n ** 0.5) + 1):
            if n % i == 0:
                while n % i == 0:
                    n //= i
                result -= result // i
        if n > 1:
            result -= result // n;
        return result;



13.5 extended gcd
-----------------------------
Extended GCD algorithm.
Return s, t, g
such that a * s + b * t = GCD(a, b)
and s and t are co-prime.

.. code-block:: python


    old_s, s = 1, 0
    old_t, t = 0, 1
    old_r, r = a, b

    while r != 0:
        quotient = old_r / r

        old_r, r = r, old_r - quotient * r
        old_s, s = s, old_s - quotient * s
        old_t, t = t, old_t - quotient * t

    return old_s, old_t, old_r



13.6 factorial
-----------------------------


.. code-block:: python

    def factorial(n, mod=None):
        """Calculates factorial iteratively.
        If mod is not None, then return (n! % mod)
        Time Complexity - O(n)"""
        if not (isinstance(n, int) and n >= 0):
            raise ValueError("'n' must be a non-negative integer.")
        if mod is not None and not (isinstance(mod, int) and mod > 0):
            raise ValueError("'mod' must be a positive integer")
        result = 1
        if n == 0:
            return 1
        for i in range(2, n+1):
            result *= i
            if mod:
                result %= mod
        return result


    def factorial_recur(n, mod=None):
        """Calculates factorial recursively.
        If mod is not None, then return (n! % mod)
        Time Complexity - O(n)"""
        if not (isinstance(n, int) and n >= 0):
            raise ValueError("'n' must be a non-negative integer.")
        if mod is not None and not (isinstance(mod, int) and mod > 0):
            raise ValueError("'mod' must be a positive integer")
        if n == 0:
            return 1
        result = n * factorial(n - 1, mod)
        if mod:
            result %= mod
        return result



13.7 gcd
-----------------------------


.. code-block:: python

    def gcd(a, b):
        """Computes the greatest common divisor of integers a and b using
        Euclid's Algorithm.
        """
        while b != 0:
            a, b = b, a % b
        return a


    def lcm(a, b):
        """Computes the lowest common multiple of integers a and b."""
        return a * b / gcd(a, b)




13.8 generate stobogrammtic
-----------------------------
A strobogrammatic number is a number that looks
the same when rotated 180 degrees (looked at upside down).

Find all strobogrammatic numbers that are of length = n.

For example,
Given n = 2, return ["11","69","88","96"].


.. code-block:: python

    def gen_strobogrammatic(n):
        """
        :type n: int
        :rtype: List[str]
        """
        return helper(n, n)


    def helper(n, length):
        if n == 0:
            return [""]
        if n == 1:
            return ["1", "0", "8"]
        middles = helper(n-2, length)
        result = []
        for middle in middles:
            if n != length:
                result.append("0" + middle + "0")
            result.append("8" + middle + "8")
            result.append("1" + middle + "1")
            result.append("9" + middle + "6")
            result.append("6" + middle + "9")
        return result


    def strobogrammatic_in_range(low, high):
        """
        :type low: str
        :type high: str
        :rtype: int
        """
        res = []
        count = 0
        low_len = len(low)
        high_len = len(high)
        for i in range(low_len, high_len + 1):
            res.extend(helper2(i, i))
        for perm in res:
            if len(perm) == low_len and int(perm) < int(low):
                continue
            elif len(perm) == high_len and int(perm) > int(high):
                continue
            else:
                count += 1
        return count


    def helper2(n, length):
        if n == 0:
            return [""]
        if n == 1:
            return ["0", "8", "1"]
        mids = helper(n-2, length)
        res = []
        for mid in mids:
            if n != length:
                res.append("0"+mid+"0")
            res.append("1"+mid+"1")
            res.append("6"+mid+"9")
            res.append("9"+mid+"6")
            res.append("8"+mid+"8")
        return res



13.9 hailstone
-----------------------------


.. code-block:: python

    def hailstone(n):
      """Return the 'hailstone sequence' from n to 1
         n: The starting point of the hailstone sequence
      """

      sequence = [n]
      while n > 1:
        if n%2 != 0:
          n = 3*n + 1
        else:
          n = int(n/2)
        sequence.append(n)
      return sequence

13.10 is strobogrammatic
-----------------------------
A strobogrammatic number is a number that looks
the same when rotated 180 degrees (looked at upside down).

Write a function to determine if a number is strobogrammatic.
The number is represented as a string.

For example, the numbers "69", "88", and "818" are all strobogrammatic.



.. code-block:: python

    def is_strobogrammatic(num):
        """
        :type num: str
        :rtype: bool
        """
        comb = "00 11 88 69 96"
        i = 0
        j = len(num) - 1
        while i <= j:
            x = comb.find(num[i]+num[j])
            if x == -1:
                return False
            i += 1
            j -= 1
        return True


    def is_strobogrammatic2(num: str):
        """Another implementation."""
        return num == num[::-1].replace('6', '#').replace('9', '6').replace('#', '9')


13.11 moduler exponetial
-----------------------------


.. code-block:: python

    def modular_exponential(base, exponent, mod):
        """Computes (base ^ exponent) % mod.
        Time complexity - O(log n)
        Use similar to Python in-built function pow."""
        if exponent < 0:
            raise ValueError("Exponent must be positive.")
        base %= mod
        result = 1

        while exponent > 0:
            # If the last bit is 1, add 2^k.
            if exponent & 1:
                result = (result * base) % mod
            exponent = exponent >> 1
            # Utilize modular multiplication properties to combine the computed mod C values.
            base = (base * base) % mod

        return result



13.12 next bigger
-----------------------------
I just bombed an interview and made pretty much zero
progress on my interview question.

Given a number, find the next higher number which has the
exact same set of digits as the original number.
For example: given 38276 return 38627.
             given 99999 return -1. (no such number exists)

Condensed mathematical description:

Find largest index i such that array[i − 1] < array[i].
(If no such i exists, then this is already the last permutation.)

Find largest index j such that j ≥ i and array[j] > array[i − 1].

Swap array[j] and array[i − 1].

Reverse the suffix starting at array[i].

.. code-block:: python

    import unittest


    def next_bigger(num):

        digits = [int(i) for i in str(num)]
        idx = len(digits) - 1

        while idx >= 1 and digits[idx-1] >= digits[idx]:
            idx -= 1

        if idx == 0:
            return -1  # no such number exists

        pivot = digits[idx-1]
        swap_idx = len(digits) - 1

        while pivot >= digits[swap_idx]:
            swap_idx -= 1

        digits[swap_idx], digits[idx-1] = digits[idx-1], digits[swap_idx]
        digits[idx:] = digits[:idx-1:-1]   # prefer slicing instead of reversed(digits[idx:])

        return int(''.join(str(x) for x in digits))


    class TestSuite(unittest.TestCase):

        def test_next_bigger(self):

            self.assertEqual(next_bigger(38276), 38627)
            self.assertEqual(next_bigger(12345), 12354)
            self.assertEqual(next_bigger(1528452), 1528524)
            self.assertEqual(next_bigger(138654), 143568)

            self.assertEqual(next_bigger(54321), -1)
            self.assertEqual(next_bigger(999), -1)
            self.assertEqual(next_bigger(5), -1)


    if __name__ == '__main__':

        unittest.main()


13.13 next perfect square
-----------------------------
This program will look for the next perfect square.
Check the argument to see if it is a perfect square itself, if it is not then return -1
otherwise look for the next perfect square
for instance if you pass 121 then the script should return the next perfect square which is 144.

.. code-block:: python


    def find_next_square(sq):
        root = sq ** 0.5
        if root.is_integer():
            return (root + 1)**2
        return -1


    # Another way:

    def find_next_square2(sq):
        x = sq**0.5
        return -1 if x % 1 else (x+1)**2


13.14 nth digit
-----------------------------


.. code-block:: python

    def find_nth_digit(n):
        """find the nth digit of given number.
        1. find the length of the number where the nth digit is from.
        2. find the actual number where the nth digit is from
        3. find the nth digit and return
        """
        length = 1
        count = 9
        start = 1
        while n > length * count:
            n -= length * count
            length += 1
            count *= 10
            start *= 10
        start += (n-1) / length
        s = str(start)
        return int(s[(n-1) % length])

13.15 prime check
-----------------------------


.. code-block:: python

    def prime_check(n):
        """Return True if n is a prime number
        Else return False.
        """

        if n <= 1:
            return False
        if n == 2 or n == 3:
            return True
        if n % 2 == 0 or n % 3 == 0:
            return False
        j = 5
        while j * j <= n:
            if n % j == 0 or n % (j + 2) == 0:
                return False
            j += 6
        return True


13.16 primes sieve of eratosthenes
--------------------------------------
Return list of all primes less than n,
Using sieve of Eratosthenes.

Modification:
We don't need to check all even numbers, we can make the sieve excluding even
numbers and adding 2 to the primes list by default.

We are going to make an array of: x / 2 - 1 if number is even, else x / 2
(The -1 with even number it's to exclude the number itself)
Because we just need numbers [from 3..x if x is odd]

# We can get value represented at index i with (i*2 + 3)

For example, for x = 10, we start with an array of x / 2 - 1 = 4
[1, 1, 1, 1]
 3  5  7  9

For x = 11:
[1, 1, 1, 1, 1]
 3  5  7  9  11  # 11 is odd, it's included in the list

With this, we have reduced the array size to a half,
and complexity it's also a half now.


.. code-block:: python

    def get_primes(n):
        """Return list of all primes less than n,
        Using sieve of Eratosthenes.
        """
        if n <= 0:
            raise ValueError("'n' must be a positive integer.")
        # If x is even, exclude x from list (-1):
        sieve_size = (n // 2 - 1) if n % 2 == 0 else (n // 2)
        sieve = [True for _ in range(sieve_size)]   # Sieve
        primes = []      # List of Primes
        if n >= 2:
            primes.append(2)      # 2 is prime by default
        for i in range(sieve_size):
            if sieve[i]:
                value_at_i = i*2 + 3
                primes.append(value_at_i)
                for j in range(i, sieve_size, value_at_i):
                    sieve[j] = False
        return primes


13.17 pythagoras
-----------------------------
input two of the three side in right angled triangle and return the third. use "?"
to indicate the unknown side.

.. code-block:: python

    def pythagoras(opposite,adjacent,hypotenuse):
        try:
            if opposite == str("?"):
                return ("Opposite = " + str(((hypotenuse**2) - (adjacent**2))**0.5))
            elif adjacent == str("?"):
                return ("Adjacent = " + str(((hypotenuse**2) - (opposite**2))**0.5))
            elif hypotenuse == str("?"):
                return ("Hypotenuse = " + str(((opposite**2) + (adjacent**2))**0.5))
            else:
                return "You already know the answer!"
        except:
            raise ValueError("invalid argument were given.")


13.18 rabin miller
-----------------------------
Rabin-Miller primality test
returning False implies that n is guaranteed composite
returning True means that n is probably prime
with a 4 ** -k chance of being wrong



.. code-block:: python

    import random


    def is_prime(n, k):

        def pow2_factor(num):
            """factor n into a power of 2 times an odd number"""
            power = 0
            while num % 2 == 0:
                num /= 2
                power += 1
            return power, num

        def valid_witness(a):
            """
            returns true if a is a valid 'witness' for n
            a valid witness increases chances of n being prime
            an invalid witness guarantees n is composite
            """
            x = pow(int(a), int(d), int(n))

            if x == 1 or x == n - 1:
                return False

            for _ in range(r - 1):
                x = pow(int(x), int(2), int(n))

                if x == 1:
                    return True
                if x == n - 1:
                    return False

            return True

        # precondition n >= 5
        if n < 5:
            return n == 2 or n == 3  # True for prime

        r, d = pow2_factor(n - 1)

        for _ in range(k):
            if valid_witness(random.randrange(2, n - 2)):
                return False

        return True


13.19 rsa
-----------------------------
RSA encryption algorithm
a method for encrypting a number that uses seperate encryption and decryption keys
this file only implements the key generation algorithm

there are three important numbers in RSA called n, e, and d
e is called the encryption exponent
d is called the decryption exponent
n is called the modulus

these three numbers satisfy
((x ** e) ** d) % n == x % n

to use this system for encryption, n and e are made publicly available, and d is kept secret
a number x can be encrypted by computing (x ** e) % n
the original number can then be recovered by computing (E ** d) % n, where E is
the encrypted number

fortunately, python provides a three argument version of pow() that can compute powers modulo
a number very quickly:
(a ** b) % c == pow(a,b,c)

.. code-block:: python

    import random


    def generate_key(k, seed=None):
        """
        the RSA key generating algorithm
        k is the number of bits in n
        """

        def modinv(a, m):
            """calculate the inverse of a mod m
            that is, find b such that (a * b) % m == 1"""
            b = 1
            while not (a * b) % m == 1:
                b += 1
            return b

        def gen_prime(k, seed=None):
            """generate a prime with k bits"""

            def is_prime(num):
                if num == 2:
                    return True
                for i in range(2, int(num ** 0.5) + 1):
                    if num % i == 0:
                        return False
                return True

            random.seed(seed)
            while True:
                key = random.randrange(int(2 ** (k - 1)), int(2 ** k))
                if is_prime(key):
                    return key

        # size in bits of p and q need to add up to the size of n
        p_size = k / 2
        q_size = k - p_size

        e = gen_prime(k, seed)  # in many cases, e is also chosen to be a small constant

        while True:
            p = gen_prime(p_size, seed)
            if p % e != 1:
                break

        while True:
            q = gen_prime(q_size, seed)
            if q % e != 1:
                break

        n = p * q
        l = (p - 1) * (q - 1)  # calculate totient function
        d = modinv(e, l)

        return int(n), int(e), int(d)


    def encrypt(data, e, n):
        return pow(int(data), int(e), int(n))


    def decrypt(data, d, n):
        return pow(int(data), int(d), int(n))



    # sample usage:
    # n,e,d = generate_key(16)
    # data = 20
    # encrypted = pow(data,e,n)
    # decrypted = pow(encrypted,d,n)
    # assert decrypted == data


13.20 sqrt precision factor
-----------------------------
Given a positive integer N and a precision factor P,
it produces an output
with a maximum error P from the actual square root of N.

Example:
Given N = 5 and P = 0.001, can produce output x such that
2.235 < x < 2.237. Actual square root of 5 being 2.236.

.. code-block:: python

    def square_root(n, epsilon=0.001):
        """Return square root of n, with maximum absolute error epsilon"""
        guess = n / 2

        while abs(guess * guess - n) > epsilon:
            guess = (guess + (n / guess)) / 2

        return guess


13.21 summing digits
-----------------------------
Recently, I encountered an interview question whose description was as below:

The number 89 is the first integer with more than one digit whose digits when raised up to consecutive powers give the same
number. For example, 89 = 8**1 + 9**2 gives the number 89.

The next number after 89 with this property is 135 = 1**1 + 3**2 + 5**3 = 135.

Write a function that returns a list of numbers with the above property. The function will receive range as parameter.

.. code-block:: python


    def sum_dig_pow(a, b):
        result = []

        for number in range(a, b + 1):
            exponent = 1  # set to 1
            summation = 0    # set to 1
            number_as_string = str(number)

            tokens = list(map(int, number_as_string))  # parse the string into individual digits

            for k in tokens:
                summation = summation + (k ** exponent)
                exponent += 1

            if summation == number:
                result.append(number)
        return result


    # Some test cases:
    assert sum_dig_pow(1, 10) == [1, 2, 3, 4, 5, 6, 7, 8, 9]
    assert sum_dig_pow(1, 100) == [1, 2, 3, 4, 5, 6, 7, 8, 9, 89]


