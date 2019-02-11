chapter 14: Matrix
=================================



14.1 bomb enemy
-------------------------------------
Given a 2D grid, each cell is either a wall 'W',
an enemy 'E' or empty '0' (the number zero),
return the maximum enemies you can kill using one bomb.
The bomb kills all the enemies in the same row and column from
the planted point until it hits the wall since the wall is too strong
to be destroyed.
Note that you can only put the bomb at an empty cell.

Example:
For the given grid

0 E 0 0
E 0 W E
0 E 0 0

return 3. (Placing a bomb at (1,1) kills 3 enemies)


.. code-block:: python

    def max_killed_enemies(grid):
        if not grid: return 0
        m, n = len(grid), len(grid[0])
        max_killed = 0
        row_e, col_e = 0, [0] * n
        # iterates over all cells in the grid
        for i in range(m):
            for j in range(n):
                # makes sure we are next to a wall.
                if j == 0 or grid[i][j-1] == 'W':
                    row_e = row_kills(grid, i, j)
                # makes sure we are next to a wall.
                if i == 0 or grid[i-1][j] == 'W':
                    col_e[j] = col_kills(grid, i, j)
                # makes sure the cell contains a 0
                if grid[i][j] == '0':
                    # updates the variable
                    max_killed = max(max_killed, row_e + col_e[j])

        return max_killed

    # calculate killed enemies for row i from column j
    def row_kills(grid, i, j):
        num = 0
        len_row = len(grid[0])
        while j < len_row and grid[i][j] != 'W':
            if grid[i][j] == 'E':
                num += 1
            j += 1
        return num

    # calculate killed enemies for  column j from row i
    def col_kills(grid, i, j):
        num = 0
        len_col = len(grid)
        while i < len_col and grid[i][j] != 'W':
            if grid[i][j] == 'E':
                num += 1
            i += 1
        return num



    # ----------------- TESTS -------------------------

    """
        Testsuite for the project
    """

    import unittest

    class TestBombEnemy(unittest.TestCase):
        def test_3x4(self):
            grid1 = [["0","E","0","0"],
                    ["E","0","W","E"],
                    ["0","E","0","0"]]
            self.assertEqual(3,max_killed_enemies(grid1))
        def test_4x4(self):
            grid1 = [
                    ["0", "E", "0", "E"],
                    ["E", "E", "E", "0"],
                    ["E", "0", "W", "E"],
                    ["0", "E", "0", "0"]]
            grid2 = [
                    ["0", "0", "0", "E"],
                    ["E", "0", "0", "0"],
                    ["E", "0", "W", "E"],
                    ["0", "E", "0", "0"]]
            self.assertEqual(5,max_killed_enemies(grid1))
            self.assertEqual(3,max_killed_enemies(grid2))

    if __name__ == "__main__":
        unittest.main()



14.2 copy transform
-------------------


.. code-block:: python

    def rotate_clockwise(matrix):
        new = []
        for row in reversed(matrix):
            for i, elem in enumerate(row):
                try:
                    new[i].append(elem)
                except IndexError:
                    new.insert(i, [])
                    new[i].append(elem)
        return new

    def rotate_counterclockwise(matrix):
        new = []
        for row in matrix:
            for i, elem in enumerate(reversed(row)):
                try:
                    new[i].append(elem)
                except IndexError:
                    new.insert(i, [])
                    new[i].append(elem)
        return new

    def top_left_invert(matrix):
        new = []
        for row in matrix:
            for i, elem in enumerate(row):
                try:
                    new[i].append(elem)
                except IndexError:
                    new.insert(i, [])
                    new[i].append(elem)
        return new

    def bottom_left_invert(matrix):
        new = []
        for row in reversed(matrix):
            for i, elem in enumerate(reversed(row)):
                try:
                    new[i].append(elem)
                except IndexError:
                    new.insert(i, [])
                    new[i].append(elem)
        return new

    if __name__ == '__main__':
        def print_matrix(matrix, name):
            print('{}:\n['.format(name))
            for row in matrix:
                print('  {}'.format(row))
            print(']\n')

        matrix = [
            [1, 2, 3],
            [4, 5, 6],
            [7, 8, 9],
        ]

        print_matrix(matrix, 'initial')
        print_matrix(rotate_clockwise(matrix), 'clockwise')
        print_matrix(rotate_counterclockwise(matrix), 'counterclockwise')
        print_matrix(top_left_invert(matrix), 'top left invert')
        print_matrix(bottom_left_invert(matrix), 'bottom left invert')



