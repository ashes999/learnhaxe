To read from the command line, use the `Sys.args()` call. It returns an array of arguments:

```
var args = Sys.args();
var firstArg = args[0];
// ...
```

eg. `neko Main.n --debug trace` returns the array `["--debug", "trace"]`.

If you use `haxe blah.hx --interp`, you get the array `["blah.hx", "--interp"]`
