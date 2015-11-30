meta-tags: haxe
meta-publishedOn: 2015-11-28

Haxe includes convenient string-modification methods like `trim` and `replace`. They don't exist on the `String` class by default, but need to be included via the `StringTools` extension:

```
import StringTools;
```

Like C#, this is a static extension (syntactic sugar); you can find the full list of extension methods on the [StringTools API page](http://api.haxe.org/StringTools.html).

You can learn more about static extensions via the [Static Extensions documentation page](http://haxe.org/manual/lf-static-extension.html).
