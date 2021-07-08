参考：https://github.com/youngyangyang04/leetcode-master

# 二叉树

#### [235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

*难度简单*

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

例如，给定如下二叉搜索树: root = [6,2,8,0,4,7,9,null,null,3,5]

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/binarysearchtree_improved.png)

 

**示例 1:**

```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。
```

**示例 2:**

```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
输出: 2
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
```

 

**代码：**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root: return None
        if p.val <= root.val <= q.val or q.val <= root.val <= p.val: return root
        if p.val < root.val and q.val < root.val: return self.lowestCommonAncestor(root.left, p, q)
        if p.val > root.val and q.val > root.val: return self.lowestCommonAncestor(root.right, p, q)
```



#### [701. 二叉搜索树中的插入操作](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)

*难度中等*

给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 输入数据 **保证** ，新值和原始二叉搜索树中的任意节点值都不同。

**注意**，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回 **任意有效的结果** 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/05/insertbst.jpg)

```
输入：root = [4,2,7,1,3], val = 5
输出：[4,2,7,1,3,5]
解释：另一个满足题目要求可以通过的树是：
```

**示例 2：**

```
输入：root = [40,20,60,10,30,50,70], val = 25
输出：[40,20,60,10,30,50,70,null,null,25]
```

**示例 3：**

```
输入：root = [4,2,7,1,3,null,null,null,null,null,null], val = 5
输出：[4,2,7,1,3,5]
```

 

 

**提示：**

- 给定的树上的节点数介于 `0` 和 `10^4` 之间
- 每个节点都有一个唯一整数值，取值范围从 `0` 到 `10^8`
- `-10^8 <= val <= 10^8`
- 新值和原始二叉搜索树中的任意节点值都不同

**代码：**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def insertIntoBST(self, root: TreeNode, val: int) -> TreeNode:
        if not root: return TreeNode(val)
        if root.val > val: root.left = self.insertIntoBST(root.left, val)
        if root.val <= val: root.right = self.insertIntoBST(root.right, val)
        return root
```



#### [450. 删除二叉搜索树中的节点](https://leetcode-cn.com/problems/delete-node-in-a-bst/)

*难度中等*

给定一个二叉搜索树的根节点 **root** 和一个值 **key**，删除二叉搜索树中的 **key** 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：

1. 首先找到需要删除的节点；
2. 如果找到了，删除它。

**说明：** 要求算法时间复杂度为 O(h)，h 为树的高度。

**示例:**

```
root = [5,3,6,2,4,null,7]
key = 3

    5
   / \
  3   6
 / \   \
2   4   7

给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。

一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示。

    5
   / \
  4   6
 /     \
2       7

另一个正确答案是 [5,2,6,null,4,null,7]。

    5
   / \
  2   6
   \   \
    4   7
```

**代码：**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def deleteNode(self, root: TreeNode, key: int) -> TreeNode:
        def appendLeft2Right(left, right):
            while right.left: right = right.left
            right.left = left
        if not root: return None
        if root.val == key:
            if not root.right and not root.left: return None
            if not root.right: return root.left
            if not root.left: return root.right
            appendLeft2Right(root.left, root.right)
            return root.right
        if root.val > key: root.left = self.deleteNode(root.left, key)
        if root.val < key: root.right = self.deleteNode(root.right, key)
        return root
```



#### [669. 修剪二叉搜索树](https://leetcode-cn.com/problems/trim-a-binary-search-tree/)

*难度中等*

给你二叉搜索树的根节点 `root` ，同时给定最小边界`low` 和最大边界 `high`。通过修剪二叉搜索树，使得所有节点的值在`[low, high]`中。修剪树不应该改变保留在树中的元素的相对结构（即，如果没有被移除，原有的父代子代关系都应当保留）。 可以证明，存在唯一的答案。

所以结果应当返回修剪好的二叉搜索树的新的根节点。注意，根节点可能会根据给定的边界发生改变。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/09/trim1.jpg)

```
输入：root = [1,0,2], low = 1, high = 2
输出：[1,null,2]

```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/09/09/trim2.jpg)

```
输入：root = [3,0,4,null,2,null,null,1], low = 1, high = 3
输出：[3,2,null,1]

```

**示例 3：**

```
输入：root = [1], low = 1, high = 2
输出：[1]

