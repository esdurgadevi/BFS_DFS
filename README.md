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
