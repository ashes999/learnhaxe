meta-id: ef7605006e3be9b2f02e75c913ef9370c1a92941

meta-tags: Haxe
meta-publishedOn: 2016-03-22
If you google for how to deserialize some JSON into a Haxe class, you will most likely find [this API page on JSON parsing](http://haxe.org/manual/std-Json-parsing.html).

While this method allows you to deserialize JSON into a struct, it only works with a `typedef`. It won't work with an actual class (something that has methods).

The good news is that it is possible to deserialize to a class instance; the bad news is that it relies on reflection. The snippet below uses reflection to iterate over all fields in the `dynamic` instance returned by `haxe.json.parse`, and reflectively sets the values on `myInstance`:

```
var raw = haxe.Json.parse(sys.io.File.getContent(configFile));
var myInstance:WhateverClassYouWant = new WhateverClassYouWant();

var structsFields:Array<String> = Reflect.fields(raw);
var classFields:Array<String> = Type.getInstanceFields(Type.getClass(myInstance));

for (field in structsFields)
{
    if (classFields.indexOf(field) > -1)
    {
        var value:Dynamic = Reflect.field(raw, field);
        Reflect.setField(myInstance, field, value);
    }
}
```

A couple of caveats to pay attention to:

- We only set the field on `myInstance` if it exists. This is safer than trusting the JSON blindly.
- Any fields that aren't in `raw` (or in the JSON) won't be set, and will have their default values.

As it stands, this solution can work pretty well as a class deserializer. You can use a similar method to reflect and serialize a class to JSON.