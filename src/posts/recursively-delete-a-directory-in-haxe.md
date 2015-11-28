Haxe doesn't include any built-in APIs to delete a directory recursively (including all subdirectories and files). Although the `sys.FileSystem` class [includes a deleteDirectory method](http://api.haxe.org/sys/FileSystem.html#deleteDirectory), it throws an exception if the directory contains subdirectories or files.

This code recursively deletes directories and their contents:

```
private function deleteDirRecursively(path:String) : Void
{
  if (sys.FileSystem.exists(path) && sys.FileSystem.isDirectory(path))
  {
    var entries = sys.FileSystem.readDirectory(path);
    for (entry in entries) {
      if (sys.FileSystem.isDirectory(path + '/' + entry)) {
        deleteDirRecursively(path + '/' + entry);
        sys.FileSystem.deleteDirectory(path + '/' + entry);
      } else {
        sys.FileSystem.deleteFile(path + '/' + entry);
      }
    }
  }
}
```

It works by deleting all files in a directory, and by recursively deleting subdirectories and removing them if they have contents.
