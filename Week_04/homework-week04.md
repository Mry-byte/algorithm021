#### [860. 柠檬水找零](https://leetcode-cn.com/problems/lemonade-change/)

思路：贪心策咯

找零尽量使用大面值的零钱

```plain
public boolean lemonadeChange(int[] bills) {
        int f5 = 0, f10 = 0;
        for (int i = 0; i < bills.length; i++) {
            int money = bills[i];
            if (money == 5) {
                f5++;
            } else if (money == 10) {
                if (f5 == 0) return false;
                f5--;
                f10++;
            } else {
                if (f5 > 0 && f10 > 0) {
                    f5--;
                    f10--;
                } else if (f5 >= 3) {
                    f5 -= 3;
                } else {
                    return false;
                }
            }
        }
        return true;
    }
```
#### [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

思路：贪心算法，股票赚钱就是低买高卖赚差价

```plain
class Solution {
    public int maxProfit(int[] prices) {
        int profit = 0;
        for (int i = 1; i < prices.length; i++) {
            int diff = prices[i] - prices[i - 1];
            if (diff > 0)
                profit += diff;
        }
        return profit;
    }
}
```
#### [455. 分发饼干](https://leetcode-cn.com/problems/assign-cookies/)

思路：贪心算法，尽量用大尺寸的饼干先满足胃口大的孩子，充分利用资源

```plain
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int index = s.length - 1;// 饼干数组下标
        int count = 0;
        for (int i = g.length - 1; i >= 0; i--) {
            if (index >= 0 && g[i] <= s[index]) {
                count++;
                index--;
            }
        }
        return count;
    }
}
```
#### [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

![图片](https://uploader.shimo.im/f/WiBi0PdSUSLMa3NT.png!thumbnail?fileGuid=j9qhrPWt3QKhWkr9)

思路：

深度优先搜索DFS

主循环：循环遍历每一个格子，判断grid[i][j]== '1'时说明此方格代表一个岛屿，count++，然后以此方格为基点做深度优先搜索将相连的方格 grid[i][j]== '1'的都删除，表示他们代表一个岛屿。

dfs方法：按左下右上顺序搜索相连的方格，并检查是否满足grid[i][j]== '1'，满足则grid[i][j] = '0'，表示删除此方格，避免重复搜索；不满足则结束本次搜索；

终结条件：

* 方格超出边界
* 方格grid[i][j] = '0'
```plain
// dfs 深度优先搜索
public int numIslands(char[][] grid) {
    if (grid.length == 0 && grid[0].length == 0)
        return 0;
    int count = 0;// 岛屿数量
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] == '1') {
                count++;
                dfs5(grid, i, j);
            }
        }
    }
    return count;
}
private void dfs5(char[][] grid, int i, int j) {
    // 终结条件
    if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] == '0')
        return;
    grid[i][j] = '0';
    dfs5(grid, i - 1, j);// 左
    dfs5(grid, i, j + 1); // 下
    dfs5(grid, i + 1, j);// 右
    dfs5(grid, i, j - 1);// 上
}
```
广度优先搜索
```plain
// bfs 广度优先搜索
public int numIslands(char[][] grid) {
    if (grid.length == 0 && grid[0].length == 0)
        return 0;
    int count = 0;// 岛屿数量
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] == '1') {
                count++;
                dfs6(grid, i, j);
            }
        }
    }
    return count;
}
private void dfs6(char[][] grid, int i, int j) {
    Queue<int[]> queue = new LinkedList<>();
    queue.add(new int[]{i, j});
    while (!queue.isEmpty()) {
        int[] cur = queue.remove();
        i = cur[0];
        j = cur[1];
        // 判断边界
        if (i >= 0 && i < grid.length && j >= 0 && j < grid[0].length && grid[i][j] == '1') {
            grid[i][j] = '0';
            queue.add(new int[]{i - 1, j});
            queue.add(new int[]{i + 1, j});
            queue.add(new int[]{i, j - 1});
            queue.add(new int[]{i, j + 1});
        }
    }
}
```
#### [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

```plain
public int search(int[] nums, int target) {
int left = 0, right = nums.length - 1, mid;
while (left <= right) {
mid = left + (right - left) / 2;
if (target == nums[mid]) {
return mid;
}
// 前半部分有序
if (nums[left] <= nums[mid]) {
if (target >= nums[left] && target < nums[mid]) {
right = mid - 1;
} else {
left = mid + 1;
}
} else {
if (target > nums[mid] && target <= nums[right]) {
left = mid + 1;
} else {
right = mid - 1;
}
}
}
return -1;
}
```
#### 
