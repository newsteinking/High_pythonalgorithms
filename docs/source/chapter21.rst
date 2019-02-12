chapter 21: tree
===================================================



21.1 avl
------------------------------------------


.. code-block:: python

    from tree.tree import TreeNode


    class AvlTree(object):
        """
        An avl tree.
        """

        def __init__(self):
            # Root node of the tree.
            self.node = None
            self.height = -1
            self.balance = 0

        def insert(self, key):
            """
            Insert new key into node
            """
            # Create new node
            n = TreeNode(key)
            if not self.node:
                self.node = n
                self.node.left = AvlTree()
                self.node.right = AvlTree()
            elif key < self.node.val:
                self.node.left.insert(key)
            elif key > self.node.val:
                self.node.right.insert(key)
            self.re_balance()

        def re_balance(self):
            """
            Re balance tree. After inserting or deleting a node,
            """
            self.update_heights(recursive=False)
            self.update_balances(False)

            while self.balance < -1 or self.balance > 1:
                if self.balance > 1:
                    if self.node.left.balance < 0:
                        self.node.left.rotate_left()
                        self.update_heights()
                        self.update_balances()
                    self.rotate_right()
                    self.update_heights()
                    self.update_balances()

                if self.balance < -1:
                    if self.node.right.balance > 0:
                        self.node.right.rotate_right()
                        self.update_heights()
                        self.update_balances()
                    self.rotate_left()
                    self.update_heights()
                    self.update_balances()

        def update_heights(self, recursive=True):
            """
            Update tree height
            """
            if self.node:
                if recursive:
                    if self.node.left:
                        self.node.left.update_heights()
                    if self.node.right:
                        self.node.right.update_heights()

                self.height = 1 + max(self.node.left.height, self.node.right.height)
            else:
                self.height = -1

        def update_balances(self, recursive=True):
            """
            Calculate tree balance factor

            """
            if self.node:
                if recursive:
                    if self.node.left:
                        self.node.left.update_balances()
                    if self.node.right:
                        self.node.right.update_balances()

                self.balance = self.node.left.height - self.node.right.height
            else:
                self.balance = 0

        def rotate_right(self):
            """
            Right rotation
            """
            new_root = self.node.left.node
            new_left_sub = new_root.right.node
            old_root = self.node

            self.node = new_root
            old_root.left.node = new_left_sub
            new_root.right.node = old_root

        def rotate_left(self):
            """
            Left rotation
            """
            new_root = self.node.right.node
            new_left_sub = new_root.left.node
            old_root = self.node

            self.node = new_root
            old_root.right.node = new_left_sub
            new_root.left.node = old_root

        def in_order_traverse(self):
            """
            In-order traversal of the tree
            """
            result = []

            if not self.node:
                return result

            result.extend(self.node.left.in_order_traverse())
            result.append(self.node.key)
            result.extend(self.node.right.in_order_traverse())
            return result



21.2 bst
------------------------------------------


bst - array to bst
------------------------------------------
Given an array where elements are sorted in ascending order,
convert it to a height balanced BST.

.. code-block:: python

    class TreeNode(object):
        def __init__(self, x):
            self.val = x
            self.left = None
            self.right = None


    def array_to_bst(nums):
        if not nums:
            return None
        mid = len(nums)//2
        node = TreeNode(nums[mid])
        node.left = array_to_bst(nums[:mid])
        node.right = array_to_bst(nums[mid+1:])
        return node



bst - bst closest value
------------------------------------------
# Given a non-empty binary search tree and a target value,
# find the value in the BST that is closest to the target.

# Note:
# Given target value is a floating point.
# You are guaranteed to have only one unique value in the BST
# that is closest to the target.


# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

.. code-block:: python

    def closest_value(root, target):
        """
        :type root: TreeNode
        :type target: float
        :rtype: int
        """
        a = root.val
        kid = root.left if target < a else root.right
        if not kid:
            return a
        b = closest_value(kid, target)
        return min((a,b), key=lambda x: abs(target-x))


bst - bst
------------------------------------------
Implement Binary Search Tree. It has method:
1. Insert
2. Search
3. Size
4. Traversal (Preorder, Inorder, Postorder)

.. code-block:: python

    import unittest

    class Node(object):
        def __init__(self, data):
            self.data = data
            self.left = None
            self.right = None

    class BST(object):
        def __init__(self):
            self.root = None

        def get_root(self):
            return self.root

        """
            Get the number of elements
            Using recursion. Complexity O(logN)
        """
        def size(self):
            return self.recur_size(self.root)

        def recur_size(self, root):
            if root is None:
                return 0
            else:
                return 1 + self.recur_size(root.left) + self.recur_size(root.right)

        """
            Search data in bst
            Using recursion. Complexity O(logN)
        """
        def search(self, data):
            return self.recur_search(self.root, data)

        def recur_search(self, root, data):
            if root is None:
                return False
            if root.data == data:
                return True
            elif data > root.data:     # Go to right root
                return self.recur_search(root.right, data)
            else:                      # Go to left root
                return self.recur_search(root.left, data)

        """
            Insert data in bst
            Using recursion. Complexity O(logN)
        """
        def insert(self, data):
            if self.root:
                return self.recur_insert(self.root, data)
            else:
                self.root = Node(data)
                return True

        def recur_insert(self, root, data):
            if root.data == data:      # The data is already there
                return False
            elif data < root.data:     # Go to left root
                if root.left:          # If left root is a node
                    return self.recur_insert(root.left, data)
                else:                  # left root is a None
                    root.left = Node(data)
                    return True
            else:                      # Go to right root
                if root.right:         # If right root is a node
                    return self.recur_insert(root.right, data)
                else:
                    root.right = Node(data)
                    return True

        """
            Preorder, Postorder, Inorder traversal bst
        """
        def preorder(self, root):
            if root:
                print(str(root.data), end = ' ')
                self.preorder(root.left)
                self.preorder(root.right)

        def inorder(self, root):
            if root:
                self.inorder(root.left)
                print(str(root.data), end = ' ')
                self.inorder(root.right)

        def postorder(self, root):
            if root:
                self.postorder(root.left)
                self.postorder(root.right)
                print(str(root.data), end = ' ')

    """
        The tree is created for testing:

                        10
                     /      \
                   6         15
                  / \       /   \
                4     9   12      24
                     /          /    \
                    7         20      30
                             /
                           18
    """

    class TestSuite(unittest.TestCase):
        def setUp(self):
            self.tree = BST()
            self.tree.insert(10)
            self.tree.insert(15)
            self.tree.insert(6)
            self.tree.insert(4)
            self.tree.insert(9)
            self.tree.insert(12)
            self.tree.insert(24)
            self.tree.insert(7)
            self.tree.insert(20)
            self.tree.insert(30)
            self.tree.insert(18)

        def test_search(self):
            self.assertTrue(self.tree.search(24))
            self.assertFalse(self.tree.search(50))

        def test_size(self):
            self.assertEqual(11, self.tree.size())

    if __name__ == '__main__':
        unittest.main()



