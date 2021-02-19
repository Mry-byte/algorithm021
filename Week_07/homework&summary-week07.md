7. 字典树和并查集
8. 字典树的数据结构

字典树，即Trie树，又称单词查找或键树，是一种树形结构。典型应用是用于统计和排序大量的字符串（但不仅限于字符串），所以经常被搜索引擎系统用于文本词频统计。

优点：最大限度地减少无谓的字符串比较，查询效率比哈希表高。

1. 字典树的核心思想

空间换时间，利用字符串的公共前缀来降低查询时间的开销以达到提高效率的目的。

1. 字典树的基本性质

1、结点本身不存完整单词；

2、从根结点到某结点，路径上经过的字符连接起来，为该结点对应的字符串；

3、每个结点的所有子结点路径代表的字符都不相同。

字典树代码模板：

```java
class Trie {
    private boolean isEnd;
    private Trie[] next;
    /** Initialize your data structure here. */
    public Trie() {
        isEnd = false;
        next = new Trie[26];
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        if (word == null || word.length() == 0) return;
        Trie curr = this;
        char[] words = word.toCharArray();
        for (int i = 0;i < words.length;i++) {
            int n = words[i] - 'a';
            if (curr.next[n] == null) curr.next[n] = new Trie();
            curr = curr.next[n];
        }
        curr.isEnd = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        Trie node = searchPrefix(word);
        return node != null && node.isEnd;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        Trie node = searchPrefix(prefix);
        return node != null;
    }
    private Trie searchPrefix(String word) {
        Trie node = this;
        char[] words = word.toCharArray();
        for (int i = 0;i < words.length;i++) {
            node = node.next[words[i] - 'a'];
            if (node == null) return null;
        }
        return node;
    }
}
```
[208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)
```java
class Trie {
    private boolean isEnd;
    private Trie[] next;
    /** Initialize your data structure here. */
    public Trie() {
        isEnd = false;
        next = new Trie[26];
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        if(word == null || word.length() == 0) return;
        Trie curr = this;
        char[] words = word.toCharArray();
        for(int i = 0; i < words.length; i++){
            int n = words[i] - 'a';
            if(curr.next[n] == null) curr.next[n] = new Trie();
            curr = curr.next[n];
        }
        curr.isEnd = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        Trie node = searchPrefix(word);
        return node != null && node.isEnd;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        Trie node = searchPrefix(prefix);
        return node != null;
    }
    private Trie searchPrefix(String word){
        Trie node = this;
        char[] words = word.toCharArray();
        for(int i = 0; i < words.length; i++) {
            node = node.next[words[i] - 'a'];
            if(node == null) return null;
        }
        return node;
    }
}
/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```
[212. 单词搜索 II](https://leetcode-cn.com/problems/word-search-ii/)
经典题型，使用字典树+dfs，逻辑思维简单易懂

```java
class Solution {
    private static final int[] dx = new int[]{0,0,-1,1};
    private static final int[] dy = new int[]{-1,1,0,0};
    public List<String> findWords(char[][] board, String[] words) {
        if(words.length == 0) return new ArrayList<>();
        // 构建字典树
         Trie root = new Trie();
        for(String word : words){
            root.insert(word);
        }
        // 遍历board + dfs递归 向四周扩撒构建字符串，并在字典树中查找是否存在此字符串
        Set<String> res = new HashSet<>();
        int m = board.length,n = board[0].length;
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                dfs(board,root,i,j,"",res);
            }
        }
        return new ArrayList<String>(res);
    }
    public void dfs(char[][] board, Trie root, int i, int j, String currStr, Set res){
        // 递归结束条件,边界处理以及访问过的字符不能再次访问
        if(i < 0 || j < 0 || i == board.length || j == board[0].length || board[i][j] == '@') return;
        // 递归结束条件 2
        char currChar = board[i][j];
        if(root.next[currChar - 'a'] == null) return;
        // 处理当前层
        char temp = currChar;
        board[i][j] = '@';
        // 构建字符串
        currStr += currChar;
        root = root.next[currChar - 'a'];
        if(root.isEnd){
            res.add(currStr);
        }
        // 向上下左右递归下探
        for(int k = 0; k < dx.length; k++){
            dfs(board,root,i + dx[k], j + dy[k],currStr,res);
        }
        // 将字符串置为空字符串
        currStr = "";
        // 恢复当前层字符
        board[i][j] = temp;
    }
    class Trie {
    private boolean isEnd;
    private Trie[] next;
    /** Initialize your data structure here. */
    public Trie() {
        isEnd = false;
        next = new Trie[26];
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        if(word == null || word.length() == 0) return;
        Trie curr = this;
        char[] words = word.toCharArray();
        for(int i = 0; i < words.length; i++){
            int n = words[i] - 'a';
            if(curr.next[n] == null) curr.next[n] = new Trie();
            curr = curr.next[n];
        }
        curr.isEnd = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        Trie node = searchPrefix(word);
        return node != null && node.isEnd;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        Trie node = searchPrefix(prefix);
        return node != null;
    }
    private Trie searchPrefix(String word){
        Trie node = this;
        char[] words = word.toCharArray();
        for(int i = 0; i < words.length; i++) {
            node = node.next[words[i] - 'a'];
            if(node == null) return null;
        }
        return node;
    }
}
}
```
1. 并查集数据结构：跟树有些类似，只不过和树是相反的。在树这个数据结构里面，每个结点会记录它的子结点。在并查集里，每个结点会记录它的父节点；

![图片](https://uploader.shimo.im/f/qg8SH3yyfOwN2Lmx.png!thumbnail?fileGuid=6KwxkCvh9X9JXDjd)

1. 并查集适用场景：组团、配对问题
2. 基本操作
    * makeSet(s)：建立一个新的并查集，其中包含s个单元素集合。
    * unionSet(x,y)：把元素x和元素y所在的集合合并，要求x和y所在的集合不相交，如果相交则不合并。
    * find(x)：找到元素x所在的集合的代表，该操作也可以用于判断两个元素是否位于同一个集合，只要将他们各自的代表比较一下就可以了。

[参考链接](https://zhuanlan.zhihu.com/p/93647900/)

代码模板：

```java
class UnionFind {
    private int count = 0;
    private int[] parent;
    public UnionFind(int n) {
        count = n;
        parent = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }
    public int find(int p) {
        while (p != parent[p]) {
            parent[p] = parent[parent[p]];
            p = parent[p];
        }
        return p;
    }
    public void union(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        if (rootP == rootQ) return;
        parent[rootP] = rootQ;
        count--;
    }
}
```
[547. 省份数量](https://leetcode-cn.com/problems/number-of-provinces/)
思路：DFS 深度优先搜索

```java
class Solution {
    public int findCircleNum(int[][] isConnected) {
        if(isConnected.length == 0) return 0;
        boolean[] statistics = new boolean[isConnected.length];
        int res = 0;
        for(int i = 0; i < isConnected.length; i++){
            if(!statistics[i]){
                dfs(isConnected,statistics,i);
                res++;
            }
        }
        return res;
    }
    public void dfs(int[][] isConnected,boolean[] statistics, int i){
        for(int j = 0; j < isConnected.length; j++){
            if(isConnected[i][j] == 1 && !statistics[j]){
                statistics[j] = true;
                dfs(isConnected,statistics,j);
            }
        }
    }
}
```
思路：BFS 广度优先搜索
```java
class Solution {
    public int findCircleNum(int[][] isConnected) {
        if(isConnected.length == 0) return 0;
        boolean[] statistics = new boolean[isConnected.length];
        int res = 0;
        Queue<Integer> queue = new LinkedList<>();
        for(int i = 0; i < isConnected.length; i++){
            if(!statistics[i]){
                queue.offer(i);
                res++;
                while(!queue.isEmpty()){
                    int m = queue.poll();
                    for(int n = 0; n < isConnected.length; n++){
                        if(isConnected[m][n] == 1 && !statistics[n]){
                            queue.offer(n);
                            statistics[n] = true;
                        }
                    }
                }
            }
        }
        return res;
    }
}
```
思路：并查集
```java
class Solution {
    public int findCircleNum(int[][] isConnected) {
        if(isConnected.length == 0) return 0;
        // 初始化并查集
        UnionFind union = new UnionFind(isConnected.length);
        // 遍历isConnected中每个关联关系，有相连的城市合并为相同的省份
        for(int i = 0; i < isConnected.length; i++){
            for(int j = 0; j < isConnected.length; j++){
                if(isConnected[i][j] == 1){
                    union.union(i,j);
                }
            }
        }
        return union.count;
    }
    class UnionFind {
        private int count = 0;
        private int[] parent;
        public UnionFind(int n) {
            count = n;
            parent = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }
        public int find(int p) {
            while (p != parent[p]) {
                parent[p] = parent[parent[p]];
                p = parent[p];
            }
            return p;
        }
        public void union(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            if (rootP == rootQ) return;
            parent[rootP] = rootQ;
            count--;
        }
    }
}
```
[200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)
思路：DFS 深度优先搜索算法

```java
class Solution {
    private static int[] dx = new int[]{0,0,-1,1};
    private static int[] dy = new int[]{-1,1,0,0};
    public int numIslands(char[][] grid) {
        if (grid.length == 0) return 0;
        int count = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == '1') {
                    count++;
                    dfs(grid, i, j);
                }
            }
        }
        return count;
    }
    public void dfs(char[][] grid, int i, int j){
        if (i < 0 || j < 0 || i >= grid.length || j >= grid[0].length || grid[i][j] == '0') return;
        grid[i][j] = '0';
        for(int k = 0; k < dx.length; k++){
            dfs(grid,i+dx[k],j+dy[k]);
        }
    }
}
```
思路：BFS 广度优先搜索
```java
class Solution {
    public int numIslands(char[][] grid) {
        if (grid.length == 0) return 0;
        int count = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == '1') {
                    count++;
                    bfs(grid, i, j);
                }
            }
        }
        return count;
    }
    public void bfs(char[][] grid, int i,int j){
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[]{i,j});
        while(!queue.isEmpty()){
            int[] curr = queue.poll();
            i = curr[0];
            j = curr[1];
            if(i >= 0 && i < grid.length && j >= 0 && j < grid[0].length && grid[i][j] == '1'){
                grid[i][j] = '0';
                queue.offer(new int[]{i,j-1});
                queue.offer(new int[]{i,j+1});
                queue.offer(new int[]{i-1,j});
                queue.offer(new int[]{i+1,j});
            }
        }
    }
}
```
思路：并查集
```java
class Solution {
    private int m,n;
    public int numIslands(char[][] grid) {
        if (grid.length == 0) return 0;
        m = grid.length;
        n = grid[0].length;
        UnionFind union = new UnionFind(m*n);
        int spaces = 0;// 空地的数量
        int[][] direction = new int[][]{{1,0},{0,1}};
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '0') {
                    spaces++;
                }else{
                    for(int[] dire : direction){
                        int newx = i + dire[0];
                        int newy = j + dire[1];
                        // 判断边界
                        if(newx < m && newy < n && grid[newx][newy] == '1'){
                            // 合并陆地
                            union.union(getIndex(i,j),getIndex(newx,newy));
                        }
                    }
                }
            }
        }
        return union.getCount() - spaces;
    }
    public int getIndex(int i,int j){
        return i*n + j;
    }
    class UnionFind{
        private int count;
        private int[] parent;
        UnionFind(int n){
            count = n;
            parent = new int[n];
            for(int i = 0; i < n; i++){
                parent[i] = i;
            }
        }
        private int find(int p){
            while(p != parent[p]){
                parent[p] = parent[parent[p]];
                p = parent[p];
            }
            return p;
        }
        private void union(int p, int q){
            int P = find(p);
            int Q = find(q);
            if(P == Q) return;
            parent[P] = Q;
            count--;
        }
        public int getCount(){
            return count;
        }
    }
}
```
[130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)
思路：DFS 深度优先搜索

1、遍历 board ，从边界为‘O’的开始递归，查找相联通的‘O’

2、将与边界相联通的‘O’转换为‘#’，剩余‘O’就是被‘X’包围的

3、再次遍历 board，将‘#’转换为原来的‘O’，将‘O’转换为‘X’

```java
class Solution {
    private static int[] dx = new int[]{0,0,-1,1};
    private static int[] dy = new int[]{-1,1,0,0};
    public void solve(char[][] board) {
        if (board.length == 0) return;
        int m = board.length,n = board[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 首先从边界为‘O’的开始递归下探,将与边界‘O’联通的‘O’都替换为‘#’
                boolean isEdge = i == 0 || j == 0 || i == m-1 || j == n-1;
                if(isEdge && board[i][j] == 'O'){
                    dfs(board,i,j);
                }
            }
        }
        // 将递归后的结果进行转换，‘#’代表和边界联通的，剩余的‘O’代表和边界不连通，需要转换为‘X’
        for (int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(board[i][j] == '#'){
                    board[i][j] = 'O';
                }else if(board[i][j] == 'O'){
                    board[i][j] = 'X';
                }
            }
        }
    }
    public void dfs(char[][] board, int i, int j){
        // 递归结束条件
        if (i < 0 || j < 0 || i >= board.length || j >= board[0].length || board[i][j] == 'X' || board[i][j] == '#') return;
        board[i][j] = '#';
        // 左右上下，递归下探
        for (int k = 0; k < dx.length; k++){
            dfs(board,i+dx[k],j+dy[k]);
        }
    }
}
```
思路：BFS 广度优先搜索
```java
class Solution {
    private static int[] dx = new int[]{0,0,-1,1};
    private static int[] dy = new int[]{-1,1,0,0};
    public void solve(char[][] board) {
        if (board.length == 0) return;
        int m = board.length,n = board[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 首先从边界为‘O’的开始递归下探,将与边界‘O’联通的‘O’都替换为‘#’
                boolean isEdge = i == 0 || j == 0 || i == m-1 || j == n-1;
                if(isEdge && board[i][j] == 'O'){
                    bfs(board,i,j);
                }
            }
        }
        // 将递归后的结果进行转换，‘#’代表和边界联通的，剩余的‘O’代表和边界不连通，需要转换为‘X’
        for (int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(board[i][j] == '#'){
                    board[i][j] = 'O';
                }else if(board[i][j] == 'O'){
                    board[i][j] = 'X';
                }
            }
        }
    }
    public void bfs(char[][] board, int i,int j){
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[]{i,j});
        while(!queue.isEmpty()){
            int[] curr = queue.poll();
            i = curr[0];
            j = curr[1];
            if(i >= 0 && i < board.length && j >= 0 && j < board[0].length && board[i][j] == 'O'){
                board[i][j] = '#';
                for (int k = 0; k < dx.length; k++){
                    queue.offer(new int[]{i+dx[k],j+dy[k]});
                }
            }
        }
    }
}
```
思路：并查集
1、首先创建一个虚拟结点，将所有在边界上为‘O’的点都联通到虚拟结点上；

2、然后再将非边界上的‘O’按是否与边界元素‘O’相连分组；

3、按分组后的情况，将与边界‘O’相连的即就是与虚拟结点相连的都转换为‘O’，不与边界‘O’相连的说明就是被‘X’包围的，转换为‘X’；

```java
class Solution {
    private int[] dx = new int[]{0,0,-1,1};
    private int[] dy = new int[]{-1,1,0,0};
    private int rows,clos;
    public void solve(char[][] board) {
        rows = board.length;
        if (rows == 0) return;
        clos = board[0].length;
        UnionFind union = new UnionFind(rows*clos+1);
        int dummyNode = rows*clos;
        for (int i = 0; i < rows; i++){
            for(int j = 0; j < clos; j++){
                if (board[i][j] == 'O'){
                    if (i == 0 || j == 0 || i == rows - 1 || j == clos - 1){
                        union.union(getIndex(i,j),dummyNode);
                    }else{
                        for(int k = 0; k < dx.length; k++){
                            int currI = i + dx[k];
                            int currJ = j + dy[k];
                            if (currI >= 0 && currI < rows && currJ >= 0 && currJ < clos && board[currI][currJ] == 'O'){
                                union.union(getIndex(i,j),getIndex(currI,currJ));
                            }
                        }
                    }
                }
            }
        }
        for (int i = 0; i < rows; i++){
            for (int j = 0; j < clos; j++) {
                if (union.find(getIndex(i,j)) == union.find(dummyNode)){
                    board[i][j] = 'O';
                }else {
                    board[i][j] = 'X';
                }
            }
        }
    }
    public int getIndex (int i, int j){
        return i * clos + j;
    }
    class UnionFind{
        private int count;
        private int[] parent;
        UnionFind(int n){
            count = n;
            parent = new int[n];
            for (int i = 0; i < n; i++){
                parent[i] = i;
            }
        }
        private int find(int p){
            while(p != parent[p]){
                parent[p] = parent[parent[p]];
                p = parent[p];
            }
            return p;
        }
        private void union(int p, int q){
            int P = find(p);
            int Q = find(q);
            if (p == Q) return;
            parent[P] = Q;
            count--;
        }
    }
}
```
