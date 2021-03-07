# 总结

[TOC]



## KEY WORDS

空间换时间

双指针

## 递归

==自顶向下==

- 画递归树

- 剪枝
  - 重复子问题

实现调用自身的函数的诀窍在于，每当递归函数调用自身时，它都会将给定的问题拆解为子问题。递归调用继续进行，直到到子问题无需进一步递归就可以解决的地步。

为了确保递归函数不会导致无限循环，它应具有以下属性：

- 一个简单的基本案例（**basic case**）（或一些案例） —— 能够不使用递归来产生答案的终止方案。

- 一组规则，也称作递推关系（recurrence relation），可将所有其他情况拆分到基本案例。

注意，函数可能会有多个位置进行自我调用。

【摘要】 递归具有很多的优点，它可以将一个大的问题划分为小的子问题，然后再逐步细分，达到解决问题的目的。递归的实现借用了栈的建立和销毁，所以它是很方便的。但是递归也有一些缺点，比如说，如果递归调用太深，栈桢消耗过大，就会出现栈溢出的问题，因此，在我们使用递归之前，应该仔细考虑适不适合使用递归来解决这个问题。同时，递归深度太深，也会使得运算时间大大增加，所以递归的结论一般都是在理论的基础上的。

**递归算法的时间复杂度：递归的总次数*每次递归的数量。**

**递归算法的空间复杂度：递归的深度*每次递归创建变量的个数**。

> https://www.nowcoder.com/discuss/585365

## 动态规划

==自底向上==

- 一般形式是求最值
  - 最长递增子序列
  - 最小编辑距离
- 核心问题穷举（暴力）
  - 存在**重叠子问题**=>空间换时间
    - 备忘录
    - DP table
      - 「**状态压缩**」
      - 每次只需要用到DP table 的一部分，不需要全部
  - 具有**最优子结构**
    - 子问题间必须互相独立
- 难点**转移方程**

**明确 base case -> 明确「状态」-> 明确「选择」 -> 定义 dp 数组/函数的含义**。

## 回溯

深度遍历就有回溯

> **解决一个回溯问题，实际上就是一个决策树的遍历过程**。你只需要思考 3 个问题：
>
> 1、路径：也就是已经做出的选择。
>
> 2、选择列表：也就是你当前可以做的选择。
>
> 3、结束条件：也就是到达决策树底层，无法再做选择的条件。
>
> **其核心就是 for 循环里面的递归，在递归调用之前「做选择」，在递归调用之后「撤销选择」**

```
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return

    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
```

就拿深度遍历来说

```C++
void Traverse_PreOrder1(BiTree *root)     
{
    stack<BinTree*> s;
    BiTree *p=root;
    while(p!=NULL||!s.empty())//出口 p==NULL&&s.empty
    {
        while(p!=NULL)//访问p节点，并入栈
        {
            cout<<p->data<<"";//做选择
            s.push(p);  //添加路径
            p=p->lchild;
        }
        if(!s.empty())
        {
            p=s.top();
            s.pop();
            p=p->rchild;
        }
    }
}
```

而这里的回溯是遍历完左子树后再去遍历右子树，我们遇到的提大多数不是这种自带有序遍历的数，所以需要其他的辅助一下。



# LeeCode

## 1.链表中间结点

### 方法一：数组

思路和算法

链表的缺点在于不能通过下标访问对应的元素。因此我们可以考虑对链表进行遍历，同时将遍历到的元素依次放入数组 A 中。如果我们遍历到了 N 个元素，那么链表以及数组的长度也为 N，对应的中间节点即为 A[N/2]。

```C++
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        vector<ListNode*> A = {head};
        while (A.back()->next != NULL)
            A.push_back(A.back()->next);
        return A[A.size() / 2];
    }
};
```


复杂度分析

时间复杂度：O(N)，其中 N 是给定链表中的结点数目。

空间复杂度：O(N)，即数组 A 用去的空间。

### 方法二：单指针法

我们可以对方法一进行空间优化，省去数组 A。

我们可以对链表进行两次遍历。第一次遍历时，我们统计链表中的元素个数 N；第二次遍历时，我们遍历到第 N/2 个元素（链表的首节点为第 0 个元素）时，将该元素返回即可。

```C++
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        int n = 0;
        ListNode* cur = head;
        while (cur != nullptr) {
            ++n;
            cur = cur->next;
        }
        int k = 0;
        cur = head;
        while (k < n / 2) {
            ++k;
            cur = cur->next;
        }
        return cur;
    }
};
```

复杂度分析

时间复杂度：O(N)，其中 N 是给定链表的结点数目。

空间复杂度：O(1)，只需要常数空间存放变量和指针。

### 方法三：快慢指针法

思路和算法

我们可以继续优化方法二，用两个指针 slow 与 fast 一起遍历链表。slow 一次走一步，fast 一次走两步。那么当 fast 到达链表的末尾时，slow 必然位于中间。

```C++
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};
```


复杂度分析

时间复杂度：O(N)，其中 N 是给定链表的结点数目。

空间复杂度：O(1)，只需要常数空间存放 slow 和 fast 两个指针。


## 2.按摩师接客问题

一个有名的按摩师会收到源源不断的预约请求，每个预约都可以选择接或不接。在每次预约服务之间要有休息时间，因此她不能接受相邻的预约。给定一个预约请求序列，替按摩师找到最优的预约集合（总预约时间最长），返回总的分钟.

**示例 1：**

```
输入： [1,2,3,1]
输出： 4
解释： 选择 1 号预约和 3 号预约，总时长 = 1 + 3 = 4。
```

### 方法一：动态规划

思路和算法

定义 $dp[i][0]$ 表示考虑前 i 个预约，第 i 个预约不接的最长预约时间，$dp[i][1]$表示考虑前 i 个预约，第 i 个预约接的最长预约时间。

从前往后计算 dp 值，假设我们已经计算出前i−1 个dp 值，考虑计算 $dp[i][0/1]$ 的答案。

首先考虑 $dp[i][0]$ 的转移方程，由于这个状态下第 i 个预约是不接的，所以第 i-1 个预约接或不接都可以，故可以从 $dp[i-1][0]$和 $dp[i-1][1]$两个状态转移过来，转移方程即为：

$dp[i][0]=max(dp[i-1][0],dp[i-1][1])$

对于 $dp[i][1]$ ，由于这个状态下第 i 个预约要接，根据题目要求按摩师不能接受相邻的预约，所以第 i−1 个预约不能接受，故我们只能从 $dp[i-1][0]$ 这个状态转移过来，转移方程即为：

$dp[i][1]=dp[i-1][0]+nums_i$


其中$nums_i$表示第 i 个预约的时长。

最后答案即为 $max(dp[n][0],dp[n][1])$，其中 n 表示预约的个数。

再回来看转移方程，我们发现计算 $dp[i][0/1]$ 时，只与前一个状态 $dp[i-1][0/1]$有关，所以我们可以不用开数组，只用两个变量 $dp_0$，$dp_1$ 分别存储 $dp[i-1][0]$和$dp[i-1][1]$答案，然后去转移更新答案即可。

```c++
class Solution {
public:
    int massage(vector<int>& nums) {
        int n = (int)nums.size();
        if (!n) return 0;
        int dp0 = 0, dp1 = nums[0];
        for (int i = 1; i < n; ++i){
        	int tdp0 = max(dp0, dp1); // 计算 dp[i][0]
        	int tdp1 = dp0 + nums[i]; // 计算 dp[i][1]

        	dp0 = tdp0; // 用 dp[i][0] 更新 dp_0
        	dp1 = tdp1; // 用 dp[i][1] 更新 dp_1
    	}
    	return max(dp0, dp1);
	}
};    
```


