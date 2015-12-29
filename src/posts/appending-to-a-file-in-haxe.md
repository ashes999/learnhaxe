meta-id: f71a8c2503ea1f094fc155b87ac2249cde3e61f9

meta-tags: Haxe
meta-publishedOn: 2015-12-28

It's not obvious how to append to a file in Haxe. If you check the [sys.io.File API](http://api.haxe.org/sys/io/File.html), you'll find an [append method](http://api.haxe.org/sys/io/File.html#append), which looks promising, but outputs an instance of `FileOutput`.

The `FileOutput` [API](http://api.haxe.org/sys/io/FileOutput.html) looks pretty sparse and unusable, until you click on the [base-class `Output` API](http://api.haxe.org/haxe/io/Output.html). There, you see promising methods, like [`writeString`](http://api.haxe.org/haxe/io/Output.html#writeString).

To combine these together, you can append a string to a file like so (don't forget to close the output stream):

```
public function append(message:String, fileName:String) {
  var output:FileOutput = sys.io.File.append(fileName, false);
  output.writeString(message);
  output.close()
}
```
