meta-publishedOn: 2015-12-10
meta-tags: Haxe

While reading through the [Haxe manual](http://haxe.org/manual/) (which, by the way, is a good reference, but not a good learning resource), I stumbled upon [this page about properties](http://haxe.org/manual/class-field-property-type-system-impact.html):

> The presence of properties has several consequences on the type system. Most importantly, it is necessary to understand that properties are a compile-time feature and thus **require the types to be known.** If we were to assign a class with properties to `Dynamic`, field access would **not** respect accessor methods. Likewise, access restrictions no longer apply and all access is virtually public.

(If you don't understand properties yet, go and read [this](http://haxe.org/manual/class-field-property.html) and [this](http://haxe.org/manual/class-field-property-common-combinations.html) first.) This example demonstrates how this works:

```
class Test {
    static function main() {
        var p = new Props();
        p.x = 35;
        trace('p.x is ${p.x}');

        var p2:Dynamic = new Props();
        p2.x = 35;
        trace('p2.x is ${p2.x}');
    }
}

class Props {
    public function new() { }
    public var x(get, set):Int;

    function get_x() : Int
    {
        return -1;
    }

    function set_x(x:Int) : Int
    {
        return -2;
    }
}
```

(You can also try [this example online](http://try.haxe.org/#CA2Cf).) The output is:

```
16:42:46:878   p.x is -1
16:42:46:878   p2.x is 35
```

Because the Haxe runtime knows that `p` is an instance of `Props`, it can apply the getter/setter functions. Because `p2` is typed as `Dynamic`, the runtime doesn't respect the field access. (In this case, the runtime treats the field like a regular field or public variable.)

Use `Dynamic` judiciously, and tread carefully if you use it on instances of classes that have properties. If you rely on your getter/setter to set up certain invariants about your properties, you  may find them no longer respected.
