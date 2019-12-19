# rotting-oranges
Every minute, any fresh orange that is adjacent (4-directionally) to a rotten orange becomes rotten. Return the minimum number of minutes that must elapse until no cell has a fresh orange.  If this is impossible, return -1 instead.

https://leetcode.com/problems/rotting-oranges/

In a given grid, each cell can have one of three values:

the value 0 representing an empty cell;
the value 1 representing a fresh orange;
the value 2 representing a rotten orange.

Example 1:
```
Input: [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
```
Example 2:
```
Input: [[2,1,1],[0,1,1],[1,0,1]]
Output: -1
```
Explanation:  The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.

Example 3:
```
Input: [[0,2]]
Output: 0
```
Explanation:  Since there are already no fresh oranges at minute 0, the answer is just 0.
 
Note:
```
1 <= grid.length <= 10
1 <= grid[0].length <= 10
grid[i][j] is only 0, 1, or 2.
```
Solution:
```
class Solution {
    public int orangesRotting(int[][] grid) {
        if( grid.length < 1 || grid[0].length < 1 ){
            return -1;
        }
        int xlength = grid.length;
        int ylength = grid[0].length;
        List<int[]> rottenPositions = new LinkedList<int[]>();
        List<int[]> prevRottenPositions;
        int freshOranges = 0;
        int minutes = 0;
        boolean stuck = false;
        for ( int i = 0; i < grid.length; i++){
            for ( int j = 0; j < grid[0].length; j++) {
                if ( grid[i][j] == 1 ){
                    freshOranges++;
                } else if ( grid[i][j] == 2 ) {
                    rottenPositions.add(new int[]{i,j});
                }
            }
        }
        while( freshOranges > 0) {
            if( stuck ) {
                return -1;
            }
            stuck = true;
            prevRottenPositions = new LinkedList(rottenPositions);
            for( int[] p : prevRottenPositions) {
                List<int[]> candidatePositions = Arrays.asList( new int[]{p[0]+1, p[1]},
                                     new int[]{p[0]-1, p[1]},
                                     new int[]{p[0], p[1]+1},
                                     new int[]{p[0], p[1]-1});
                
                for ( int[] sp : candidatePositions ) {
                    if( 0 <= sp[0] && sp[0] < grid.length && 
                        0 <= sp[1] && sp[1] < grid[sp[0]].length && 
                        grid[sp[0]][sp[1]] == 1) {
                        grid[sp[0]][sp[1]] = 2;
                        rottenPositions.add(new int[]{sp[0], sp[1]});
                        freshOranges--;
                        stuck = false;
                    }   
                }
            }
            minutes++;
        }
        return minutes;       
    }
}
```
