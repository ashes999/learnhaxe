meta-id: cbffafe81ad944abc74d2bcaa12f367cd8487d54

meta-tags: Haxe
meta-publishedOn: 2015-11-27

To read from the command line, use the `Sys.args()` call. It returns an array of arguments:

```
var args = Sys.args();
var firstArg = args[0];
// ...
```

This works when you compile and execute your Haxe code. For example, if you compile a `Main.hx` class with neko, and then run `neko Main.n --debug trace`, you get the array `["--debug", "trace"]`.

However, it doesn't work with `--interp`. If you run `haxe -main Main --interp`, you get the array `["-main", "Main", "--interp"]`. Trying to append any more arguments will result in the error `Could not process argument <foo>`
