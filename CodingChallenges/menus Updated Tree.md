# Dasher menu updated

At DoorDash, menus are updated daily even hourly to keep them up-to-date. Each menu can be regarded as a tree. When the merchant sends us the latest menu, can we calculate
how many nodes have changed/added/deleted?

Assume each Node structure is as below:
```
class Node {
        String key;
        int value;
        List children;
}
```

Assume there are no duplicate nodes with the same key.

Output: Return the number of changed nodes in the tree.

- Existing tree
```
               a(1)
            /      
         b(2)      c(3)
        /     \         
      d(4)    e(5)      f(6)
```
- New tree
```
            a(1)
               
               c(3)
                  
                   f(66)
```

a(1) a is the key, 1 is the value
For example, there are a total of 4 changed nodes. Node b, Node d, Node e are automatically set to inactive. The value of Node f changed as well.

- Existing tree
```
            a(1)
          /      
        b(2‍‍‌‌‌‍‍‌‌‍‍‍‌‍‍‌‌‍)      c(3)
      /       \      
  d(4)      e(5)      g(7)
```
- New tree
```
                a(1)
            /         
         b(2)         h(8)
      /    |   \           
e(5)   d(4)   f(6)       g(7)
```

There are a total of 5 changed nodes. Node f is a newly-added node. c(3) and old g(7) are deactivated and h(8) and g(7) are newly added nodes

-> followup print out the changes


## Approach we use Tree.

```java
import java.util.*;

class Node {
    String key;
    int value;
    List<Node> children;

    public Node(String key, int value) {
        this.key = key;
        this.value = value;
        this.children = new ArrayList<>();
    }
}

class MenuComparator {

    static class Change {
        String key;
        String action;

        public Change(String key, String action) {
            this.key = key;
            this.action = action;
        }
    }

    public static List<Change> compareMenus(Node oldMenu, Node newMenu) {
        Map<String, Node> oldMenuMap = buildMenuMap(oldMenu);
        List<Change> changes = new ArrayList<>();
        compareNodes(oldMenuMap, newMenu, changes);
        return changes;
    }

    private static Map<String, Node> buildMenuMap(Node menu) {
        Map<String, Node> menuMap = new HashMap<>();
        buildMenuMapHelper(menu, menuMap);
        return menuMap;
    }

    private static void buildMenuMapHelper(Node node, Map<String, Node> menuMap) {
        menuMap.put(node.key, node);
        for (Node child : node.children) {
            buildMenuMapHelper(child, menuMap);
        }
    }

    private static void compareNodes(Map<String, Node> oldMenuMap, Node newNode, List<Change> changes) {
        if (!oldMenuMap.containsKey(newNode.key)) {
            // Node is added
            changes.add(new Change(newNode.key, "added"));
        } else {
            Node oldNode = oldMenuMap.get(newNode.key);

            if (oldNode.value != newNode.value) {
                // Value has changed
                changes.add(new Change(newNode.key, "value changed"));
            }

            for (Node newChild : newNode.children) {
                compareNodes(oldMenuMap, newChild, changes);
            }
        }
    }

    public static void main(String[] args) {
        // Example usage
        Node oldMenu1 = buildTree1();
        Node newMenu1 = buildTree2();

        List<Change> changes1 = compareMenus(oldMenu1, newMenu1);
        printChanges(changes1);

        Node oldMenu2 = buildTree3();
        Node newMenu2 = buildTree4();

        List<Change> changes2 = compareMenus(oldMenu2, newMenu2);
        printChanges(changes2);
    }

    private static void printChanges(List<Change> changes) {
        for (Change change : changes) {
            System.out.println("Node '" + change.key + "' " + change.action);
        }
        System.out.println("Total changes: " + changes.size());
        System.out.println();
    }

    private static Node buildTree1() {
        Node a = new Node("a", 1);
        Node b = new Node("b", 2);
        Node c = new Node("c", 3);
        Node d = new Node("d", 4);
        Node e = new Node("e", 5);
        Node f = new Node("f", 6);

        a.children.add(b);
        b.children.add(d);
        b.children.add(e);
        a.children.add(c);
        c.children.add(f);

        return a;
    }

    private static Node buildTree2() {
        Node a = new Node("a", 1);
        Node c = new Node("c", 3);
        Node f = new Node("f", 66);

        a.children.add(c);
        c.children.add(f);

        return a;
    }

    private static Node buildTree3() {
        Node a = new Node("a", 1);
        Node b = new Node("b", 2);
        Node c = new Node("c", 3);
        Node d = new Node("d", 4);
        Node e = new Node("e", 5);
        Node g = new Node("g", 7);
        Node f = new Node("f", 6);
        a.children.add(b);
        b.children.add(d);
        b.children.add(e);
        a.children.add(c);
        c.children.add(f);
        a.children.add(g);

        return a;
    }

    private static Node buildTree4() {
        Node a = new Node("a", 1);
        Node b = new Node("b", 2);
        Node h = new Node("h", 8);
        Node d = new Node("d", 4);
        Node f = new Node("f", 6);
        Node g = new Node("g", 7);
        Node e = new Node("e", 5);

        a.children.add(b);
        b.children.add(d);
        b.children.add(e);
        a.children.add(h);
        h.children.add(f);
        h.children.add(g);

        return a;
    }
}
```