bst - BSTiterator
------------------------------------------


.. code-block:: python

    class BSTIterator:
        def __init__(self, root):
            self.stack = []
            while root:
                self.stack.append(root)
                root = root.left

        def has_next(self):
            return bool(self.stack)

        def next(self):
            node = self.stack.pop()
            tmp = node
            if tmp.right:
                tmp = tmp.right
                while tmp:
                    self.stack.append(tmp)
                    tmp = tmp.left
            return node.val




bst - count left node
------------------------------------------
Write a function count_left_node returns the number of left children in the
tree. For example: the following tree has four left children (the nodes
storing the values 6, 3, 7, and 10):

                    9
                 /      \
               6         12
              / \       /   \
            3     8   10      15
                 /              \
                7                18

    count_left_node = 4


.. code-block:: python

    import unittest
    from bst import Node
    from bst import bst

    def count_left_node(root):
        if root is None:
            return 0
        elif root.left is None:
            return count_left_node(root.right)
        else:
            return 1 + count_left_node(root.left) + count_left_node(root.right)

    """
        The tree is created for testing:

                        9
                     /      \
                   6         12
                  / \       /   \
                3     8   10      15
                     /              \
                    7                18

        count_left_node = 4

    """

    class TestSuite(unittest.TestCase):
        def setUp(self):
            self.tree = bst()
            self.tree.insert(9)
            self.tree.insert(6)
            self.tree.insert(12)
            self.tree.insert(3)
            self.tree.insert(8)
            self.tree.insert(10)
            self.tree.insert(15)
            self.tree.insert(7)
            self.tree.insert(18)

        def test_count_left_node(self):
            self.assertEqual(4, count_left_node(self.tree.root))

    if __name__ == '__main__':
        unittest.main()



bst - delete node
------------------------------------------
Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

Basically, the deletion can be divided into two stages:

Search for a node to remove.
If the node is found, delete the node.
Note: Time complexity should be O(height of tree).

Example:

root = [5,3,6,2,4,null,7]
key = 3

    5
   / \
  3   6
 / \   \
2   4   7

Given key to delete is 3. So we find the node with value 3 and delete it.

One valid answer is [5,4,6,2,null,null,7], shown in the following BST.

    5
   / \
  4   6
 /     \
2       7

Another valid answer is [5,2,6,null,4,null,7].

    5
   / \
  2   6
   \   \
    4   7

.. code-block:: python


    class Solution(object):
        def delete_node(self, root, key):
            """
            :type root: TreeNode
            :type key: int
            :rtype: TreeNode
            """
            if not root: return None

            if root.val == key:
                if root.left:
                    # Find the right most leaf of the left sub-tree
                    left_right_most = root.left
                    while left_right_most.right:
                        left_right_most = left_right_most.right
                    # Attach right child to the right of that leaf
                    left_right_most.right = root.right
                    # Return left child instead of root, a.k.a delete root
                    return root.left
                else:
                    return root.right
            # If left or right child got deleted, the returned root is the child of the deleted node.
            elif root.val > key:
                root.left = self.deleteNode(root.left, key)
            else:
                root.right = self.deleteNode(root.right, key)
            return root



bst - depth sum
------------------------------------------
Write a function depthSum returns the sum of the values stored
in a binary search tree of integers weighted by the depth of each value.

For example:

                    9
                 /      \
               6         12
              / \       /   \
            3     8   10      15
                 /              \
                7                18

    depth_sum = 1*9 + 2*(6+12) + 3*(3+8+10+15) + 4*(7+18)

.. code-block:: python


    import unittest
    from bst import Node
    from bst import bst

    def depth_sum(root, n):
        if root:
            return recur_depth_sum(root, 1)

    def recur_depth_sum(root, n):
        if root is None:
            return 0
        elif root.left is None and root.right is None:
            return root.data * n
        else:
            return n * root.data + recur_depth_sum(root.left, n+1) + recur_depth_sum(root.right, n+1)

    """
        The tree is created for testing:

                        9
                     /      \
                   6         12
                  / \       /   \
                3     8   10      15
                     /              \
                    7                18

        depth_sum = 1*9 + 2*(6+12) + 3*(3+8+10+15) + 4*(7+18)

    """

    class TestSuite(unittest.TestCase):
        def setUp(self):
            self.tree = bst()
            self.tree.insert(9)
            self.tree.insert(6)
            self.tree.insert(12)
            self.tree.insert(3)
            self.tree.insert(8)
            self.tree.insert(10)
            self.tree.insert(15)
            self.tree.insert(7)
            self.tree.insert(18)

        def test_depth_sum(self):
            self.assertEqual(253, depth_sum(self.tree.root, 4))

    if __name__ == '__main__':
        unittest.main()


bst - height
------------------------------------------
Write a function height returns the height of a tree. The height is defined to
be the number of levels. The empty tree has height 0, a tree of one node has
height 1, a root node with one or two leaves as children has height 2, and so on
For example: height of tree is 4

                    9
                 /      \
               6         12
              / \       /   \
            3     8   10      15
                 /              \
                7                18

    height = 4

