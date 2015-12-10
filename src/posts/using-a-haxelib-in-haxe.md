meta-id: db9fdfd20a2f8ad7a6adbe9d90f323c6e230c5f1

meta-tags: Haxelib
meta-publishedOn: 2015-11-29

Haxe includes a built-in package manager, [haxelib](http://haxe.org/manual/haxelib.html). You can install, upgrade, and manage libraries [via the command-line](http://lib.haxe.org/).

Once you install a library through haxelib, you need to tell the compiler to include it when you compile. As mentioned [on this page](http://haxe.org/manual/haxelib-using-haxe.html), you specify the library via the command-line:

`haxe -lib <libName> ...`

This includes the library code. Don't forget to add the relevant `import ...` statements to include library classes (if the library uses packages).

 To include multiple libraries, just add more `-lib <foo>` arguments.