14.3 count paths
-------------------
#
# Count the number of unique paths from a[0][0] to a[m-1][n-1]
# We are allowed to move either right or down from a cell in the matrix.
# Approaches-
# (i) Recursion- Recurse starting from a[m-1][n-1], upwards and leftwards,
#                add the path count of both recursions and return count.
# (ii) Dynamic Programming- Start from a[0][0].Store the count in a count
#                           matrix. Return count[m-1][n-1]
# T(n)- O(mn), S(n)- O(mn)
#


.. code-block:: python


    def count_paths(m, n):
        if m < 1 or n < 1:
            return -1
        count = [[None for j in range(n)] for i in range(m)]

        # Taking care of the edge cases- matrix of size 1xn or mx1
        for i in range(n):
            count[0][i] = 1
        for j in range(m):
            count[j][0] = 1

        for i in range(1, m):
            for j in range(1, n):
                # Number of ways to reach a[i][j] = number of ways to reach
                #                                   a[i-1][j] + a[i][j-1]
                count[i][j] = count[i - 1][j] + count[i][j - 1]

        print(count[m - 1][n - 1])


    def main():
        m, n = map(int, input('Enter two positive integers: ').split())
        count_paths(m, n)


    if __name__ == '__main__':
        main()


14.4 crout matrix decomposition
--------------------------------------
Crout matrix decomposition is used to find two matrices that, when multiplied
give our input matrix, so L * U = A.
L stands for lower and L has non-zero elements only on diagonal and below.
U stands for upper and U has non-zero elements only on diagonal and above.

This can for example be used to solve systems of linear equations.
The last if is used  if  to avoid dividing by zero.

Example:
We input the A matrix:
[[1,2,3],
[3,4,5],
[6,7,8]]

We get:
L = [1.0,  0.0, 0.0]
    [3.0, -2.0, 0.0]
    [6.0, -5.0, 0.0]
U = [1.0,  2.0, 3.0]
    [0.0,  1.0, 2.0]
    [0.0,  0.0, 1.0]

We can check that L * U = A.

I think the complexity should be O(n^3).

.. code-block:: python

    def crout_matrix_decomposition(A):
        n = len(A)
        L = [[0.0] * n for i in range(n)]
        U = [[0.0] * n for i in range(n)]
        for j in range(n):
            U[j][j] = 1.0
            for i in range(j, n):
                alpha = float(A[i][j])
                for k in range(j):
                    alpha -= L[i][k]*U[k][j]
                L[i][j] = float(alpha)
            for i in range(j+1, n):
                tempU = float(A[j][i])
                for k in range(j):
                    tempU -= float(L[j][k]*U[k][i])
                if int(L[j][j]) == 0:
                    L[j][j] = float(0.1**40)
                U[j][i] = float(tempU/L[j][j])
        return (L,U)



14.5 rotate image
-------------------
You are given an n x n 2D mat representing an image.

Rotate the image by 90 degrees (clockwise).

Follow up:
Could you do this in-place?

.. code-block:: python

    # clockwise rotate
    # first reverse up to down, then swap the symmetry
    # 1 2 3     7 8 9     7 4 1
    # 4 5 6  => 4 5 6  => 8 5 2
    # 7 8 9     1 2 3     9 6 3

    def rotate(mat):
        if not mat:
            return mat
        mat.reverse()
        for i in range(len(mat)):
            for j in range(i):
                mat[i][j], mat[j][i] = mat[j][i], mat[i][j]


    if __name__ == "__main__":
        mat = [[1,2,3],
               [4,5,6],
               [7,8,9]]
        print(mat)
        rotate(mat)
        print(mat)


14.6 search in sorted matrix
------------------------------------
#
# Search a key in a row wise and column wise sorted (non-decreasing) matrix.
# m- Number of rows in the matrix
# n- Number of columns in the matrix
# T(n)- O(m+n)
#

.. code-block:: python

    def search_in_a_sorted_matrix(mat, m, n, key):
        i, j = m-1, 0
        while i >= 0 and j < n:
            if key == mat[i][j]:
                print ('Key %s found at row- %s column- %s' % (key, i+1, j+1))
                return
            if key < mat[i][j]:
                i -= 1
            else:
                j += 1
        print ('Key %s not found' % (key))


    def main():
        mat = [
               [2, 5, 7],
               [4, 8, 13],
               [9, 11, 15],
               [12, 17, 20]
              ]
        key = 13
        print (mat)
        search_in_a_sorted_matrix(mat, len(mat), len(mat[0]), key)


    if __name__ == '__main__':
        main()



14.7 sparse dot vector
--------------------------------
Suppose we have very large sparse vectors, which contains a lot of zeros and double .

find a data structure to store them
get the dot product of them

