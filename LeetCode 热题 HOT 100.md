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

额，看评论说不用单调栈，**维护单调递减的栈，当前元素每次和栈顶元素比较，大就更新答案，小就入栈**就行，然后照着这个思路写来一下，过了

```java
class Solution {
    public int maxProfit(int[] prices) {
        Deque<Integer> q = new LinkedList<>();
        q.push(prices[0]);

        int ans = 0;
        for(int i = 1; i < prices.length; i++){
            int tmp = q.peek();
            if(prices[i] > tmp){
                ans = Math.max(ans, prices[i]-tmp);
            }else{
                q.push(prices[i]);
            }
        }
        return ans;
    }
}
```

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
        // st.getFirst()-->第一个，栈底
        // st.getLast()-->最后一个，栈顶

        int[] tmp = new int[prices.length+1];
        System.arraycopy(prices, 0, tmp, 0, prices.length);
        tmp[prices.length] = -1; // 哨兵，保证所有的元素出栈

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

**贪心**，因为股票就买卖一次，那么贪心的想法很自然就是取最左最小值，取最右最大值，那么得到的差值就是最大利润

```java
class Solution {
    public int maxProfit(int[] prices) {
        int low = Integer.MAX_VALUE;
        int ans = 0;
        for(int price : prices){
            low = Math.min(low, price); // 取最左最小价格
            ans = Math.max(ans, price-low); // 直接取最大区间利润
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
        
//        这样也行，arr.size()不除2也行
//        for(int i = 0; i < arr.size()/2; i++){
//            if(arr.get(i) != arr.get(arr.size()-1-i)){
//                return false;
//            }
//        }
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

## ---------------------------

## [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

这个简洁了许多，和【加法模板】一样的思路

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode ans = new ListNode(0);
        ListNode cur = ans;

        int sum = 0, carry = 0;
        
        // while ( A 没完 || B 没完)
        while(l1 != null || l2 != null){
            
            // A 的当前位
            int x = (l1 == null ? 0 : l1.val);
            // B 的当前位
            int y = (l2 == null ? 0 : l2.val);

            // 和 = A 的当前位 + B 的当前位 + 进位carry
            sum = x+y+carry;
            
            // 进位 = 和 / 10;
            carry = sum / 10;

            // 当前位 = 和 % 10;
            cur.next = new ListNode(sum % 10);
            cur = cur.next;

            if(l1 != null){
                l1 = l1.next;
            }
            if(l2 != null){
                l2 = l2.next;
            }
        }
        
        // 判断还有进位吗
        if(carry != 0){
            cur.next = new ListNode(carry);
        }
        return ans.next;
    }
}
```

## [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

**双指针**

```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0;
        int right = height.length-1;
        int ans = 0;
        while(left < right){
            int area = Math.min(height[left], height[right])*(right-left);
            ans = Math.max(ans, area);
            if(height[left] > height[right]){
                right--;
            }else{
                left++;
            }
        }
        return ans;
    }
}
```

## [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

回溯，还是有一点不太懂啊

```java
class Solution {
    List<String> ans = new ArrayList<>();
    Map<Character, String> phoneMap = new HashMap<Character, String>() {{
        put('2', "abc");
        put('3', "def");
        put('4', "ghi");
        put('5', "jkl");
        put('6', "mno");
        put('7', "pqrs");
        put('8', "tuv");
        put('9', "wxyz");
    }};

    public List<String> letterCombinations(String digits) {
        if(digits.length() == 0){
            return ans;
        }
        backtrack(new StringBuilder(), 0, digits);
        return ans;
    }

    public void backtrack(StringBuilder combinations, int idx, String nextDigits){
        if(idx == nextDigits.length()){
            ans.add(combinations.toString());
            return;
        }
        char ch = nextDigits.charAt(idx);
        String tmp = phoneMap.get(ch);
        for(int i = 0; i < tmp.length(); i++){
            combinations.append(tmp.charAt(i));
            backtrack(combinations, idx+1, nextDigits);
            combinations.deleteCharAt(idx);
        }
    }
}
```

## [19. 删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