```

**示例 4：**

```
输入：root = [1,null,2], low = 1, high = 3
输出：[1,null,2]

```

**示例 5：**

```
输入：root = [1,null,2], low = 2, high = 4
输出：[2]

```

 

**提示：**

- 树中节点数在范围 `[1, 104]` 内
- `0 <= Node.val <= 104`
- 树中每个节点的值都是唯一的
- 题目数据保证输入是一棵有效的二叉搜索树
- `0 <= low <= high <= 104`

**代码：**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def trimBST(self, root: TreeNode, low: int, high: int) -> TreeNode:
        if not root: return None
        if root.val < low: return self.trimBST(root.right, low, high)
        if root.val > high: return self.trimBST(root.left, low, high)
        root.left = self.trimBST(root.left, low, high)
        root.right = self.trimBST(root.right, low, high)
        return root

```





#### [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

*难度简单*

给你一个整数数组 `nums` ，其中元素已经按 **升序** 排列，请你将其转换为一棵 **高度平衡** 二叉搜索树。

**高度平衡** 二叉树是一棵满足「每个节点的左右两个子树的高度差的绝对值不超过 1 」的二叉树。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/18/btree1.jpg)

```
输入：nums = [-10,-3,0,5,9]
输出：[0,-3,9,-10,null,5]
解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案：

```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/02/18/btree.jpg)

```
输入：nums = [1,3]
输出：[3,1]
解释：[1,3] 和 [3,1] 都是高度平衡二叉搜索树。

```

 

**提示：**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` 按 **严格递增** 顺序排列

**代码：**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
        if not nums: return None
        index_mid = len(nums) // 2
        node_cur = TreeNode(nums[index_mid])
        node_cur.left = self.sortedArrayToBST(nums[:index_mid])
        node_cur.right = self.sortedArrayToBST(nums[index_mid+1:])
        return node_cur

```





#### [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

*难度中等*

给出二叉 **搜索** 树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 `node` 的新值等于原树中大于或等于 `node.val` 的值之和。

提醒一下，二叉搜索树满足下列约束条件：

- 节点的左子树仅包含键 **小于** 节点键的节点。
- 节点的右子树仅包含键 **大于** 节点键的节点。
- 左右子树也必须是二叉搜索树。

**注意：**本题和 1038: https://leetcode-cn.com/problems/binary-search-tree-to-greater-sum-tree/ 相同

 

**示例 1：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/05/03/tree.png)**

```
输入：[4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
输出：[30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]

```

**示例 2：**

```
输入：root = [0,null,1]
输出：[1,null,1]

```

**示例 3：**

```
输入：root = [1,0,2]
输出：[3,3,2]

```

**示例 4：**

```
输入：root = [3,2,4,1]
输出：[7,9,4,10]

```

 

**提示：**

- 树中的节点数介于 `0` 和 `104` 之间。
- 每个节点的值介于 `-104` 和 `104` 之间。
- 树中的所有值 **互不相同** 。
- 给定的树为二叉搜索树。

**代码：**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def convertBST(self, root: TreeNode) -> TreeNode:
        # 反过来的中序遍历
        self.valSum = 0  # 记录累加和
        def helper(node):
            if not node: return
            helper(node.right)
            self.valSum += node.val  # 累加当前节点
            node.val = self.valSum  # 把累加结果赋给当前节点
            helper(node.left)
        helper(root)
        return root
```



# 回溯

#### [77. 组合](https://leetcode-cn.com/problems/combinations/)

*难度中等*

给定两个整数 *n* 和 *k*，返回 1 ... *n* 中所有可能的 *k* 个数的组合。

**示例:**

```
输入: n = 4, k = 2
输出:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

**代码：**

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        res, tmp = [], []
        def helper(start, _k, tmp):  # start是剩余要抽取的数的开始，_k是还需要抽取多少个，tmp是保存当前队列
            if n - start + 1 < _k: return  # 剩下的已经不可能再抽取够了，所以直接返回
            if _k == 0: res.append(tmp[::])  # 如果已经抽够了，把当前tmp加到返回结果之中
            for i in range(start, n + 1):  # 遍历剩下的
                tmp.append(i)
                helper(i + 1, _k - 1, tmp)
                tmp.pop()  # 弹出前面加进来的，恢复成原状
        helper(1, k, tmp)
        return res
