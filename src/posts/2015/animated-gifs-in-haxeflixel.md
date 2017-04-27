meta-id: 8076d551e1181e1b76b8e376b849d9794daaa498
meta-title: Animated GIFs in HaxeFlixel
meta-tags: HaxeFlixel
meta-publishedOn: 2015-01-10

Why would you want to display animated GIFs in a HaxeFlixel game?

Simple: every mobile device has a maximum texture size; you can see this on the Haxe [mobile targets](http://haxeflixel.com/documentation/mobile-targets/) page. The smallest maximum is `1024×1024`.

What if you want a large animated background of that size, or of a similar size? You can’t even use a spritesheet, because it would run over the maximum texture size.

Enter animated GIFs. Since GIFs generally use diffs for pixel changes from frame to frame, they’re small, cheap, fast, and high-quality (if you stick to the 256-colour limit).

## YAGP + HaxeFlixel = GIFs

HaxeFlixel doesn’t currently natively support GIFs. Instead, I looked to third-party libraries. `haxe-gif` froze on Flash (which is a game breaker for me), and instead I ended up using [yagp](https://github.com/Yanrishatum/yagp). From their `README.md`:

> This library provides simple API to parse animated GIF images. In difference between haxe-gif library, this library successfully parses most of the Gif images (exclude too big images).

The library must be installed through `haxelib` with the GitHub repository, as it’s not on the central repository yet.

It takes a few lines to get a GIF set up:

```
// Parsing from openfl.utils.ByteArray:
var gif:Gif = GifDecoder.parseByteArray(Assets.getBytes(“gif_file.gif”));
var gif:Gif = GifDecoder.parseText(text);

// Simple player example:
var player:GifPlayer = new GifPlayer(gif);
// openfl.display.Bitmap wrapper for GifPlayer.
// GifPlayer provides only BitmapData for further displaying and must be updated manually.
var wrapper:GifPlayerWrapper = new GifPlayerWrapper(player);
addChild(wrapper);
```

I admittedly don’t understand why I need a `GifPlayer` and `GifPlayerWrapper`. The library works quite well, and performs quite well, even on mobile. Integrating with HaxeFlixel only required two other things:

1. Update `Project.xml` and add ``. This will tell `lime` to process your `gif` files as binary, so you can load them via `Assets.getBytes`.
2. Add the sprite to the stage via `FlxG.addChildBelowMouse(wrapper);`

## Caveats

Yes, it works, but how well? Here are some issues I ran into:

- Loading and displaying the GIF is quite slow (on the order of a few seconds).
- The GIF plays smoothly, but leaks memory; even if you change states, some memory leaks
- The GIF library isn’t part of HaxeFlixel, so you need to scale it up or down appropriately (`GifPlayerWrapper` has `scaleX` and `scaleY` properties you can use)

Overall, it works quite well, and I will definitely look into this later if and when I need large animations.
