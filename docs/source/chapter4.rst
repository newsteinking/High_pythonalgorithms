chapter 4: bfs
==================================================


4.1 maze search
-------------------------
BFS time complexity : O(|E|)
BFS space complexity : O(|V|)

do BFS from (0,0) of the grid and get the minimum number of steps needed to get to the lower right column

only step on the columns whose value is 1

if there is no path, it returns -1


.. code-block:: python

    def maze_search(grid):
        dx = [0,0,-1,1]
        dy = [-1,1,0,0]
        n = len(grid)
        m = len(grid[0])
        q = [(0,0,0)]
        visit = [[0]*m for _ in range(n)]
        if grid[0][0] == 0:
            return -1
        visit[0][0] = 1
        while q:
            i, j, step = q.pop(0)
            if i == n-1 and j == m-1:
                return step
            for k in range(4):
                x = i + dx[k]
                y = j + dy[k]
                if x>=0 and x<n and y>=0 and y<m:
                    if grid[x][y] ==1 and visit[x][y] == 0:
                        visit[x][y] = 1
                        q.append((x,y,step+1))
        return -1



4.2 shortest distance from alll buidings
----------------------------------------------
do BFS from each building, and decrement all empty place for every building visit
when grid[i][j] == -b_nums, it means that grid[i][j] are already visited from all b_nums
and use dist to record distances from b_nums


.. code-block:: python


    import collections


    def shortest_distance(grid):
        if not grid or not grid[0]:
            return -1

        matrix = [[[0,0] for i in range(len(grid[0]))] for j in range(len(grid))]

        count = 0    # count how many building we have visited
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == 1:
                    bfs(grid, matrix, i, j, count)
                    count += 1

        res = float('inf')
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                if matrix[i][j][1]==count:
                    res = min(res, matrix[i][j][0])

        return res if res!=float('inf') else -1

    def bfs(grid, matrix, i, j, count):
        q = [(i, j, 0)]
        while q:
            i, j, step = q.pop(0)
            for k, l in [(i-1,j), (i+1,j), (i,j-1), (i,j+1)]:
                # only the position be visited by count times will append to queue
                if 0<=k<len(grid) and 0<=l<len(grid[0]) and \
                        matrix[k][l][1]==count and grid[k][l]==0:
                    matrix[k][l][0] += step+1
                    matrix[k][l][1] = count+1
                    q.append((k, l, step+1))



4.3 word ladder
-------------------------
Given two words (begin_word and end_word), and a dictionary's word list,
find the length of shortest transformation sequence
from beginWord to endWord, such that:

Only one letter can be changed at a time
Each intermediate word must exist in the word list
For example,

Given:
begin_word = "hit"
end_word = "cog"
word_list = ["hot","dot","dog","lot","log"]
As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
.
Note:
Return -1 if there is no such transformation sequence.
All words have the same length.
All words contain only lowercase alphabetic characters.



.. code-block:: python


    import unittest


    def ladder_length(begin_word, end_word, word_list):
        """
        Bidirectional BFS!!!
        :type begin_word: str
        :type end_word: str
        :type word_list: Set[str]
        :rtype: int
        """
        if len(begin_word) != len(end_word):
            return -1   # not possible

        if begin_word == end_word:
            return 0

        # when only differ by 1 character
        if sum(c1 != c2 for c1, c2 in zip(begin_word, end_word)) == 1:
            return 1

        begin_set = set()
        end_set = set()
        begin_set.add(begin_word)
        end_set.add(end_word)
        result = 2
        while begin_set and end_set:

            if len(begin_set) > len(end_set):
                begin_set, end_set = end_set, begin_set

            next_begin_set = set()
            for word in begin_set:
                for ladder_word in word_range(word):
                    if ladder_word in end_set:
                        return result
                    if ladder_word in word_list:
                        next_begin_set.add(ladder_word)
                        word_list.remove(ladder_word)
            begin_set = next_begin_set
            result += 1
            # print(begin_set)
            # print(result)
        return -1


    def word_range(word):
        for ind in range(len(word)):
            temp = word[ind]
            for c in [chr(x) for x in range(ord('a'), ord('z') + 1)]:
                if c != temp:
                    yield word[:ind] + c + word[ind + 1:]


    class TestSuite(unittest.TestCase):

        def test_ladder_length(self):

            # hit -> hot -> dot -> dog -> cog
            self.assertEqual(5, ladder_length('hit', 'cog', ["hot", "dot", "dog", "lot", "log"]))

            # pick -> sick -> sink -> sank -> tank == 5
            self.assertEqual(5, ladder_length('pick', 'tank',
                                              ['tock', 'tick', 'sank', 'sink', 'sick']))

            # live -> life == 1, no matter what is the word_list.
            self.assertEqual(1, ladder_length('live', 'life', ['hoho', 'luck']))

            # 0 length from ate -> ate
            self.assertEqual(0, ladder_length('ate', 'ate', []))

            # not possible to reach !
            self.assertEqual(-1, ladder_length('rahul', 'coder', ['blahh', 'blhah']))


    if __name__ == '__main__':

        unittest.main()


