# Doordash Restaurant Menus

We want to implement an in-memory tree key value store for Doordash Restaurant Menus.
Definitions:

- path is a `/`separate string describing the node. Example `/Tres Potrillos/tacos/al_pastor`

##Requirements :

Values are all strings
- get(path): String -> returns the value of the node at the given path
- create(path, value) -> creates a new node and sets it to the given value. Should error out if the node already exists or if the node’s parent does not exist. That is /Sweetgreen/naan_roll cannot be created if /Sweetgreen has not already been created
- delete(path) -> deletes a node, but ONLY if it has no children

```java
import java.util.HashMap;
import java.util.Map;

class FileSystem {

    // The TrieNode data structure.
    class TrieNode {
        String name;
        Map<String, TrieNode> children;
        String value;

        TrieNode(String name) {
            this.name = name;
            this.children = new HashMap<>();
            this.value = null;
        }
    }

    TrieNode root;

    // Root node contains the empty string.
    public FileSystem() {
        this.root = new TrieNode("");
    }

    public boolean createPath(String path, String value) {
        String[] components = path.split("/");
        TrieNode current = root;

        for (int i = 1; i < components.length; i++) {
            String currentComponent = components[i];
            if (!current.children.containsKey(currentComponent)) {
               // If it doesn't and it is the last node, add it to the Trie.
                if (i == components.length - 1) {
                    current.children.put(currentComponent, new TrieNode(currentComponent));
                } else {
                    return false;
                }    
            }
            current = current.children.get(currentComponent);
        }

        if (current.value != null) {
            // Node already exists, cannot create again
            return false;
        }

        current.value = value;
        return true;
    }

    public String get(String path) {
         // Obtain all the components
        String[] components = path.split("/");
        
        // Start "curr" from the root node.
        TrieNode cur = root;
        
        // Iterate over all the components.
        for (int i = 1; i < components.length; i++) {
            
            String currentComponent = components[i];
            
            // For each component, we check if it exists in the current node's dictionary.
            if (!cur.children.containsKey(currentComponent)) {
                return null;   
            }
            
            cur = cur.children.get(currentComponent);
        }
        
        return cur.value;
    }

    public boolean delete(String path) {
        String[] components = path.split("/");
        TrieNode current = root;

        for (int i = 1; i < components.length - 1; i++) {
            String currentComponent = components[i];
            if (!current.children.containsKey(currentComponent)) {
                return false;
            }
            current = current.children.get(currentComponent);
        }

        String lastComponent = components[components.length - 1];
        if (!current.children.containsKey(lastComponent)) {
            return false;
        }

        TrieNode nodeToDelete = current.children.get(lastComponent);

        if (!nodeToDelete.children.isEmpty()) {
            // Node has children, cannot be deleted
            return false;
        }

        current.children.remove(lastComponent);
        return true;
    }

    public static void main(String[] args) {
        FileSystem fileSystem = new FileSystem();
        System.out.println("/Sweetgreen/naan_roll "+fileSystem.createPath("/Sweetgreen/naan_roll", "Delicious Naan Rolls")); // false
        System.out.println("/Sweetgreen "+fileSystem.createPath("/Sweetgreen", "Healthy Salads")); // true
        System.out.println("/Sweetgreen/naan_roll "+fileSystem.createPath("/Sweetgreen/naan_roll", "Delicious Naan Rolls")); // true
       
        System.out.println("/Tres Potrillos "+fileSystem.createPath("/Tres Potrillos", "Authentic Al Pastor Tacos")); // true

        System.out.println("Value at /Sweetgreen: " + fileSystem.get("/Sweetgreen")); // Healthy Salads
        System.out.println("Value at /Sweetgreen/naan_roll: " + fileSystem.get("/Sweetgreen/naan_roll")); // null

        System.out.println(fileSystem.delete("/Sweetgreen/naan_roll")); // false

        System.out.println(fileSystem.delete("/Sweetgreen")); // true

        System.out.println("Value at /Sweetgreen after deletion: " + fileSystem.get("/Sweetgreen")); // null
    }
}

```
