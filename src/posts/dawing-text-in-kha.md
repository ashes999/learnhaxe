meta-id: 2021132733bbb4db085755a1bcac908c5a3f4ed7
meta-title: Drawing Text in Kha
meta-tags: Kha
meta-publishedOn: 2016-11-18

Drawing text in Kha requires a couple of steps:

- Find and add a font (TTF) into your `Assets` directory
- Load the font (along with all assets)
- Draw the font on screen via `drawString`

Here's a minimal example demonstrating these steps, assuming that you downloaded the `Inconsolata` font into `Assets/fonts/inconsolata_regular.ttf`:

```
package;

import kha.Framebuffer;
import kha.Scheduler;
import kha.System;
import kha.Image;
import kha.Scaler;
import kha.Color;
import kha.Font;
import kha.Assets;

class Unknown
{	
	private static var bgColor = Color.fromValue(0x26004d);
	private var font:Font;
	private var initialized:Bool = false;
	
	public function new()
	{
		System.notifyOnRender(render);
		Scheduler.addTimeTask(update, 0, 1 / 60);
		
		Assets.loadEverything(function()
		{
			initialized = true;
			font = Assets.fonts.Inconsolata_Regular;
		});
	}

	public function render(framebuffer:Framebuffer): Void
	{
		if (!initialized)
		{
			return;
		}

		frames += 1;

		// clear our backbuffer using graphics2
        var g = framebuffer.g2;
        g.begin(bgColor);

		g.font = font;
		g.fontSize = 24;
        g.color = Color.fromValue(0xFF0000); // red text

        g.drawString("Hello, world!", 50, 20); // Draw "Hello, World!" at (50, 20)
		
        g.end();
	}
}
```

This is an untested example, with lots of things removed (eg. back buffering). It demonstrates the main steps:

- Load the font (`Assets.loadEverything`)
- Assign the font to a variable once it's loaded (anonymous function)
- Use the font to draw to screen