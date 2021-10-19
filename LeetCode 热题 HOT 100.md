# LeetCode 热题 HOT 100

## [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

直接暴力破解

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] ans = new int[2];
        for(int i = 0; i < nums.length; i++){
            for(int j = i+1; j < nums.length; j++){
                if(nums[i] + nums[j] == target){
                    ans[0] = i;
                    ans[1] = j;
                    return ans;
                }
            }
        }
        return ans;
    }
}
```

使用HashMap，虽然我想不出

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] ans = new int[2];
        for(int i = 0; i < nums.length; i++){
            for(int j = i+1; j < nums.length; j++){
                if(nums[i] + nums[j] == target){
                    ans[0] = i;
                    ans[1] = j;
                    return ans;
                }
            }
        }
        return ans;
    }
}
```

## [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

1. 遇左括号入栈
2. 遇右括号出栈，并且判断是否一致，如不一致提前返回false
3. 如果最后面栈为空返回true，否则false

做过好多遍了，还是记不住

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if(ch == '('){
                stack.push(')');
            }else if(ch == '{'){
                stack.push('}');
            }else if(ch == '['){
                stack.push(']');
            }else if(stack.isEmpty() || stack.pop() != ch){
                return false;
            }
        }
        return stack.isEmpty();
    }
}
```

## [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

- 类似归并排序的`merge`

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode ans = new ListNode(0);
        ListNode tmp = ans;
        while(l1 != null && l2 != null){
            if(l1.val < l2.val){
                tmp.next = l1;
                l1 = l1.next;
            }else{
                tmp.next = l2;
                l2 = l2.next;
            }
            tmp = tmp.next;
        }
        if(l1 != null){
            tmp.next = l1;
        }
        if(l2 != null){
            tmp.next = l2;
        }
        return ans.next;
    }
}
```

- 递归版（我是想不到的，想到了也写不出）

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null){
            return l2;
        }
        if(l2 == null){
            return l1;
        }
        if(l1.val < l2.val){
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        }else{
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
```

## [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

碰到过很多遍了，动态规划，贪心，依旧不是很清楚

- 如果 `sum > 0`，则说明 sum 对结果有增益效果，则 sum 保留并加上当前遍历数字
- 如果 `sum <= 0`，则说明 sum 对结果无增益效果，需要舍弃，则 sum 直接更新为当前遍历数字
- 每次比较 `sum` 和 `ans`的大小，将最大值置为`ans`，遍历结束返回结果

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int ans = nums[0];
        int sum = 0;
        for(int i = 0; i < nums.length; i++){
            if(sum > 0){
                sum += nums[i];
            }else{
                sum = nums[i];
            }
            ans = Math.max(ans, sum);
        }
        return ans;
    }
}
```

## [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

动态规划

````java
class Solution {
    public int climbStairs(int n) {
        int[] dp = new int[n+1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i <= n; i++){
            dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n];
    }
}
````

斐波那契数列公式

```java
class Solution {
    public int climbStairs(int n) {
        double sqrt_5 = Math.sqrt(5);
        double fib_n = Math.pow((1 + sqrt_5) / 2, n + 1) - Math.pow((1 - sqrt_5) / 2,n + 1);
        return (int)(fib_n / sqrt_5);
    }
}
```

## [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

```java
class Solution {
    List<Integer> list = new ArrayList<>();

    public List<Integer> inorderTraversal(TreeNode root) {
        inorder(root);
        return list;
    }

    public void inorder(TreeNode root){
        if(root == null){
            return;
        }
        inorder(root.left);
        list.add(root.val);
        inorder(root.right);
    }
}
```

## [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

递归结束条件

- 都为空指针返回`true`
- 只有一个为空返回`false`

递归过程

- 判断两个当前节点的值是否相等
- 判断 A 的右子树与 B 的左子树是否对称
- 判断 A 的左子树与 B 的右子树是否对称

好整洁的code

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return isMirror(root, root);
    }

    public boolean isMirror(TreeNode root1, TreeNode root2){
        if(root1 == null && root2 == null){
            return true;
        }
        if(root1 == null || root2 == null){
            return false;
        }
        return (root1.val == root2.val) && 
        isMirror(root1.left, root2.right) && isMirror(root1.right, root2.left);
    }
}
```

## [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        return Math.max(maxDepth(root.left), maxDepth(root.right))+1;
    }
}
```

