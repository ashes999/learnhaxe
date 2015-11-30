meta-tags: Haxe
meta-publishedOn: 2015-11-28

To sort an array of strings alphabetically, you can use the `Array.Sort` method (see: [API](http://api.haxe.org/Array.html#sort)). It takes a function parameter `f(a, b)` that returns a negative integer if `a` is greater, zero if `a` and `b` are equal, and a positive integer if `b` is greater.

To sort the array, you can use this method:

```
someArray.sort(function(a:String, b:String):Int {
  a = a.toUpperCase();
  b = b.toUpperCase();

  if (a < b) {
    return -1;
  }
  else if (a > b) {
    return 1;
  } else {
    return 0;
  }
});
```

Alternatively, you can create the function `sortAlphabetically(a:String, b:String):Int` and call `someArray.sort(sortAlphabetically)`.
