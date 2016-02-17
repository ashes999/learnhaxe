meta-id: 8c77cbc26271d70cbd896e810e6dde00d911c5b7

meta-tags: Haxe
meta-publishedOn: 2016-02-17

How do you iterate over all fields of an [anonymous structure](http://haxe.org/manual/types-anonymous-structure.html) (possibly behind a `typedef`) in Haxe and print out all the values?

The [Haxe reflection docs](http://haxe.org/manual/std-reflection.html) suggest:

> The reflection API consists of two classes:
> - Reflect: A lightweight API which work best on anonymous structures, with limited support for classes.
> - Type: A more robust API for working with classes and enums.

`Reflect` seems promising. From perusing the API docs, I noticed a `fields` method. [The docs](http://api.haxe.org/Reflect.html#fields) say:

> static fields (o:Dynamic):Array<String>
Returns the fields of structure o.
>
> This method is only guaranteed to work on anonymous structures. Refer to Type.getInstanceFields for a function supporting class instances.

That looks promising. Running `Reflect.fields({ "name": "Butterfly", "version": "0.3" })` gives me the array `["name", "version"]` back.

`Reflect` also includes [a `getProperty` method](http://api.haxe.org/Reflect.html#getProperty), which returns the value of a property. Plug this into a for-loop, like so:

```
var target = { "name": "Butterfly", "version": "0.3" };
var fields = Reflect.fields(target);
for (field in fields) {
  var value = Reflect.getProperty(target, field);
  trace('${field} => ${value}');
}
```

This traces:

> name => Butterfly
> version => 0.3

Also, note that Reflect also [contains a `field` method](http://api.haxe.org/Reflect.html#field) which is syntactically similar to `getProperty` (same inputs and outputs); the difference is that `field` ignores accessors, while `getProperty` applies accessors.

Given the choice, in this specific case of anonymous structures, I would use `field` instead of `getProperty` for readability.  This:

`for (var field in Reflect.fields(target)) { Reflect.field(...) }`

Reads more sensibly than:

`for (var field in Reflect.fields(target)) { Reflect.getProperty(...) }`

If you apply this on something other than an anonymous structure, you should probably opt for `getProperty` instead.
