# Paint House
## https://leetcode.com/problems/paint-house
There is a row of n houses, where each house can be painted one of three colors: red, blue, or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by an n x 3 cost matrix costs.

For example, costs[0][0] is the cost of painting house 0 with the color red; costs[1][2] is the cost of painting house 1 with color green, and so on...
Return the minimum cost to paint all houses.

```java
Example 1:

Input: costs = [[17,2,17],[16,16,5],[14,3,19]]
Output: 10
Explanation: Paint house 0 into blue, paint house 1 into green, paint house 2 into blue.
Minimum cost: 2 + 5 + 3 = 10.

Example 2:

Input: costs = [[7,6,2]]
Output: 2
```

## Constraints:

1. costs.length == n
2. costs[i].length == 3
3. 1 <= n <= 100
4. 1 <= costs[i][j] <= 20

## Implementation 1a : DP Runtime = O(n), Space = O(n) where n = houses * colors 
```java
class Solution {
    public int minCost(int[][] costs) {
        int houses = costs.length;
        int[][] grid = new int[houses][3];
        for(int color = 0; color < 3; color++)
           grid[0][color] = costs[0][color];
        for(int house = 1; house < houses; house++) {
            for(int color = 0; color < 3; color++) {
                 int cost = costs[house][color];
                 if(color == 0)
                    cost += Math.min(grid[house-1][1], grid[house-1][2]);
                 else if(color == 1)
                    cost += Math.min(grid[house-1][0], grid[house-1][2]);
                 else
                    cost += Math.min(grid[house-1][0], grid[house-1][1]);
                 
                 grid[house][color] = cost;     
            }
        }
        int minCost = Math.min(grid[houses-1][0], grid[houses-1][1]);
        minCost = Math.min(minCost, grid[houses-1][2]);
        return minCost; 
    }
}
```

## Implementation 2a : DP, Space Optimized , Runtime = O(n), Space = O(1) where n = houses * colors
```java
class Solution {
    public int minCost(int[][] costs) {
        int houses = costs.length;
        int[] results = new int[3];
        for(int color = 0; color < 3; color++)
           results[color] = costs[0][color];
        for(int house = 1; house < houses; house++) {
            int[] current = new int[3];
            for(int color = 0; color < 3; color++) {
                 int cost = costs[house][color];
                 if(color == 0)
                    cost += Math.min(results[1], results[2]);
                 else if(color == 1)
                    cost += Math.min(results[0], results[2]);
                 else
                    cost += Math.min(results[0], results[1]);
                 
                 current[color] = cost;     
            }
            results = current;
        }
        int minCost = Math.min(results[0], results[1]);
        minCost = Math.min(minCost, results[2]);
        return minCost; 
    }
}
```

# Implementation 2b : Since there are only three colors we can write it more concisely
```java
class Solution {
    public int minCost(int[][] costs) {
        int houses = costs.length;
        int[] results = new int[3];
        for(int color = 0; color < 3; color++)
           results[color] = costs[0][color];
        for(int house = 1; house < houses; house++) {
            int[] current = new int[3];
            current[0] = costs[house][0] + Math.min(results[1], results[2]);
            current[1] = costs[house][1] + Math.min(results[0], results[2]);
            current[2] = costs[house][2] + Math.min(results[0], results[1]);
            results = current;
        }
        return Math.min(Math.min(results[0], results[1]), results[2]); 
    }
}
```

# References :
https://www.youtube.com/watch?v=-w67-4tnH5U