复杂度分析

时间复杂度：O(n)，其中 n 为预约的个数。我们有 2n 个状态需要计算，每次状态转移需要 O(1) 的时间，所以一共需要 O(2n)=O(n) 的时间复杂度。

空间复杂度：O(1)，只需要常数的空间存放若干变量

## [3.卡牌分组](https://leetcode-cn.com/problems/x-of-a-kind-in-a-deck-of-cards/solution/qia-pai-fen-zu-by-leetcode-solution/)

给定一副牌，每张牌上都写着一个整数。

此时，你需要选定一个数字 X，使我们可以将整副牌按下述规则分成 1 组或更多组：

每组都有 X 张牌。
组内所有的牌上都写着相同的整数。
仅当你可选的 X >= 2 时返回 true。

> 示例 1：输入：[1,2,3,4,4,3,2,1]
> 输出：true
> 解释：可行的分组是 [1,1]，[2,2]，[3,3]，[4,4]
> 示例 2：输入：[1,1,1,2,2,2,3,3]
> 输出：false
> 解释：没有满足要求的分组。
> 示例 3：输入：[1]
> 输出：false
> 解释：没有满足要求的分组。
> 示例 4：输入：[1,1]
> 输出：true
> 解释：可行的分组是 [1,1]
> 示例 5：输入：[1,1,2,2,2,2]
> 输出：true
> 解释：可行的分组是 [1,1]，[2,2]，[2,2]
>
> 提示：1 <= deck.length <= 10000
> 0 <= deck[i] < 10000

### 方法一：暴力

思路:     我们枚举所有可行的 X，判断是否有满足条件的 X 即可。
算法
		我们从 $22$ 开始，从小到大枚举 $X$。
		由于每一组都有 $X$ 张牌，那么$ X$ 必须是卡牌总数$ N$ 的约数。
		其次，对于写着数字 $i$ 的牌，如果有 $\textit{count}_i$ 张，由于题目要求「组内所有的牌上都写着相同的整数」，那么 X 也必须是 $\textit{count}_i$  的约数，即：$count_imodX==0$
		所以对于每一个枚举到的 $X$，我们只要先判断$X$ 是否为$ N $的约数，然后遍历所有牌中存在的数字$ i$，看它们对应牌的数量 $\textit{count}_i$是否满足上述要求。如果都满足等式，则 $X $为符合条件的解，否则需要继续令 $X$ 增大，枚举下一个数字。

```C
public:
    bool hasGroupsSizeX(vector<int>& deck) {
        int N = (int)deck.size();
        for (int c: deck) count[c]++;

        vector<int> values;
        for (int i = 0; i < 10000; ++i) {
            if (count[i] > 0) {
                values.emplace_back(count[i]);
            }
        }

        for (int X = 2; X <= N; ++X) {
            if (N % X == 0) {
                bool flag = 1;
                for (int v: values) {
                    if (v % X != 0) {
                        flag = 0;
                        break;
                    }
                }
                if (flag) return true;
            }
        }
        return false;
    }
};
```

复杂度分析

时间复杂度：O(N^2)，其中 N是卡牌个数。最多枚举 N个可能的 X，对于每个 X，要遍历的数字 i 最多有 N 个。

空间复杂度：O(N + C)或 O(N)，其中 C是数组 deck 中数的范围，在本题中 C的值为 10000。在 C++ 和 Java 代码中，我们先用频率数组 count 存储了每个数字 i 出现的次数 count[i]，随后将所有超过零的次数转移到数组 values 中，方便进行遍历，因此需要使用长度为 C的 count 数组以及长度不超过 N 的 values 数组，空间复杂度为 O(N + C)。在 Python 代码中，我们直接使用哈希映射存储每个数字 i 以及出现的次数，因此空间复杂度为 O(N)。

### 方法二：最大公约数

思路和算法

由于方法一已经提及 $X$ 一定为 $\textit{count} _i$的约数，这个条件是对所有牌中存在的数字 i 成立的，所以我们可以推出，只有当 X 为所有$\textit{count} _i$的约数，即所有$\textit{count} _i$的最大公约数的约数时，才存在可能的分组。公式化来说，我们假设牌中存在的数字集合为 a, b, c, d, e，那么只有当 X 为

$gcd(\textit{count}_a,\textit{count}_b,\textit{count}_c,\textit{count}_d,\textit{count}_e)$
因此我们只要求出所有$ \textit{count} _i$最大公约数 g，判断 g 是否大于等于 22 即可，如果大于等于 22，则满足条件，否则不满足

```C++
class Solution {
    int cnt[10000];
public:
    bool hasGroupsSizeX(vector<int>& deck) {
        for (auto x: deck) cnt[x]++;
        int g = -1;
        for (int i = 0; i < 10000; ++i)if(cnt[i]) {
            if (~g) g = gcd(g, cnt[i]);
            else g = cnt[i];
        }
        return g >= 2 ? true : false;
    }
};
```

复杂度分析

- 时间复杂度：$O(N \log C)$，其中 N 是卡牌的个数，C是数组 deck 中数的范围，在本题中 C 的值为 10000。求两个数最大公约数的复杂度是$ O(\log C)$，需要求最多 N - 1次。

空间复杂度：$O(N + C) $或$O(N)$。

## [4.圆圈中剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/solution/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-by-lee/)

0,1,,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。

例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

```C++ 
示例 1：

输入: n = 5, m = 3
输出: 3
示例 2：

输入: n = 10, m = 17
输出: 2
```

### 方法一：数学 + 递归

思路

题目中的要求可以表述为：给定一个长度为 n 的序列，每次向后数 m 个元素并删除，那么最终留下的是第几个元素？

这个问题很难快速给出答案。但是同时也要看到，这个问题似乎有拆分为较小子问题的潜质：如果我们知道对于一个长度 n - 1 的序列，留下的是第几个元素，那么我们就可以由此计算出长度为 n 的序列的答案。

算法

我们将上述问题建模为函数 f(n, m)，该函数的返回值为最终留下的元素的序号。

首先，长度为 n 的序列会先删除第 m % n 个元素，然后剩下一个长度为 n - 1 的序列。那么，我们可以递归地求解 f(n - 1, m)，就可以知道对于剩下的 n - 1 个元素，最终会留下第几个元素，我们设答案为 x = f(n - 1, m)。

由于我们删除了第 m % n 个元素，将序列的长度变为 n - 1。当我们知道了 f(n - 1, m) 对应的答案 x 之后，我们也就可以知道，长度为 n 的序列最后一个删除的元素，应当是从 m % n 开始数的第 x 个元素。因此有 f(n, m) = (m % n + x) % n = (m + x) % n。

我们递归计算 f(n, m), f(n - 1, m), f(n - 2, m), ... 直到递归的终点 f(1, m)。当序列长度为 1 时，一定会留下唯一的那个元素，它的编号为 0。

> 最终剩下一个人时的安全位置肯定为0，反推安全位置在人数为n时的编号
> 人数为1： 0
> 人数为2： (0+m) % 2
> 人数为3： ((0+m) % 2 + m) % 3
> 人数为4： (((0+m) % 2 + m) % 3 + m) % 4
> ........
> 迭代推理到n就可以得出答案

下面的代码实现了上述的递归函数。

```C++
class Solution {
    int f(int n, int m) {
        if (n == 1)
            return 0;
        int x = f(n - 1, m);
        return (m + x) % n;
    }
public:
    int lastRemaining(int n, int m) {
        return f(n, m);
    }
};
```