.. code-block:: python

    import unittest
    from bst import Node
    from bst import bst

    def height(root):
        if root is None:
            return 0
        else:
            return 1 + max(height(root.left), height(root.right))


        The tree is created for testing:

                        9
                     /      \
                   6         12
                  / \       /   \
                3     8   10      15
                     /              \
                    7                18

        count_left_node = 4



    class TestSuite(unittest.TestCase):
        def setUp(self):
            self.tree = bst()
            self.tree.insert(9)
            self.tree.insert(6)
            self.tree.insert(12)
            self.tree.insert(3)
            self.tree.insert(8)
            self.tree.insert(10)
            self.tree.insert(15)
            self.tree.insert(7)
            self.tree.insert(18)

        def test_height(self):
            self.assertEqual(4, height(self.tree.root))

    if __name__ == '__main__':
        unittest.main()



bst - is bst
------------------------------------------
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes
with keys less than the node's key.
The right subtree of a node contains only nodes
with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
Example 1:
    2
   / \
  1   3
Binary tree [2,1,3], return true.
Example 2:
    1
   / \
  2   3
Binary tree [1,2,3], return false.

.. code-block:: python

    def is_bst(root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root:
            return True
        stack = []
        pre = None
        while root and stack:
            while root:
                stack.append(root)
                root = root.left
            root = stack.pop()
            if pre and root.val <= pre.val:
                return False
            pre = root
            root = root.right
        return True



bst - kth smallest
------------------------------------------


.. code-block:: python

    class Node:

        def __init__(self, val, left=None, right=None):
            self.val = val
            self.left = left
            self.right = right


    def kth_smallest(root, k):
        stack = []
        while root or stack:
            while root:
                stack.append(root)
                root = root.left
            root = stack.pop()
            k -= 1
            if k == 0:
                break
            root = root.right
        return root.val


    class Solution(object):
        def kth_smallest(self, root, k):
            """
            :type root: TreeNode
            :type k: int
            :rtype: int
            """
            count = []
            self.helper(root, count)
            return count[k-1]

        def helper(self, node, count):
            if not node:
                return

            self.helper(node.left, count)
            count.append(node.val)
            self.helper(node.right, count)

    if __name__ == '__main__':
        n1 = Node(100)
        n2 = Node(50)
        n3 = Node(150)
        n4 = Node(25)
        n5 = Node(75)
        n6 = Node(125)
        n7 = Node(175)
        n1.left, n1.right = n2, n3
        n2.left, n2.right = n4, n5
        n3.left, n3.right = n6, n7
        print(kth_smallest(n1, 2))
        print(Solution().kth_smallest(n1, 2))


bst - lowest common ancestor
------------------------------------------
Given a binary search tree (BST),
find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia:
    “The lowest common ancestor is defined between two
    nodes v and w as the lowest node in T that has both v and w
    as descendants (where we allow a node to be a descendant of itself).”

        _______6______
       /              \
    ___2__          ___8__
   /      \        /      \
   0      _4       7       9
         /  \
         3   5

For example, the lowest common ancestor (LCA) of nodes 2 and 8 is 6.
Another example is LCA of nodes 2 and 4 is 2,
since a node can be a descendant of itself according to the LCA definition.



.. code-block:: python

    def lowest_common_ancestor(root, p, q):
        """
        :type root: Node
        :type p: Node
        :type q: Node
        :rtype: Node
        """
        while root:
            if p.val > root.val < q.val:
                root = root.right
            elif p.val < root.val > q.val:
                root = root.left
            else:
                return root



bst - num empty
------------------------------------------
Write a function num_empty returns returns the number of empty branches in a
tree. Function should count the total number of empty branches among the nodes
of the tree. A leaf node has two empty branches. In the case, if root is None,
it considered as a 1 empty branch
For example: the following tree has 10 empty branch (* is empty branch)

                    9 __
                 /      \___
               6            12
              / \          /   \
            3     8       10      15
          /  \   / \     /  \    /   \
         *    * 7   *   *    *  *    18
               / \                   /  \
              *   *                 *    *

    empty_branch = 10


.. code-block:: python

    import unittest
    from bst import Node
    from bst import bst

    def num_empty(root):
        if root is None:
            return 1
        elif root.left is None and root.right:
            return 1 + num_empty(root.right)
        elif root.right is None and root.left:
            return 1 + num_empty(root.left)
        else:
            return num_empty(root.left) + num_empty(root.right)

    """
        The tree is created for testing:

                        9
                     /      \
                   6         12
                  / \       /   \
                3     8   10      15
                     /              \
                    7                18

        num_empty = 10

    """

    class TestSuite(unittest.TestCase):
        def setUp(self):
            self.tree = bst()
            self.tree.insert(9)
            self.tree.insert(6)
            self.tree.insert(12)
            self.tree.insert(3)
            self.tree.insert(8)
            self.tree.insert(10)
            self.tree.insert(15)
            self.tree.insert(7)
            self.tree.insert(18)

        def test_num_empty(self):
            self.assertEqual(10, num_empty(self.tree.root))

    if __name__ == '__main__':
        unittest.main()


bst - predecessor
------------------------------------------


.. code-block:: python

    def predecessor(root, node):
        pred = None
        while root:
            if node.val > root.val:
                pred = root
                root = root.right
            else:
                root = root.left
        return pred



bst - serialize deserialize
------------------------------------------


.. code-block:: python

    class TreeNode(object):
        def __init__(self, x):
            self.val = x
            self.left = None
            self.right = None


    def serialize(root):
        def build_string(node):
            if node:
                vals.append(str(node.val))
                build_string(node.left)
                build_string(node.right)
            else:
                vals.append("#")
        vals = []
        build_string(root)
        return " ".join(vals)


    def deserialize(data):
        def build_tree():
            val = next(vals)
            if val == "#":
                return None
            node = TreeNode(int(val))
            node.left = build_tree()
            node.right = build_tree()
            return node
        vals = iter(data.split())
        return build_tree()


bst - successor
------------------------------------------


.. code-block:: python


    def successor(root, node):
        succ = None
        while root:
            if node.val < root.val:
                succ = root
                root = root.left
            else:
                root = root.right
        return succ


bst - unique bst
------------------------------------------
Given n, how many structurally unique BST's
(binary search trees) that store values 1...n?

For example,
Given n = 3, there are a total of 5 unique BST's.

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
"""


"""
Taking 1~n as root respectively:
1 as root: # of trees = F(0) * F(n-1)  // F(0) == 1
2 as root: # of trees = F(1) * F(n-2)
3 as root: # of trees = F(2) * F(n-3)
...
n-1 as root: # of trees = F(n-2) * F(1)
n as root:   # of trees = F(n-1) * F(0)

So, the formulation is:
F(n) = F(0) * F(n-1) + F(1) * F(n-2) + F(2) * F(n-3) + ... + F(n-2) * F(1) + F(n-1) * F(0)

.. code-block:: python

    def num_trees(n):
        """
        :type n: int
        :rtype: int
        """
        dp = [0] * (n+1)
        dp[0] = 1
        dp[1] = 1
        for i in range(2, n+1):
            for j in range(i+1):
                dp[i] += dp[i-j] * dp[j-1]
        return dp[-1]


21.3 red black tree
------------------------------------------
Implementation of Red-Black tree.


.. code-block:: python

    class RBNode:
        def __init__(self, val, is_red, parent=None, left=None, right=None):
            self.val = val
            self.parent = parent
            self.left = left
            self.right = right
            self.color = is_red


    class RBTree:
        def __init__(self):
            self.root = None

        def left_rotate(self, node):
            # set the node as the left child node of the current node's right node
            right_node = node.right
            if right_node is None:
                return
            else:
                # right node's left node become the right node of current node
                node.right = right_node.left
                if right_node.left is not None:
                    right_node.left.parent = node
                right_node.parent = node.parent
                # check the parent case
                if node.parent is None:
                    self.root = right_node
                elif node is node.parent.left:
                    node.parent.left = right_node
                else:
                    node.parent.right = right_node
                right_node.left = node
                node.parent = right_node

        def right_rotate(self, node):
            # set the node as the right child node of the current node's left node
            left_node = node.left
            if left_node is None:
                return
            else:
                # left node's right  node become the left node of current node
                node.left = left_node.right
                if left_node.right is not None:
                    left_node.right.parent = node
                left_node.parent = node.parent
                # check the parent case
                if node.parent is None:
                    self.root = left_node
                elif node is node.parent.left:
                    node.parent.left = left_node
                else:
                    node.parent.right = left_node
                left_node.right = node
                node.parent = left_node

        def insert(self, node):
            # the inserted node's color is default is red
            root = self.root
            insert_node_parent = None
            # find the position of inserted node
            while root is not None:
                insert_node_parent = root
                if insert_node_parent.val < node.val:
                    root = root.right
                else:
                    root = root.left
            # set the n ode's parent node
            node.parent = insert_node_parent
            if insert_node_parent is None:
                # case 1  inserted tree is null
                self.root = node
            elif insert_node_parent.val > node.val:
                # case 2 not null and find left or right
                insert_node_parent.left = node
            else:
                insert_node_parent.right = node
            node.left = None
            node.right = None
            node.color = 1
            # fix the tree to
            self.fix_insert(node)

        def fix_insert(self, node):
            # case 1 the parent is null, then set the inserted node as root and color = 0
            if node.parent is None:
                node.color = 0
                self.root = node
                return
                # case 2 the parent color is black, do nothing
            # case 3 the parent color is red
            while node.parent and node.parent.color is 1:
                if node.parent is node.parent.parent.left:
                    uncle_node = node.parent.parent.right
                    if uncle_node and uncle_node.color is 1:
                        # case 3.1 the uncle node is red
                        # then set parent and uncle color is black and grandparent is red
                        # then node => node.parent
                        node.parent.color = 0
                        node.parent.parent.right.color = 0
                        node.parent.parent.color = 1
                        node = node.parent.parent
                        continue
                    elif node is node.parent.right:
                        # case 3.2 the uncle node is black or null, and the node is right of parent
                        # then set his parent node is current node
                        # left rotate the node and continue the next
                        node = node.parent
                        self.left_rotate(node)
                    # case 3.3 the uncle node is black and parent node is left
                    # then parent node set black and grandparent set red
                    node.parent.color = 0
                    node.parent.parent.color = 1
                    self.right_rotate(node.parent.parent)
                else:
                    uncle_node = node.parent.parent.left
                    if uncle_node and uncle_node.color is 1:
                        # case 3.1 the uncle node is red
                        # then set parent and uncle color is black and grandparent is red
                        # then node => node.parent
                        node.parent.color = 0
                        node.parent.parent.left.color = 0
                        node.parent.parent.color = 1
                        node = node.parent.parent
                        continue
                    elif node is node.parent.left:
                        # case 3.2 the uncle node is black or null, and the node is right of parent
                        # then set his parent node is current node
                        # left rotate the node and continue the next
                        node = node.parent
                        self.right_rotate(node)
                    # case 3.3 the uncle node is black and parent node is left
                    # then parent node set black and grandparent set red
                    node.parent.color = 0
                    node.parent.parent.color = 1
                    self.left_rotate(node.parent.parent)
            self.root.color = 0

        def transplant(self, node_u, node_v):
            """
            replace u with v
            :param node_u: replaced node
            :param node_v:
            :return: None
            """
            if node_u.parent is None:
                self.root = node_v
            elif node_u is node_u.parent.left:
                node_u.parent.left = node_v
            elif node_u is node_u.parent.right:
                node_u.parent.right = node_v
            # check is node_v is None
            if node_v:
                node_v.parent = node_u.parent

        def maximum(self, node):
            """
            find the max node when node regard as a root node
            :param node:
            :return: max node
            """
            temp_node = node
            while temp_node.right is not None:
                temp_node = temp_node.right
            return temp_node

        def minimum(self, node):
            """
            find the minimum node when node regard as a root node
            :param node:
            :return: minimum node
            """
            temp_node = node
            while temp_node.left:
                temp_node = temp_node.left
            return temp_node

        def delete(self, node):
            # find the node position
            node_color = node.color
            if node.left is None:
                temp_node = node.right
                self.transplant(node, node.right)
            elif node.right is None:
                temp_node = node.left
                self.transplant(node, node.left)
            else:
                # both child exits ,and find minimum child of right child
                node_min = self.minimum(node.right)
                node_color = node_min.color
                temp_node = node_min.right
                ##
                if node_min.parent != node:
                    self.transplant(node_min, node_min.right)
                    node_min.right = node.right
                    node_min.right.parent = node_min
                self.transplant(node, node_min)
                node_min.left = node.left
                node_min.left.parent = node_min
                node_min.color = node.color
            # when node is black, then need to fix it with 4 cases
            if node_color == 0:
                self.delete_fixup(temp_node)

        def delete_fixup(self, node):
            # 4 cases
            while node != self.root and node.color == 0:
                # node is not root and color is black
                if node == node.parent.left:
                    # node is left node
                    node_brother = node.parent.right

                    # case 1: node's red, can not get black node
                    # set brother is black and parent is red
                    if node_brother.color == 1:
                        node_brother.color = 0
                        node.parent.color = 1
                        self.left_rotate(node.parent)
                        node_brother = node.parent.right

                    # case 2: brother node is black, and its children node is both black
                    if (node_brother.left is None or node_brother.left.color == 0) and (
                                    node_brother.right is None or node_brother.right.color == 0):
                        node_brother.color = 1
                        node = node.parent
                    else:

                        # case 3: brother node is black , and its left child node is red and right is black
                        if node_brother.right is None or node_brother.right.color == 0:
                            node_brother.color = 1
                            node_brother.left.color = 0
                            self.right_rotate(node_brother)
                            node_brother = node.parent.right

                        # case 4: brother node is black, and right is red, and left is any color
                        node_brother.color = node.parent.color
                        node.parent.color = 0
                        node_brother.right.color = 0
                        self.left_rotate(node.parent)
                    node = self.root
                    break
                else:
                    node_brother = node.parent.left
                    if node_brother.color == 1:
                        node_brother.color = 0
                        node.parent.color = 1
                        self.left_rotate(node.parent)
                        node_brother = node.parent.right
                    if (node_brother.left is None or node_brother.left.color == 0) and (
                                    node_brother.right is None or node_brother.right.color == 0):
                        node_brother.color = 1
                        node = node.parent
                    else:
                        if node_brother.left is None or node_brother.left.color == 0:
                            node_brother.color = 1
                            node_brother.right.color = 0
                            self.left_rotate(node_brother)
                            node_brother = node.parent.left
                        node_brother.color = node.parent.color
                        node.parent.color = 0
                        node_brother.left.color = 0
                        self.right_rotate(node.parent)
                    node = self.root
                    break
            node.color = 0

        def inorder(self):
            res = []
            if not self.root:
                return res
            stack = []
            root = self.root
            while root or stack:
                while root:
                    stack.append(root)
                    root = root.left
                root = stack.pop()
                res.append({'val': root.val, 'color': root.color})
                root = root.right
            return res


    if __name__ == "__main__":
        rb = RBTree()
        children = [11, 2, 14, 1, 7, 15, 5, 8, 4]
        for child in children:
            node = RBNode(child, 1)
            print(child)
            rb.insert(node)
        print(rb.inorder())



21.4 segment tree
------------------------------------------
Segment_tree creates a segment tree with a given array and function,
allowing queries to be done later in log(N) time
function takes 2 values and returns a same type value

.. code-block:: python

    class SegmentTree:
        def __init__(self,arr,function):
            self.segment = [0 for x in range(3*len(arr)+3)]
            self.arr = arr
            self.fn = function
            self.maketree(0,0,len(arr)-1)

        def make_tree(self,i,l,r):
            if l==r:
                self.segment[i] = self.arr[l]
            elif l<r:
                self.make_tree(2*i+1,l,int((l+r)/2))
                self.make_tree(2*i+2,int((l+r)/2)+1,r)
                self.segment[i] = self.fn(self.segment[2*i+1],self.segment[2*i+2])

        def __query(self,i,L,R,l,r):
            if l>R or r<L or L>R or l>r:
                return None
            if L>=l and R<=r:
                return self.segment[i]
            val1 = self.__query(2*i+1,L,int((L+R)/2),l,r)
            val2 = self.__query(2*i+2,int((L+R+2)/2),R,l,r)
            print(L,R," returned ",val1,val2)
            if val1 != None:
                if val2 != None:
                    return self.fn(val1,val2)
                return val1
            return val2


        def query(self,L,R):
            return self.__query(0,0,len(self.arr)-1,L,R)

    '''
    Example -
    mytree = SegmentTree([2,4,5,3,4],max)
    mytree.query(2,4)
    mytree.query(0,3) ...

    mytree = SegmentTree([4,5,2,3,4,43,3],sum)
    mytree.query(1,8)

21.5 traversal
------------------------------------------


.. code-block:: python


traversal - inorder
------------------------------------------
Time complexity : O(n)

.. code-block:: python

    class Node:

        def __init__(self, val, left=None, right=None):
            self.val = val
            self.left = left
            self.right = right


    def inorder(root):
        res = []
        if not root:
            return res
        stack = []
        while root or stack:
            while root:
                stack.append(root)
                root = root.left
            root = stack.pop()
            res.append(root.val)
            root = root.right
        return res

    # Recursive Implementation
    def inorder_rec(root, res=None):
        if root is None:
            return []
        if res is None:
            res = []
        inorder_rec(root.left, res)
        res.append(root.val)
        inorder_rec(root.right, res)
        return res

    if __name__ == '__main__':
        n1 = Node(100)
        n2 = Node(50)
        n3 = Node(150)
        n4 = Node(25)
        n5 = Node(75)
        n6 = Node(125)
        n7 = Node(175)
        n1.left, n1.right = n2, n3
        n2.left, n2.right = n4, n5
        n3.left, n3.right = n6, n7

        assert inorder(n1)     == [25, 50, 75, 100, 125, 150, 175]
        assert inorder_rec(n1) == [25, 50, 75, 100, 125, 150, 175]



traversal - level order
------------------------------------------
Given a binary tree, return the level order traversal of
its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree [3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7
return its level order traversal as:
[
  [3],
  [9,20],
  [15,7]
]

.. code-block:: python

    def level_order(root):
        ans = []
        if not root:
            return ans
        level = [root]
        while level:
            current = []
            new_level = []
            for node in level:
                current.append(node.val)
                if node.left:
                    new_level.append(node.left)
                if node.right:
                    new_level.append(node.right)
            level = new_level
            ans.append(current)
        return ans


traversal - postorder
------------------------------------------
Time complexity : O(n)

.. code-block:: python

    class Node:

        def __init__(self, val, left=None, right=None):
            self.val = val
            self.left = left
            self.right = right


    def postorder(root):
        res_temp = []
        res = []
        if not root:
            return res
        stack = []
        stack.append(root)
        while stack:
            root = stack.pop()
            res_temp.append(root.val)
            if root.left:
                stack.append(root.left)
            if root.right:
                stack.append(root.right)
        while res_temp:
            res.append(res_temp.pop())
        return res

    # Recursive Implementation
    def postorder_rec(root, res=None):
        if root is None:
            return []
        if res is None:
            res = []
        postorder_rec(root.left, res)
        postorder_rec(root.right, res)
        res.append(root.val)
        return res




traversal - preorder
------------------------------------------
Time complexity : O(n)

.. code-block:: python

    class Node:

        def __init__(self, val, left=None, right=None):
            self.val = val
            self.left = left
            self.right = right


    def preorder(root):
        res = []
        if not root:
            return res
        stack = []
        stack.append(root)
        while stack:
            root = stack.pop()
            res.append(root.val)
            if root.right:
                stack.append(root.right)
            if root.left:
                stack.append(root.left)
        return res

    # Recursive Implementation
    def preorder_rec(root, res=None):
        if root is None:
            return []
        if res is None:
            res = []
        res.append(root.val)
        preorder_rec(root.left, res)
        preorder_rec(root.right, res)
        return res




traversal - zigzag
------------------------------------------
Given a binary tree, return the zigzag level order traversal
of its nodes' values.
(ie, from left to right, then right to left
for the next level and alternate between).

For example:
Given binary tree [3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7
return its zigzag level order traversal as:
[
  [3],
  [20,9],
  [15,7]
]

.. code-block:: python


    def zigzag_level(root):
        res = []
        if not root:
            return res
        level = [root]
        flag = 1
        while level:
            current = []
            new_level = []
            for node in level:
                current.append(node.val)
                if node.left:
                    new_level.append(node.left)
                if node.right:
                    new_level.append(node.right)
            level = new_level
            res.append(current[::flag])
            flag *= -1
        return res


21.6 trie
------------------------------------------

trie - add and search
------------------------------------------
We are asked to design an efficient data structure
that allows us to add and search for words.
The search can be a literal word or regular expression
containing “.”, where “.” can be any letter.

Example:
addWord(“bad”)
addWord(“dad”)
addWord(“mad”)
search(“pad”) -> false
search(“bad”) -> true
search(“.ad”) -> true
search(“b..”) -> true


.. code-block:: python


    import collections

    class TrieNode(object):
        def __init__(self, letter, is_terminal=False):
            self.children = dict()
            self.letter = letter
            self.is_terminal = is_terminal

    class WordDictionary(object):
        def __init__(self):
            self.root = TrieNode("")

        def add_word(self, word):
            cur = self.root
            for letter in word:
                if letter not in cur.children:
                    cur.children[letter] = TrieNode(letter)
                cur = cur.children[letter]
            cur.is_terminal = True

        def search(self, word, node=None):
            cur = node
            if not cur:
                cur = self.root
            for i, letter in enumerate(word):
                # if dot
                if letter == ".":
                    if i == len(word) - 1: # if last character
                        for child in cur.children.itervalues():
                            if child.is_terminal:
                                return True
                        return False
                    for child in cur.children.itervalues():
                        if self.search(word[i+1:], child) == True:
                            return True
                    return False
                # if letter
                if letter not in cur.children:
                    return False
                cur = cur.children[letter]
            return cur.is_terminal

    class WordDictionary2(object):
        def __init__(self):
            self.word_dict = collections.defaultdict(list)


        def add_word(self, word):
            if word:
                self.word_dict[len(word)].append(word)

        def search(self, word):
            if not word:
                return False
            if '.' not in word:
                return word in self.word_dict[len(word)]
            for v in self.word_dict[len(word)]:
                # match xx.xx.x with yyyyyyy
                for i, ch in enumerate(word):
                    if ch != v[i] and ch != '.':
                        break
                else:
                    return True
            return False


trie - trie
------------------------------------------
Implement a trie with insert, search, and startsWith methods.

Note:
You may assume that all inputs are consist of lowercase letters a-z.


.. code-block:: python

    import collections


    class TrieNode:
        def __init__(self):
            self.children = collections.defaultdict(TrieNode)
            self.is_word = False


    class Trie:
        def __init__(self):
            self.root = TrieNode()

        def insert(self, word):
            current = self.root
            for letter in word:
                current = current.children[letter]
            current.is_word = True

        def search(self, word):
            current = self.root
            for letter in word:
                current = current.children.get(letter)
                if current is None:
                    return False
            return current.is_word

        def starts_with(self, prefix):
            current = self.root
            for letter in prefix:
                current = current.children.get(letter)
                if current is None:
                    return False
            return True





21.7 bin tree to list
------------------------------------------



.. code-block:: python


    class Node():
        def __init__(self, val = 0):
            self.val = val
            self.left = None
            self.right = None

    def bin_tree_to_list(root):
        """
        type root: root class
        """
        if not root:
            return root
        root = bin_tree_to_list_util(root)
        while root.left:
            root = root.left
        return root

    def bin_tree_to_list_util(root):
        if not root:
            return root
        if root.left:
            left = bin_tree_to_list_util(root.left)
            while left.right:
                left = left.right
            left.right = root
            root.left = left
        if root.right:
            right = bin_tree_to_list_util(root.right)
            while right.left:
                right = right.left
            right.left = root
            root.right = right
        return root

    def print_tree(root):
        while root:
            print(root.val)
            root = root.right

    tree = Node(10)
    tree.left = Node(12)
    tree.right = Node(15)
    tree.left.left  = Node(25)
    tree.left.left.right  = Node(100)
    tree.left.right = Node(30)
    tree.right.left = Node(36)

    head = bin_tree_to_list(tree)
    print_tree(head)






21.8 binary tree paths
------------------------------------------

.. code-block:: python

    def binary_tree_paths(root):
        res = []
        if not root:
            return res
        dfs(res, root, str(root.val))
        return res

    def dfs(res, root, cur):
        if not root.left and not root.right:
            res.append(cur)
        if root.left:
            dfs(res, root.left, cur+'->'+str(root.left.val))
        if root.right:
            dfs(res, root.right, cur+'->'+str(root.right.val))



21.9 deepest left
------------------------------------------
# Given a binary tree, find the deepest node
# that is the left child of its parent node.

# Example:

     # 1
   # /   \
  # 2     3
 # / \     \
# 4   5     6
           # \
            # 7
# should return 4.


.. code-block:: python

    class Node:
        def __init__(self, val = None):
            self.left = None
            self.right = None
            self.val = val

    class DeepestLeft:
        def __init__(self):
            self.depth = 0
            self.Node = None

    def find_deepest_left(root, is_left, depth, res):
        if not root:
            return
        if is_left and depth > res.depth:
            res.depth = depth
            res.Node = root
        find_deepest_left(root.left, True, depth + 1, res)
        find_deepest_left(root.right, False, depth + 1, res)

    root = Node(1)
    root.left = Node(2)
    root.right = Node(3)
    root.left.left = Node(4)
    root.left.right = Node(5)
    root.right.right = Node(6)
    root.right.right.right = Node(7)

    res = DeepestLeft()
    find_deepest_left(root, True, 1, res)
    if res.Node:
        print(res.Node.val)



21.10 invert tree
------------------------------------------
# invert a binary tree


.. code-block:: python

    def reverse(root):
        if not root:
            return
        root.left, root.right = root.right, root.left
        if root.left:
            reverse(root.left)
        if root.right:
            reverse(root.right)


21.11 is balanced
------------------------------------------


.. code-block:: python

    def is_balanced(root):
        """
        O(N) solution
        """
        return -1 != get_depth(root)

    def get_depth(root):
        """
        return 0 if unbalanced else depth + 1
        """
        if not root:
            return 0
        left  = get_depth(root.left)
        right = get_depth(root.right)
        if abs(left-right) > 1 or left == -1 or right == -1:
            return -1
        return 1 + max(left, right)

    ################################

    def is_balanced(root):
        """
        O(N^2) solution
        """
        left = max_height(root.left)
        right = max_height(root.right)
        return abs(left-right) <= 1 and is_balanced(root.left) and is_balanced(root.right)

    def max_height(root):
        if not root:
            return 0
        return max(max_height(root.left), max_height(root.right)) + 1




21.12 is subtree
------------------------------------------
Given two binary trees s and t, check if t is a subtree of s.
A subtree of a tree t is a tree consisting of a node in t and
all of its descendants in t.

Example 1:

Given s:

     3
    / \
   4   5
  / \
 1   2

Given t:

   4
  / \
 1   2
Return true, because t is a subtree of s.

Example 2:

Given s:

     3
    / \
   4   5
  / \
 1   2
    /
   0

Given t:

     3
    /
   4
  / \
 1   2
Return false, because even though t is part of s,
it does not contain all descendants of t.

Follow up:
What if one tree is significantly lager than the other?


.. code-block:: python


    import collections


    def is_subtree(big, small):
        flag = False
        queue = collections.deque()
        queue.append(big)
        while queue:
            node = queue.popleft()
            if node.val == small.val:
                flag = comp(node, small)
                break
            else:
                queue.append(node.left)
                queue.append(node.right)
        return flag


    def comp(p, q):
        if not p and not q:
            return True
        if p and q:
            return p.val == q.val and comp(p.left,q.left) and comp(p.right, q.right)
        return False





21.13 is symmetric
------------------------------------------
Given a binary tree, check whether it is a mirror of
itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

    1
   / \
  2   2
 / \ / \
3  4 4  3
But the following [1,2,2,null,3,null,3] is not:
    1
   / \
  2   2
   \   \
   3    3
Note:
Bonus points if you could solve it both recursively and iteratively.


.. code-block:: python

    # TC: O(b) SC: O(log n)
    def is_symmetric(root):
        if not root:
            return True
        return helper(root.left, root.right)


    def helper(p, q):
        if not p and not q:
            return True
        if not p or not q or q.val != p.val:
            return False
        return helper(p.left, q.right) and helper(p.right, q.left)


    def is_symmetric_iterative(root):
        if not root:
            return True
        stack = [[root.left, root.right]]
        while stack:
            left, right = stack.pop()  # popleft
            if not left and not right:
                continue
            if not left or not right:
                return False
            if left.val == right.val:
                stack.append([left.left, right.right])
                stack.append([left.right, right.left])
            else:
                return False
        return True


21.14 logest consecutive
------------------------------------------
Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node
in the tree along the parent-child connections.
The longest consecutive path need to be from parent to child
(cannot be the reverse).

For example,
   1
    \
     3
    / \
   2   4
        \
         5
Longest consecutive sequence path is 3-4-5, so return 3.
   2
    \
     3
    /
   2
  /
 1


.. code-block:: python

    def longest_consecutive(root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0
        max_len = 0
        dfs(root, 0, root.val, max_len)
        return max_len


    def dfs(root, cur, target, max_len):
        if not root:
            return
        if root.val == target:
            cur += 1
        else:
            cur = 1
        max_len = max(cur, max_len)
        dfs(root.left, cur, root.val+1, max_len)
        dfs(root.right, cur, root.val+1, max_len)


21.15 lowest common ancestor
------------------------------------------
Given a binary tree, find the lowest common ancestor
(LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia:
    “The lowest common ancestor is defined between two nodes
    v and w as the lowest node in T that has both v and w as
    descendants
    (where we allow a node to be a descendant of itself).”

        _______3______
       /              \
    ___5__          ___1__
   /      \        /      \
   6      _2       0       8
         /  \
         7   4
For example, the lowest common ancestor (LCA) of nodes 5 and 1 is 3.
Another example is LCA of nodes 5 and 4 is 5,
since a node can be a descendant of itself according to the LCA definition.


.. code-block:: python


    def lca(root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if not root or root is p or root is q:
            return root
        left = lca(root.left, p, q)
        right = lca(root.right, p, q)
        if left and right:
            return root
        return left if left else right


21.16 max height
------------------------------------------
Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the
longest path from the root node down to the farthest leaf node.


.. code-block:: python

    class Node():
        def __init__(self, val = 0):
            self.val = val
            self.left = None
            self.right = None

    # def max_height(root):
        # if not root:
            # return 0
        # return max(maxDepth(root.left), maxDepth(root.right)) + 1

    # iterative
    def max_height(root):
        if not root:
            return 0
        height = 0
        queue = [root]
        while queue:
            height += 1
            level = []
            while queue:
                node = queue.pop(0)
                if node.left:
                    level.append(node.left)
                if node.right:
                    level.append(node.right)
            queue = level
        return height

    def print_tree(root):
        if root:
            print(root.val)
            print_tree(root.left)
            print_tree(root.right)

    tree = Node(10)
    tree.left = Node(12)
    tree.right = Node(15)
    tree.left.left  = Node(25)
    tree.left.left.right  = Node(100)
    tree.left.right = Node(30)
    tree.right.left = Node(36)

    height = max_height(tree)
    print_tree(tree)
    print("height:", height)


21.17 max path sum
------------------------------------------

.. code-block:: python

    def max_path_sum(root):
        maximum = float("-inf")
        helper(root, maximum)
        return maximum


    def helper(root, maximum):
        if not root:
            return 0
        left = helper(root.left, maximum)
        right = helper(root.right, maximum)
        maximum = max(maximum, left+right+root.val)
        return root.val + maximum


21.18 min height
------------------------------------------

.. code-block:: python


    class Node():
        def __init__(self, val = 0):
            self.val = val
            self.left = None
            self.right = None


    def min_depth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0
        if not root.left or not root.right:
            return max(self.minDepth(root.left), self.minDepth(root.right))+1
        return min(self.minDepth(root.left), self.minDepth(root.right)) + 1


    # iterative
    def min_height(root):
        if not root:
            return 0
        height = 0
        level = [root]
        while level:
            height += 1
            new_level = []
            for node in level:
                if not node.left and not node.right:
                    return height
                if node.left:
                    new_level.append(node.left)
                if node.right:
                    new_level.append(node.right)
            level = new_level
        return height


    def print_tree(root):
        if root:
            print(root.val)
            print_tree(root.left)
            print_tree(root.right)

    tree = Node(10)
    tree.left = Node(12)
    tree.right = Node(15)
    tree.left.left  = Node(25)
    tree.left.left.right  = Node(100)
    tree.left.right = Node(30)
    tree.right.left = Node(36)

    height = min_height(tree)
    print_tree(tree)
    print("height:", height)



21.19 path sum
------------------------------------------
Given a binary tree and a sum, determine if the tree has a root-to-leaf
path such that adding up all the values along the path equals the given sum.

For example:
Given the below binary tree and sum = 22,
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.


.. code-block:: python

    def has_path_sum(root, sum):
        """
        :type root: TreeNode
        :type sum: int
        :rtype: bool
        """
        if not root:
            return False
        if not root.left and not root.right and root.val == sum:
            return True
        sum -= root.val
        return has_path_sum(root.left, sum) or has_path_sum(root.right, sum)


21.20 path sum2
------------------------------------------
Given a binary tree and a sum, find all root-to-leaf
paths where each path's sum equals the given sum.

For example:
Given the below binary tree and sum = 22,
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
return
[
   [5,4,11,2],
   [5,8,4,5]
]


.. code-block:: python

    def path_sum(root, sum):
        if not root:
            return []
        res = []
        dfs(root, sum, [], res)
        return res

    def dfs(root, sum, ls, res):
        if not root.left and not root.right and root.val == sum:
            ls.append(root.val)
            res.append(ls)
        if root.left:
            dfs(root.left, sum-root.val, ls+[root.val], res)
        if root.right:
            dfs(root.right, sum-root.val, ls+[root.val], res)


    # DFS with stack
    def path_sum2(root, s):
        if not root:
            return []
        res = []
        stack = [(root, [root.val])]
        while stack:
            node, ls = stack.pop()
            if not node.left and not node.right and sum(ls) == s:
                res.append(ls)
            if node.left:
                stack.append((node.left, ls+[node.left.val]))
            if node.right:
                stack.append((node.right, ls+[node.right.val]))
        return res


    # BFS with queue
    def path_sum3(root, sum):
        if not root:
            return []
        res = []
        queue = [(root, root.val, [root.val])]
        while queue:
            node, val, ls = queue.pop(0)  # popleft
            if not node.left and not node.right and val == sum:
                res.append(ls)
            if node.left:
                queue.append((node.left, val+node.left.val, ls+[node.left.val]))
            if node.right:
                queue.append((node.right, val+node.right.val, ls+[node.right.val]))
        return res


21.21 pretty print
------------------------------------------
# a -> Adam -> Book -> 4
# b -> Bill -> Computer -> 5
#           -> TV -> 6
#      Jill -> Sports -> 1
# c -> Bill -> Sports -> 3
# d -> Adam -> Computer -> 3
#      Quin -> Computer -> 3
# e -> Quin -> Book -> 5
#           -> TV -> 2
# f -> Adam -> Computer -> 7


.. code-block:: python

    from __future__ import print_function


    def tree_print(tree):
        for key in tree:
            print(key, end=' ')  # end=' ' prevents a newline character
            tree_element = tree[key]  # multiple lookups is expensive, even amortized O(1)!
            for subElem in tree_element:
                print(" -> ", subElem, end=' ')
                if type(subElem) != str:  # OP wants indenting after digits
                    print("\n ")  # newline and a space to match indenting
            print()  # forces a newline

21.22 same tree
------------------------------------------
Given two binary trees, write a function to check
if they are equal or not.

Two binary trees are considered equal if they are
structurally identical and the nodes have the same value.


.. code-block:: python

    def is_same_tree(p, q):
        if not p and not q:
            return True
        if p and q and p.val == q.val:
            return is_same_tree(p.left, q.left) and is_same_tree(p.right, q.right)
        return False

    # Time Complexity O(min(N,M))
    # where N and M are the number of nodes for the trees.

    # Space Complexity O(min(height1, height2))
    # levels of recursion is the mininum height between the two trees.


21.23 tree
------------------------------------------


.. code-block:: python


    class TreeNode:
        def __init__(self, val=0):
            self.val = val
            self.left = None
            self.right = None
