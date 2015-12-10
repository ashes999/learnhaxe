meta-id: 360543b0c690a888880aed50c84094f3a0c6b5a8

meta-tags: Haxe
meta-publishedOn: 2015-11-27

To check if a directory exists, you can use `sys.FileSystem.exists(path)`. This returns true if a file *or directory* exists at the specified path.

To be sure it's a directory, you can also include `sys.FileSystem.isDirectory(path)`. You can combine them together:

```
if (sys.FileSystem.exists(path) && sys.FileSystem.isDirectory(path)) {
  // path exists and is a directory
} else {
  // path doesn't exist, or is a file or something else
}
```