复杂度分析

时间复杂度：O(n)，需要求解的函数值有 n 个。

空间复杂度：O(n)，函数的递归深度为 n，需要使用 O(n) 的栈空间。

### 方法二：数学 + 迭代

思路与算法

上面的递归可以改写为迭代，避免递归使用栈空间。

```C++
class Solution {
public:
    int lastRemaining(int n, int m) {
        int f = 0;
        for (int i = 2; i != n + 1; ++i)
            f = (m + f) % i;
        return f;
    }
};
```



复杂度分析

时间复杂度：O(n)，需要求解的函数值有 n 个。

空间复杂度：O(1)，只使用常数个变量。

## [5.回文](https://leetcode-cn.com/problems/palindrome-number/solution/jing-xin-hui-zong-python3de-5chong-shi-xian-fang-f/)

精心汇总python3的5种实现方法
"""
解题思路: (五种实现方法)

1. 转成字符串:
a. 双向队列
b. 双指针
2. 不转字符串:
a. 模拟字符串的双向队列(使用math库的log函数 获取整数的位数)
b. 反转后面一半的整数(使用math库的log函数 获取整数的位数)
c. 反转后面一半的整数(不适用log函数) (通过原整数x 与 reverse_x 的大小判断)
"""
```python
 class Solution:

    # 方法一: 将int转化成str类型: 双向队列
    # 复杂度: O(n^2) [每次pop(0)都是O(n)..比较费时]
    def isPalindrome(x: int) -> bool:
        lst = list(str(x))
        while len(lst) > 1:
            if lst.pop(0) != lst.pop():
                return  False
        return True


    # 方法二: 将int转化成str类型: 双指针 (指针的性能一直都挺高的)
    # 复杂度: O(n)
    def isPalindrome(x: int) -> bool:
        lst = list(str(x))
        L, R = 0, len(lst)-1
        while L <= R:
            if lst[L] != lst[R]:
                return  False
            L += 1
            R -= 1
        return True
 # 方法三: 进阶:不将整数转为字符串来解决: 使用log来计算x的位数
 # 复杂度: O(n)
 def isPalindrome(self, x: int) -> bool:
    """
    模仿上面字符串的方法:分别取'第一位的数'与'第二位的数'对比
                    (弊端是:频繁计算,导致速度变慢)(下面的方法三只反转一半数字,可以提高性能)
    """
    if x < 0:
        return False
    elif x == 0:
        return True
    else:
        import math
        length = int(math.log(x, 10)) + 1
        L = length-1
        print("l = ", L)
        for i in range(length//2):
            if x // 10**L != x % 10:
                return False
            x = (x % 10**L) //10
            L -= 2
        return True

# 方法四: 进阶:不将整数转为字符串来解决: 使用log来计算x的位数
# 复杂度: O(n)
def isPalindrome(self, x: int) -> bool:
    """
    只反转后面一半的数字!!(节省一半的时间)
    """
    if x < 0 or (x!=0 and x%10==0):
        return False
    elif x == 0:
        return True
    else:
        import math
        length = int(math.log(x, 10)) + 1
        reverse_x = 0
        for i in range(length//2):
            remainder = x % 10
            x = x // 10
            reverse_x = reverse_x * 10 + remainder
        # 当x为奇数时, 只要满足 reverse_x == x//10 即可
        if reverse_x == x or reverse_x == x//10:
            return True
        else:
            return False

# 方法五: 进阶:不将整数转为字符串来解决: 不使用log函数
# 复杂度: O(n)
def isPalindrome(self, x: int) -> bool:
    """
    只反转后面一半的数字!!(节省一半的时间)
    """
    if x < 0 or (x!=0 and x%10==0):
        return False
    elif x == 0:
        return True
    else:
        reverse_x = 0
        while x > reverse_x:
            remainder = x % 10
            reverse_x = reverse_x * 10 + remainder
            x = x // 10
        # 当x为奇数时, 只要满足 reverse_x//10 == x 即可
        if reverse_x == x or reverse_x//10 == x:
            return True
        else:
            return False
x = 10
x = 10001
print(Solution().isPalindrome(x))
```

## 6.  两数之和

#### [剑指 Offer 57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

难度简单64

输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

ava 题解 简单明了

可以利用set来找出答案，但是题中说数组是递增的，故也可以用双指针。

```java
public int[] twoSum(int[] nums, int target) {

    int left = 0, right = nums.length - 1;

    while (left < right) {

        int cur = nums[left] + nums[right];

        if (cur == target)
            return new int[]{nums[left], nums[right]};
        else if (cur > target)
            right--;
        else 
            left++;
    }
    return new int[]{};
}
```

```java
public int[] twoSum(int[] nums, int target) {

    Set<Integer> set = new HashSet<>();
    for (int num : nums) {

        if (!set.contains(target - num))
            set.add(num);
        else 
            return new int[]{num, target - num};
    }

    return new int[]{};
}
```

## 7. 前序中序建树



```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    return buildTreeHelper(preorder, 0, preorder.length, inorder, 0, inorder.length);
}

private TreeNode buildTreeHelper(int[] preorder, int p_start, int p_end, int[] inorder, int i_start, int i_end) {
    // preorder 为空，直接返回 null
    if (p_start == p_end) {
        return null;
    }
    int root_val = preorder[p_start];
    TreeNode root = new TreeNode(root_val);
    //在中序遍历中找到根节点的位置
    int i_root_index = 0;
    for (int i = i_start; i < i_end; i++) {
        if (root_val == inorder[i]) {
            i_root_index = i;
            break;
        }
    }
    int leftNum = i_root_index - i_start;
    //递归的构造左子树
    root.left = buildTreeHelper(preorder, p_start + 1, p_start + leftNum + 1, inorder, i_start, i_root_index);
    //递归的构造右子树
    root.right = buildTreeHelper(preorder, p_start + leftNum + 1, p_end, inorder, i_root_index + 1, i_end);
    return root;
}
```

优化

上边的代码很好理解，但存在一个问题，**在中序遍历中找到根节点的位置每次都得遍历中序遍历的数组去寻找**，参考这里 ，我们可以用一个HashMap把中序遍历数组的每个元素的值和下标存起来，这样寻找根节点的位置就可以直接得到了。

```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    HashMap<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < inorder.length; i++) {
        map.put(inorder[i], i);
    }
    return buildTreeHelper(preorder, 0, preorder.length, inorder, 0, inorder.length, map);
}

private TreeNode buildTreeHelper(int[] preorder, int p_start, int p_end, int[] inorder, int i_start, int i_end,
                                 HashMap<Integer, Integer> map) {
    if (p_start == p_end) {
        return null;
    }
    int root_val = preorder[p_start];
    TreeNode root = new TreeNode(root_val);
    int i_root_index = map.get(root_val);
    int leftNum = i_root_index - i_start;
    root.left = buildTreeHelper(preorder, p_start + 1, p_start + leftNum + 1, inorder, i_start, i_root_index, map);
    root.right = buildTreeHelper(preorder, p_start + leftNum + 1, p_end, inorder, i_root_index + 1, i_end, map);
    return root;
}
```

git config --global user.name "xiang3999"

git config --global user.email "xiang3999@qq.com"

ssh-keygen -t rsa -C "xiang3999@qq.com"

作者：windliang
链接：https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by--22/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 8. 斐波拉契数列

$F(n)=F(n-1)+F(n-2)$，简单题

乍一看这不就是简单的递归嘛

### 直接递归