```

#### [216. 组合总和 III](https://leetcode-cn.com/problems/combination-sum-iii/)

*难度中等*

找出所有相加之和为 ***n*** 的 ***k\*** 个数的组合***。\***组合中只允许含有 1 - 9 的正整数，并且每种组合中不存在重复的数字。

**说明：**

- 所有数字都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: k = 3, n = 7
输出: [[1,2,4]]

```

**示例 2:**

```
输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]

```

**代码：**

```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        def helper(i, val_sum):
            if val_sum > n: return
            if len(path) == k and val_sum == n:
                res.append(path[:])
                return
            for i in range(i, 9-(k-len(path))+2):
                val_sum += i
                path.append(i)
                helper(i+1, val_sum)
                val_sum -= i
                path.pop()
        res, path = [], []
        helper(1, 0)
        return res

```

#### [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

*难度中等*

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/original_images/17_telephone_keypad.png)

 

**示例 1：**

```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]

```

**示例 2：**

```
输入：digits = ""
输出：[]

```

**示例 3：**

```
输入：digits = "2"
输出：["a","b","c"]

```

 

**提示：**

- `0 <= digits.length <= 4`
- `digits[i]` 是范围 `['2', '9']` 的一个数字。

**代码：**

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if digits == "": return []
        digits_map = {'2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl', '6': 'mno', '7': 'pqrs', '8': 'tuv','9': 'wxyz'}
        def helper(i):  # i 是字母在digits中的索引
            if i == len(digits): res.append("".join(tmp))  # 指向了最后一个字母，把拼接好的字符串放入到结果中
            else:
                for c in digits_map[digits[i]]:
                    tmp.append(c)
                    helper(i+1)
                    tmp.pop()
        res, tmp = [], []
        helper(0)
        return res

```



#### [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

*难度中等*

给定一个**无重复元素**的数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的数字可以无限制重复被选取。

**说明：**

- 所有数字（包括 `target`）都是正整数。
- 解集不能包含重复的组合。 

**示例 1：**

```
输入：candidates = [2,3,6,7], target = 7,
所求解集为：
[
  [7],
  [2,2,3]
]

```

**示例 2：**

```
输入：candidates = [2,3,5], target = 8,
所求解集为：
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]

```

 

**提示：**

- `1 <= candidates.length <= 30`
- `1 <= candidates[i] <= 200`
- `candidate` 中的每个元素都是独一无二的。
- `1 <= target <= 500`

**代码：**

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res, path = [], []
        def helper(sumnow, startindex):
            if sumnow == target:
                res.append(path[:])
                return
            for i in range(startindex, len(candidates)):
                if sumnow + candidates[i] > target: continue
                path.append(candidates[i])
                helper(sumnow + candidates[i], i)
                path.pop()
        helper(0, 0)
        return res

```

#### [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)

*难度中等*

给定一个数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的每个数字在每个组合中只能使用一次。

**说明：**

- 所有数字（包括目标数）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

**示例 2:**

```
输入: candidates = [2,5,2,1,2], target = 5,
所求解集为:
[
  [1,2,2],
  [5]
]
```

**代码：**

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        res, path = [], []
        candidates.sort()
        def helper(path_sum, start_index):
            if path_sum == target:
                res.append(path[:])
                return
            for i in range(start_index, len(candidates)):
                if path_sum + candidates[i] > target: break
                if i > start_index and candidates[i-1] == candidates[i]: continue
                path.append(candidates[i])
                helper(path_sum + candidates[i], i + 1)
                path.pop()
        helper(0, 0)
        return res
```

#### [131. 分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)

*难度中等*

给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

**回文串** 是正着读和反着读都一样的字符串。

 

**示例 1：**

```
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]

```

**示例 2：**

```
输入：s = "a"
输出：[["a"]]

```

 

**提示：**

- `1 <= s.length <= 16`
- `s` 仅由小写英文字母组成

**代码：**

```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        n = len(s)
        f = [[True] * n for _ in range(n)]
        for i in range(n - 1, -1, -1):
            for j in range(i + 1, n):
                f[i][j] = (s[i] == s[j]) and f[i + 1][j - 1]
        ret = list()
        ans = list()
        def dfs(i: int):
            if i == n:
                ret.append(ans[:])
                return
            for j in range(i, n):
                if f[i][j]:
                    ans.append(s[i:j+1])
                    dfs(j + 1)
                    ans.pop()
        dfs(0)
        return ret

