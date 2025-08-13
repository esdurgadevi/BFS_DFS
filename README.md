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
