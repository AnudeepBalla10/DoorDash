# DicePermutations


Write a function that takes n number of dice/die and returns all possible permutations of result. 
For example, when you have n=2 dice, we want to return: [(1,1), (1,2),(1,3)...(6,5),(6,6)]

```java
import java.util.ArrayList;
import java.util.List;

public class DicePermutations {

    public static void main(String[] args) {
        int n = 1; // Number of dice
        generatePermutations(n, new ArrayList<>());
    }

    private static void generatePermutations(int n, List<Integer> combination) {
        if (n == 0) {
            System.out.println(combination);
            return;
        }

        for (int i = 1; i <= 6; i++) {
            combination.add(i);
            generatePermutations(n - 1, combination);
            combination.remove(combination.size() - 1); // Backtrack
        }
    }
}

```