```java
public class Solution {
    public int fib(int N) {
        if (N <= 1) {
            return N;
        }
        return fib(N-1) + fib(N-2);
    }
}
```

以上解决问题，可是一提交

![image-20210103125247624](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20210103125247624.png)

用时和内存消耗也太多了吧

来分析一下这个的时间复杂度和空间复杂度

![在这里插入图片描述](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/5575fb4ab0df264cba3a47172961895aaa7a52072b8241b8ea6d3a6b7f084c60-file_1577091644006)

 **复杂度分析**

时间复杂度：$O(2^N)$。这是计算斐波那契数最慢的方法。因为它需要指数的时间。
空间复杂度：$O(N)$，在堆栈中我们需要与 $N$ 成正比的空间大小。该堆栈跟踪` fib(N) `的函数调用，随着堆栈的不断增长如果没有足够的内存则会导致 `StackOverflowError`。

### **优化**

这里看递归树可以发现很多的值都被重复计算了，所以如果能保存下计算以后的值就好了。

![img](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/1600679989-dMEQCS-file_1600679989062)

#### 备忘录自底向上的方法

自底向上通过迭代计算斐波那契数的子问题并存储已计算的值，通过已计算的值进行计算。减少递归带来的重复计算。

```java
class Solution {
    public int fib(int N) {
        if (N <= 1) {
            return N;
        }
        return memoize(N);
    }

    public int memoize(int N) {
      int[] cache = new int[N + 1];
      cache[1] = 1;

      for (int i = 2; i <1+N; i++) {
          cache[i] = cache[i-1] + cache[i-2];
      }
      return cache[N];
    }
}
```

**复杂分析**

- 时间复杂度：$O(N)$,  `for`循环了N-1次
- 空间复杂度：$O(N)$，使用了空间大小为 `N` 的数组。

#### 备忘录自顶向下的方法

我们先计算存储子问题的答案，然后利用子问题的答案计算当前斐波那契数的答案。我们将递归计算，但是通过记忆化不重复计算已计算的值。

![img](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/1600679989-exCmXp-file_1600679989061)

**计算过程**

- 如果$ N <= 1$，则返回$ N$。
- 调用和返回 `memoize(N)`。
- 如果 $N $对应的斐波那契数存在，则返回。
- 否则将计算 $N$ 对应的斐波那契数为` memoize(N-1) + memoize(N-2)`。

```java
class Solution {
    private Integer[] cache = new Integer[31];

    public int fib(int N) {
        if (N <= 1) {
            return N;
        }
        cache[0] = 0;
        cache[1] = 1;
        return memoize(N);
    }

    public int memoize(int N) {
      if (cache[N] != null) {
          return cache[N];
      }
      cache[N] = memoize(N-1) + memoize(N-2);
      return memoize(N);
    }
}
```

咋一看其实这个和自底向上差别不大，都在用数组存储已经计算的值，但是自底向上是直接从底层计算到需要的值，而自顶向下是从顶部向下询问，这里的思想是不同的。

**复杂度分析**

时间复杂度：$O(N)$。
空间复杂度：$O(N)$，内存中使用的堆栈大小。

这里的优化都是基于存储已经计算的值，避免重复计算。

#### 自底向上进行迭代

在记忆化自底向上的方法中，是用了数组来存储每个N对应的值，但是这里只需要计算一次，不用重复使用，可以直接简化为$F(n)=F(n-1)+F(n-2)$，用3个值来进行迭代。

**算法**：

若$ N <= 1$，则返回 $N$。
若 $N == 2$，则返回 `fib(2-1) + fib(2-2) = 1`。
使用迭代的方法，我们至少需要三个变量存储 `fib(N)`, `fib(N-1)` 和 `fib(N-2)`。
预置初始值：
`current = 0`。
`prev1 = 1`，代表 `fib(N-1)`。
`prev2 = 1`，代表` fib(N-2)`
我们从 $3$ 计算到$4$；$0$，$1$，$2$对应的斐波那契数是预先计算。
设置` current = fib(N-1) + fib(N-2)`，因为 `current` 代表的是当前计算的斐波那契数。
设置 `prev2 = fib(N-1)`。
设置 `prev1 = current`。
当我们到达` N+1`，将退出循环并返回 `current`。

```java
class Solution {
    public int fib(int N) {
        if (N <= 1) {
            return N;
        }
        int pre1=0;
        int pre2=1;
        int ans;
      for(int i= 2;i<N+1;i++){
          ans=pre1+pre2;
          pre1=pre2;
          pre2=ans;
      }
      return ans;
    }
}
```

![image-20210103133641275](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20210103133641275.png)

**复杂度分析**

时间复杂度：$O(N)$。
空间复杂度：$O(1)$，仅仅使用了 ans，pre1，pre2。

下面可以从数学的角度

公式：

![在这里插入图片描述](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/d5e05b3f46910d4bf79d3b235e5a3d7803b63bce092c6c20a53c16d228e33113.png)

```java
class Solution {
    public int fib(int N) {
        double goldenRatio = (1 + Math.sqrt(5)) / 2;
        return (int)Math.round(Math.pow(goldenRatio, N)/ Math.sqrt(5));
    }
}
```

复杂度

两个都是常数阶

或者是使用矩阵的方法：

![在这里插入图片描述](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/09fd107ad5ce57b564aaae069bacd1b04fbb5033197d99305e3a45764bf07fee.png)

## 9.找零钱

[322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

示例 1：

输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
示例 2：

输入：coins = [2], amount = 3
输出：-1
示例 3：

输入：coins = [1], amount = 0
输出：0

```java
public class Solution {
    public int coinChange(int[] coins, int amount) {
        if (amount < 1) {
            return 0;
        }
        return coinChange(coins, amount, new int[amount]);
    }

    private int coinChange(int[] coins, int rem, int[] count) {
        if (rem < 0) {
            return -1;
        }
        if (rem == 0) {
            return 0;
        }
        if (count[rem - 1] != 0) {
            return count[rem - 1];
        }
        int min = Integer.MAX_VALUE;
        for (int coin : coins) {
            int res = coinChange(coins, rem - coin, count);
            if (res >= 0 && res < min) {
                min = 1 + res;
            }
        }
        count[rem - 1] = (min == Integer.MAX_VALUE) ? -1 : min;
        return count[rem - 1];
    }
}
```

## 10 二分查找

![在这里插入图片描述](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/8ce178fcc07617d6448a086593c0bacc0d126d922a9a96f5c0b7995f1a16547a-file_1578027100677)

```JAVA
class Solution {
  public int search(int[] nums, int target) {
    int pivot, left = 0, right = nums.length - 1;
    while (left <= right) {
      pivot = left + (right - left) / 2;  //防止溢出
      if (nums[pivot] == target) return pivot;
      if (target < nums[pivot]) right = pivot - 1;
      else left = pivot + 1;
    }
    return -1;
  }
}
```

提醒：`pivot = left + (right - left) / 2;` 

如果是`right+left`两个都是大数的话就可能出现溢出。

![二分查找.png](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/8dfd46d9ba8374bae37d3583def5878bd7462698dc9d2311c159d110aecc7557-二分查找.png)

主要原因：写法1 是$[L,R)$,写法2 是$[L,R]$

用二分法查找边界。

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```



```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int[] res = new int[] {-1, -1};
        res[0] = binarySearch(nums, target, true);
        res[1] = binarySearch(nums, target, false);
        return res;
    }
    //leftOrRight为true找左边界 false找右边界
    public int binarySearch(int[] nums, int target, boolean leftOrRight) {
        int res = -1;
        int left = 0, right = nums.length - 1, mid;
        while(left <= right) {
            mid = left + (right - left) / 2;
            if(target < nums[mid])
                right = mid - 1;
            else if(target > nums[mid])
                left = mid + 1;
            else {
                //这里改变了。
                res = mid;
                //处理target == nums[mid]
                if(leftOrRight)
                    // 别返回，锁定左侧边界
                    right = mid - 1;
                else                    
                    // 别返回，锁定右侧边界
                    left = mid + 1;
            }
        }
        return res;
    }
}
```

## 11 双指针

- 快慢指针
  - 解决主要解决链表中的问题，比如典型的判定链表中是否包含环；

- 左右指针
  - 主要解决数组（或者字符串）中的问题，比如二分查找。

### 快慢指针

#### [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
boolean hasCycle(ListNode head) {
    ListNode fast, slow;
    fast = slow = head;
    while (fast != null && fast.next != null) {
        fast = fast.next.next;
        slow = slow.next;

        if (fast == slow) return true;
    }
    return false;
}
```