## [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

**单调栈作用是:用O(n)的时间得知所有位置两边第一个比他大(或小)的数的位置。**

思路：

- 在 prices 数组的末尾加上一个 **哨兵**👨‍✈️(也就是一个很小的元素，这里设为 0))，就相当于作为股市收盘的标记(后面就清楚他的作用了)
- 假如栈空或者入栈元素大于栈顶元素，直接入栈
- 假如入栈元素小于栈顶元素则循环弹栈，直到入栈元素大于栈顶元素或者栈空
- 在每次弹出的时候，我们拿他与买入的值(也就是栈底)做差，维护一个最大值

```java
class Solution {
    public int maxProfit(int[] prices) {
        int ans = 0;
        Deque<Integer> st = new LinkedList<>(); // 要获取首元素和尾元素，用双端队列

        int[] tmp = new int[prices.length+1];
        System.arraycopy(prices, 0, tmp, 0, prices.length);
        tmp[prices.length] = -1; // 哨兵

        for (int price : tmp) {
            while (!st.isEmpty() && st.getFirst() > price) { // 维护单调栈
                ans = Math.max(ans, st.getFirst() - st.getLast()); // 维护最大值
                st.pop();
            }
            st.push(price);
        }
        return ans;
    }
}
```

暴力求解（超时）

```java
class Solution {
    public int maxProfit(int[] prices) {
        int ans = 0;
        for(int i = 0; i < prices.length; i++){
            for(int j = i+1; j < prices.length; j++){
                ans = Math.max(ans, prices[j]-prices[i]);
            }
        }
        return ans;
    }
}
```

## [136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

首先想到的就是HashMap（还有桶排序思想，但是不知道数据的范围）

```java
class Solution {
    public int singleNumber(int[] nums) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int num : nums){
            map.put(num, map.getOrDefault(num, 0)+1);
        }
        for(Map.Entry<Integer, Integer> m : map.entrySet()){
            if(m.getValue() == 1){
                return m.getKey();
            }
        }
        return -1;
    }
}
```

**异或**

一个值和0进行按位异或操作所得为该值，相同的两个值进行异或操作，所得为0（甲 按位异或 0 得 甲，甲 按位异或 甲 得 0）

由于每个重复元素重复两次，故他们在遍历后将相互抵消，而唯一元素只出现一次，故将得到保留

```java
class Solution {
    public int singleNumber(int[] nums) {
        int ans = 0;
        for(int num : nums){
            ans ^= num;
        }
        return ans;
    }
}
```

## [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

**哈希表**

最容易想到的方法是遍历所有节点，每次遍历到一个节点时，判断该节点此前是否被访问过

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        Set<ListNode> set = new HashSet<>();
        while(head != null){
            if(!set.add(head)){
                return true;
            }
            head = head.next;
        }
        return false;
    }
}
```

**快慢指针**

我们定义两个指针，一快一满。慢指针每次只移动一步，而快指针每次移动两步。

初始时，慢指针在位置 head，而快指针在位置 head.next。

如果在移动的过程中，快指针反过来追上慢指针，就说明该链表为环形链表。

否则快指针将到达链表尾部，该链表不为环形链表。

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head == null || head.next == null){
            return false;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while(slow != fast){
            if(fast == null || fast.next == null){
                return false;
            }
            slow = slow.next;
            fast = fast.next.next;
        }
        return true;
    }
}
```

## [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)

**辅助栈**

- 只需要设计一个数据结构，使得每个元素 a 与其相应的最小值 m 时刻保持一一对应。因此我们可以使用一个辅助栈，与元素栈同步插入与删除，用于存储与每个元素对应的最小值
- 当一个元素要入栈时，我们取当前辅助栈的栈顶存储的最小值，与当前元素比较得出最小值，将这个最小值插入辅助栈中；
- 当一个元素要出栈时，我们把辅助栈的栈顶元素也一并弹出
- 在任意一个时刻，栈内元素的最小值就存储在辅助栈的栈顶元素中