```

重点：KMP(https://blog.csdn.net/v_JULY_v/article/details/7041827)、排序、动态规划

# KMP:

#### [28. 实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)

实现 [strStr()](https://baike.baidu.com/item/strstr/811469) 函数。

给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串出现的第一个位置（下标从 0 开始）。如果不存在，则返回  `-1` 。

 

**说明：**

当 `needle` 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 `needle` 是空字符串时我们应当返回 0 。这与 C 语言的 [strstr()](https://baike.baidu.com/item/strstr/811469) 以及 Java 的 [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)) 定义相符。

 

**示例 1：**

```
输入：haystack = "hello", needle = "ll"
输出：2

```

**示例 2：**

```
输入：haystack = "aaaaa", needle = "bba"
输出：-1

```

**示例 3：**

```
输入：haystack = "", needle = ""
输出：0

```

 

**提示：**

- `0 <= haystack.length, needle.length <= 5 * 104`
- `haystack` 和 `needle` 仅由小写英文字符组成

**代码：**

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        len_needle = len(needle)
        if len_needle == 0: return 0
        next_pos = [0] * len_needle
        # 获取next
        j = 0
        for i in range(1, len_needle):
            while j > 0 and needle[i] != needle[j]: j = next_pos[j - 1]
            if needle[i] == needle[j]: j += 1
            next_pos[i] = j
        # 进行匹配
        j = 0
        for i in range(len(haystack)):
            while j > 0 and haystack[i] != needle[j]: j = next_pos[j - 1]
            if haystack[i] == needle[j]: j += 1
            if j == len_needle: return i - len_needle + 1
        return -1

```

# 动态规划

#### [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

难度中等1348

给定不同面额的硬币 `coins` 和一个总金额 `amount`。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 `-1`。

你可以认为每种硬币的数量是无限的。

 

**示例 1：**

```
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1

```

**示例 2：**

```
输入：coins = [2], amount = 3
输出：-1

```

**示例 3：**

```
输入：coins = [1], amount = 0
输出：0

```

**示例 4：**

```
输入：coins = [1], amount = 1
输出：1

```

**示例 5：**

```
输入：coins = [1], amount = 2
输出：2

```

 

**提示：**

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`

**代码：**

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [0] + [float("inf")] * amount  # 用来记录X元钱最少需要多少硬币（0元0个硬币）
        for coin in coins:
            for i in range(coin, amount + 1):
                dp[i] = min(dp[i], dp[i - coin] + 1)
        return dp[amount] if dp[amount] != float('inf') else -1

```

#### [518. 零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/)

难度中等549

给你一个整数数组 `coins` 表示不同面额的硬币，另给一个整数 `amount` 表示总金额。

请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金额，返回 `0` 。

假设每一种面额的硬币有无限个。 

题目数据保证结果符合 32 位带符号整数。

 



**示例 1：**

```
输入：amount = 5, coins = [1, 2, 5]
输出：4
解释：有四种方式可以凑成总金额：
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1

```

**示例 2：**

```
输入：amount = 3, coins = [2]
输出：0
解释：只用面额 2 的硬币不能凑成总金额 3 。

```

**示例 3：**

```
输入：amount = 10, coins = [10] 
输出：1
```

 

**提示：**

- `1 <= coins.length <= 300`
- `1 <= coins[i] <= 5000`
- `coins` 中的所有值 **互不相同**
- `0 <= amount <= 5000`

**代码：**

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [1] + [0] * amount
        for coin in coins:
            for i in range(coin, amount + 1):
                dp[i] += dp[i - coin]
        return dp[amount]
```

#### [300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

难度中等1682

给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。

子序列是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。

**示例 1：**

```
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```

**示例 2：**

```
输入：nums = [0,1,0,3,2,3]
输出：4
```

**示例 3：**

```
输入：nums = [7,7,7,7,7,7,7]
输出：1