#### [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

  输入一个链表，判断是否是环形链表(尾节点与中间节点链接)
	输入：head = [3,2,0,-4], pos = 1
	输出：tail connects to node index 1
	解释：链表中有一个环，其尾部连接到第二个节点。
	输入：head = [1], pos = -1
	输出：no cycle
	解释：链表中没有环。

![fig1](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/142_fig1.png)

链表头到环入口长度为--a
环入口到相遇点长度为--b
相遇点到环入口长度为--c
slow a+b
fast a+n(b+c)
2*slow=fast
==>a=c+(n−1)(b+c)  所以两个指针分别从链表头和相遇点出发，最后一定相遇于环入口
Time O(n ) Space  O(1)

```java
ListNode detectCycle(ListNode head) {
    ListNode fast, slow;
    fast = slow = head;
    while (fast != null && fast.next != null) {
        fast = fast.next.next;
        slow = slow.next;
        if (fast == slow) break;
    }
    if (fast == null || fast.next == null) {
        // fast 遇到空指针说明没有环
        return null;
    }

    slow = head;
    while (slow != fast) {
        fast = fast.next;
        slow = slow.next;
    }
    return slow;
}
```

**链表中点**

#### [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

```java
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode fast, slow;
        fast = slow = head;
        // 快指针先前进 n 步
        while (n > 0) {
            n--;
            fast = fast.next;
        }
        if (fast == null) {
            // 如果此时快指针走到头了，
            // 说明倒数第 n 个节点就是第一个节点
            return head.next;
        }
        // 让慢指针和快指针同步向前
        while (fast != null && fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }
        // slow.next 就是倒数第 n 个节点，删除它
        slow.next = slow.next.next;
        return head; 
    }
```

倒数 可以利用栈的先进后出的性质来，也就是逆置链表后再删除。

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0, head);
    Deque<ListNode> stack = new LinkedList<ListNode>();
    ListNode cur = dummy;
    while (cur != null) {
        stack.push(cur);
        cur = cur.next;
    }
    for (int i = 0; i < n; ++i) {
        stack.pop();
    }
    ListNode prev = stack.peek();
    prev.next = prev.next.next;
    ListNode ans = dummy.next;
    return ans;
}
```





### 左右指针

二分查找

两数之和

#### [344. 反转字符串](https://leetcode-cn.com/problems/reverse-string/)

```java
void reverseString(char[] arr) {
    int left = 0;
    int right = arr.length - 1;
    while (left < right) {
        // 交换 arr[left] 和 arr[right]
        char temp = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;
        left++; right--;
    }
}
```



## 	12 最长子序列

### 概念

最长公共子序列（Longest Common Subsequence，简称 LCS）是一道非常经典的面试题目，因为它的解法是典型的二维动态规划，大部分比较困难的字符串问题都和这个问题一个套路，比如说编辑距离。而且，这个算法稍加改造就可以用于解决其他问题，所以说 LCS 算法是值得掌握的。

  1. 子序列(subsequence)： 
  	一个特定序列的子序列就是将给定序列中零个或多个元素去掉后得到的结果(不改变元素间相对次序)。例如序列$<A,B,C,B,D,A,B>$的子序列有：$<A,B>、<B,C,A>、<A,B,C,D,A>$等。
  2. 公共子序列(common subsequence)： 
  给定序列X和Y，序列Z是X的子序列，也是Y的子序列，则Z是X和Y的公共子序列。例如$X=<A,B,C,B,D,A,B>，Y=<B,D,C,A,B,A>$，那么序列$Z=<B,C,A>$为X和Y的公共子序列，其长度为3。但Z不是X和Y的最长公共子序列，而序列$<B,C,B,A>和<B,D,A,B>$也均为X和Y的最长公共子序列，长度为4，而X和Y不存在长度大于等于5的公共子序列。
  3. 最长公共子序列问题(LCS:longest-common-subsequence problem)：
  顾名思义是这两个序列中公共子序列最长的那个。
### 问题分析

例如：这里我们求$text1="abcde"$,$text2="ace"$
		1. $S\{s1,s2,s3....si\} \     \ T\{t1,t2,t3,t4....tj\}$
		2.　子问题划分
(1) 如果S的最后一位等于T的最后一位，则最大子序列就是{s1,s2,s3...si-1}和{t1,t2,t3...tj-1}的最大子序列+1
(2) 如果S的最后一位不等于T的最后一位，那么最大子序列就是
		① {s1,s2,s3..si}和 {t1,t2,t3...tj-1} 最大子序列
		② {s1,s2,s3...si-1}和{t1,t2,t3....tj} 最大子序列
		以上两个自序列的最大值
		3.　边界
		只剩下{s1}和{t1}，如果相等就返回1，不等就返回0
		4.　使用一个表格来存储dp的结果
		如果 $S [ i ] = = T[j] 则dp[i][j] = dp[ i-1 ][j-1] + 1$
		否则$dp[i][j] = max(dp[i][j-1],dp[i-1][j])$
这里使用DP二维数组表示：

|      | a                 | b    | c                   | d    | e    |
| ---- | ----------------- | ---- | ------------------- | ---- | ---- |
| a    | 1("a"与"a"LCS=1)  | 1    | 1                   | 1    | 1    |
| c    | 1("ac"与"a"LCS=1) | 1    | 2("ac"与"abc"LCS=2) | 2    | 2    |
| e    | 2                 | 2    | 2                   | 2    | 3    |

代码实现
```c++
int LCS(string text1, string text2) {
        int length1 = text1.length(), length2 = text2.length();
        std::vector<int> temp(length2 + 1, 0);
        std::vector<std::vector<int>> dp(length1 + 1, temp);

        for (int i = 1; i < length1 + 1; i++) {
            for (int j = 1; j < length2 + 1; j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[length1][length2];
    }
```


> 更多资料：
> [算法导论-----最长公共子序列LCS（动态规划）](https://blog.csdn.net/so_geili/article/details/53737001)
> [动态规划解最长公共子序列(LCS)](https://blog.csdn.net/weixin_40673608/article/details/84262695)
> [动态规划之最长公共子序列（LCS）](https://leetcode-cn.com/problems/longest-common-subsequence/solution/dong-tai-gui-hua-zhi-zui-chang-gong-gong-zi-xu-lie/)
> 

## 13.最长连续序列

[128. Longest Consecutive Sequence](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

方法一：排序
参考代码

```JAVA
public int longestConsecutive(int[] nums) {
    if (nums.length == 0) return 0;

    Arrays.sort(nums);

    // max 最终结果, curr 当前长度, last 上个数字
    int max = 1, curr = 1, last = nums[0];
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] == last) continue;
        if (nums[i] == last + 1) curr++; // 符合连续，长度 +1
        else {
            max = Math.max(max, curr); // 连不上了，记录长度
            curr = 1; // 重新开始
        }
        last = nums[i];
    }
    max = Math.max(max, curr); // 别忘了最后一段的连续区间
    return max;
}
```
复杂度分析
时间复杂度：$O(nlog(n)))$
空间复杂度：$O(1)$
副作用：影响原数组
方法二：集合
思路
利用 $O(1)$时间复杂度「查询是否有下一个」
优化：如果有比自己小一点的，那自己不查，让小的去查（详见代码）
贪心思想？
参考代码
```JAVa
public int longestConsecutive(int[] nums) {
    if (nums.length == 0) return 0;

    int n = nums.length, max = 1;
    Set<Integer> set = new HashSet<>();
    for (int v : nums) set.add(v);

    for (int v : nums) {
        // 技巧：如果有比自己小一点的，那自己不查，让小的去查
        if (set.contains(v - 1)) continue;

        int r = v; // r: right 表示「以 v 开头，能连续到多少」
        while (set.contains(r + 1)) r++; // 逐个查看
        max = Math.max(max, r - v + 1); // 记录区间 [v, r] 长度
    }
    return max;
}
```
复杂度分析
时间复杂度：$O(n)$)
虽 for 内有 while，但每个元素最多被查 2 次
第一次在 set.contains(v - 1)，如元素 5 被 6 查
第二次在 set.contains(r + 1)，如元素 5 被 4 查
空间复杂度：$O(n)$
方法三：哈希表
思路
虽然代码结构与上述 「方法二：集合」十分相似，但思路由差异，值得提及
少了方法二的优化：只对小的执行查询
利用前面已知的右边界，快速找到当前需要的右边界（详见代码）
记忆化
参考代码
```java
public int longestConsecutive(int[] nums) {
    if (nums.length == 0) return 0;

    Map<Integer, Integer> map = new HashMap<>(); // 记录区间 [v, r]
    for (int v : nums) map.put(v, v);

    int max = 1;
    for (int v : nums) {
        int r = v;
        while (map.containsKey(r + 1))
            r = map.get(r + 1); // 利用前面已知的右边界，快速找到当前需要的右边界
        map.put(v, r);
        max = Math.max(max, r - v + 1);
    }
    return max;
}
```
复杂度分析
时间复杂度：$O(n)$
空间复杂度：$O(n)$
方法四：并查集
思路
初始：所有元素各自为战
首次遍历：所有元素 x 向各自邻居 x + 1，发起结盟，并「以大者为领队」
若有邻居，才结盟成功
领队，即 区间右边界
不只是元素 x 与邻居 x + 1 结盟，而是整个 x 所在队伍与整个 x + 1 所在队伍结盟
如 [1, 2, 3] 与 [4, 5] 两个队伍结盟
二次遍历：记录所有人与其领队距离
距离，即 区间右边界 - 当前元素 + 1
参考代码
```java
public int longestConsecutive(int[] nums) {
    if (nums.length == 0) return 0;
    
    // 首次遍历，与邻居结盟
    UnionFind uf = new UnionFind(nums);
    for (int v : nums)
        uf.union(v, v + 1); // uf.union() 结盟

    // 二次遍历，记录领队距离
    int max = 1;
    for (int v : nums)
        max = Math.max(max, uf.find(v) - v + 1); // uf.find() 查找领队
    return max;
}

