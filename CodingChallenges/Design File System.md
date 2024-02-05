# Design File System

> You are asked to design a file system that allows you to create new paths and associate them with different values.

The format of a path is one or more concatenated strings of the form: / followed by one or more lowercase English letters. For example, "/leetcode" and "/leetcode/problems" are valid paths while an empty string "" and "/" are not.

Implement the FileSystem class:

bool createPath(string path, int value) Creates a new path and associates a value to it if possible and returns true. Returns false if the path already exists or its parent path doesn't exist.
int get(string path) Returns the value associated with path or returns -1 if the path doesn't exist.


## Approach:
we need to create 2 methods,
- createPath(String path, int value)
- get(String path)

## Given Conditions: 
- empty string "" and "/" are not allowed

Simple Approach is to have a HaspMap to track all the paths and values,
split the given string by "/" and check the subString before the last "/" for parent path. 

## Code:
```java

class FileSystem {

    // The TrieNode data structure.
    class TrieNode {
        
        String name;
        int val = -1;
        Map<String, TrieNode> map = new HashMap<>();
        
        TrieNode (String name) {
            this.name = name;
        }
    }
    
    TrieNode root;
    
    // Root node contains the empty string.
    public FileSystem() {
        this.root = new TrieNode("");
    }
    
    public boolean createPath(String path, int value) {
        
        // Obtain all the components
        String[] components = path.split("/");
        
        // Start "curr" from the root node.
        TrieNode cur = root;
        
        // Iterate over all the components.
        for (int i = 1; i < components.length; i++) {
            
            String currentComponent = components[i];
            
            // For each component, we check if it exists in the current node's dictionary.
            if (cur.map.containsKey(currentComponent) == false) {
                
                // If it doesn't and it is the last node, add it to the Trie.
                if (i == components.length - 1) {
                    cur.map.put(currentComponent, new TrieNode(currentComponent));
                } else {
                    return false;
                }    
            }
            
            cur = cur.map.get(currentComponent);
        }
        
        // Value not equal to -1 means the path already exists in the trie. 
        if (cur.val != -1) {
            return false;
        }
        
        cur.val = value;
        return true;
    }
    
    public int get(String path) {
        
        // Obtain all the components
        String[] components = path.split("/");
        
        // Start "curr" from the root node.
        TrieNode cur = root;
        
        // Iterate over all the components.
        for (int i = 1; i < components.length; i++) {
            
            String currentComponent = components[i];
            
            // For each component, we check if it exists in the current node's dictionary.
            if (cur.map.containsKey(currentComponent) == false) {
                return -1;   
            }
            
            cur = cur.map.get(currentComponent);
        }
        
        return cur.val;
    }
}
```