```

 

**提示：**

- `1 <= nums.length <= 2500`
- `-104 <= nums[i] <= 104`

 

**进阶：**

- 你可以设计时间复杂度为 `O(n2)` 的解决方案吗？
- 你能将算法的时间复杂度降低到 `O(n log(n))` 吗?

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        dp = [1] * len(nums)
        res = 1
        for i in range(1, len(nums)):
            for j in range(i - 1, -1, -1):
                if nums[j] < nums[i]:
                    dp[i] = max(dp[j] + 1, dp[i])
                    res = max(res, dp[i])
        return res

```

#### [516. 最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)

难度中等466

给定一个字符串 `s` ，找到其中最长的回文子序列，并返回该序列的长度。可以假设 `s` 的最大长度为 `1000` 。

 

**示例 1:**
输入:

```
"bbbab"

```

输出:

```
4

```

一个可能的最长回文子序列为 "bbbb"。

**示例 2:**
输入:

```
"cbbd"

```

输出:

```
2

```

一个可能的最长回文子序列为 "bb"。

 

**提示：**

- `1 <= s.length <= 1000`
- `s` 只包含小写英文字母

**代码：**

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        s_length = len(s)
        dp = [[0] * s_length for _ in range(s_length)]
        for i in range(s_length): dp[i][i] = 1
        for i in range(s_length - 1, -1, -1):
            for j in range(i + 1, s_length):
                if s[i] == s[j]: dp[i][j] = dp[i + 1][j - 1] + 2
                else: dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
        return dp[0][s_length - 1]

```



2.字符串相似度/编辑距离（edit distance）

3.最长公共子序列(Longest Common Subsequence,lcs)

4.最长递增子序列（Longest Increasing Subsequence,lis）

5.最大连续子序列和/积

6.矩阵链乘法

7.0-1背包问题

8.有代价的最短路径

9.瓷砖覆盖（状态压缩DP）

10.工作量划分

11.三路取苹果

# 排序

![img](https://images2018.cnblogs.com/blog/849589/201804/849589-20180402133438219-1946132192.png)

#### 1.冒泡排序

![img](https://images2017.cnblogs.com/blog/849589/201710/849589-20171015223238449-2146169197.gif)

**过程：**

1. 比较相邻的元素。如果第一个比第二个大，就交换它们两个；
2. 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对，这样在最后的元素应该会是最大的数；
3. 针对所有的元素重复以上的步骤，除了最后一个；
4. 重复步骤1~3，直到排序完成。

**代码：**

```python
def BubbleSort(arr):
	"""冒泡排序"""
	arr_length = len(arr)
	for i in range(arr_length - 1):
		for j in range(arr_length - 1 - i):
			if arr[j] > arr[j + 1]: arr[j], arr[j + 1] = arr[j + 1], arr[j]
	return arr

```

#### 2.选择排序

![img](https://images2017.cnblogs.com/blog/849589/201710/849589-20171015224719590-1433219824.gif)

**过程：**

> 选择排序(Selection-sort)是一种简单直观的排序算法。它的工作原理：首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。 

n个记录的直接选择排序可经过n-1趟直接选择排序得到有序结果。具体算法描述如下：

1. 初始状态：无序区为R[1..n]，有序区为空；
2. 第i趟排序(i=1,2,3…n-1)开始时，当前有序区和无序区分别为R[1..i-1]和R(i..n）。该趟排序从当前无序区中-选出关键字最小的记录 R[k]，将它与无序区的第1个记录R交换，使R[1..i]和R[i+1..n)分别变为记录个数增加1个的新有序区和记录个数减少1个的新无序区；
3. n-1趟结束，数组有序化了。

**代码：**

```python
def SelectionSort(arr):
	"""选择排序"""
	arr_length = len(arr)
	for i in range(arr_length - 1):
		index_min = i
		for j in range(i + 1, arr_length):
			if arr[j] < arr[index_min]: index_min = j
		arr[i], arr[index_min] = arr[index_min], arr[i]
	return arr