class UnionFind {
    private int count;
    private Map<Integer, Integer> parent; // (curr, leader)

    UnionFind(int[] arr) {
        parent = new HashMap<>();
        for (int v : arr)
            parent.put(v, v); // 初始时，各自为战，自己是自己的领队

        count = parent.size(); // 而非 arr.length，因可能存在同 key 的情况
        // 感谢 [@icdd](/u/icdd/) 同学的指正
    }

    // 结盟
    void union(int p, int q) {
        // 不只是 p 与 q 结盟，而是整个 p 所在队伍 与 q 所在队伍结盟
        // 结盟需各领队出面，而不是小弟出面
        Integer rootP = find(p), rootQ = find(q);
        if (rootP == rootQ) return;
        if (rootP == null || rootQ == null) return;

        // 结盟
        parent.put(rootP, rootQ); // 谁大听谁
        // 应取 max，而本题已明确 p < q 才可这么写
        // 当前写法有损封装性，算法题可不纠结

        count--;
    }

    // 查找领队
    Integer find(int p) {
        if (!parent.containsKey(p))
            return null;

        // 递归向上找领队
        int root = p;
        while (root != parent.get(root))
            root = parent.get(root);

        // 路径压缩：扁平化管理，避免日后找领队层级过深
        while (p != parent.get(p)) {
            int curr = p;
            p = parent.get(p);
            parent.put(curr, root);
        }

        return root;
    }
}
```
复杂度分析
时间复杂度：$O(n)$
空间复杂度：$O(n)$

作者：lzhlyle
链接：https://leetcode-cn.com/problems/longest-consecutive-sequence/solution/java-pai-xu-ji-he-ha-xi-biao-bing-cha-ji-by-lzhlyl/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。







## 14.LRU

题目描述

设计LRU缓存结构，该结构在构造时确定大小，假设大小为K，并有如下两个功能

- set(key, value)：将记录(key, value)插入该结构
- get(key)：返回key对应的value值

[要求]

1. set和get方法的时间复杂度为O(1)
2. 某个key的set或get操作一旦发生，认为这个key的记录成了最常使用的。
3. 当缓存的大小超过K时，移除最不经常使用的记录，即set或get最久远的。

若opt=1，接下来两个整数x, y，表示set(x, y)
若opt=2，接下来一个整数x，表示get(x)，若x未出现过或已被移除，则返回-1
对于每个操作2，输出一个答案

示例1

```JAVA
import java.util.*;


