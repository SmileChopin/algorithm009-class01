学习笔记

最小路径和
https://leetcode-cn.com/problems/minimum-path-sum/

精选解法
class Solution {
    public int minPathSum(int[][] grid) {
        for(int i = 0; i < grid.length; i++) {
            for(int j = 0; j < grid[0].length; j++) {
                if(i == 0 && j == 0) continue;
                else if(i == 0)  grid[i][j] += grid[i][j - 1];
                else if(j == 0)  grid[i][j] += grid[i - 1][j];
                else grid[i][j] += Math.min(grid[i - 1][j], grid[i][j - 1]);
            }
        }
        return grid[grid.length - 1][grid[0].length - 1];
    }
}
    public int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for(int i = 0; i < m; i++){
        	for(int j = 0; j < n; j++){
            	if(i == 0 && j != 0) grid[i][j] += grid[i][j-1];
            	if(i != 0 && j == 0) grid[i][j] += grid[i-1][j];
            	if (i != 0 && j != 0) grid[i][j] += Math.min(grid[i-1][j], grid[i][j-1]);
        	}
        }
        return grid[m-1][n-1];
    }

不同路径
public int uniquePaths(int m, int n) {
    int[] cur = new int[n];
    Arrays.fill(cur, 1);

    for (int i = 1; i < m; i++){
        for (int j = 1; j  < n;j++){
            cur[j] += cur[j - 1];
        }
    }
    return cur[n - 1];
}
排列组合的解法（直接数学解出） 总共的步数是m+n-2，而行数是n，以总步数为8，行数为3做举例，结果为

8×7×6/(3×2×1)，以这个公式得出解法

    public int uniquePaths(int m, int n) {
        if(m == 1 || n == 1)
            return 1;
        m--;
        n--;
        if(m < n) {      // Swap, so that m is the bigger number
            m = m + n;
            n = m - n;
            m = m - n;
        }
        long res = 1;
        int j = 1;
        for(int i = m+1; i <= m+n; i++, j++){       // Instead of taking factorial, keep on multiply & divide
            res *= i;
            res /= j;
        }
            
        return (int)res;
    }
不同路径Ⅱ
https://leetcode.com/problems/unique-paths-ii/

 public int uniquePathsWithObstacles(int[][] obstacleGrid) {

        int R = obstacleGrid.length;
        int C = obstacleGrid[0].length;

        // If the starting cell has an obstacle, then simply return as there would be
        // no paths to the destination.
        if (obstacleGrid[0][0] == 1) {
            return 0;
        }

        // Number of ways of reaching the starting cell = 1.
        obstacleGrid[0][0] = 1;

        // Filling the values for the first column
        for (int i = 1; i < R; i++) {
            obstacleGrid[i][0] = (obstacleGrid[i][0] == 0 && obstacleGrid[i - 1][0] == 1) ? 1 : 0;
        }

        // Filling the values for the first row
        for (int i = 1; i < C; i++) {
            obstacleGrid[0][i] = (obstacleGrid[0][i] == 0 && obstacleGrid[0][i - 1] == 1) ? 1 : 0;
        }

        // Starting from cell(1,1) fill up the values
        // No. of ways of reaching cell[i][j] = cell[i - 1][j] + cell[i][j - 1]
        // i.e. From above and left.
        for (int i = 1; i < R; i++) {
            for (int j = 1; j < C; j++) {
                if (obstacleGrid[i][j] == 0) {
                    obstacleGrid[i][j] = obstacleGrid[i - 1][j] + obstacleGrid[i][j - 1];
                } else {
                    obstacleGrid[i][j] = 0;
                }
            }
        }

        // Return value stored in rightmost bottommost cell. That is the destination.
        return obstacleGrid[R - 1][C - 1];
    }
精选解法二：

public int uniquePathsWithObstacles(int[][] obstacleGrid) {
    int width = obstacleGrid[0].length;
    int[] dp = new int[width];
    dp[0] = 1;
    for (int[] row : obstacleGrid) {
        for (int j = 0; j < width; j++) {
            if (row[j] == 1)
                dp[j] = 0;
            else if (j > 0)
                dp[j] += dp[j - 1];
        }
    }
    return dp[width - 1];
}瀛︿範绗旇