```

#### 3.插入排序

> 插入排序（Insertion-Sort）的算法描述是一种简单直观的排序算法。它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。

**过程：**

一般来说，插入排序都采用in-place在数组上实现。具体算法描述如下：

1. 从第一个元素开始，该元素可以认为已经被排序；
2. 取出下一个元素，在已经排序的元素序列中从后向前扫描；
3. 如果该元素（已排序）大于新元素，将该元素移到下一位置；
4. 重复步骤3，直到找到已排序的元素小于或者等于新元素的位置；
5. 将新元素插入到该位置后；
6. 重复步骤2~5。

![img](https://images2017.cnblogs.com/blog/849589/201710/849589-20171015225645277-1151100000.gif)

**代码：**

```python
def InsertionSort(arr):
	"""插入排序"""
	arr_length = len(arr)
	for i in range(1, arr_length):
		preIndex = i - 1
		current = arr[i]
		while preIndex >= 0 and arr[preIndex] > current:
			arr[preIndex + 1] = arr[preIndex]
			preIndex -= 1
		arr[preIndex + 1] = current
	return arr
```

#### 4.希尔排序

> 1959年Shell发明，第一个突破O(n<sup>2</sup>)的排序算法，是简单插入排序的改进版。它与插入排序的不同之处在于，它会优先比较距离较远的元素。希尔排序又叫**缩小增量排序**。

![img](https://images2018.cnblogs.com/blog/849589/201803/849589-20180331170017421-364506073.gif)

**过程：**

先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，具体算法描述：

1. 选择一个增量序列 t<sub>1</sub>，t<sub>2</sub>，…，t<sub>k</sub>，其中t<sub>i</sub> > t<sub>j</sub>，t<sub>k</sub> = 1；
2. 按增量序列个数 k，对序列进行 k 趟排序；
3. 每趟排序，根据对应的增量 t<sub>i</sub>，将待排序列分割成若干长度为m 的子序列，分别对各子表进行直接插入排序。仅增量因子为 1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。

**代码：**

```python
def ShellSort(arr):
	"""希尔排序"""
	arr_length = len(arr)
	gap = arr_length // 2
	while gap > 0:
		for i in range(gap, arr_length):
			j = i
			current = arr[i]
			while j - gap >= 0 and current < arr[j - gap]:
				arr[j] = arr[j - gap]
				j = j - gap
			arr[j] = current
		gap //= 2
	return arr

```

#### 5.归并排序

> 归并排序是建立在归并操作上的一种有效的排序算法。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为2-路归并。 

![img](https://images2017.cnblogs.com/blog/849589/201710/849589-20171015230557043-37375010.gif)

**过程：**

1. 把长度为n的输入序列分成两个长度为n/2的子序列；
2. 对这两个子序列分别采用归并排序；
3. 将两个排序好的子序列合并成一个最终的排序序列。

**代码：**

```python
def MergeSort(arr):
	"""归并排序"""
	arr_length = len(arr)
	if arr_length < 2: return arr
	middle_idx = arr_length // 2
	arr_left, arr_right = MergeSort(arr[:middle_idx]), MergeSort(arr[middle_idx:])  # 分
	temp_res = []
	while arr_left and arr_right:  # 合
		if arr_left[0] <= arr_right[0]: temp_res.append(arr_left.pop(0))
		else: temp_res.append(arr_right.pop(0))
	return temp_res + arr_left + arr_right

```

#### 6.快速排序

> 快速排序的基本思想：通过一趟排序将待排记录分隔成独立的两部分，其中一部分记录的关键字均比另一部分的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序。

![img](https://images2017.cnblogs.com/blog/849589/201710/849589-20171015230936371-1413523412.gif)

**过程：**

快速排序使用分治法来把一个串（list）分为两个子串（sub-lists）。具体算法描述如下：

1. 从数列中挑出一个元素，称为 “基准”（pivot）；
2. 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
3. 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。

**代码：**

```python
def QuickSort(arr, left=0, right=None):
	"""快速排序"""
	if not right: right = len(arr) - 1
	if left >= right: return arr
	i, j, flag_val = left, right, arr[left]
	while i < j:
		while i < j and arr[j] > flag_val: j -= 1
		if i < j:
			arr[i] = arr[j]
			i += 1
		while i < j and arr[i] < flag_val: i += 1
		if i < j:
			arr[j] = arr[i]
			j -= 1
	arr[i] = flag_val
	QuickSort(arr, left, i - 1)
	QuickSort(arr, i + 1, right)
	return arr

