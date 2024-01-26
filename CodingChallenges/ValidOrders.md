## Doordash virtual onsite question:
Given a set list of pickups and deliveries for order, figure out if the given list is valid or not.
A delivery cannot happen for an order before pickup.

Examples below:
- [P1, P2, D1, D2]==>valid

- [P1, D1, P2, D2]==>valid

- [P1, D2, D1, P2]==>invalid
- [P1, D2]==>invalid
- [P1, P2]==>invalid
- [P1, D1, D1]==>invalid
- []==>valid
- [P1, P1, D1]==>invalid
- [P1, P1, D1, D1]==>invalid
- [P1, D1, P1]==>invalid
- [P1, D1, P1, D1]==>invalid

```java
import java.util.HashMap;
import java.util.Map;

public class OrderValidator {
    
    public static boolean isValidSequence(String[] sequence) {
       Map<String, Integer> orderCount = new HashMap<>();
       for(String action: sequence){
           String order = action.substring(1);
           boolean isPickUp = action.startsWith("P");
           
           if(isPickUp){
               orderCount.put(order, orderCount.getOrDefault(order,0)+1);
           }else{
               if( !orderCount.containsKey(order) ||orderCount.get(order) == 0) {
                   return false;
               }
                orderCount.put(order, orderCount.get(order)-1);
           }
       }
            for(int count:orderCount.values()){
                if(count!=0){
                    return false;
                }
            }
                
          return true;
    }

    public static void main(String[] args) {
        String[][] sequences = {
            {"P1", "P2", "D1", "D2"},
            {"P1", "D1", "P2", "D2"},
            {"P1", "D2", "D1", "P2"},
            {"P1", "D2"},
            {"P1", "P2"},
            {"P1", "D1", "D1"},
            {},
            {"P1", "P1", "D1"},
            {"P1", "P1", "D1", "D1"},
            {"P1", "D1", "P1"},
            {"P1", "D1", "P1", "D1"}
        };
        
        for (String[] sequence : sequences) {
            System.out.println(isValidSequence(sequence));
        }
    }
}
```