.. code-block:: python

    def vector_to_index_value_list(vector):
        return [(i, v) for i, v in enumerate(vector) if v != 0.0]


    def dot_product(iv_list1, iv_list2):

        product = 0
        p1 = len(iv_list1) - 1
        p2 = len(iv_list2) - 1

        while p1 >= 0 and p2 >= 0:
            i1, v1 = iv_list1[p1]
            i2, v2 = iv_list2[p2]

            if i1 < i2:
                p1 -= 1
            elif i2 < i1:
                p2 -= 1
            else:
                product += v1 * v2
                p1 -= 1
                p2 -= 1

        return product


    def __test_simple():
        print(dot_product(vector_to_index_value_list([1., 2., 3.]),
                          vector_to_index_value_list([0., 2., 2.])))
        # 10


    def __test_time():
        vector_length = 1024
        vector_count = 1024
        nozero_counut = 10

        def random_vector():
            import random
            vector = [0 for _ in range(vector_length)]
            for i in random.sample(range(vector_length), nozero_counut):
                vector[i] = random.random()
            return vector

        vectors = [random_vector() for _ in range(vector_count)]
        iv_lists = [vector_to_index_value_list(vector) for vector in vectors]

        import time

        time_start = time.time()
        for i in range(vector_count):
            for j in range(i):
                dot_product(iv_lists[i], iv_lists[j])
        time_end = time.time()

        print(time_end - time_start, 'seconds')


    if __name__ == '__main__':
        __test_simple()
        __test_time()


14.8 sparse mul
-------------------
Given two sparse matrices A and B, return the result of AB.

You may assume that A's column number is equal to B's row number.

Example:

A = [
  [ 1, 0, 0],
  [-1, 0, 3]
]

B = [
  [ 7, 0, 0 ],
  [ 0, 0, 0 ],
  [ 0, 0, 1 ]
]


     |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
                  | 0 0 1 |

.. code-block:: python

    # Python solution without table (~156ms):
    def multiply(self, a, b):
        """
        :type A: List[List[int]]
        :type B: List[List[int]]
        :rtype: List[List[int]]
        """
        if a is None or b is None: return None
        m, n, l = len(a), len(b[0]), len(b[0])
        if len(b) != n:
            raise Exception("A's column number must be equal to B's row number.")
        c = [[0 for _ in range(l)] for _ in range(m)]
        for i, row in enumerate(a):
            for k, eleA in enumerate(row):
                if eleA:
                    for j, eleB in enumerate(b[k]):
                        if eleB: c[i][j] += eleA * eleB
        return c


    # Python solution with only one table for B (~196ms):
    def multiply(self, a, b):
        """
        :type A: List[List[int]]
        :type B: List[List[int]]
        :rtype: List[List[int]]
        """
        if a is None or b is None: return None
        m, n, l = len(a), len(a[0]), len(b[0])
        if len(b) != n:
            raise Exception("A's column number must be equal to B's row number.")
        c = [[0 for _ in range(l)] for _ in range(m)]
        table_b = {}
        for k, row in enumerate(b):
            table_b[k] = {}
            for j, eleB in enumerate(row):
                if eleB: table_b[k][j] = eleB
        for i, row in enumerate(a):
            for k, eleA in enumerate(row):
                if eleA:
                    for j, eleB in table_b[k].iteritems():
                        c[i][j] += eleA * eleB
        return c

    # Python solution with two tables (~196ms):
    def multiply(self, a, b):
        """
        :type A: List[List[int]]
        :type B: List[List[int]]
        :rtype: List[List[int]]
        """
        if a is None or b is None: return None
        m, n = len(a), len(b[0])
        if len(b) != n:
            raise Exception("A's column number must be equal to B's row number.")
        l = len(b[0])
        table_a, table_b = {}, {}
        for i, row in enumerate(a):
            for j, ele in enumerate(row):
                if ele:
                    if i not in table_a: table_a[i] = {}
                    table_a[i][j] = ele
        for i, row in enumerate(b):
            for j, ele in enumerate(row):
                if ele:
                    if i not in table_b: table_b[i] = {}
                    table_b[i][j] = ele
        c = [[0 for j in range(l)] for i in range(m)]
        for i in table_a:
            for k in table_a[i]:
                if k not in table_b: continue
                for j in table_b[k]:
                    c[i][j] += table_a[i][k] * table_b[k][j]
        return c


14.9 spiral traversal
-----------------------------
Given a matrix of m x n elements (m rows, n columns),
return all elements of the matrix in spiral order.
For example,
Given the following matrix:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]


You should return [1,2,3,6,9,8,7,4,5].



