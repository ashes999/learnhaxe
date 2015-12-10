meta-id: e6e6fad5afbacb434fa773adec7cb0211ae59b53

meta-tags: HaxeFlixel
meta-publishedOn: 2015-04-09

`FlxButton` is a hybrid of an image and a text object. You can set both properties separately:

```
var button = new FlxButton(0, 0, "Click Me!", myCallback);
// Load a custom image
button.loadGraphic("assets/images/blue-button.png");
button.label.setFormat("assets/fonts/awesome-font.ttf", 20, FlxColor.WHITE);
```

This code:

- Creates a new button with "Click Me!" as the caption
- Sets the background image to `assets/images/blue-button.png`
- Sets the font to a custom font, white letters, size 20

You can also create a button from a spritesheet. From [this blog post](http://coinflipstudios.com/devblog/?p=225):

```
button.loadGraphic("assets/images/button-spritesheet.png", false, 128, 128);
```

This creates a button using `button-spritesheet.png`. It uses 128x128 as the frame size; so the first 128x128 is the "normal" sprite, the next 128x128 frame is the "mouse over" sprite, and the last frame is the "clicked" sprite. You can see what that looks like [from the same blog post](http://coinflipstudios.com/devblog/?p=225) (scroll all the way to the end).
