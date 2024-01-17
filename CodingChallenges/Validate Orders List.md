# Validate Orders List
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
import java.util.Arrays;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class Main {


    public boolean isValid(List<String> orders){
        Set<Character> p_set = new HashSet<>();
        Set<Character> d_set = new HashSet<>();


        for(String order : orders){
            char task_type = order.charAt(0);
            char task_num = order.charAt(1);
            if(task_type == 'P'){
                if(p_set.contains(task_num)){
                    return false;
                }
                p_set.add(task_num);
                
            } else if(task_type == 'D'){
                if(d_set.contains(task_num) || !p_set.contains(task_num)){
                    return false;
                }
                d_set.add(task_num);
            } else {
                return false;
            }
        }
        return p_set.size() == d_set.size();
    }


    public static void main(String[] args){

        Main obj = new Main();
        System.out.println(obj.isValid(Arrays.asList("P1", "P2", "D1", "D2")));
        System.out.println(obj.isValid(Arrays.asList("P1", "D1", "P2", "D2")));
        System.out.println(obj.isValid(Arrays.asList("P1", "D2", "D1", "P2")));
        System.out.println(obj.isValid(Arrays.asList("P1", "D2")));
        System.out.println(obj.isValid(Arrays.asList("P1", "P2")));
        System.out.println(obj.isValid(Arrays.asList("P1", "D1", "D1")));
        System.out.println(obj.isValid(Arrays.asList()));
        System.out.println(obj.isValid(Arrays.asList("P1", "P1", "D1")));
        System.out.println(obj.isValid(Arrays.asList("P1", "P1", "D1", "D1")));
        System.out.println(obj.isValid(Arrays.asList("P1", "D1", "P1")));
        System.out.println(obj.isValid(Arrays.asList("P1", "D1", "P1", "D1")));

    }
}
```