哈哈，被我试了4次试出来了，虽然遍历了两次

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        int cnt = 0;
        ListNode tmp = head;
        while(tmp != null){
            cnt++;
            tmp = tmp.next;
        }

        if(cnt == n){
            return head.next;
        }
        
        n = cnt - n;
        cnt = 0;
        tmp = head;
        ListNode pre = null;
        while(tmp != null){
            if(cnt == n){
                if(pre != null){ // 判不判断都行
                    pre.next = tmp.next;
                }
            }
            cnt++;
            pre = tmp;
            tmp = tmp.next;
        }
        return head;
    }
}
```

快慢指针，快指针先走n步，然后快慢一起走，直到快指针走到最后

要注意的是可能是要删除第一个节点，这个时候可以直接返回

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode slow = head;
        ListNode fast = head;

        while(n != 0){
            fast = fast.next;
            n--;
        }
        // 如果删除的是第一个结点，直接返回
        if(fast == null){
            return head.next;
        }

        while(fast.next != null){
            slow = slow.next;
            fast = fast.next;
        }
        slow.next = slow.next.next;
        return head;
    }
}
```

## [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

回溯法（做减法），然而还是不怎么会

- 当前左右括号都有大于 0 个可以使用的时候，才产生分支；
- 产生左分支的时候，只看当前是否还有左括号可以使用；
- 产生右分支的时候，还受到左分支的限制，右边剩余可以使用的括号数量一定得在严格大于左边剩余的数量的时候
- 在左边和右边剩余的括号数都等于 0 的时候结算

```java
class Solution {
    List<String> ans = new ArrayList<>();

    public List<String> generateParenthesis(int n) {
        if(n == 0){
            return ans;
        }
        dfs("", n, n);
        return ans;
    }

    public void dfs(String cur, int left, int right){
        if(left == 0 && right == 0){
            ans.add(cur);
            return;
        }
        if(left > right){
            return;
        }
        if(left > 0){
            dfs(cur + "(", left-1, right);
        }
        if(right > 0){
            dfs(cur + ")", left, right-1);
        }
    }

}
```

## [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

写了好久了，前面从头直接找一直有些点没有过，然后刚刚换了一种思路，就过了

从头找，从后找，直接返回就行

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int[] ans = {-1, -1};
        for(int i = 0; i < nums.length; i++){
            if(nums[i] == target){
                ans[0] = i;
                break;
            }
        }
        if(ans[0] == -1){
            return ans;
        }
        for(int i = nums.length-1; i >= 0; i--){
            if(nums[i] == target){
                ans[1] = i;
                break;
            }
        }
        return ans;
    }
}
```

二分搜索找左右边界

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int[] ans = {-1, -1};
        ans[0] = leftBound(nums, target);
        if(ans[0] == -1){
            return ans;
        }
        ans[1] = rightBound(nums, target);
        return ans;
    }

    public int leftBound(int[] nums, int target){
        int left = 0;
        int right = nums.length;
        while(left < right){
            int mid = left + (right-left)/2;
            if(nums[mid] == target){
                right = mid;
            }else if(nums[mid] > target){
                right = mid;
            }else if(nums[mid] < target){
                left = mid+1;
            }
        }
        if(left == nums.length){
            return -1;
        }
        return nums[left] == target ? left : -1; 
    }

    public int rightBound(int[] nums, int target){
        int left = 0;
        int right = nums.length;
        while(left < right){
            int mid = left + (right-left)/2;
            if(nums[mid] == target){
                left = mid+1;
            }else if(nums[mid] > target){
                right = mid;
            }else if(nums[mid] < target){
                left = mid+1;
            }
        }
        if(left == 0){
            return -1;
        }
        return nums[left-1] == target ? left-1:-1;
    }
}
```

## [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

回溯还是有点不太会用

```java
class Solution {
    List<List<Integer>> ans = new ArrayList<>();
    
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        if (candidates.length == 0) {
            return ans;
        }
        ArrayList<Integer> track = new ArrayList<>();
        backtrack(candidates, 0, target, track);
        return ans;
    }

    public void backtrack(int[] candidates, int begin, int target, ArrayList<Integer> track) {
        if (target < 0) {
            return;
        }
        if (target == 0) {
            ans.add(new ArrayList<>(track));
            return;
        }
        for (int i = begin; i < candidates.length; i++) {
            track.add(candidates[i]);
            backtrack(candidates, i, target - candidates[i], track);
            track.remove(track.size()-1);
        }
    }
}
```

## [46. 全排列](https://leetcode-cn.com/problems/permutations/)

**回溯法框架**

写了这么久的题目，现在也终于懂一点回溯法的了，哎

