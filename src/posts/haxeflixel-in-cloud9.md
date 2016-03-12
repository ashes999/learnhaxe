meta-id: 0515f44276a8a55ed37895e362623a3dab6d175e
meta-tags: Haxe, OpenFL, HaxeFlixel
meta-publishedOn: 2016-03-12

[Cloud9](https://c9.io/) provides remote, cloud-based development enviornments. Under the hood, they provision you a Linux VM with Docker. By default, they provide pre-build environments for Rails, C++, PHP, and other enviornments. But not Haxe.

It turns out that setting up Haxe, OpenFL, and HaxeFlixel is really simple.

- For Haxe, [download](http://haxe.org/download/) and run the Linux 64-bit binaries.
- For OpenFL, run `haxelib install openfl`.
- For HaxeFlixel, run `haxelib install flixel`. (Don't forget to install and setup `flixel-tools` too.)

That's it! You can build your Haxe apps, or your OpenFL/HaxeFlixel games in Flash. To view them:

- Find the binary in the workspace view
- Right-click and pick Preview
- Click the square/arrows icon to open in a new tab

This opens the SWF in a new window/tab where it runs properly.