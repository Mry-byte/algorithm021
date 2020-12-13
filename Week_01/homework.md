### 第一周

#### 简单：

* 用 add first 或 add last 这套新的 API 改写 Deque 的代码
* 分析 Queue 和 Priority Queue 的源码
* [删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)（Facebook、字节跳动、微软在半年内面试中考过）
* [旋转数组](https://leetcode-cn.com/problems/rotate-array/)（微软、亚马逊、PayPal 在半年内面试中考过）
* [合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)（亚马逊、字节跳动在半年内面试常考）
* [合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)（Facebook 在半年内面试常考）
* [两数之和](https://leetcode-cn.com/problems/two-sum/)（亚马逊、字节跳动、谷歌、Facebook、苹果、微软在半年内面试中高频常考）
* [移动零](https://leetcode-cn.com/problems/move-zeroes/)（Facebook、亚马逊、苹果在半年内面试中考过）
* [加一](https://leetcode-cn.com/problems/plus-one/)（谷歌、字节跳动、Facebook 在半年内面试中考过）
#### 中等：

* [设计循环双端队列](https://leetcode.com/problems/design-circular-deque)（Facebook 在 1 年内面试中考过）
#### 困难：

* [接雨水](https://leetcode.com/problems/trapping-rain-water/)（亚马逊、字节跳动、高盛集团、Facebook 在半年内面试常考）

---


1、用 add first 或 add last 这套新的 API 改写 Deque 的代码

2、分析 Queue 和 Priority Queue 的源码

3、[删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)（Facebook、字节跳动、微软在半年内面试中考过）

双指针：

关键点

1、使用 i 和 j 两个指针遍历数组 ，j 在 i 前寻找不重复元素

2、发现不重复元素往前移动至第一个重复元素位置

3、最终数组长度为指针 i 走过的路程长度

```plain
public int removeDuplicates(int[] nums) {
    if (nums == null || nums.length <= 0) {
        return 0;
    }
    int i = 0;
    int j = 1;
    while (j < nums.length) {
        if (nums[i] != nums[j]) {
            nums[i + 1] = nums[j];
            i++;
        }
        j++;
    }
    return i + 1;
}
```
4、[旋转数组](https://leetcode-cn.com/problems/rotate-array/)（微软、亚马逊、PayPal 在半年内面试中考过）
5、[合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)（亚马逊、字节跳动在半年内面试常考）

解题思路：

递归找到两个链表中相对较小的结点

递归三要素：

终止条件：l1 、l2 结点为null

递归体： 找到相对较小的一个结点

返回值：将找到的结点返回

```plain
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    if (l1 == null)
        return l2;
    if (l2 == null)
        return l1;
    ListNode head = new ListNode();
    if (l1.val < l2.val) {
        head.val = l1.val;
        head.next = mergeTwoLists(l1.next, l2);
    } else {
        head.val = l2.val;
        head.next = mergeTwoLists(l2.next, l1);
    }
    return head;
}
```
6、[合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)（Facebook 在半年内面试常考）
7、[两数之和](https://leetcode-cn.com/problems/two-sum/)（亚马逊、字节跳动、谷歌、Facebook、苹果、微软在半年内面试中高频常考）

暴力法：枚举所有两个数相加的和

```plain
public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length - 1; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if (target == (nums[i] + nums[j])) {
                return new int[]{i, j};
            }
        }
    }
    return new int[0];
}
```
哈希映射法：
```plain
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        if (map.containsKey(target - nums[i])) {
            return new int[]{map.get(target - nums[i]), i};
        }
        map.put(nums[i], i);
    }
    return new int[0];
}
```
8、[移动零](https://leetcode-cn.com/problems/move-zeroes/)（Facebook、亚马逊、苹果在半年内面试中考过）
解题思路：

第一层循环：将所有非零元素移动到数组开头

第二层循环：将剩余元素皆赋值为0

```plain
public void moveZeroes(int[] nums) {
    int p = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            nums[p++] = nums[i];
        }
    }
    for (int j = p; j < nums.length; j++) {
        nums[j] = 0;
    }
}
```
9、[加一](https://leetcode-cn.com/problems/plus-one/)（谷歌、字节跳动、Facebook 在半年内面试中考过）
10、[设计循环双端队列](https://leetcode.com/problems/design-circular-deque)（Facebook 在 1 年内面试中考过）

11、[接雨水](https://leetcode.com/problems/trapping-rain-water/)（亚马逊、字节跳动、高盛集团、Facebook 在半年内面试常考）

### 第二周

#### 简单：

* 写一个关于 HashMap 的小总结。
说明：对于不熟悉 Java 语言的同学，此项作业可选做。
* [有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/description/)（亚马逊、Facebook、谷歌在半年内面试中考过）
* [两数之和](https://leetcode-cn.com/problems/two-sum/description/)（近半年内，亚马逊考查此题达到 216 次、字节跳动 147 次、谷歌 104 次，Facebook、苹果、微软、腾讯也在近半年内面试常考）
* [N 叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/description/)（亚马逊在半年内面试中考过）
* HeapSort ：自学[https://www.geeksforgeeks.org/heap-sort/](https://www.geeksforgeeks.org/heap-sort/)
#### 中等：

* [字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)（亚马逊在半年内面试中常考）
* [二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)（亚马逊、字节跳动、微软在半年内面试中考过）
* [二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)（字节跳动、谷歌、腾讯在半年内面试中考过）
* [N 叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)（亚马逊在半年内面试中考过）
* [丑数](https://leetcode-cn.com/problems/chou-shu-lcof/)（字节跳动在半年内面试中考过）
* [前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)（亚马逊在半年内面试中常考）

---


1、写一个关于 HashMap 的小总结。

[https://shimo.im/docs/RXVpdYJTGwQRjtkQ/](https://shimo.im/docs/RXVpdYJTGwQRjtkQ/)《java8 HashMap 源码分析》，可复制链接后用石墨文档 App 或小程序打开

2、有效的字母异位词

给定两个字符串*s*和*t*，编写一个函数来判断*t*是否是*s*的字母异位词。

异位词：长度一样，包含的字母都一样，每个字符出现的频率也一样，只是顺序不同而已

思路：

（1）、将字符串转换为字符数组

（2）、对字符数组进行排序

（3）、比较排序后的字符数组，如果相等就是异位词

```plain
public boolean isAnagram1(String s, String t) {
    if (s.isEmpty() && t.isEmpty()) {
        return true;
    }
    if (s.length() != t.length()) {
        return false;
    }
    char[] sChars = s.toCharArray();
    char[] tChars = t.toCharArray();
    Arrays.sort(sChars);
    Arrays.sort(tChars);
    return Arrays.equals(sChars, tChars);
}
```
3、两数之和
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

来源：力扣（LeetCode）

链接：[https://leetcode-cn.com/problems/two-sum](https://leetcode-cn.com/problems/two-sum)

著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

思路：a+b = target , 及 a = target - b ,也就是说只要在整数数组中找到于 target - nums[i] 相等数的下标就可以了，使用map集合，用数组中的数作为key，相对应的下标作为value，循环遍历寻找是否存在key为target - nums[i]的元素

```plain
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        if (map.containsKey(target - nums[i])) {
            return new int[]{map.get(target - nums[i]), i};
        }
        map.put(nums[i], i);
    }
    return new int[0];
}
```
4、N叉树的前序遍历
前序：根-子

递归：

```plain
public List<Integer> preorder(Node root) {
    List<Integer> res = new ArrayList<>();
    order(root, res);
    return res;
}
private void order(Node root, List<Integer> res) {
    if (root == null) {
        return;
    }
    res.add(root.val);
    for (Node node : root.children) {
        order1(node, res);
    }
}
```
5、字母异位词分组
思路：排序+哈希

1）、将字符串转化为字符数组并排序

2）、将异位词作为key值，对比转换后的每一个字符串是否是异位词，是的存相同key值下的集合中，不同存新创建的集合中

```plain
public List<List<String>> groupAnagrams(String[] strs) {
    if (strs.length == 0) {
        return new ArrayList<>();
    }
    Map<String, List<String>> map = new HashMap<>();
    for (String s : strs) {
        char[] chars = s.toCharArray();
        Arrays.sort(chars);
        String key = String.valueOf(chars);
        if (!map.containsKey(key)) {
            map.put(key, new ArrayList<>());
        }
        map.get(key).add(s);
    }
    return new ArrayList<>(map.values());
}
```
6、二叉树的中序遍历
递归思想：中序，左-根-右

时间复杂度：O(N)

```plain
public List<Integer> inorderTraversal1(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    inorder(root, res);
    return res;
}
private void inorder(TreeNode root, List<Integer> res) {
    if (root == null) {
        return;
    }
    inorder2(root.left, res);
    res.add(root.val);
    inorder2(root.right, res);
}
```
7、二叉树的前序遍历
前序：根-左-右

递归思想：递归三要素：结束条件、递归体、返回值

```plain
public List<Integer> inorderTraversal1(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    inorder(root, res);
    return res;
}
private void inorder(TreeNode root, List<Integer> res) {
    if (root == null) {
        return;
    }
    res.add(root.val);
    inorder2(root.left, res);
    inorder2(root.right, res);
}
```
8、