加个`visited`直接判断要快一些，就不用每次查找了

```java
class Solution {
    List<List<Integer>> ans = new LinkedList<>();

    public List<List<Integer>> permute(int[] nums) {
        LinkedList<Integer> track = new LinkedList<>();
        int[] visited = new int[nums.length];
        backtrack(nums, track, visited);
        return ans;
    }

    public void backtrack(int[] nums, LinkedList<Integer> track, int[] visited){
        if(track.size() == nums.length){
            ans.add(new LinkedList(track));
            return;
        }
        for(int i = 0; i < nums.length; i++){
            if(visited[i] == 1){
                continue;
            }
            visited[i] = 1;
            track.add(nums[i]);
            backtrack(nums, track, visited);
            visited[i] = 0;
            track.removeLast();
        }
    }
}
```

## [49. 字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

在《编程珠玑》里面看到过，用排序，但具体怎么实现不知道

使用哈希表存储每一组字母异位词，哈希表的键为一组字母异位词的标志，哈希表的值为一组字母异位词列表

遍历每个字符串，对于每个字符串，得到该字符串所在的一组字母异位词的标志，将当前字符串加入该组字母异位词的列表中。

遍历全部字符串之后，哈希表中的每个键值对即为一组字母异位词。

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();
        for(String str : strs){
            char[] arr = str.toCharArray();
            Arrays.sort(arr);
            String key = new String(arr);
            List<String> list = map.getOrDefault(key, new ArrayList<String>());
            list.add(str);
            map.put(key, list);
        }
        return new ArrayList<List<String>>(map.values());
    }
}
```

## [56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/)

似懂非懂，好吧，其实还是不是很清楚

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        // 先按照区间起始位置排序，升序排序
        Arrays.sort(intervals, (v1, v2) -> v1[0] - v2[0]);
        
        int[][] res = new int[intervals.length][2];
        int idx = -1;
        for (int[] interval: intervals) {
            // 如果结果数组是空的，或者当前区间的起始位置 > 结果数组中最后区间的终止位置，
            // 则不合并，直接将当前区间加入结果数组。
            if (idx == -1 || interval[0] > res[idx][1]) {
                res[++idx] = interval;
            } else {
                // 反之将当前区间合并至结果数组的最后区间
                res[idx][1] = Math.max(res[idx][1], interval[1]);
            }
        }
        return Arrays.copyOf(res, idx + 1);
    }
}
```

## [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)

好吧，动态规划还是不会，现在连最简单的都不会了。。

由于我们每一步只能从向下或者向右移动一步，因此要想走到 (i, j)(i,j)，如果向下走一步，那么会从 (i-1, j)(i−1,j) 走过来；如果向右走一步，那么会从 (i, j-1)(i,j−1) 走过来

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] ans = new int[m][n];
        for(int i = 0; i < m; i++){
            ans[i][0] = 1;
        }
        for(int i = 0; i < n; i++){
            ans[0][i] = 1;
        }
        for(int i = 1; i < m; i++){
            for(int j = 1;j < n; j++){
                ans[i][j] = ans[i-1][j] + ans[i][j-1];
            }
        }
        return ans[m-1][n-1];
    }
}
```

## [75. 颜色分类](https://leetcode-cn.com/problems/sort-colors/)

直接排序即可

```java
class Solution {
    public void sortColors(int[] nums) {
        Arrays.sort(nums);
    }
}
```

## [78. 子集](https://leetcode-cn.com/problems/subsets/)

```java
class Solution {
    List<List<Integer>> ans = new ArrayList<>();
    
    public List<List<Integer>> subsets(int[] nums) {
        ArrayList<Integer> path = new ArrayList<>();
        backtrack(nums, path, 0);
        return ans;
    }

    public void backtrack(int[] nums, ArrayList<Integer> path, int begin){
        ans.add(new ArrayList<>(path));
        for(int i = begin; i < nums.length; i++){
            path.add(nums[i]);
            backtrack(nums, path, i+1);
            path.remove(path.size()-1);
        }
    }
}
```

## [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)

回溯，虽然叫我写是写不出来的

```java
class Solution {
    int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    boolean[][] visited;
    char[] arr;
    int len;

