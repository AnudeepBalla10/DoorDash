You're given an array of pending orders at restaurants. A dasher must complete all orders in given hours h. You must use all hours. Return the minimum speed at which the dasher must fulfill the orders. Return -1 if not possible.

Examples:

orders = [4, 3, 1]. So index 0 has 4 orders, index 1 has 3 orders and index 3 has 1 order
hours = 4
result speed would be 3 orders per hour
You would need to spend 2 hours at the first restaurant to fulfill 4 orders, 1hr at second restaurant and so on. You cannot use "left over" capacity from restaurants.

orders = [2,2,2]
hours = 3
result speed in this case would be 2 orders per hour

orders = [100, 2, 2]
hours = 4
result speed would be 50 orders per hour

orders = [100, 100]
hours = 3
result would be -1


```java
public class DasherDelivery {

    // Method to check if it is possible to complete the orders with the given speed in the given hours
    private static boolean canCompleteOrders(int[] orders, int hours, int speed) {
        int hoursSpent = 0;
        for (int order : orders) {
            hoursSpent += Math.ceil((double)order / speed);
        }
        return hoursSpent <= hours;
    }
    
    // Method to find the minimum speed needed to complete all orders in the given hours
    public static int findMinimumSpeed(int[] orders, int hours) {
        // If there are more orders than hours, it's not possible to complete all orders
        //if (orders.length > hours) return -1;

        // Find the maximum number of orders in a single restaurant
        int maxOrders = 0;
        for (int order : orders) {
            maxOrders = Math.max(maxOrders, order);
        }
        
        // Binary search to find the minimum speed
        int left = 1; // Minimum possible speed
        int right = maxOrders; // Maximum orders in a single restaurant is the maximum speed needed
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (canCompleteOrders(orders, hours, mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        
        // Check if the found speed allows us to use exactly all hours
        if (!canCompleteOrders(orders, hours, left) || canCompleteOrders(orders, hours - 1, left)) {
            return -1; // It's not possible to use all hours, or we can complete in less than given hours
        }
        return left;
    }
    
    public static void main(String[] args) {
        System.out.println(findMinimumSpeed(new int[]{4, 3, 1}, 4)); // 3
        System.out.println(findMinimumSpeed(new int[]{2, 2, 2}, 3)); // 2
        System.out.println(findMinimumSpeed(new int[]{100, 2, 2}, 4)); // 50
        System.out.println(findMinimumSpeed(new int[]{100, 100}, 3)); // -1
    }
}

```
