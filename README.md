# BFS_DFS
### 994. Rotting Oranges
[Leetcode link](https://leetcode.com/problems/rotting-oranges/)
<br>
You are given an m x n grid where each cell can have one of three values:

0 representing an empty cell,
1 representing a fresh orange, or
2 representing a rotten orange.
Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return -1.

 

Example 1:


Input: grid = [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
Example 2:

Input: grid = [[2,1,1],[0,1,1],[1,0,1]]
Output: -1
Explanation: The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.
Example 3:

Input: grid = [[0,2]]
Output: 0
Explanation: Since there are already no fresh oranges at minute 0, the answer is just 0.
 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 10
grid[i][j] is 0, 1, or 2.

```class Solution {
    public static boolean is_valid(int x,int y,int[][] grid)
    {
        if(x>=0 && y>=0 && x<grid.length && y<grid[0].length && grid[x][y]==1) return true;
        return false;
    }
    public int orangesRotting(int[][] grid) {
        int[] x = {0,-1,0,1};
        int[] y = {-1,0,1,0};
        int fresh = 0;
        Queue<int[]> q = new LinkedList<>();
        for(int i=0;i<grid.length;i++)
        {
            for(int j=0;j<grid[0].length;j++) 
            {
                if(grid[i][j] == 2) q.add(new int[]{i,j});
                if(grid[i][j] == 1) fresh++;
            }
        }
        int c = 0;
        System.out.print(fresh);
        while(!q.isEmpty() && fresh>0)
        {
            int s = q.size();
            while(s>0)
            {
                int[] t = q.poll();
                s--;
                for(int i=0;i<4;i++)
                {
                    int newx = t[0]+x[i];
                    int newy = t[1]+y[i];
                    if(is_valid(newx,newy,grid)) 
                    {
                        fresh--;
                        grid[newx][newy] = 2;
                        q.add(new int[]{newx,newy});
                    }
                }
            }
            c++;
        } 
        if(fresh!=0) return -1;
        return c;
    }
}
```
> [Reference](https://www.youtube.com/watch?v=yf3oUhkvqA0&t=41s)

### 547. Number of Provinces
[Leetcode link](https://leetcode.com/problems/number-of-provinces/?envType=problem-list-v2&envId=breadth-first-search)
<br>
There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c.

A province is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise.

Return the total number of provinces.

 

Example 1:


Input: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
Output: 2
Example 2:


Input: isConnected = [[1,0,0],[0,1,0],[0,0,1]]
Output: 3
 

Constraints:

1 <= n <= 200
n == isConnected.length
n == isConnected[i].length
isConnected[i][j] is 1 or 0.
isConnected[i][i] == 1
isConnected[i][j] == isConnected[j][i]

```java
class Solution {
    public int findCircleNum(int[][] isConnected) {
        int c = 0,n = isConnected.length;
        boolean[] v = new boolean[n];

        for(int i=0;i<n;i++)
        {
            if(!v[i])
            {
                c++;
                find(i,isConnected,v);
            }
        }
        return c;
    }
    public void find(int idx,int[][] isConnected,boolean[] v)
    {
        for(int i=0;i<isConnected.length;i++)
        {
            if(!v[i] && isConnected[idx][i] == 1) 
            {
                v[i] = true;
                find(i,isConnected,v);
            }
        }
    }
}
```
- In this code we find the connected component that is called provinces.
- So what we simply need is if we take one node then we conver the nodes that are connected to the current node then the next non visited node will taken
- The idea is from the node 1 till the last node we traverse for first node we repeatedly do the bfs what are the nodes can be covered we cover then we count the next non visited node.
- Then return the count.
> [Reference](https://www.youtube.com/watch?v=ACzkVtewUYA&list=PLgUwDviBIf0oE3gA41TKO2H5bHpPd7fzn&index=8)
### 200. Number of Islands
[Leetcode link](https://leetcode.com/problems/number-of-islands/description/?envType=problem-list-v2&envId=breadth-first-search)
<br>
Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

 

Example 1:

Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
Example 2:

Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 300
grid[i][j] is '0' or '1'.

```java
class Solution {
    public int[] arrx = {0,-1,0,1};
    public int[] arry = {-1,0,1,0};
    public boolean is_valid(int x,int y,char[][] grid,boolean[][] v)
    {
        if(x>=0 && y>=0 && x<grid.length && y<grid[0].length && grid[x][y] == '1' && !v[x][y])
        {
            return true;
        }
        return false;
    }
    public int numIslands(char[][] grid) {
        boolean[][] v = new boolean[grid.length][grid[0].length];
        int c = 0;
        for(int i=0;i<grid.length;i++)
        {
            for(int j=0;j<grid[0].length;j++)
            {
                if(grid[i][j] == '1' && !v[i][j])
                {
                    c++;
                    find(i,j,grid,v);
                }
            }
        }
        return c;
    }
    public void find(int x,int y,char[][] grid,boolean[][] v)
    {
        for(int i=0;i<4;i++)
        {
            int newx = x+arrx[i];
            int newy = y+arry[i];
            if(is_valid(newx,newy,grid,v))
            {
                v[newx][newy] = true;
                find(newx,newy,grid,v);
            }
        }
    }
}
```
- In this problem also we find the same thing that is number of islands so we find the group of ones that are surronded by the zero so we count the surronded ones.
- If the x and y axis is 1 then we collect all the ones surronded to it from the top bottom, left and the right so each element we check if it is one and also if it is not visited so it is satisfy the both condition we call the find function.
- What that function will do means get the four position of the current position also check if that four position are 1 and also that all are not visited if condition satisfy then call the same function for the currect coordinates.
> [References](https://www.youtube.com/watch?v=muncqlKJrH0&list=PLgUwDviBIf0oE3gA41TKO2H5bHpPd7fzn&index=8)

### 733. Flood Fill
[Leetcode link](https://leetcode.com/problems/flood-fill/)
<br>
You are given an image represented by an m x n grid of integers image, where image[i][j] represents the pixel value of the image. You are also given three integers sr, sc, and color. Your task is to perform a flood fill on the image starting from the pixel image[sr][sc].

To perform a flood fill:

Begin with the starting pixel and change its color to color.
Perform the same process for each pixel that is directly adjacent (pixels that share a side with the original pixel, either horizontally or vertically) and shares the same color as the starting pixel.
Keep repeating this process by checking neighboring pixels of the updated pixels and modifying their color if it matches the original color of the starting pixel.
The process stops when there are no more adjacent pixels of the original color to update.
Return the modified image after performing the flood fill.

 

Example 1:

Input: image = [[1,1,1],[1,1,0],[1,0,1]], sr = 1, sc = 1, color = 2

Output: [[2,2,2],[2,2,0],[2,0,1]]

Explanation:



From the center of the image with position (sr, sc) = (1, 1) (i.e., the red pixel), all pixels connected by a path of the same color as the starting pixel (i.e., the blue pixels) are colored with the new color.

Note the bottom corner is not colored 2, because it is not horizontally or vertically connected to the starting pixel.

Example 2:

Input: image = [[0,0,0],[0,0,0]], sr = 0, sc = 0, color = 0

Output: [[0,0,0],[0,0,0]]

Explanation:

The starting pixel is already colored with 0, which is the same as the target color. Therefore, no changes are made to the image.

 

Constraints:

m == image.length
n == image[i].length
1 <= m, n <= 50
0 <= image[i][j], color < 216
0 <= sr < m
0 <= sc < n

```java
class Solution {
    public boolean is_valid(int x,int y,int[][] a,int set,boolean[][] v)
    {
        if(x>=0 && y>=0 && x<a.length && y<a[0].length)
        {
            if(a[x][y] == set && !v[x][y]) return true;
            else return false;
        }
        return false;
    }
    public int[][] floodFill(int[][] image, int sr, int sc, int color) {
        boolean[][] v = new boolean[image.length][image[0].length];
        int set = image[sr][sc];
        image[sr][sc] = color;
        find(sr,sc,image,color,set,v);
        return image;
    }

    public void find(int x,int y,int[][] image,int color,int set,boolean[][] v)
    {
        int[] dx = {0,-1,0,1};
        int[] dy = {-1,0,1,0};
        for(int i=0;i<4;i++)
        {
            int newx = x+dx[i]; 
            int newy = y+dy[i];
            if(is_valid(newx,newy,image,set,v))
            {
                image[newx][newy] = color;
                v[newx][newy] = true;
                find(newx,newy,image,color,set,v);
            }
        }
    }
}
```
- In this code we fill the color that is they give sr and sc in the image matrix in the sr and sc position what color will be there we fo to four distance up down left and right if the going cell containe the first color means we change the color and from the cell we explore the cells.
- So valid function is x>=0 And y>=0 and x<n and y<m and not visited and the cell must be equal to the first color if all the condition are satisfy we return true.
- other wise we return false
- if it is valid we call the same function
> [Refrence](https://www.youtube.com/watch?v=C-2_uSRli8o&list=PLgUwDviBIf0oE3gA41TKO2H5bHpPd7fzn&index=9)
### Undirected Graph Cycle
[Geeks for Geeks](https://www.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/1)
<br>
Given an undirected graph with V vertices and E edges, represented as a 2D vector edges[][], where each entry edges[i] = [u, v] denotes an edge between vertices u and v, determine whether the graph contains a cycle or not. The graph can have multiple component.

Examples:

Input: V = 4, E = 4, edges[][] = [[0, 1], [0, 2], [1, 2], [2, 3]]
Output: true
Explanation: 
 
1 -> 2 -> 0 -> 1 is a cycle.
Input: V = 4, E = 3, edges[][] = [[0, 1], [1, 2], [2, 3]]
Output: false
Explanation: 
 
No cycle in the graph.
Constraints:
1 ≤ V ≤ 105
1 ≤ E = edges.size() ≤ 105

```java
class Solution {
    public boolean find(List<List<Integer>> li,int i,boolean[] v)
    {
        v[i] = true;
        
        Queue<int[]> q = new LinkedList<>();
        q.add(new int[]{i,-1});
        
        while(!q.isEmpty())
        {
            int[] t = q.poll();
            List<Integer> temp = li.get(t[0]);
            for(int x:temp)
            {
                if(!v[x]) 
                {
                    v[x] = true;
                    q.add(new int[]{x,t[0]});
                }
                else if(t[1] != x) return true;
            }
        }
        return false;
    }
    public boolean isCycle(int V, int[][] edges) {
        List<List<Integer>> li = new ArrayList<>();
        
        for(int i=0;i<V;i++) li.add(new ArrayList<>());
        
        for(int i=0;i<edges.length;i++)
        {
            int u = edges[i][0];
            int v = edges[i][1];
            
            li.get(u).add(v);
            li.get(v).add(u);
        }
        
        boolean[] v = new boolean[V];
        
        for(int i=0;i<V;i++)
        {
            if(!v[i])
            {
                if(find(li,i,v)) return true;
            }
        }
        return false;
    }
}
```
- In this what we do is make traverse the graph berath wise check current nodes adjacency node is visited or not if it is not visited then add to the queue current as a parent it it is visited means check current nodes parent or not if that is not even the current node parent means other node that will visit this node so cycle will appear so that time return true in final return false.
> [Reference](https://www.youtube.com/watch?v=BPlrALf1LDU&list=PLgUwDviBIf0oE3gA41TKO2H5bHpPd7fzn&index=11)

### Undirected Graph Cycle
[Geeks for Geeks](https://www.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/1)
<br>
Given an undirected graph with V vertices and E edges, represented as a 2D vector edges[][], where each entry edges[i] = [u, v] denotes an edge between vertices u and v, determine whether the graph contains a cycle or not. The graph can have multiple component.

Examples:

Input: V = 4, E = 4, edges[][] = [[0, 1], [0, 2], [1, 2], [2, 3]]
Output: true
Explanation: 
 
1 -> 2 -> 0 -> 1 is a cycle.
Input: V = 4, E = 3, edges[][] = [[0, 1], [1, 2], [2, 3]]
Output: false
Explanation: 
 
No cycle in the graph.
Constraints:
1 ≤ V ≤ 105
1 ≤ E = edges.size() ≤ 105

```java
class Solution {
    public boolean isCycle(int V, int[][] edges) {
        if(V<=2) return false;
        boolean[] v = new boolean[V];
        List<List<Integer>> li = new ArrayList<>();
        for(int i=0;i<V;i++)
        {
            li.add(new ArrayList<>());
        }
        for(int[] x:edges)
        {
            li.get(x[0]).add(x[1]);
            li.get(x[1]).add(x[0]);
        }
        boolean ans = false;
        for(int i=0;i<V;i++)
        {
            if(!v[i]) 
            {
                //v[i] = true;
                if(find(i,-1,v,li)) return true;
            }
        }
        return false;
    }
    public boolean find(int n,int p,boolean[] v,List<List<Integer>> li)
    {
        v[n] = true;
        List<Integer> t = li.get(n);
        for(int x:t)
        {
            if(x != p && v[x]) return true;
            if(!v[x]) 
            {
                v[x] = true;
                if(find(x,n,v,li)) return true;
            }
        }
        return false;
    }
}
```
- In this what we do is make traverse the graph berath wise check current nodes adjacency node is visited or not if it is not visited then add to the queue current as a parent it it is visited means check current nodes parent or not if that is not even the current node parent means other node that will visit this node so cycle will appear so that time return true in final return false.
> [Reference](https://www.youtube.com/watch?v=zQ3zgFypzX4&list=PLgUwDviBIf0oE3gA41TKO2H5bHpPd7fzn&index=12)

### 498. Diagonal Traverse
[Leetcode link](https://leetcode.com/problems/diagonal-traverse/description/?envType=daily-question&envId=2025-08-25)
<br>
Given an m x n matrix mat, return an array of all the elements of the array in a diagonal order.

 

Example 1:


Input: mat = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,4,7,5,3,6,8,9]
Example 2:

Input: mat = [[1,2],[3,4]]
Output: [1,2,3,4]
 

Constraints:

m == mat.length
n == mat[i].length
1 <= m, n <= 104
1 <= m * n <= 104
-105 <= mat[i][j] <= 105

```java
class Solution {
    public boolean is_valid(int x,int y,boolean[][] v)
    {
        return (x>=0 && y>=0 && x<v.length && y<v[0].length && !v[x][y]);
    }
    public int[] findDiagonalOrder(int[][] mat) {
        int[] ans = new int[mat.length*mat[0].length];
        Queue<int[]> q = new LinkedList<>();
        int x = 0,y = 0,flag = 0,idx = 0;
        q.add(new int[]{0,0});
        boolean[][] v = new boolean[mat.length][mat[0].length];
        v[0][0] = true;
        while(!q.isEmpty())
        {
            List<Integer> li = new ArrayList<>();
            int size = q.size();
            while(size>0)
            {
                int[] curr = q.poll();
                li.add(mat[curr[0]][curr[1]]);
                if(is_valid(curr[0]+1,curr[1],v))
                {
                    v[curr[0]+1][curr[1]] = true;
                    q.add(new int[]{curr[0]+1,curr[1]});
                }
                if(is_valid(curr[0],curr[1]+1,v))
                {
                    v[curr[0]][curr[1]+1] = true;
                    q.add(new int[]{curr[0],curr[1]+1});
                }
                size--;
            }
            if(flag == 1) Collections.reverse(li);
            for(int temp:li) ans[idx++] = temp;
            flag = 1-flag;
        }
        return ans;   
    }
}
```
### 934. Shortest Bridge
[Leetcode link](https://leetcode.com/problems/shortest-bridge/)
<br>
You are given an n x n binary matrix grid where 1 represents land and 0 represents water.

An island is a 4-directionally connected group of 1's not connected to any other 1's. There are exactly two islands in grid.

You may change 0's to 1's to connect the two islands to form one island.

Return the smallest number of 0's you must flip to connect the two islands.

 

Example 1:

Input: grid = [[0,1],[1,0]]
Output: 1
Example 2:

Input: grid = [[0,1,0],[0,0,0],[0,0,1]]
Output: 2
Example 3:

Input: grid = [[1,1,1,1,1],[1,0,0,0,1],[1,0,1,0,1],[1,0,0,0,1],[1,1,1,1,1]]
Output: 1
 

Constraints:

n == grid.length == grid[i].length
2 <= n <= 100
grid[i][j] is either 0 or 1.
There are exactly two islands in grid.

```java
class Solution {
    public int[] ax = {0,-1,0,1};
    public int[] ay = {-1,0,1,0};
    public boolean is_valid(int x,int y,int[][] grid,boolean[][] v)
    {
        return (x>=0 && y>=0 && x<grid.length && y<grid[0].length && !v[x][y]);
    }
    public int shortestBridge(int[][] grid) {
        int ans = 1;
        boolean flag = false;
        Queue<int[]> q = new LinkedList<>();
        boolean[][] v = new boolean[grid.length][grid[0].length];
        for(int i=0;i<grid.length;i++)
        {
            for(int j=0;j<grid[0].length;j++)
            {
                if(grid[i][j] == 1)
                {
                    v[i][j] = true;
                    find(i,j,grid,v,q);
                    flag = true;
                    break;
                }
            }
            if(flag == true) break;
        }
        while(!q.isEmpty())
        {
            int size = q.size();
            while(size>0)
            {
                int[] temp = q.poll();
                for(int i=0;i<4;i++)
                {
                    int nx = temp[0]+ax[i];
                    int ny = temp[1]+ay[i];
                    if(is_valid(nx,ny,grid,v))
                    {
                        if(grid[nx][ny] == 1) return ans;
                        q.add(new int[]{nx,ny});
                        v[nx][ny] = true; 
                    }
                }
                size--;
            }
            ans++;
            //System.out.println(temp[0]+" "+temp[1]);
        }
        return ans;
    }
    public void find(int x,int y,int[][] grid,boolean[][] v,Queue<int[]> q)
    {
        if(grid[x][y] == 0) 
        {
            q.add(new int[]{x,y});
            return;
        }
        for(int i=0;i<4;i++)
        {
            int nx = x+ax[i];
            int ny = y+ay[i];
            if(is_valid(nx,ny,grid,v))
            {
                v[nx][ny] = true;
                find(nx,ny,grid,v,q);
            }
        }
    }
}
```
- First we find the first occuring island(1) and cover all the 1's surronding to this 1 in 4 directions then whenever 0 occur we added that position to the queueu
- No at each level we find any one is occur that is the second island so using bfs we find the second island in minimum distance.

### 542. 01 Matrix
[Leetcode link](https://leetcode.com/problems/01-matrix/description/)
<br>
Given an m x n binary matrix mat, return the distance of the nearest 0 for each cell.

The distance between two cells sharing a common edge is 1.

 

Example 1:


Input: mat = [[0,0,0],[0,1,0],[0,0,0]]
Output: [[0,0,0],[0,1,0],[0,0,0]]
Example 2:


Input: mat = [[0,0,0],[0,1,0],[1,1,1]]
Output: [[0,0,0],[0,1,0],[1,2,1]]
 

Constraints:

m == mat.length
n == mat[i].length
1 <= m, n <= 104
1 <= m * n <= 104
mat[i][j] is either 0 or 1.
There is at least one 0 in mat.

```java
class Solution {
    public boolean is_valid(int x,int y,int[][] g,boolean[][] v)
    {
        return (x>=0 && y>=0 && x<g.length && y<g[0].length && !v[x][y]);
    }
    public int[] x = {0,-1,0,1};
    public int[] y = {-1,0,1,0};
    public int[][] updateMatrix(int[][] mat) {
        int[][] ans = new int[mat.length][mat[0].length];
        Queue<int[]> q = new LinkedList<>();
        boolean[][] v = new boolean[mat.length][mat[0].length];
        for(int i=0;i<mat.length;i++)
        {
            for(int j=0;j<mat[0].length;j++)
            {
                if(mat[i][j] == 0) 
                {
                    q.add(new int[]{i,j,0}); 
                    v[i][j] = true;
                }
            }
        }
        while(!q.isEmpty())
        {
            int[] temp = q.poll();
            for(int i=0;i<4;i++)
            {
                int nx = temp[0]+x[i];
                int ny = temp[1]+y[i];
                if(is_valid(nx,ny,mat,v))
                {
                    v[nx][ny] = true;
                    ans[nx][ny] = temp[2]+1;
                    q.add(new int[]{nx,ny,temp[2]+1});
                }
            }
        }
        return ans;
    }
}
```
> [Refrence](https://www.youtube.com/watch?v=edXdVwkYHF8)
### 130. Surrounded Regions
[Leetcode link](https://leetcode.com/problems/surrounded-regions/)
<br>
You are given an m x n matrix board containing letters 'X' and 'O', capture regions that are surrounded:

Connect: A cell is connected to adjacent cells horizontally or vertically.
Region: To form a region connect every 'O' cell.
Surround: The region is surrounded with 'X' cells if you can connect the region with 'X' cells and none of the region cells are on the edge of the board.
To capture a surrounded region, replace all 'O's with 'X's in-place within the original board. You do not need to return anything.

 

Example 1:

Input: board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]

Output: [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]

Explanation:


In the above diagram, the bottom region is not captured because it is on the edge of the board and cannot be surrounded.

Example 2:

Input: board = [["X"]]

Output: [["X"]]

 

Constraints:

m == board.length
n == board[i].length
1 <= m, n <= 200
board[i][j] is 'X' or 'O'.

```java
class Solution {
    public boolean is_valid(int x,int y,char[][] b,boolean[][] v)
    {
        return (x>=0 && y>=0 && x<b.length && y<b[0].length && !v[x][y] && b[x][y] == 'O');
    }
    public int[] ax = {0,-1,0,1};
    public int[] ay = {-1,0,1,0};
    public void solve(char[][] board) {
        boolean[][] v = new boolean[board.length][board[0].length];
        //first row
        for(int i=0;i<board[0].length;i++)
        {
            if(board[0][i] == 'O') 
            {
                board[0][i] = 'S';
                v[0][i] = true;
                dfs(0,i,board,v);
            }
        }
        //last row
        int n = board.length;
        for(int i=0;i<board[0].length;i++)
        {
            if(board[n-1][i] == 'O') 
            {
                board[n-1][i] = 'S';
                v[n-1][i] = true;
                dfs(n-1,i,board,v);
            }
        }
        //first column
        for(int i=0;i<board.length;i++)
        {
            if(board[i][0] == 'O') 
            {
                board[i][0] = 'S';
                v[i][0] = true;
                dfs(i,0,board,v);
            }
        }
        //last column
        n = board[0].length;
        for(int i=0;i<board.length;i++)
        {
            if(board[i][n-1] == 'O') 
            {
                v[i][n-1] = true;
                board[i][n-1] = 'S';
                dfs(i,n-1,board,v);
            }
        }

        for(int i=0;i<board.length;i++)
        {
            for(int j=0;j<board[0].length;j++)
            {
                if(board[i][j] == 'S') board[i][j] = 'O';
                else board[i][j] = 'X';
            } 
        }
        return;
    }
    public void dfs(int x,int y,char[][] board,boolean[][] v)
    {
        for(int i=0;i<4;i++)
        {
            int nx = x+ax[i];
            int ny = y+ay[i];
            if(is_valid(nx,ny,board,v))
            {
                v[nx][ny] = true;
                board[nx][ny] = 'S';
                dfs(nx,ny,board,v);
            }
        }
    }
}
```
- In this first we find and mark the final region that is border of the array as S and directly or indirectly connected to this O as also S
- So except the S all the O's are must be converted as X.

### 1020. Number of Enclaves
[Leetcode link](https://leetcode.com/problems/number-of-enclaves/)
<br>
You are given an m x n binary matrix grid, where 0 represents a sea cell and 1 represents a land cell.

A move consists of walking from one land cell to another adjacent (4-directionally) land cell or walking off the boundary of the grid.

Return the number of land cells in grid for which we cannot walk off the boundary of the grid in any number of moves.

 

Example 1:


Input: grid = [[0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0]]
Output: 3
Explanation: There are three 1s that are enclosed by 0s, and one 1 that is not enclosed because its on the boundary.
Example 2:


Input: grid = [[0,1,1,0],[0,0,1,0],[0,0,1,0],[0,0,0,0]]
Output: 0
Explanation: All 1s are either on the boundary or can reach the boundary.
 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 500
grid[i][j] is either 0 or 1.

```java
class Solution {
    public int[] ax ={0,-1,0,1};
    public int[] ay = {-1,0,1,0};
    public int ones = 0;
    public boolean is_valid(int x,int y,int[][] g,boolean[][] v)
    {
        return (x>=0 && y>=0 && x<g.length && y<g[0].length && !v[x][y] && g[x][y] == 1);
    }
    public int numEnclaves(int[][] grid) {
        boolean[][] v = new boolean[grid.length][grid[0].length];
        for(int i=0;i<grid.length;i++)
        {
            for(int j=0;j<grid[0].length;j++)
            {
                if((grid[i][j] == 1)) 
                {
                    ones++;
                }
            }
        }
        //first row
        for(int i=0;i<grid[0].length;i++)
        {
            if(grid[0][i] == 1 && !v[0][i]) 
            {
                v[0][i] = true;
                ones--;
                dfs(0,i,grid,v);
            }
        }
        //last row
        int n = grid.length;
        for(int i=0;i<grid[0].length;i++)
        {
            if(grid[n-1][i] == 1 && !v[n-1][i]) 
            {
                v[n-1][i] = true;
                ones--;
                dfs(n-1,i,grid,v);
            }
        }
        //first column
        for(int i=0;i<grid.length;i++)
        {
            if(grid[i][0] == 1 && !v[i][0]) 
            {
                ones--;
                v[i][0] = true;
                dfs(i,0,grid,v);
            }
        }
        //last column
        n = grid[0].length;
        for(int i=0;i<grid.length;i++)
        {
            if(grid[i][n-1] == 1 && !v[i][n-1]) 
            {
                ones--;
                v[i][n-1] = true;
                dfs(i,n-1,grid,v);
            }
        }
        return ones<=0?0:ones;
    }
    public void dfs(int x,int y,int[][] grid,boolean[][] v)
    {
        for(int i=0;i<4;i++)
        {
            int nx = x+ax[i];
            int ny = y+ay[i];
            if(is_valid(nx,ny,grid,v))
            {
                ones--;
                v[nx][ny] = true;
                dfs(nx,ny,grid,v);
            }
        }
    }
}
```
- In this they ask which are the land not possible to reach the boundry that is which is not directly or indirectly connected to the boundry.
- So first i find all the ones in the grid
- then minus the boundry ones using dfs
### Other method simple optimization
```java
class Solution {
    public int[] ax ={0,-1,0,1};
    public int[] ay = {-1,0,1,0};
    public int ones = 0;
    public boolean is_valid(int x,int y,int[][] g,boolean[][] v)
    {
        return (x>=0 && y>=0 && x<g.length && y<g[0].length && !v[x][y] && g[x][y] == 1);
    }
    public int numEnclaves(int[][] grid) {
        boolean[][] v = new boolean[grid.length][grid[0].length];
        for(int i=0;i<grid.length;i++)
        {
            for(int j=0;j<grid[0].length;j++)
            {
                if((grid[i][j] == 1)) 
                {
                    ones++;
                    if(i==0 || j==0 || i==grid.length-1 || j==grid[0].length-1)
                    {
                        if(!v[i][j])
                        {
                            v[i][j] = true;
                            ones--;
                            dfs(i,j,grid,v);
                        }
                    }
                }
            }
        }
        return ones;
    }
    public void dfs(int x,int y,int[][] grid,boolean[][] v)
    {
        for(int i=0;i<4;i++)
        {
            int nx = x+ax[i];
            int ny = y+ay[i];
            if(is_valid(nx,ny,grid,v))
            {
                ones--;
                v[nx][ny] = true;
                dfs(nx,ny,grid,v);
            }
        }
    }
}
```
- in this in single traversel we check if it is one or not
- then check the boundry condition if it is boundry and not visited then call dfs and each time decrease the one count.

### 127. Word Ladder
[Leetcode link](https://leetcode.com/problems/word-ladder/)
<br>
A transformation sequence from word beginWord to word endWord using a dictionary wordList is a sequence of words beginWord -> s1 -> s2 -> ... -> sk such that:

Every adjacent pair of words differs by a single letter.
Every si for 1 <= i <= k is in wordList. Note that beginWord does not need to be in wordList.
sk == endWord
Given two words, beginWord and endWord, and a dictionary wordList, return the number of words in the shortest transformation sequence from beginWord to endWord, or 0 if no such sequence exists.

 

Example 1:

Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> cog", which is 5 words long.
Example 2:

Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: 0
Explanation: The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.
 

Constraints:

1 <= beginWord.length <= 10
endWord.length == beginWord.length
1 <= wordList.length <= 5000
wordList[i].length == beginWord.length
beginWord, endWord, and wordList[i] consist of lowercase English letters.
beginWord != endWord
All the words in wordList are unique.

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        int ans = 1;
        Queue<String> q = new LinkedList<>();
        Set<String> visited = new HashSet<>();
        Set<String> word = new HashSet<>(wordList);
        visited.add(beginWord);
        q.add(beginWord);
        while(!q.isEmpty())
        {
            int size = q.size();
            while(size>0)
            {
                String t = q.poll();
                System.out.println(t);
                if(t.equals(endWord)) return ans;
                char[] temparray = t.toCharArray();
                for(int i=0;i<temparray.length;i++)
                {
                    char original = temparray[i];
                    for(char j='a';j<='z';j++)
                    {
                        temparray[i] = j;
                        String curr = new String(temparray);
                        if(!visited.contains(curr) && word.contains(curr))
                        {
                            visited.add(curr);
                            q.add(curr);
                        }
                    }
                    temparray[i]=original;
                }
                size--;
            }
            ans++;
        }
        return 0;   
    }
}
```