    public boolean exist(char[][] board, String word) {
        if(board.length == 0){
            return false;
        }
        int r = board.length;
        int c = board[0].length;
        visited = new boolean[r][c];
        arr = word.toCharArray();
        len = word.length();
        for(int i = 0; i < r; i++){
            for(int j = 0; j < c; j++){
                if(dfs(board, i, j, 0)){
                    return true;
                }
            }
        }
        return false;
    }

    public boolean dfs(char[][] board, int r, int c, int begin){
        if(begin == len-1){
            return board[r][c] == arr[begin];
        }
        if(board[r][c] == arr[begin]){
            visited[r][c] = true;
            for(int[] dir : directions){
                int x = r + dir[0];
                int y = c + dir[1];
                if(inArea(board, x, y) && !visited[x][y]){
                    if(dfs(board, x, y, begin+1)){
                        return true;
                    }
                }
            }
            visited[r][c] = false;
        }
        return false;
    }

    public boolean inArea(char[][] board, int r, int c){
        return 0 <= r && r < board.length && 0 <= c && c < board[0].length;
    }
}
```

## [96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)

动态规划，不会，再见

**卡特兰数**（Catalan number）是 **组合数学** 中一个常出现在各种 **计数问题** 中的 **数列**

```
C0=1,Cn+1=2(2n+1)*Cn/(n+2)
```

数列的前几项为：1, 1, 2, 5, 14, 42, 132, 429, 1430, 4862，...

```
例题：n 个元素进栈序列为：1，2，3，4，...，n，则有多少种出栈序列。
```

```java
class Solution {
    public int numTrees(int n) {
        long c = 1;
        for(int i = 0; i < n; i++){
            c = (c*2*(2*i+1) / (i+2)); 
        }
        return (int)c;
    }
}
```

## [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

哎，以前的做得题目都忘得差不多了

中序遍历，然后判断是否是升序序列

```java
class Solution {
    List<Integer> list = new ArrayList<>();

    public boolean isValidBST(TreeNode root) {
        inOrder(root);
        for(int i = 1; i < list.size(); i++){
            if(list.get(i-1) >= list.get(i)){
                return false;
            }
        }
        return true;
    }

    public void inOrder(TreeNode root){
        if(root != null){ss
            inOrder(root.left);
            list.add(root.val);
            inOrder(root.right);
        }
    }
}
```

直接判断，不是很懂

```java
class Solution {
    long pre = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        // 访问左子树
        if (!isValidBST(root.left)) {
            return false;
        }
        // 访问当前节点：如果当前节点小于等于中序遍历的前一个节点，说明不满足BST，返回 false；否则继续遍历。
        if (root.val <= pre) {
            return false;
        }
        pre = root.val;
        // 访问右子树
        return isValidBST(root.right);
    }
}
```

## [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        if(root == null){
            return ans;
        }

        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);

        while(!q.isEmpty()){
            int cnt = q.size();
            List<Integer> tmp = new ArrayList<>();
            while(cnt > 0){
                TreeNode cur = q.poll();
                tmp.add(cur.val);

                if(cur.left != null){
                    q.offer(cur.left);
                }
                if(cur.right != null){
                    q.offer(cur.right);
                }
                cnt--;
            }
            ans.add(tmp);
        }
        return ans;
    }
}
```

## [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

老题目了，然而还是记不得了。但现在貌似也比之前理解更加深刻了一下，对于树的递归

- 先中序遍历中找到根，然后根的左边为左子树，右边为右子树
- 然后再递归下去就行了

```java
class Solution {
    
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        TreeNode tree = build(preorder, 0, preorder.length, inorder, 0, inorder.length);
        return tree;
    }

    public TreeNode build(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd){
        if(preStart == preEnd){
            return null;
        }
        TreeNode root = new TreeNode(preorder[preStart]);
        int i = 0;
        for(; i < inEnd; i++){
            if(inorder[i] == preorder[preStart]){
                break;
            }
        }
        int leftNum = i - inStart;
        root.left = build(preorder, preStart+1, preStart+1+leftNum, inorder, inStart, i);
        root.right = build(preorder, preStart+1+leftNum, preEnd, inorder, i+1, inEnd);
        return root;
    }   
}
```

可以改进一下，用`map`存储一下中序的下标，这样就不用每次都遍历了