public class Solution {
    /**
     * lru design
     * @param operators int整型二维数组 the ops
     * @param k int整型 the k
     * @return int整型一维数组
     */
    public int[] LRU (int[][] operators, int k) {
        // write code here
        // write code here
        LinkedHashMap<Integer,Integer> lruMap = new LinkedHashMap<Integer,Integer>();
        ArrayList<Integer> result = new ArrayList();
         
        for(int[] opat:operators){
            int key = opat[1];
            switch(opat[0]){
                case 1:
                    int value = opat[2];
                    if(lruMap.size()<k){
                        lruMap.put(key,value);
                    }else{
                        Iterator ot = lruMap.keySet().iterator();
                        lruMap.remove(ot.next());
                        lruMap.put(key,value);
                    }
                    break;
                case 2:
                    if(lruMap.containsKey(key)){
                        int val = lruMap.get(key);
                        result.add(val);
                        lruMap.remove(key);
                        lruMap.put(key,val);
                    }else{
                        result.add(-1);
                    }
                    break;
                default:
            }
        }
        int[] resultArr = new int[result.size()];
            int i=0;
            for(int a:result){
                resultArr[i++]=a;
            }
        return resultArr;
    }
}
```

## 15.滑动窗口



[76.minimum-window-substring](https://leetcode-cn.com/problems/minimum-window-substring/)



```java
class Solution {
    public String minWindow(String s, String t) {
        HashMap <Character, Integer> need = new HashMap<>();
        HashMap <Character, Integer> window = new HashMap<>();
        for (char c :  t.toCharArray()) need.put(c, need.getOrDefault(c, 0) + 1);

        int left = 0, right = 0;
        int valid = 0;
        // 记录最小覆盖字串的起始索引及长度
        int start = 0, len = Integer.MAX_VALUE;
        while (right < s.length()) {
            char c = s.charAt(right);
            right++;
            // 判断取出的字符是否在字串中
            if (need.containsKey(c)) {
                window.put(c, window.getOrDefault(c,0) + 1);
                if (window.get(c).equals(need.get(c))) {
                    valid++;
                }
            }

            // 判断是否需要收缩（已经找到合适的覆盖串）
            while (valid == need.size()) {
                if (right - left < len) {
                    start = left;
                    len = right - left;
                }

                char c1 = s.charAt(left);
                left++;
                if (need.containsKey(c1)) {
                    if (window.get(c1).equals(need.get(c1))) {
                        valid--;
                    }
                     window.put(c1, window.getOrDefault(c1, 0) - 1);
                }

            }
        }

        return len == Integer.MAX_VALUE ? "" : s.substring(start, start + len);
    }
}
```

LeetCode 567 题，Permutation in String

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        HashMap<Character,Integer> need =new HashMap<>();
        HashMap<Character,Integer> window=new HashMap<>();
        for(char c:s1.toCharArray()){
            need.put(c,need.getOrDefault(c,0)+1);
        }
        int left=0,right=0;
        int valid=0;

        int start=0,len=Integer.MAX_VALUE;

        while(right<s2.length())
        {
            char c=s2.charAt(right);
            right++;
            if(need.containsKey(c))
            {
                window.put(c,window.getOrDefault(c,0)+1);
                if(need.get(c).intValue()==window.get(c).intValue())
                {
                    valid++;
                }
            }
            while(right-left>=s1.length())
            {
                if (valid == need.size())
                    return true;
                char d = s2.charAt(left);
                left++;
                // 进行窗口内数据的一系列更新
                if (need.containsKey(d)) {
                    if (window.get(d).intValue()==(need.get(d).intValue()))
                        valid--;
                        window.put(d,window.get(d).intValue()-1);
                }
            }
        }
        return false;
    }
}
```

[438. Find All Anagrams in a String](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

不知到问题出现在哪里

![image-20210118001832083](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20210118001832083.png)

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        HashMap<Character,Integer> need =new HashMap<>();
        HashMap<Character,Integer> win= new HashMap<>();

        for(char temp:p.toCharArray())
        {
            need.put(temp,need.getOrDefault(temp,0)+1);
        }
        int right=0,left=0;
        int count=0;
        List<Integer> res =new ArrayList<Integer>();
        while(right<s.length())
        {
            char c=s.charAt(right);
            right++;
            if(need.containsKey(c))
            {
                win.put(c,win.getOrDefault(c,0)+1);
                if(win.get(c).equals(need.get(c)));
                {
                    count++;
                }
            }
            while(count== need.size())
            {
                if(right - left == p.length())
                    res.add(left);
                char d=s.charAt(left);
                left++;
                if(need.containsKey(d))
                {
                    if(need.get(d).equals(win.get(d)))
                    {    
                        count--;
                    }
                    win.put(d,win.get(d)-1);
                }
            }
         }
        return res;
    }
}
```

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        // 构建目标容器和滑窗命中容器
        HashMap<Character,Integer> need = new HashMap<>();
        HashMap<Character,Integer> window = new HashMap<>();
        // 初始化目标容器
        for(int i=0;i<p.length();i++){
            need.put(p.charAt(i),need.getOrDefault(p.charAt(i),0)+1);
        }
        // 设置滑窗参数
        int left = 0;
        int right = 0;
        int valid_num = 0;
        List<Integer> markIndex = new ArrayList();
        // 滑窗到达边缘停止
        while(right < s.length()){
            // 先扩大滑窗
            char addChar = s.charAt(right);
            right++;
            // 如果元素是目标元素,算作滑窗命中
            if(need.containsKey(addChar)){
                window.put(addChar,window.getOrDefault(addChar,0)+1);
                if(window.get(addChar).equals(need.get(addChar))){
                    valid_num++;
                }
            }
            // 如果当前命中数包含所有目标元素,则开始缩减窗口
            while(valid_num == need.size()){
                // 这题要求子串不包括其他字符,必须连续,所以要校验长度
                // 如果长度符合,那就记住前下标
                if(right - left == p.length()){
                    markIndex.add(left);
                }
                char removeChar = s.charAt(left);
                left++;
                // 判断和缩小滑窗边界
                if(need.containsKey(removeChar)){
                    if(need.get(removeChar).equals(window.get(removeChar))){
                        valid_num--;
                    }
                    window.put(removeChar,window.get(removeChar)-1);
                }
            }
        }
        // 返回值
        return markIndex;
    }
}
```

结案了，一个不规范的`;`引发的问题，我找了一天都没找到，买完菜回来再仔细一看，发现了这个万恶的错误。

![image-20210119172302392](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20210119172302392.png)

## 16. 最长公共前缀

方法一：横向扫描
用$ \textit{LCP}(S_1 \ldots S_n)$ 表示字符串 $S_1 \ldots S_n $ 的最长公共前缀。
可以得到以下结论：
$\textit{LCP}(S_1 \ldots S_n) = \textit{LCP}(\textit{LCP}(\textit{LCP}(S_1, S_2),S_3),\ldots S_n)$

基于该结论，可以得到一种查找字符串数组中的最长公共前缀的简单方法。依次遍历字符串数组中的每个字符串，对于每个遍历到的字符串，更新最长公共前缀，当遍历完所有的字符串以后，即可得到字符串数组中的最长公共前缀。


如果在尚未遍历完所有的字符串时，最长公共前缀已经是空串，则最长公共前缀一定是空串，因此不需要继续遍历剩下的字符串，直接返回空串即可。

![fig1](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/14_fig1.png)

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) {
            return "";
        }
        String prefix = strs[0];
        int count = strs.length;
        for (int i = 1; i < count; i++) {
            prefix = longestCommonPrefix(prefix, strs[i]);
            if (prefix.length() == 0) {
                break;
            }
        }
        return prefix;
    }

    public String longestCommonPrefix(String str1, String str2) {
        int length = Math.min(str1.length(), str2.length());
        int index = 0;
        while (index < length && str1.charAt(index) == str2.charAt(index)) {
            index++;
        }
        return str1.substring(0, index);
    }
}
```

时间复杂度：$O(mn)$

空间复杂度： $O(1)$

方法二：纵向扫描
方法一是横向扫描，依次遍历每个字符串，更新最长公共前缀。另一种方法是纵向扫描。纵向扫描时，从前往后遍历所有字符串的每一列，比较相同列上的字符是否相同，如果相同则继续对下一列进行比较，如果不相同则当前列不再属于公共前缀，当前列之前的部分为最长公共前缀。

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) {
            return "";
        }
        int length = strs[0].length();
        int count = strs.length;
        for (int i = 0; i < length; i++) {
            char c = strs[0].charAt(i);
            for (int j = 1; j < count; j++) {
                if (i == strs[j].length() || strs[j].charAt(i) != c) {
                    return strs[0].substring(0, i);
                }
            }
        }
        return strs[0];
    }
}

```

时间复杂度：$O(mn)$

空间复杂度： $O(1)$

方法三：分治
注意到 $\textit{LCP}LCP$ 的计算满足结合律，有以下结论：

$\textit{LCP}(S_1 \ldots S_n) = \textit{LCP}(\textit{LCP}(S_1 \ldots S_k), \textit{LCP} (S_{k+1} \ldots S_n))$

