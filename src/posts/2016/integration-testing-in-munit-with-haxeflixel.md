meta-id: 73e442436a0dbe9c5e3b943dd860f517f95b8a1d

meta-title: Integration Testing in MUnit with HaxeFlixel
meta-tags: HaxeFlixel
meta-publishedOn: 2016-03-28

How can you test your HaxeFlixel game code? Testing games is tricky, because it's often hard to circumvent/simulate input (keyboard, mouse, etc.), and view code is often tightly coupled to logic.

HaxeFlixel's demos (probably inadvertantly) encourage a simple, direct style of development where you throw most of your code into states. This makes it difficult to test.

### Testing Strategies

I suggest you use two strategies for testing:

1) As much as possible, **write your logic in simple Haxe classes.** Keep as much logic as you can out of `FlxState` classes, and try not to extend the Flixel classes.  This makes it difficult to handle things like "create a `Player` class that responds to keyboard input." To do this, you probably need an abstraction (method/event) in the `Player` class that responds to input, and some code in an `InputSystem` class or the `FlxState` that broadcasts input when it receives it from HaxeFlixel. Which brings me to the second strategy.

2) **Test whatever is easy to test in your `FlxState` classes.** You almost certainly cannot test everything (and have to go through great lengths to test everything). Sometimes, the cost and complexity doesn't justify the fragile tests. Test whatever you can reasonably test.

### Testing with HaxeFlixel

Since unit testing simple Haxe classes with `munit` is easy, I documented the steps you need to set up integration testing.  The key is to realize that **instead of using MUnit, you'll use lime as the test runner.**

Why? Because, as Gama11 mentioned [here](https://groups.google.com/d/msg/haxeflixel/ow87nKG3t80/eKI4KFDtAAAJ), HaxeFlixel relies on `lime` to build projects. Lime performs additional processing, like including libraries from `include.xml` files.

1) **Delete your `test.hxml` file.** You won't need it. 

2)  **Add a `project.xml` file to your `test` folder.** You can use this as a starter.

```
<?xml version="1.0" encoding="utf-8"?>
<project>
	<meta title="UnitTests" version="1.0.0" />
	<app file="TestMain" main="TestMain" />
	
	<set name="SWF_VERSION" value="11.8" />
	<source path="../src" />
	
	<set name="no-custom-backend" />
	<set name="unit-test" />
	
	<haxelib name="munit" />
	<haxelib name="flixel" />
	<haxelib name="hamcrest" />
	
	<haxedef name="FLX_UNIT_TEST" />
</project>
```

This transforms your `test` directory into a stand-alone HaxeFlixel project that you can build with `lime`.

3) **Modify `TestMain.hx`.** (If you don't have a copy, run `haxelib run munit test` to generate one.) Add two lines as the first two lines in the constructor:

```
public function new()
{
   // Flixel was not designed for unit testing so we can only have one instance for now.
   Lib.current.stage.addChild(new FlxGame(800, 600, null, 1, 60, 60, true));
   // ...
}
```

(The comment comes from the same line in the HaxeFlixel code base.)

That's it! Now you can write code instantiating and using `FlxState` instances like normal, and testing conditions.

To run the tests, just run `lime test <platform>` from the `test` directory. (Neko works out of the box; Flash requires some additional trickery to send the results to the MUnit test runner.)