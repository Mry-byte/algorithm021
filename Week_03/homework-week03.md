#### [剑指 Offer 68 - II. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

思路：递归

三种情况

```plain
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
// 递归结束
/**
* 1、root == null
* 2、root == p
* 3、root == q
*/
if (root == null || root == p || root == q) {
return root;
}
// 根结点的左右子树中查找
TreeNode left = lowestCommonAncestor(root.left, p, q);
TreeNode right = lowestCommonAncestor(root.right, p, q);
/**
* 1、如果p\q都存在且一个为左结点，一个为右结点，那么根结点root就是最近公共祖先
* 2、如果p\q都存在且都是左结点，那么在根结点root的左子树中查找，返回的结点left即为最近公共祖先
* 3、如果p\q都存在且都是右结点，那么在根结点root的右子树中查找，返回的结点right即为最近公共祖先
* 4、如果p\q在左右子树中都查找不到，则不存在最近公共祖先
*/
return left == null ? right : right == null ? left : root;
}
```
#### [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

#### [50. Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)

```plain
// 分治法
// n个x相乘,分两种情况计算
// 如果 n 是奇数个则计算 ((n/2)*x)*((n/2)*x)*x
// 如果 n 是偶数个则计算 (n/2)*x)*((n/2)*x)
public double myPow(double x, int n) {
int power = n;
// 特殊情况
if (n < 0) {
x = 1 / x;
power = -n;
}
return power(x, power);
}
private double power(double x, int power) {
//1.国际惯例，递归结束条件
if (power == 0) {
return 1.0;
}
//2.递归体
double result = power(x, power / 2);
//3.返回值
/*if (power % 2 == 0) {
return result * result;
} else {
return result * result * x;
}*/
// 简化
return power % 2 == 0 ? result * result : result * result * x;
}
```
#### [78. 子集](https://leetcode-cn.com/problems/subsets/)

![图片](https://uploader.shimo.im/f/TBfnLQZW7k9BEKvU.png!thumbnail?fileGuid=VCQwtJjycJkyWHcD)

```plain
public static List<List<Integer>> subsets(int[] nums) {
List<List<Integer>> res = new ArrayList<>();
if (nums == null) {
return res;
}
dfs(0, res, nums, new ArrayList<Integer>());
return res;
}
private static void dfs(int i, List<List<Integer>> res, int[] nums, List<Integer> list) {
if (i == nums.length) {
res.add(new ArrayList<>(list));
return;
}
// 不选
dfs(i + 1, res, nums, list);
// 选
list.add(nums[i]);
dfs(i + 1, res, nums, list);
// 重置
list.remove(list.size() - 1);
}
```
#### [77. 组合](https://leetcode-cn.com/problems/combinations/)

```plain
public List<List<Integer>> combine(int n, int k) {
List<List<Integer>> res = new ArrayList<>();
if (n <= 0 || k > n) {
return res;
}
Deque<Integer> path = new ArrayDeque<>();
dfs3(n, k, 1, path, res);
return res;
}
private void dfs3(int n, int k, int begin, Deque<Integer> path, List<List<Integer>> res) {
// 结束条件，path 大小等于k
if (k == path.size()) {
res.add(new ArrayList<>(path));
return;
}
//
for (int i = begin; i <= n - (k - path.size()) + 1; i++) {
//选择一个数字
path.addLast(i);
//进入下一层 , 起点 + 1 ， 保证选过的数字不会被重复选择
dfs3(n, k, i + 1, path, res);
//回溯到上一层需要清除当前层已选的路径
path.removeLast();
}
}
```
#### [46. 全排列](https://leetcode-cn.com/problems/permutations/)

```plain
public List<List<Integer>> permute(int[] nums) {
List<List<Integer>> res = new ArrayList<>();
if (nums == null || nums.length == 0) {
return res;
}
Deque<Integer> path = new ArrayDeque<>();
boolean[] used = new boolean[nums.length];
dfs4(nums, 0, path, used, res);
return res;
}
private void dfs4(int[] nums, int depth, Deque<Integer> path, boolean[] used, List<List<Integer>> res) {
if (depth == nums.length) {
res.add(new ArrayList<>(path));
return;
}
for (int i = 0; i < nums.length; i++) {
if (!used[i]) {
path.addLast(nums[i]);
used[i] = true;
// 进入下层
dfs4(nums, depth + 1, path, used, res);
// 回溯上层前，清除当先层选择的数字
used[i] = false;
path.removeLast();
}
}
}
```