其中 $\textit{LCP}(S_1 \ldots S_n)$是字符串 $S_1 \ldots S_n$的最长公共前缀，$1 < k < n$。

基于上述结论，可以使用分治法得到字符串数组中的最长公共前缀。对于问题$ \textit{LCP}(S_i\cdots S_j)$，可以分解成两个子问题$ \textit{LCP}(S_i \ldots S_{mid})$ 与 $\textit{LCP}(S_{mid+1} \ldots S_j)$，其中 $mid=\frac{i+j}{2}$。对两个子问题分别求解，然后对两个子问题的解计算最长公共前缀，即为原问题的解。

![fig3](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/14_fig3.png)

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) {
            return "";
        } else {
            return longestCommonPrefix(strs, 0, strs.length - 1);
        }
    }

    public String longestCommonPrefix(String[] strs, int start, int end) {
        if (start == end) {
            return strs[start];
        } else {
            int mid = (end - start) / 2 + start;
            String lcpLeft = longestCommonPrefix(strs, start, mid);
            String lcpRight = longestCommonPrefix(strs, mid + 1, end);
            return commonPrefix(lcpLeft, lcpRight);
        }
    }

    public String commonPrefix(String lcpLeft, String lcpRight) {
        int minLength = Math.min(lcpLeft.length(), lcpRight.length());       
        for (int i = 0; i < minLength; i++) {
            if (lcpLeft.charAt(i) != lcpRight.charAt(i)) {
                return lcpLeft.substring(0, i);
            }
        }
        return lcpLeft.substring(0, minLength);
    }
}
```

复杂度分析

时间复杂度：$O(mn)$，其中 mm 是字符串数组中的字符串的平均长度，n 是字符串的数量。时间复杂度的递推式是$ T(n)=2 \cdot T(\frac{n}{2})+O(m)$，通过计算可得$ T(n)=O(mn)$

空间复杂度：$O(m \log n)$，其中 m 是字符串数组中的字符串的平均长度，n 是字符串的数量。空间复杂度主要取决于递归调用的层数，层数最大为$ \log n$，每层需要 m的空间存储返回结果。

方法四：二分查找
显然，最长公共前缀的长度不会超过字符串数组中的最短字符串的长度。用 $\textit{minLength}$ 表示字符串数组中的最短字符串的长度，则可以在$ [0,\textit{minLength}] $的范围内通过二分查找得到最长公共前缀的长度。每次取查找范围的中间值$ mid$，判断每个字符串的长度为 $\textit{mid}$ 的前缀是否相同，如果相同则最长公共前缀的长度一定大于或等于 $\textit{mid}$，如果不相同则最长公共前缀的长度一定小于 $\textit{mid}$，通过上述方式将查找范围缩小一半，直到得到最长公共前缀的长度。

![fig4](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/14_fig4.png)

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) {
            return "";
        }
        int minLength = Integer.MAX_VALUE;
        for (String str : strs) {
            minLength = Math.min(minLength, str.length());
        }
        int low = 0, high = minLength;
        while (low < high) {
            int mid = (high - low + 1) / 2 + low;
            if (isCommonPrefix(strs, mid)) {
                low = mid;
            } else {
                high = mid - 1;
            }
        }
        return strs[0].substring(0, low);
    }

    public boolean isCommonPrefix(String[] strs, int length) {
        String str0 = strs[0].substring(0, length);
        int count = strs.length;
        for (int i = 1; i < count; i++) {
            String str = strs[i];
            for (int j = 0; j < length; j++) {
                if (str0.charAt(j) != str.charAt(j)) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

## TopK

```java
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> ret;
        if (k==0 || k>input.size()) return ret;
        sort(input.begin(), input.end());
        return vector<int>({input.begin(), input.begin()+k});  
    }
};
```



```java
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> ret;
        if (k==0 || k > input.size()) return ret;
        priority_queue<int, vector<int>> pq;
        for (const int val : input) {
            if (pq.size() < k) {
                pq.push(val);
            }
            else {
                if (val < pq.top()) {
                    pq.pop();
                    pq.push(val);
                }
 
            }
        }
 
        while (!pq.empty()) {
            ret.push_back(pq.top());
            pq.pop();
        }
        return ret;
    }
};
```

```java
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> list = new ArrayList<>();
        if (input == null || input.length == 0 || k > input.length || k == 0)
            return list;
        int[] arr = new int[k + 1];//数组下标0的位置作为哨兵，不存储数据
        //初始化数组
        for (int i = 1; i < k + 1; i++)
            arr[i] = input[i - 1];
        buildMaxHeap(arr, k + 1);//构造大根堆
        for (int i = k; i < input.length; i++) {
            if (input[i] < arr[1]) {
                arr[1] = input[i];
                adjustDown(arr, 1, k + 1);//将改变了根节点的二叉树继续调整为大根堆
            }
        }
        for (int i = 1; i < arr.length; i++) {
            list.add(arr[i]);
        }
        return list;
    }
     /**
     * @Author: ZwZ
     * @Description: 构造大根堆 
     * @Param: [arr, length]  length:数组长度 作为是否跳出循环的条件
     * @return: void 
     * @Date: 2020/1/30-22:06
     */
    public void buildMaxHeap(int[] arr, int length) {
        if (arr == null || arr.length == 0 || arr.length == 1)
            return;
        for (int i = (length - 1) / 2; i > 0; i--) {
            adjustDown(arr, i, arr.length);
        }
    }
    /**
     * @Author: ZwZ
     * @Description: 堆排序中对一个子二叉树进行堆排序 
     * @Param: [arr, k, length] 
     * @return:  
     * @Date: 2020/1/30-21:55
     */
    public void adjustDown(int[] arr, int k, int length) {
        arr[0] = arr[k];//哨兵
        for (int i = 2 * k; i <= length; i *= 2) {
            if (i < length - 1 && arr[i] < arr[i + 1])
                i++;//取k较大的子结点的下标
            if (i > length - 1 || arr[0] >= arr[i])
                break;
            else {
                arr[k] = arr[i];
                k = i; //向下筛选
            }
        }
        arr[k] = arr[0];
    }
}
```



最大的第K个数

```java
public class FindKth {
    public int findKth(int[] a, int n, int K) {
        return find(a, 0, n-1, K);
    }
     
    //递归寻找数组中第K大的元素
    private int find(int[] a, int low, int high, int K) {
        int pivot = partition(a, low, high);
 
        if(pivot + 1 < K)//中轴位置少于K个，在右子数组中继续查找
            return find(a, pivot+1, high, K);
        else if(pivot + 1 > K)//中轴位置大于K个，在左子数组中继续查找
            return find(a, low, pivot-1, K);
        else//中轴元素正好是第K大的元素
            return a[pivot];
    }
     
    //将数组划分为两部分，左边较大，右边较小
    private int partition(int[] a, int low, int high) {
        // 将数组首元素作为每一轮比较的基准
        int pivotValue = a[low];
 
        while (low < high) {
            // 从右往左扫描，直到遇到比基准元素小的元素
            while (low < high && a[high] <= pivotValue)
                --high;
 
            // 将右子数组中不合格的元素放到左边不合格元素的位置（原元素已经移走）
            a[low] = a[high];
 
            // 从左往右扫描，直到遇到比基准元素大的元素
            while (low < high && a[low] >= pivotValue)
                ++low;
 
            // 将左子数组中不合格的元素放到左边不合格元素的位置（原元素已经移走）
            a[high] = a[low];
        }
 
        // 将基准元素放到中间位置
        a[low] = pivotValue;
 
        // 返回数组的中轴位置
        return low;
    }
```

