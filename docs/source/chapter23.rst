chapter 23: Unix
=======================================


23.1 path
-----------------------------------



full path
-------------------------------------
Get a full absolute path a file

.. code-block:: python

    import os
    def full_path(file):
        return os.path.abspath(os.path.expanduser(file))



join with slash
-------------------------------------
Both URL and file path joins use slashes as dividers between their parts.
For example:

path/to/dir + file --> path/to/dir/file
path/to/dir/ + file --> path/to/dir/file
http://algorithms.com/ + part --> http://algorithms.com/part
http://algorithms.com + part --> http://algorithms/part

.. code-block:: python

    import os

    def join_with_slash(base, suffix):
        # Remove / trailing
        base = base.rstrip('/')
        # Remove / leading
        suffix = suffix.lstrip('/').rstrip()
        full_path = "{}/{}".format(base, suffix)
        return full_path



simplify path
-------------------------------------
Given an absolute path for a file (Unix-style), simplify it.

For example,
path = "/home/", => "/home"
path = "/a/./b/../../c/", => "/c"

Corner Cases:

Did you consider the case where path = "/../"?
In this case, you should return "/".
Another corner case is the path might contain multiple slashes '/' together, such as "/home//foo/".
In this case, you should ignore redundant slashes and return "/home/foo".

Reference: https://leetcode.com/problems/simplify-path/description/

.. code-block:: python

    import os
    def simplify_path_v1(path):
        return os.path.abspath(path)

    def simplify_path_v2(path):
        stack, tokens = [], path.split("/")
        for token in tokens:
            if token == ".." and stack:
                stack.pop()
            elif token != ".." and token != "." and token:
                stack.append(token)
        return "/" + "/".join(stack)



split
-------------------------------------
Splitting a path into 2 parts
Example:
Input: https://algorithms/unix/test.py   (for url)
Output:
    part[0]: https://algorithms/unix
    part[1]: test.py

Input: algorithms/unix/test.py          (for file path)
Output:
    part[0]: algorithms/unix
    part[1]: test.py

.. code-block:: python


    import os

    def split(path):
        parts = []
        split_part = path.rpartition('/')
        # Takt the origin path without the last part
        parts.append(split_part[0])
        # Take the last element of list
        parts.append(split_part[2])
        return parts