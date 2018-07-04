# 690. Employee Importance (Easy/AC51)
---
You are given a data structure of employee information, which includes the employee's unique id, his importance value and his direct subordinates' id.

For example, employee 1 is the leader of employee 2, and employee 2 is the leader of employee 3. They have importance value 15, 10 and 5, respectively. Then employee 1 has a data structure like [1, 15, [2]], and employee 2 has [2, 10, [3]], and employee 3 has [3, 5, []]. Note that although employee 3 is also a subordinate of employee 1, the relationship is not direct.

Now given the employee information of a company, and an employee id, you need to return the total importance value of this employee and all his subordinates.

Example 1:
Input: [[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1
Output: 11
Explanation:
Employee 1 has importance value 5, and he has two direct subordinates: employee 2 and employee 3. They both have importance value 3. So the total importance value of employee 1 is 5 + 3 + 3 = 11.
Note:
One employee has at most one direct leader and may have several subordinates.
The maximum number of employees won't exceed 2000.

```python
"""
Round1: 60.15% Accepted
"""

"""
# Employee info
class Employee:
    def __init__(self, id, importance, subordinates):
        # It's the unique id of each node.
        # unique id of this employee
        self.id = id
        # the importance value of this employee
        self.importance = importance
        # the id of direct subordinates
        self.subordinates = subordinates
"""
def get_imp(employee_map, id):
        employee = employee_map[id]
        if len(employee.subordinates) == 0:
            return employee.importance
        return employee.importance + sum([get_imp(employee_map, e_id) for e_id in employee.subordinates])
    
class Solution:
    def getImportance(self, employees, id):
        """
        :type employees: Employee
        :type id: int
        :rtype: int
        """
        start = None
        employee_map = dict()
        for employee in employees:
            employee_map[employee.id] = employee
        return get_imp(employee_map, id)
```

# 513. Find Bottom Left Tree Value (Medium/AC56)
---
Given a binary tree, find the leftmost value in the last row of the tree.

Example 1:
Input:

    2
   / \
  1   3

Output:
1
Example 2: 
Input:

        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7

Output:
7
Note: You may assume the tree (i.e., the given root node) is not NULL.

```python
"""
Round1: 31.94% Accepted
"""

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def __init__(self):
        self.level_map = dict()
        self.max_level = 0
        
    def findBottomLeftValue(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        self.traversal(root, 0)
        return self.level_map[self.max_level][0]
        
    def traversal(self, node, level):
        if node is None:
            return
        # print("level: {}, val: {}, side: {}".format(level, node.val, side))
        self.max_level = level if level > self.max_level else self.max_level
        if level not in self.level_map:
            self.level_map[level] = list()
        self.level_map[level] = self.level_map[level] + [node.val]
        self.traversal(node.left, level + 1)
        self.traversal(node.right, level + 1)
```