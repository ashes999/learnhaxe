meta-id: 6d24e2594ba5f3031e60ff2286bb66b459b5719e

meta-title: Testing Private Code with @:allow and @:access
meta-tags: Haxe
meta-publishedOn: 2016-03-28

When writing unit tests, you sometimes face the choice of changing class/method/property access to `public` instead of `private` to facilitate testing. Sometimes, it can feel like a terrible decision to break the public interface of a class *just* to make it easier to test.

Fortunately, you don't have to do that. Haxe allows you to annotate classes and methods with two access modifiers:

- `@:allow(package.and.class.Name)`: Allow the class `Name` in the namespace `package.and.class` to access this class' private methods and properties.
- `@:access(package.and.class.Name)`: allow this method/class to access `package.and.class.Name`'s private methods and properties. Useful when you can't modify the class you want to test, or it's impractical to give access to all the test classes that need it.

(You can read more about them on the [Access Control documentation page](http://haxe.org/manual/lf-access-control.html).)

A word of caution: this borders on white-box testing (i.e. verifying the implementation works in a specific way) -- which creates fragile tests that require frequent change. I suggest, as much as possible, *not to rely on private code for testing*.

Instead, rely on some observable, public properties or methods that allow you to *indirectly*. For example, if you have a `Player` class in an RPG, you probably can't test the `levelUp` method directly; instead, you can grant your player lots of experience, and observe that it eventually does level up.
