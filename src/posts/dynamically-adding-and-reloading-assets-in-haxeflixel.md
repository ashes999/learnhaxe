meta-id: 71f1585055cedf864adadc88e898466b515dafa4

meta-tags: HaxeFlixel
meta-publishedOn: 2016-01-20

Yes, you can dynamically load (and reload) assets at runtime in your HaxeFlixel project, courtesy of OpenFL. (Caveat: I tried all this on Neko, although it should work equally on any platform.)

- First, run your project via `lime test neko`
- Second, add (or modify) your assets to the `assets` directory.
- Finally, run `lime update neko`. This copies and re-bundles the assets, in a way that the running process can access them.

## Internal Details

When you run your lime project, it actually creates a copies of the `assets` directory in `export/<os name>/neko/bin`. Interestingly, copying your asset files here isn't enough; you still need to run `lime update neko` to make them accessible to the running game.

Also, when the assets reload, they become available to calls like `getBitmapData` et all. These calls are already (currently) used by HaxeFlixel, so you don't need to do anything more to load them.

You can see how easy it is to reload assets for a live project, while debugging! I'm sure there are many, many creative possibilities for this (like creating an interactive game editor ...)