```

#### 7.堆排序

> 堆排序（Heap-sort）是指利用堆这种数据结构所设计的一种排序算法。堆积是一个近似完全二叉树的结构，并同时满足堆积的性质：即子结点的键值或索引总是小于（或者大于）它的父节点。

![img](https://images2017.cnblogs.com/blog/849589/201710/849589-20171015231308699-356134237.gif)

**过程：**

1. 将初始待排序关键字序列(R<sub>1</sub>, R<sub>2</sub> … R<sub>n</sub>)构建成大顶堆，此堆为初始的无序区；
2. 将堆顶元素R[1]与最后一个元素R[n]交换，此时得到新的无序区(R<sub>1</sub>, R<sub>2</sub> … R<sub>n-1</sub>)和新的有序区(R<sub>n</sub>)，且满足R[1,2…n-1] <= R[n]；
3. 由于交换后新的堆顶R[1]可能违反堆的性质，因此需要对当前无序区(R<sub>1</sub>, R<sub>2</sub> … R<sub>n-1</sub>)调整为新堆，然后再次将R[1]与无序区最后一个元素交换，得到新的无序区(R<sub>1</sub>, R<sub>2</sub> … R<sub>n-2</sub>)和新的有序区(R<sub>n-1</sub>, R<sub>n</sub>)。不断重复此过程直到有序区的元素个数为n-1，则整个排序过程完成。

**代码：**

```python
def HeapSort(arr):
	"""堆排序"""
	def HeapAdjust(arr, parent: int, length:int):
		temp = arr[parent]
		child = 2 * parent + 1  # 左孩子
		while child < length:
			if child + 1 < length and arr[child] < arr[child + 1]: child += 1
			if temp >= arr[child]: break
			arr[parent] = arr[child]
			parent = child
			child = 2 * child + 1
		arr[parent] = temp

	arr_length = len(arr)
	for i in range(arr_length // 2, -1, -1):
		HeapAdjust(arr, i, arr_length)
	for i in range(arr_length - 1, 0, -1):
		arr[i], arr[0] = arr[0], arr[i]
		HeapAdjust(arr, 0, i)
	return arr

```

#### 8.计数排序

> 计数排序不是基于比较的排序算法，其核心在于将输入的数据值转化为键存储在额外开辟的数组空间中。 作为一种线性时间复杂度的排序，计数排序要求输入的数据必须是有确定范围的整数。

![img](https://images2017.cnblogs.com/blog/849589/201710/849589-20171015231740840-6968181.gif)

**过程：**

1. 找出待排序的数组中最大和最小的元素；
2. 统计数组中每个值为i的元素出现的次数，存入数组C的第i项；
3. 对所有的计数累加（从C中的第一个元素开始，每一项和前一项相加）；
4. 反向填充目标数组：将每个元素i放在新数组的第C(i)项，每放一个元素就将C(i)减去1。

**代码：**

```python
import collections
def CountingSort(arr):
	arr_length = len(arr)
	res = []
	bucket = collections.defaultdict(int)
	val_max = float("-inf")
	for i in range(arr_length):
		bucket[arr[i]] += 1
		val_max = val_max if val_max > arr[i] else arr[i]
	for i in range(val_max + 1):
		if i in bucket:
			res += [i] * bucket[i]
	return res
```

#### 9.桶排序

> 桶排序是计数排序的升级版。它利用了函数的映射关系，高效与否的关键就在于这个映射函数的确定。桶排序 (Bucket sort)的工作的原理：假设输入数据服从均匀分布，将数据分到有限数量的桶里，每个桶再分别排序（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排）。

![img](https://images2017.cnblogs.com/blog/849589/201710/849589-20171015232107090-1920702011.png)

**过程：**

1. 设置一个定量的数组当作空桶；
2. 遍历输入数据，并且把数据一个一个放到对应的桶里去；
3. 对每个不是空的桶进行排序；
4. 从不是空的桶里把排好序的数据拼接起来。 

**代码：**

```python

```



# 剑指Offer

#### [剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

难度简单473

找出数组中重复的数字。

在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

**示例 1：**

```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 

```

**限制：**

```
2 <= n <= 100000

```

**代码：**

```python
class Solution:
    def findRepeatNumber(self, nums: List[int]) -> int:
        """利用坑位，把对应的数放到对应的位置"""
        for i in range(len(nums)):
            while nums[i] != i:
                if nums[nums[i]] == nums[i]: return nums[i]
                nums[nums[i]], nums[i] = nums[i], nums[nums[i]]

```

#### [剑指 Offer 04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

难度中等376

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

 

**示例:**

现有矩阵 matrix 如下：

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]

```

给定 target = `5`，返回 `true`。

给定 target = `20`，返回 `false`。

 

**限制：**

```
0 <= n <= 1000
0 <= m <= 1000

```

 

**注意：**本题与主站 240 题相同：https://leetcode-cn.com/problems/search-a-2d-matrix-ii/

**代码：**

```python
class Solution:
    def findNumberIn2DArray(self, matrix: List[List[int]], target: int) -> bool:
        if not matrix or not matrix[0]: return False
        n, m = len(matrix) - 1, len(matrix[0]) - 1
        i, j = 0, m
        while i <= n and j >= 0:
            if matrix[i][j] > target: j -= 1
            elif matrix[i][j] < target: i += 1
            else: return True
        return False

```

#### [剑指 Offer 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

难度简单137

请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

**示例 1：**

```
输入：s = "We are happy."
输出："We%20are%20happy."

```

**限制：**

```
0 <= s 的长度 <= 10000

```

**代码：**

```python
class Solution:
    def replaceSpace(self, s: str) -> str:
        res = ""
        for _s in s:
            res += _s if _s != ' ' else '%20'
        return res
```

#### [剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

**示例 1：**

```
输入：head = [1,3,2]
输出：[2,3,1]
```

**限制：**

```
0 <= 链表长度 <= 10000
```

**代码：**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reversePrint(self, head: ListNode) -> List[int]:
        return self.reversePrint(head.next) + [head.val] if head else []
```

#### [剑指 Offer 07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

难度中等497

输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7 

```

**限制：**

```
0 <= 节点个数 <= 5000

```

**注意**：本题与主站 105 题重复：https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/

**代码：**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        if not preorder: return None
        cur_node = TreeNode(preorder[0])
        cur_index = inorder.index(preorder[0])
        length_left, length_right = len(inorder[:cur_index]), len(inorder[cur_index + 1:])
        cur_node.left = self.buildTree(preorder[1:length_left + 1], inorder[:cur_index])
        cur_node.right = self.buildTree(preorder[length_left + 1: length_left + length_right + 1], inorder[cur_index + 1:])
        return cur_node

```













#### [剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

难度简单258

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 `appendTail` 和 `deleteHead` ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，`deleteHead` 操作返回 -1 )

 

**示例 1：**

```
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]

