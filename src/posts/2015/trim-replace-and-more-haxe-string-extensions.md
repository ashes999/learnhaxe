meta-id: 65efb8f2ee7e53b22f379b4b9c71f2629bf7baf5
meta-title: Trim, Replace, and More Haxe String Extensions
meta-tags: Haxe
meta-publishedOn: 2015-11-28

Haxe includes convenient string-modification methods like `trim` and `replace`. They don't exist on the `String` class by default, but need to be included via the `StringTools` extension:

```
import StringTools;
```

Like C#, this is a static extension (syntactic sugar); you can find the full list of extension methods on the [StringTools API page](http://api.haxe.org/StringTools.html).

You can learn more about static extensions via the [Static Extensions documentation page](http://haxe.org/manual/lf-static-extension.html).
