# Stream last x max

 Given a streaming data of the form (timestamp, value),
find the maximum value in the stream in the last X seconds.

 Assume time is monotonically increasing.
 Assume time is in the order of seconds.
 max_value() function finds the max in the last X seconds.


```java
import java.util.Deque;
import java.util.LinkedList;

public class StreamProcessor {
    private final int x;
    private final Deque<int[]> deque; // Each element is an array: [timestamp, value]

    public StreamProcessor(int x) {
        this.x = x;
        this.deque = new LinkedList<>();
    }

    public void setValue(int t, int v) {
        // Remove elements that are outside the X second window
        while (!deque.isEmpty() && deque.getFirst()[0] <= t - x) {
            deque.removeFirst();
        }

        // Remove elements from the end that are smaller than the current value
        while (!deque.isEmpty() && deque.getLast()[1] <= v) {
            deque.removeLast();
        }

        // Add the new value
        deque.addLast(new int[] {t, v});
    }

    public int maxValue(int cur_t) {
        // Clean up old elements
        while (!deque.isEmpty() && deque.getFirst()[0] <= cur_t - x) {
            deque.removeFirst();
        }

        // If the deque is empty, return a default value or handle it as needed
        return deque.isEmpty() ? -1 : deque.getFirst()[1];
    }

    public static void main(String[] args) {
        StreamProcessor sp = new StreamProcessor(5);
        sp.setValue(0, 5);
        sp.setValue(1, 6);
        sp.setValue(2, 4);
        System.out.println("Max value at time : " + sp.maxValue(1)); // Should output 6
    }
}

```