.. code-block:: python

    def spiral_traversal(matrix):
        res = []
        if len(matrix) == 0:
            return res
        row_begin = 0
        row_end = len(matrix) - 1
        col_begin = 0
        col_end = len(matrix[0]) - 1

        while row_begin <= row_end and col_begin <= col_end:
            for i in range(col_begin, col_end+1):
                res.append(matrix[row_begin][i])
            row_begin += 1

            for i in range(row_begin, row_end+1):
                res.append(matrix[i][col_end])
            col_end -= 1

            if row_begin <= row_end:
                for i in range(col_end, col_begin-1, -1):
                    res.append(matrix[row_end][i])
            row_end -= 1

            if col_begin <= col_end:
                for i in range(row_end, row_begin-1, -1):
                    res.append(matrix[i][col_begin])
            col_begin += 1

        return res


    if __name__ == "__main__":
        mat = [[1, 2, 3],
               [4, 5, 6],
               [7, 8, 9]]
        print(spiral_traversal(mat))



14.10 sudoku validator
----------------------------
Write a function validSolution/ValidateSolution/valid_solution() that accepts a 2D array
representing a Sudoku board, and returns true if it is a valid solution, or false otherwise.
The cells of the sudoku board may also contain 0's, which will represent empty cells.
Boards containing one or more zeroes are considered to be invalid solutions.
The board is always 9 cells by 9 cells, and every cell only contains integers from 0 to 9.

(More info at: http://en.wikipedia.org/wiki/Sudoku)

.. code-block:: python

    # Using dict/hash-table
    from collections import defaultdict
    def valid_solution_hashtable(board):
        for i in range(len(board)):
            dict_row = defaultdict(int)
            dict_col = defaultdict(int)
            for j in range(len(board[0])):
                value_row = board[i][j]
                value_col = board[j][i]
                if not value_row or value_col == 0:
                    return False
                if value_row in dict_row:
                    return False
                else:
                    dict_row[value_row] += 1

                if value_col in dict_col:
                    return False
                else:
                    dict_col[value_col] += 1

        for i in range(3):
            for j in range(3):
                grid_add = 0
                for k in range(3):
                    for l in range(3):
                        grid_add += board[i*3+k][j*3+l]
                if grid_add != 45:
                    return False
        return True


    # Without hash-table/dict
    def valid_solution(board):
        correct = [1, 2, 3, 4, 5, 6, 7, 8, 9]
        # check rows
        for row in board:
            if sorted(row) != correct:
                return False

        # check columns
        for column in zip(*board):
            if sorted(column) != correct:
                return False

        # check regions
        for i in range(3):
            for j in range(3):
                region = []
                for line in board[i*3:(i+1)*3]:
                    region += line[j*3:(j+1)*3]

                if sorted(region) != correct:
                    return False

        # if everything correct
        return True


    # Using set
    def valid_solution_set (board):
        valid = set(range(1, 10))

        for row in board:
            if set(row) != valid:
                return False

        for col in [[row[i] for row in board] for i in range(9)]:
            if set(col) != valid:
                return False

        for x in range(3):
            for y in range(3):
                if set(sum([row[x*3:(x+1)*3] for row in board[y*3:(y+1)*3]], [])) != valid:
                    return False

        return True

    # test cases
    # To avoid congestion I'll leave testing all the functions to the reader. Just change the name of the function in the below test cases.
    import unittest
    class TestSuite(unittest.TestCase):
        def test_valid(self):
            self.assertTrue(valid_solution([[5, 3, 4, 6, 7, 8, 9, 1, 2],
                             [6, 7, 2, 1, 9, 5, 3, 4, 8],
                             [1, 9, 8, 3, 4, 2, 5, 6, 7],
                             [8, 5, 9, 7, 6, 1, 4, 2, 3],
                             [4, 2, 6, 8, 5, 3, 7, 9, 1],
                             [7, 1, 3, 9, 2, 4, 8, 5, 6],
                             [9, 6, 1, 5, 3, 7, 2, 8, 4],
                             [2, 8, 7, 4, 1, 9, 6, 3, 5],
                             [3, 4, 5, 2, 8, 6, 1, 7, 9]]))

        def test_invalid(self):
            self.assertFalse(valid_solution([[5, 3, 4, 6, 7, 8, 9, 1, 2],
                             [6, 7, 2, 1, 9, 0, 3, 4, 9],
                             [1, 0, 0, 3, 4, 2, 5, 6, 0],
                             [8, 5, 9, 7, 6, 1, 0, 2, 0],
                             [4, 2, 6, 8, 5, 3, 7, 9, 1],
                             [7, 1, 3, 9, 2, 4, 8, 5, 6],
                             [9, 0, 1, 5, 3, 7, 2, 1, 4],
                             [2, 8, 7, 4, 1, 9, 6, 3, 5],
                             [3, 0, 0, 4, 8, 1, 1, 7, 9]]))

    if __name__ == "__main__":
        unittest.main()
