# Count All Valid Pickup and Delivery Options


Given n orders, each order consists of a pickup and a delivery service.

Count all valid pickup/delivery possible sequences such that delivery(i) is always after pickup(i). 

Since the answer may be too large, return it modulo 10^9 + 7.



```java

class Solution {
    //MOD is used because it was asked in the question.
    private int MOD = 1_000_000_007;
    private long[][] memo;
    
    private long totalWays(int unpicked, int undelivered) {
        if (unpicked == 0 && undelivered == 0) {
            return 1;
        }
        
        if (unpicked < 0 || undelivered < 0 || undelivered < unpicked) {
            // We can't pick or deliver more than N items // Number of deliveries can't exceed number of pickups // as we can only deliver after a pickup.
            return 0;
        }
        
        if (memo[unpicked][undelivered] != 0) {
            // Return cached value, if already present. 
            return memo[unpicked][undelivered];
        }
        
        long ans = 0;
        
        // Count all choices of picking up an order.
        ans += unpicked * totalWays(unpicked - 1, undelivered);
        // Handle integer overflow.
        ans %= MOD;
        
        // Count all choices of delivering a picked order.
        ans += (undelivered - unpicked) * totalWays(unpicked, undelivered - 1);
        // Handle integer overflow.
        ans %= MOD;
        
        return memo[unpicked][undelivered] = ans;
    }
    
    public int countOrders(int n) {
        memo = new long[n + 1][n + 1];
        return (int)totalWays(n, n);
    }
}
```