```

**示例 2：**

```
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]

```

**提示：**

- `1 <= values <= 10000`
- `最多会对 appendTail、deleteHead 进行 10000 次调用`

**代码：**

```python
class CQueue:

    def __init__(self):
        self.stack_1, self.stack_2 = [], []

    def appendTail(self, value: int) -> None:
        self.stack_2.append(value)

    def deleteHead(self) -> int:
        if not self.stack_1:
            while self.stack_2:
                self.stack_1.append(self.stack_2.pop())
        if self.stack_1: return self.stack_1.pop()
        return -1

```

#### [剑指 Offer 10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

难度简单159

写一个函数，输入 `n` ，求斐波那契（Fibonacci）数列的第 `n` 项（即 `F(N)`）。斐波那契数列的定义如下：

```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.

```

斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

 

**示例 1：**

```
输入：n = 2
输出：1
```

**示例 2：**

```
输入：n = 5
输出：5
```

 

**提示：**

- `0 <= n <= 100`

**代码：**

```python
class Solution:
    def fib(self, n: int) -> int:
        a, b, c = 0, 1, 0
        for _ in range(n):
            c = (a + b) % (1e9 + 7)
            a = b
            b = c
        return int(a)
```

#### [剑指 Offer 10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

难度简单177

一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 `n` 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

**示例 1：**

```
输入：n = 2
输出：2
```

**示例 2：**

```
输入：n = 7
输出：21
```

**示例 3：**

```
输入：n = 0
输出：1
```

**提示：**

- `0 <= n <= 100`

注意：本题与主站 70 题相同：https://leetcode-cn.com/problems/climbing-stairs/

**代码：**

```python
class Solution:
    def numWays(self, n: int) -> int:
        a, b, c = 1, 1, 1
        for _ in range(n):
            c = (a + b) % (1e9 + 7)
            a, b = b, c
        return int(a)
```

