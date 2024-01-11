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

    HashMap<String, Integer> paths;
    
    public FileSystem() {
        this.paths = new HashMap<String, Integer>();
    }
    
    public boolean createPath(String path, int value) {
        S
        // Step-1: basic path validations
        if (path.isEmpty() || (path.length() == 1 && path.equals("/")) || this.paths.containsKey(path)) {
            return false;
        }
        
        int delimIndex = path.lastIndexOf("/");
        String parent = path.substring(0, delimIndex);
        
        // Step-2: if the parent doesn't exist. Note that "/" is a valid parent.
        if (parent.length() > 1 && !this.paths.containsKey(parent)) {
            return false;
        }
        
        // Step-3: add this new path and return true.
        this.paths.put(path, value);
        return true;
    }
    
    public int get(String path) {
        return this.paths.getOrDefault(path, -1);
    }
}

/**
 * Your FileSystem object will be instantiated and called as such:
 * FileSystem obj = new FileSystem();
 * boolean param_1 = obj.createPath(path,value);
 * int param_2 = obj.get(path);
 */
```
