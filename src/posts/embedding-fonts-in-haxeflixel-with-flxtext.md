meta-id: a20bb3116db754de2bfc7f1a1a1b80e30bf876dd
meta-title: Embedding Fonts in HaxeFlixel with FlxText
meta-tags: HaxeFlixel
meta-publishedOn: 2015-12-22

The [FlxText class](api.haxeflixel.com/flixel/text/FlxText.html) provides three ways to set the font:

- By setting the [font field](http://api.haxeflixel.com/flixel/text/FlxText.html#font) to a font name (eg. `Times New Roman`). This works with embedded fonts (TTF file is in `Assets` and included in the final binaries)
- By setting the [systemFont field](http://api.haxeflixel.com/flixel/text/FlxText.html#systemFont) to a font name. This only works with system fonts, which can be tricky if your app has to work across Windows and Linux, or across mobile and desktop. There's a small list of [web safe fonts](http://webdesign.about.com/od/fonts/qt/web-safe-fonts.htm) which you can try.
- By calling the [setFormat method](http://api.haxeflixel.com/flixel/text/FlxText.html#setFormat). You can specify lots of properties together; in particular, you can specify the path to a TTF file, eg. `setFormat("assets/MyCustomFont.ttf", ...)`

The third method strikes me as the most maintainable, because it's **immediately obvious which font you're using**, and where that font file originates from. With system fonts and embedded fonts, if the font doesn't work, it's more work to triage and isolate exactly where the problem lies (did you add the font file to `Assets`? Is it embedding properly? Did you specify the correct name? Does it work on other targets?).

The third method also has the best chance to be consistent and cross-platform compatible (by virtue of using embedded fonts), because you're not relying on whatever fonts might be included in the device's system.

[Google Fonts](https://www.google.com/fonts) provides a wealth of fonts which you can use.  You can download all of them (as TTF and OTF) and embed them in your HaxeFlixel games.
