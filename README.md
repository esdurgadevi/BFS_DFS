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