```java
class MinStack {
    Deque<Integer> stack;
    Deque<Integer> minStack;

    public MinStack() {
        stack = new LinkedList<Integer>();
        minStack = new LinkedList<Integer>();
        minStack.push(Integer.MAX_VALUE);
    }
    
    public void push(int val) {
        stack.push(val);
        minStack.push(Math.min(minStack.peek(), val));
    }
    
    public void pop() {
        stack.pop();
        minStack.pop();
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        return minStack.peek();
    }
}
```

## [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

**哈希表**

没啥好说的，和上一题差不多

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        Set<ListNode> set = new HashSet<>();
        while(headA != null){
            set.add(headA);
            headA = headA.next;
        }
        while(headB != null){
            if(set.contains(headB)){
                return headB;
            }
            headB = headB.next;
        }
        return null;
    }
}
```

**双指针**

- 只有当链表` headA `和`headB` 都不为空时，两个链表才可能相交，首先要判断是否为空
- 创建两个指针 `pA`和 `pB`，初始时分别指向两个链表的头节点 `headA`和`headB`，然后将两个指针依次遍历两个链表的每个节点
- 如果指针 `pA`不为空，则将指针`pA` 移到下一个节点；如果指针 `pB`不为空，则将指针`pB`移到下一个节点。
- 如果指针 `pA` 为空，则将指针 `pA` 移到链表 `pB` 的头节点；如果指针 `pB`为空，则将指针 `pB` 移到链表`pA` 的头节点
- 当指针`pA`和`pB` 指向同一个节点或者都为空时，返回它们指向的节点或者`null`

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null){
            return null;
        }
        ListNode pA = headA;
        ListNode pB = headB;
        while(pA != pB){
            pA = (pA == null ? headB : pA.next);
            pB = (pB == null ? headA : pB.next);
        }
        return pA;
    }
}
```

## [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

**Map**

```java
class Solution {
    public int majorityElement(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int num : nums){
            map.put(num, map.getOrDefault(num, 0)+1);
        }
        int n = nums.length/2;
        for(Map.Entry<Integer, Integer> m : map.entrySet()){
            if(m.getValue() > n){
                return m.getKey();
            }
        }
        return -1;
    }
}
```

## [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

**感觉不太好理解，哎**

**以后涉及链表操作的时候，一定要在纸上画出来**

在遍历列表时，将当前节点的next 指针改为指向前一个元素。由于节点没有引用其上一个节点，因此必须事先存储其前一个元素。在更改引用之前，还需要另一个指针来存储下一个节点。不要忘记在最后返回新的头引用！

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode cur = null;
        ListNode pre = head;
        while(pre != null){
            ListNode tmp = pre.next;
            pre.next = cur;
            cur = pre;
            pre = tmp;
        }
        return cur;
    }
}
```

## [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

自己写的，居然第一次就写出来了，有点不太敢相信

其实吧，主要就是往递归那方面去想

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        return build(root);
    }

    public TreeNode build(TreeNode root){
        if(root == null){
            return null;
        }
        TreeNode tree = new TreeNode(root.val);
        tree.left = build(root.right);
        tree.right = build(root.left);
        return tree;
    }
}
```

层序遍历

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null){
            return null;
        }
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            TreeNode tmp = q.poll();
            
            TreeNode left = tmp.left;
            tmp.left = tmp.right;
            tmp.right = left;
            
            if(tmp.left != null){
                q.offer(tmp.left);
            }
            if(tmp.right != null){
                q.offer(tmp.right);
            }
        }
        return root;
    }
}
```

## [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

转为数组，再判断即可

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        List<Integer> arr = new ArrayList<>();
        while(head != null){
            arr.add(head.val);
            head = head.next;
        }

        int front = 0;
        int back = arr.size()-1;
        while(front < back){
            if(!arr.get(front).equals(arr.get(back))){
                return false;
            }
            front++;
            back--;
        }
        return true;
    }
}
```

