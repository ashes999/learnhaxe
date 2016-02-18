meta-id: b19a1902c374eaec7e2c9b93dd4063f65932329f
meta-title: HaxeFlixel on OUYA
meta-tags: HaxeFlixel
meta-publishedOn: 2015-03-30

HaxeFlixel supports OUYA integration. Since OUYA uses an Android-based system, you get most of what you want for free.

Issues of concern:

- Controller support
- OUYA detection
- Screen resolution and how to scale
- Placement of UI elements

The first two present technical challenges, while the latter two are usability challenges.

### Controller Support

HaxeFlixel includes OUYA support, including analog stick support. Some code:

```
import flixel.input.gamepad.OUYAButtonID;

class SomeState extends FlxState {
  override public function update() : Void {
    var gamePad = FlxG.gamepads.lastActive;
    var gamePadX:Float = 0;
    var gamePadY:Float = 0;
    if (gamePad == null)
    {
      #if (OUYA)
        // TODO: freeze/notify that the gamepad is off.
        return;
      #end
    } else {
      // Caveat: these will change to LEFT_ANALOG_STICK _X/_Y in the latest version
      var xAxisValue = gamePad.getXAxis(OUYAButtonID.LEFT_ANALOGUE_X);
      var yAxisValue = gamePad.getYAxis(OUYAButtonID.LEFT_ANALOGUE_Y);
    }
  }
}
```

This code demonstrates getting input from the latest (only) controller. It does assume only one controller. It uses the left analog stick to get the directions (`getXAxis`/`getYAxis` return a float from `-1` to `+1`).

As far as D-pad support, `FlxG.keys.anyPressed(["UP"])` (and `DOWN`, `LEFT`, and `RIGHT`) automatically take into account the D-pad. You don't need to write any additional code.

### OUYA Detection

How can you tell if you're running on an OUYA, as opposed to a generic Android device? I don't know yet. If you figure something out, please comment and let me know.

You can't use controllers, because a user can unplug their controller at any time.

### Screen Resolution

OUYA runs one of three resolutions: `1920x1080`, `1280x720`, and, surprisingly, `640x480` -- the latter if your device doesn't support HD resolutions natively (eg. you're using an old monitor and a flaky HDMI-to-DVI converter).

If you want crisp, clean images (without aliasing/jaggies), *and* you don't support other Android device resolutions, I suggest a resolution matching the OUYA ones, or an integer-scaled version (eg. `960x540` scales up to 2x on the OUYA).

### Placement of UI Elements

Due to the nature of LCD screens and the OUYA, you often get overdraw (the edges of your game area aren't visible). To compensate for this, you need to:

- Account for 5-10% of your screen being unusable, and locate the most important UI elements inside that area. Don't let the player character get too close to the edge!
- Try to keep your UI elements as centered as possible

That's all you should need to create an OUYA game.
