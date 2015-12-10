meta-id: 62b21fe546dfcfa10f24252bf75d127d8f8180d2

meta-tags: Haxe
meta-publishedOn: 2015-02-26

[Prototyping a pluggable-architecture](http:/ashes999.github.io/learnhaxe/neko-plugin-architecture-with-reflection.html) led me to the path of building my games against the `neko` target, and no other. That's manageable. The next question is, **how can I release a single zip that contains my game**, *without requiring users to install Neko themselves*?

# On Windows

On Windows, Neko does a great job of linking to libraries in the current directory. When I build my Haxeflixel game against the `neko` target on Windows, copy it to a clean VM, and run it, Neko complains about these three files missing (in order):

- `std.ndll` (Haxe standard library)
- `regexp.ndll` (Haxe regular expressions library)
- `zlib.dll` (ZLib)

Copying them over gives me **a working, stand-alone game!** The rest of my code builds as a standard HaxeFlixel game.

# On Linux

Neko is less forgiving on Linux. Neko on Linux doesn't search the current path for the Neko DLLs, despite `$loader_path` including `./` and `@executable_path`. Fortunately, Ibilon [pointed me to an easy work-around](http://community.openfl.org/t/stand-alone-neko-executables-with-neko/770/5):

- Create a `run.sh` executable script
- In your script, set `LD_LIBRARY_PATH` to the current directory and call your executable

Here's what mine looks like:

```
#!/bin/sh
export LD_LIBRARY_PATH=$( pwd )
./GameExecutable
```

Like Windows, you need to include the `std.ndll`, `regexp.ndll`, and `zlib.dll` files with your executable. This gives you **a working, stand-alone game!** And yes, you're telling Neko where to look for the Neko libraries (the current directory). That's okay.

Eventually, perhaps a new version of Neko will emerge, one that looks in the current directory for the libraries. Until then, this is good enough for me.