## [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

好巧妙

- 我们创建两个指针i和j，第一次遍历的时候指针j用来记录当前有多少非0元素。即遍历的时候每遇到一个非0元素就将其往数组左边挪，第一次遍历完后，j指针的下标就指向了最后一个非0元素下标
- 第二次遍历的时候，起始位置就从`j`开始到结束，将剩下的这段区域内的元素全部置为`0`。

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int cnt = 0;
        for(int i = 0; i < nums.length; i++){
            if(nums[i] != 0){
                nums[cnt++] = nums[i];
            }
        }
        for(int i = cnt; i < nums.length; i++){
            nums[i] = 0;
        }
    }
}
```

## [338. 比特位计数](https://leetcode-cn.com/problems/counting-bits/)

API 战士，`Integer.bitCount(i)`返回二进制表示中1的位数

```java
class Solution {
    public int[] countBits(int n) {
        int[] ans = new int[n+1];
        for(int i = 0; i <= n; i++){
            ans[i] = Integer.bitCount(i);
        }
        return ans;
    }
}
```

**Brian Kernighan 算法**

- 最直观的做法是对从 0 到 n 的每个整数直接计算「比特数」。每个int 型的数都可以用 32 位二进制数表示，只要遍历其二进制表示的每一位即可得到 1 的数目。
- Brian Kernighan 算法的原理是：对于任意整数 x，令 `x = x & (x−1)`，该运算将 x 的二进制表示的最后一个 1 变成 0。因此，对 x 重复该操作，直到 x 变成 0，则操作次数即为 x 的「比特数」。

```java
class Solution {
    public int[] countBits(int n) {
        int[] ans = new int[n+1];
        for(int i = 0; i <= n; i++){
            ans[i] = cnt(i);
        }
        return ans;
    }

    public int cnt(int n){
        int cnt = 0;
        while(n > 0){
            n &= (n-1);
            cnt++;
        }
        return cnt;
    }
}
```

## [448. 找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

```java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        int[] arr = new int[100005];
        for(int i = 1; i < nums.length; i++){
            arr[nums[i]]++;
        }
        List<Integer> ans = new ArrayList<>();
        for(int i = 1; i <= nums.length; i++){
            if(arr[i] == 0){
                ans.add(i);
            }
        }
        return ans;
    }
}
```

## [461. 汉明距离](https://leetcode-cn.com/problems/hamming-distance/)

API 战士，主要就是`Integer.bitCount()`

- 两个整数之间的汉明距离是对应位置上数字不同的位数。
- 根据以上定义，我们使用异或运算，记为⊕，当且仅当输入位不同时输出为 1
- 计算 *x* 和 y 之间的汉明距离，可以先计算 x*⊕*y，然后统计结果中等于 1 的位数

```java
class Solution {
    public int hammingDistance(int x, int y) {
        return Integer.bitCount(x^y);
    }
}
```

## [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

看了好几遍，终于弄懂了，主要就是求二叉树的高度，以每个节点为根结点求左右子树的高度，然后找到最大的那个值

```java
class Solution {
    int maxx = 0;
    
    public int diameterOfBinaryTree(TreeNode root) {
        depth(root);
        return maxx;
    }

    public int depth(TreeNode root){
        if(root == null){
            return 0;
        }
        int left = depth(root.left); // 左子树的高度
        int right = depth(root.right); // 右子树的高度
        maxx = Math.max(maxx, left+right); // 将每个节点最大直径(左子树深度+右子树深度)当前最大值比较并取大者
        return Math.max(left, right)+1; // 当前节点的深度
    }
}
```

## [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

还是有一点想不出来，但这个思路和我一样

```java
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if(root1 == null && root2 == null){
            return null;
        }
        TreeNode tree = new TreeNode((root1 == null ? 0:root1.val) + (root2 == null ? 0:root2.val));
        tree.left = mergeTrees((root1 == null ? null:root1.left), (root2 == null ? null:root2.left));
        tree.right = mergeTrees((root1 == null ? null:root1.right), (root2 == null ? null:root2.right));
        return tree;
    }
}
```

