meta-tags: haxe
meta-publishedOn: 2015-11-27

In Haxe, namespaces are called packages, and classes are upper-case. If you want to create a `MersenneTwister` class in a `com.foo.bar.random` package, it looks like this:

```
package com.foo.bar.random;

class MersenneTwister {
  // ...
}
```

Like Java, Haxe expects class names to start with an upper-case letter.  Also like Java, Haxe expects the directory structure and package to match. For the above example, you place `MersenneTwister.hx` in the directory `/com/foo/bar/random`.