```java
class Solution {
    Map<Integer, Integer> map = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for(int i = 0; i < inorder.length; i++){
            map.put(inorder[i], i);
        }
        TreeNode tree = build(preorder, 0, preorder.length, inorder, 0, inorder.length);
        return tree;
    }

    public TreeNode build(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd){
        if(preStart == preEnd){
            return null;
        }
        TreeNode root = new TreeNode(preorder[preStart]);
        int i = map.get(preorder[preStart]);
        int leftNum = i - inStart;
        root.left = build(preorder, preStart+1, preStart+1+leftNum, inorder, inStart, i);
        root.right = build(preorder, preStart+1+leftNum, preEnd, inorder, i+1, inEnd);
        return root;
    }   
}
```

## [114. 二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)

好吧，只会最简单的，思路一样，然而还是写不出

```java
class Solution {
    List<TreeNode> list = new ArrayList<>();

    public void flatten(TreeNode root) {
        preOrder(root);
        
        for(int i = 1; i < list.size(); i++){
            TreeNode pre = list.get(i-1);
            TreeNode cur = list.get(i);
            pre.left = null;
            pre.right = cur;
        }
    }

    public void preOrder(TreeNode root){
        if(root == null){
            return;
        }
        list.add(root);
        preOrder(root.left);
        preOrder(root.right);
    }
}
```

## [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

排序然后再构建新链表，就不多说什么了

差点连构建新链表都忘记了

```java
class Solution {
    public ListNode sortList(ListNode head) {
        if(head == null){
            return null;
        }
        List<Integer> list = new ArrayList<>();
        ListNode tmp = head;
        while(tmp != null){
            list.add(tmp.val);
            tmp = tmp.next;
        }
        Collections.sort(list);
        ListNode ans = new ListNode(0);
        ListNode cur = ans;
        for(int i : list){
            ListNode newNode = new ListNode(i);
            cur.next = newNode;
            cur = cur.next;
        }
        return ans.next;
    }
}
```

## [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

dfs模板，大佬太6了，清晰易懂

```java
class Solution {
    int cnt = 0;

    public int numIslands(char[][] grid) {
        for(int i = 0; i < grid.length; i++){
            for(int j = 0; j < grid[0].length; j++){
                if(grid[i][j] == '1'){
                    dfs(grid, i, j);
                    cnt++;
                }
            }
        }
        return cnt;
    }

    public void dfs(char[][] grid, int r, int c){
        
        // 如果坐标 (r, c) 超出了网格范围，直接返回
        if(!inArea(grid, r, c)){
            return;
        }
        
        // 如果这个格子不是岛屿，直接返回
        if(grid[r][c] != '1'){
            return;
        }
        
        // 将格子标记为「已遍历过」
        grid[r][c] = 2;
        
        // 访问上、下、左、右四个相邻结点
        dfs(grid, r-1, c);
        dfs(grid, r+1, c);
        dfs(grid, r, c-1);
        dfs(grid, r, c+1);
    }

    // 判断坐标 (r, c) 是否在网格中
    public boolean inArea(char[][] grid, int r, int c){
        return 0 <= r && r < grid.length && 0 <= c && c < grid[0].length;
    }
}
```

也可以加个偏移量数组`direct`

```java
class Solution {
    int cnt = 0;
    int[][] direct = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    public int numIslands(char[][] grid) {
        for(int i = 0; i < grid.length; i++){
            for(int j = 0; j < grid[0].length; j++){
                if(grid[i][j] == '1'){
                    dfs(grid, i, j);
                    cnt++;
                }
            }
        }
        return cnt;
    }

    public void dfs(char[][] grid, int r, int c){
        if(!inArea(grid, r,c)){
            return;
        }
        if(grid[r][c] != '1'){
            return;
        }
        grid[r][c] = 2;
        for(int[] dir : direct){
            int x = r + dir[0];
            int y = c + dir[1];
            dfs(grid, x, y);
        }
    }

    public boolean inArea(char[][] grid, int r, int c){
        return 0 <= r && r < grid.length && 0 <= c && c < grid[0].length;
    }
}
```

## [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)

**拓扑排序**，也是碰到了第一题关于图的问题了

