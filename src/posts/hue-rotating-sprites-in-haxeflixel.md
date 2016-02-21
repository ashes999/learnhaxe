meta-id: 73110e5375b496f64ac189281abe7cbfa1660ff6
meta-title: Hue Rotating Sprites in HaxeFlixel
meta-tags: HaxeFlixel
meta-publishedOn: 2016-02-21

I began a quest to answer a question: can I hue rotate sprites, within HaxeFlixel itself?  The answer is *yes*, although it requires a non-trivial amount of work.

The first clue is that the HaxeFlixel `FlxSprite` class contains a `pixels` field, which contains `BitmapData`; a list of actual pixel values (colours in the format `0xRRGGBB`) which you can access (and manipulate). My first try included changing these via [this formula](http://stackoverflow.com/a/8509802/210780); the results looked strange.

I abandoned that route and decided to use [this method](http://stackoverflow.com/a/8510751/210780), which uses a matrix to multiply the RGB component values based on the hue rotation.

`BitmapData` has an [applyFilter method](http://api.haxeflixel.com/flash/display/BitmapData.html#applyFilter). For the final `filter` parameter, which lacks documentation, you can pass in an instance of `ColorMatrixFilter`.

`ColorMatrixFilter` doesn't appear in the HaxeFlixel docs (it's [from Flash](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/filters/ColorMatrixFilter.html)). You pass in the values for a 5x4 matrix; the first three columns of the first three rows take in RGB values.

I translated and passed in the values provided in the Stack Overflow answer above; it looks like this:

```
var hueRotation:Int = 0; // degrees. Tested with values from 0...359
// cosA and sinA are in radians
var cosA:Float = Math.cos(hueRotation * Math.PI / 180);
var sinA:Float = Math.sin(hueRotation * Math.PI / 180);

sprite.pixels.applyFilter(sprite.pixels, sprite.pixels.rect, new Point(), new ColorMatrixFilter(
  [cosA + (1.0 - cosA) / 3.0, 1.0/3.0 * (1.0 - cosA) - Math.sqrt(1.0/3.0) * sinA, 1.0/3.0 * (1.0 - cosA) + Math.sqrt(1.0/3.0) * sinA, 0, 0,
  1.0/3.0 * (1.0 - cosA) + Math.sqrt(1.0/3.0) * sinA, cosA + 1.0/3.0*(1.0 - cosA), 1.0/3.0 * (1.0 - cosA) - Math.sqrt(1.0/3.0) * sinA, 0, 0,
  1.0/3.0 * (1.0 - cosA) - Math.sqrt(1.0/3.0) * sinA, 1.0/3.0 * (1.0 - cosA) + Math.sqrt(1.0/3.0) * sinA, cosA + 1.0/3.0 * (1.0 - cosA), 0, 0,
  0, 0, 0, 1, 0])); // identity row
```

The output worked, except for three things:

- I used several copies of the same image, and every copy looked the same. It turns out that HaxeFlixel shares `BitmapData` across instances of the same sprite; I bypassed this by setting `sprite.pixels = sprite.pixels.clone()`.
- The effect didn't apply on Flash. For Flash, you need to add `sprite.dirty = true` (possibly in a `#if flash` ... `#end` block).
- The colours are slightly off. If you rotate a simple block of colours (red, green, blue, and white), you see that the colours appear a bit darker than they should be (compare it to what you get in GIMP).

The result looks great.  And the performance? In my case, I intended to hue-rotate sprites once (and keep them at that hue forever). Manipulating the sprite's `.pixels` directly is the same as loadimg a graphic that already has the transformation -- there's no additional cost in each frame if we only do this once when the sprite is created. (Thanks to [Gama11](https://github.com/gama11) for pointing this out.)

For the time being, you can view the complete HaxeFlixel proof-of-concept project [here, on GitHub](https://github.com/ashes999/haxeflixel-hue-rotation).
