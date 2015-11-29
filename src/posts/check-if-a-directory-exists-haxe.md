Tags: haxe

To check if a directory exists, you can use `sys.FileSystem.exists(path)`. This returns true if a file *or directory* exists at the specified path.

To be sure it's a directory, you can also include `sys.FileSystem.isDirectory(path)`. You can combine them together:

```
if (sys.FileSystem.exists(path) && sys.FileSystem.isDirectory(path)) {
  // path exists and is a directory
} else {
  // path doesn't exist, or is a file or something else
}
```