曾经也还算是蛮熟悉的，但现在，也都忘得差不多了，哎

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        int[] indegrees = new int[numCourses];
        List<List<Integer>> adj = new ArrayList<>();
        Queue<Integer> q = new LinkedList<>();
		
        // 构建邻接表
        for(int i = 0; i < numCourses; i++){
            adj.add(new ArrayList<>());
        }
		
        // 入度表
        for(int[] cp : prerequisites){
            indegrees[cp[0]]++;
            adj.get(cp[1]).add(cp[0]);
        }
		
        // 拓扑排序
        for(int i = 0; i < numCourses; i++){
            if(indegrees[i] == 0){
                q.offer(i);
            }
        }
        while(!q.isEmpty()){
            int tmp = q.poll();
            numCourses--;
            for(int cur : adj.get(tmp)){
                indegrees[cur]--;
                if(indegrees[cur] == 0){
                    q.offer(cur);
                }
            }
        }
        return numCourses == 0;
    }
}
```

## [208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

似懂非懂，好吧，其实就还是不是很十分清楚

```java
class Trie {
    class TrieNode{
        boolean isEnd;
        TrieNode[] next;
        public TrieNode(){
            isEnd = false;
            next = new TrieNode[26];
        }
    }

    TrieNode root;
    public Trie() {
        root = new TrieNode();
    }
    
    public void insert(String word) {
        TrieNode node = root;
        for(char c : word.toCharArray()){
            if(node.next[c-'a'] == null){
                node.next[c-'a'] = new TrieNode();
            }
            node = node.next[c-'a'];
        }
        node.isEnd = true;
    }
    
    public boolean search(String word) {
        TrieNode node = root;
        for(char c : word.toCharArray()){
            node = node.next[c-'a'];
            if(node == null){
                return false;
            }
        }
        return node.isEnd;
    }
    
    public boolean startsWith(String prefix) {
        TrieNode node = root;
        for(char c : prefix.toCharArray()){
            node = node.next[c-'a'];
            if(node == null){
                return false;
            }
        }
        return true;
    }
}
```

## [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

**堆排**

```java
// 83% 2ms
class Solution {
    int ans = 0;

    public int findKthLargest(int[] nums, int k) {
        heapSort(nums, k);
        return ans;
    }

    public void heapSort(int[] arr, int k){
        int n = arr.length-1;
        for(int i = n/2; i >= 0; i--){
            sink(arr, i, n);
        }
        while(n >= 0){
            if(k == 0) break;
            ans = arr[0];
            swap(arr, 0, n--);
            sink(arr, 0, n);
            k--;
        }
    }

    public void sink(int[] a, int k, int n){
        while(2*k+1 <= n){
            int j = 2*k+1;
            if(j < n && a[j] < a[j+1]) j++;
            if(a[k] > a[j]) break;
            swap(a, j, k);
            k = j;
        }
    }

    public void swap(int[] a, int i, int j){
        int tmp = a[i];
        a[i] = a[j];
        a[j] = tmp;
    }
}
```

**快排**

```java
// 98% 1ms
class Solution {
    public int findKthLargest(int[] nums, int k) {
        return quickSort(nums, 0, nums.length-1, k-1);
    }

    public int quickSort(int[] nums, int low, int hight, int k){
        int pivot = randomPartition(nums, low, hight);
        if(pivot == k){
            return nums[k];
        }
        return pivot > k ? quickSort(nums, low, pivot-1, k):quickSort(nums, pivot+1, hight, k);
    }

    public int randomPartition(int[] a, int low, int hight){
        Random random = new Random();
        int i = random.nextInt(hight-low+1)+low;
        swap(a, i, low);
        return partition(a, low, hight);
    }

    // 32% 9ms
    public int partition(int[] a, int low, int hight){
        int pivotKey = a[low];
        int i = low, j = hight+1;
        while(true){
            while(++i < hight && a[i] > pivotKey);
            while(--j > low && a[j] < pivotKey);
            if(i >= j) break;
            swap(a, i, j);
        }
        a[low] = a[j];
        a[j] = pivotKey;
        return j;
    }

    public void swap(int[] a, int i, int j) {
        int temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }
}
```

## [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

也是老题目了，记得当初也还不怎么会，到现在也好久了

然而，现在也不是很清楚，主要是递归函数的作用，哎

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null){
            return null;
        }
        if(root == p || root == q){
            return root;
        }

        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        if(left == null){
            return right;
        }
        if(right == null){
            return left;
        }
        if(left != null && right != null){
            return root;
        }
        return null;
    }
}
```

## [238. 除自身以外数组的乘积](https://leetcode-cn.com/problems/product-of-array-except-self/)

好吧，直接暴力，然后，然后就**超时**了

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] ans = new int[nums.length];
        for(int i = 0; i < nums.length; i++){
            int sum = 1;
            for(int j = 0; j < nums.length; j++){
                if(i != j){
                    sum *= nums[j];
                }
            }
            ans[i] = sum;
        }
        return ans;
    }
}
```

 **乘积 = 当前数左边的乘积 * 当前数右边的乘积**。想不到

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] L = new int[n];
        int[] R = new int[n];
        int[] ans = new int[n];

        L[0] = 1;
        for(int i = 1; i < n; i++){
            L[i] = nums[i-1] * L[i-1];
        }

        R[n-1] = 1;
        for(int i = n-2; i >= 0; i--){
            R[i] = nums[i+1]*R[i+1];
        }

        for(int i = 0; i < n; i++){
            ans[i] = L[i]*R[i];
        }

        return ans;
    }
}
```

## [240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)

首先当然是直接查找啦

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        for(int i = 0; i < matrix.length; i++){
            for(int j = 0; j < matrix[0].length; j++){
                if(matrix[i][j] == target){
                    return true;
                }
            }
        }
        return false;
    }
}
```

每一行用二分查找

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        for(int[] row : matrix){
            int ans = search(row, target);
            if(ans != -1){
                return true;
            }
        }
        return false;
    }

    public int search(int[] nums, int target){
        int left = 0, right = nums.length-1;
        while(left <= right){
            int mid = left + (right-left)/2;
            if(nums[mid] == target){
                return mid;
            }else if(target > nums[mid]){
                left = mid+1;
            }else if(target < nums[mid]){
                right = mid-1;
            }
        }
        return -1;
    }
}
```

从右上角开始搜索，哎，想不到

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int i = 0;
        int j = matrix[0].length-1;
        while(i < matrix.length && j >= 0){
            if(target == matrix[i][j]){
                return true;
            }else if(target > matrix[i][j]){
                i++;
            }else if(target < matrix[i][j]){
                j--;
            }
        }
        return false;
    }
}
```

## [287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

额，第一个想到的就是用Map

```java
class Solution {
    public int findDuplicate(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int num : nums){
            map.put(num, map.getOrDefault(num, 0)+1);
        }
        for(Map.Entry<Integer, Integer> entry : map.entrySet()){
            if(entry.getValue() != 1){
                return entry.getKey();
            }
        }
        return -1;
    }
}
```

## [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

直接回溯大爆搜，毫无疑问，超时了

```java
class Solution {
    int ans = Integer.MAX_VALUE;

    public int coinChange(int[] coins, int amount) {
        if(coins.length == 0){
            return -1;
        }
        dfs(coins, amount, 0);
        if(ans == Integer.MAX_VALUE){
            return -1;
        }
        return ans;
    }

    public void dfs(int[] coins, int amount, int cnt){
        if(amount < 0){
            return;
        }
        if(amount == 0){
            ans = Math.min(ans, cnt);
            return;
        }
        for(int i = 0; i < coins.length; i++){
            backtrack(coins, amount-coins[i], cnt+1);
        }
    }
}
```

记忆化搜索，不懂一大半，哎

```java
class Solution {
    int[] memo;

    public int coinChange(int[] coins, int amount) {
        if(coins.length == 0){
            return -1;
        }
        memo = new int[amount];
        return dfs(coins, amount);
    }

    public int dfs(int[] coins, int amount){
        if(amount < 0){
            return -1;
        }
        if(amount == 0){
            return 0;
        }
        if(memo[amount-1] != 0){
            return memo[amount-1];
        }
        int min = Integer.MAX_VALUE;
        for(int i = 0; i < coins.length; i++){
            int ans = dfs(coins, amount - coins[i]);
            if(ans >= 0 && ans < min){
                min = ans + 1;
            }
        }
        memo[amount-1] = (min == Integer.MAX_VALUE ? -1 : min);
        return memo[amount-1];
    }
}
```

## [347. 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer,Integer> map = new HashMap<>();
        for(int num: nums){
            map.put(num, map.getOrDefault(num, 0)+1);
        }
        Queue<Integer> q = new PriorityQueue<>((a,b) -> map.get(a)-map.get(b));
        for(int key: map.keySet()){
            if(q.size() < k){
                q.offer(key);
            }else if(map.get(key) > map.get(q.peek())){
                q.poll();
                q.offer(key);
            }
        }
        int[] ans = new int[q.size()];
        int idx = 0;
        for(int i: q){
            ans[idx++] = i;
        }
        return ans;
    }
}
```

## [437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)

树的遍历，`dfs1`遍历每个结点，`dfs2`搜索以其为根的所有往下的路径，同时累加路径总和为`targetSum`的所有路径

```java
class Solution {
    int cnt = 0;
    int sum = 0;
    public int pathSum(TreeNode root, int targetSum) {
        sum = targetSum;
        dfs1(root);
        return cnt;
    }

    public void dfs1(TreeNode root){
        if(root == null){
            return;
        }
        dfs2(root, root.val);
        dfs1(root.left);
        dfs1(root.right);
    }

    public void dfs2(TreeNode root, int val){
        if(val == sum){
            cnt++;
        }
        if(root.left != null){
            dfs2(root.left, val+root.left.val);
        }
        if(root.right != null){
            dfs2(root.right, val+root.right.val);
        }
    }
}
```

## [494. 目标和](https://leetcode-cn.com/problems/target-sum/)

回溯，然而具体还是不知道怎么写

```java
class Solution {
    int cnt = 0;

    public int findTargetSumWays(int[] nums, int target) {
        backtrack(nums, target, 0, 0);
        return cnt;
    }

    public void backtrack(int[] nums, int target, int idx, int sum){
        if(idx == nums.length){
            if(sum == target){
                cnt++;
            }
            return;
        }
        backtrack(nums, target, idx+1, sum+nums[idx]);
        backtrack(nums, target, idx+1, sum-nums[idx]);
    }

}
```
## [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

第一次没看懂题目。。。

然后发现和中序遍历有点类似，但是没啥思路

然后看题解才发现是反序的中序遍历，正序是从小到大，反序就是从大到小，累加即可

```java
class Solution {
    int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if(root != null){
            convertBST(root.right);
            sum += root.val;
            root.val = sum;
            convertBST(root.left);
        }
        return root;
    }
}
```

## [581. 最短无序连续子数组](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/)

先拷贝一份排序，然后再找到数组两边不相同的地方即可

```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int[] tmp = Arrays.copyOf(nums, nums.length);
        Arrays.sort(tmp);

        if(Arrays.equals(nums, tmp)){
            return 0;
        }

        int i = 0;
        while(i < nums.length){
            if(nums[i] != tmp[i]){
                break;
            }
            i++;
        }
        int j = nums.length-1;
        while(j >= 0){
            if(nums[j] != tmp[j]){
                break;
            }
            j--;
        }
        return j-i+1;
    }
}
```

## [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

卧槽，看了好几遍题目都不知道什么意思，最后面看题解懂题目了

然后自己写出来了，暴力，两重for循坏即可。

期间有一个bug，没有考虑到中间的一种情况,中间有一个数，后面的都没有比它大的，因此没有赋值，直接跳到下一重循环了，还好用idea debug出来了

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] ans = new int[n];
        int idx = 0;
        for(int i = 0; i < n; i++){
            int flag = 0;
            for(int j = i+1; j < n; j++){
                if(temperatures[j] > temperatures[i]){
                    ans[idx++] = j-i;
                    flag = 1;
                    break;
                }
            }
            if(flag != 1){
                ans[idx++] = 0;
            }
        }
        return ans;
    }
}
```

看了下人家的代码，好吧，其实我是多此一举了，直接用下标赋值就行，就避免了前面的那种情况

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] ans = new int[n];
        for(int i = 0; i < n; i++){
            for(int j = i+1; j < n; j++){
                if(temperatures[j] > temperatures[i]){
                    ans[i] = j-i;;
                    break;
                }
            }
        }
        return ans;
    }
}
```

**单调栈**

遍历整个数组，如果栈不空，且当前数字大于栈顶元素，取出栈顶元素，由于当前数字大于栈顶元素的数字，而且一定是第一个大于栈顶元素的数，直接求出下标差就是二者的距离。

继续看新的栈顶元素，直到当前数字小于等于栈顶元素停止，然后将数字入栈，这样就可以一直保持递减栈，且每个数字和第一个大于它的数的距离也可以算出来。

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] ans = new int[n];

        Stack<Integer> s = new Stack<>();
        for(int i = 0; i < n; i++){
            while(!s.isEmpty() && temperatures[i] > temperatures[s.peek()]){
                int pre = s.pop();
                ans[pre] = i-pre;
            }
            s.push(i);
        }

        return ans;
    }
}
```

