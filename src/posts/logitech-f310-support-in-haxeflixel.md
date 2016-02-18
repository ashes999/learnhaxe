meta-id: ab541e331c5408001c04fd5dd1d140db2ee767de
meta-title: Logitech F310 Support in HaxeFlixel
meta-tags: HaxeFlixel
meta-publishedOn: 2016-01-12

I recently received a [Logitech F310 gamepad](http://gaming.logitech.com/en-us/product/f310-gamepad). Since HaxeFlixel already [has gamepad support](http://api.haxeflixel.com/flixel/input/gamepad/FlxGamepad.html) (buttons, deadzone, D-Pad, etc.), including support for [reading Logitech button input](http://api.haxeflixel.com/flixel/input/gamepad/LogitechButtonID.html), I figured it would work out-of-the-box, right?

Right?

It turns out that the `LogitechButtonID` class contains values based on the **Cordless Rumblepad 2** controller. If you look [at screenshots](https://www.google.com/search?site=&tbm=isch&source=hp&biw=1440&bih=799&q=rumblepad+2&oq=rumblepad+2&gs_l=img.3..0j0i5i30l9.662.1846.0.1959.11.10.0.1.1.0.151.963.4j5.9.0....0...1ac.1.64.img..1.10.964.x4VlaV7zemA), you'll notice the pads all have numbers on them.

That's no good -- the F310 uses `A`, `B`, `X`, and `Y` for buttons, with left and right triggers (`LB` and `RB` respectively). Fortunately, the LogitechButtonID class' comments indicate which value applise to which button; these values match up exactly with the F310.  For example:

```
static inline read only FOUR:Int = 11
Placement equivalent to 'Y' button on the Xbox 360 controller
```

That only leaves the D-Pads. The built in enum values don't work; instead, I used [the four `dpad*` properties](https://github.com/HaxeFlixel/flixel/blob/master/flixel/input/gamepad/FlxGamepad.hx#L39) defined on the controller itself; these work perfectly.

If you want to try this for yourself, it's quite easy:

1) Run `flixel create` and then select demo `34`, `GamepadTest`.

2) Replace the contents of the `GamepadIDs` class with the correct button enum values:

```
public static inline var A = LogitechButtonID.TWO;
public static inline var B = LogitechButtonID.THREE;
public static inline var X = LogitechButtonID.ONE;
public static inline var Y = LogitechButtonID.FOUR;
public static inline var LB = LogitechButtonID.FIVE;
public static inline var RB = LogitechButtonID.SIX;
public static inline var START = LogitechButtonID.TEN;
public static inline var SELECT = LogitechButtonID.NINE;
public static inline var LEFT_ANALOGUE = LogitechButtonID.LEFT_ANALOGUE;
public static inline var RIGHT_ANALOGUE = LogitechButtonID.RIGHT_ANALOGUE;
public static inline var LEFT_ANALOGUE_X = LogitechButtonID.LEFT_ANALOGUE_X;
public static inline var LEFT_ANALOGUE_Y = LogitechButtonID.LEFT_ANALOGUE_Y;
public static inline var RIGHT_ANALOGUE_X = LogitechButtonID.RIGHT_ANALOGUE_X;
public static inline var RIGHT_ANALOGUE_Y =  LogitechButtonID.RIGHT_ANALOGUE_Y;
```

3) Modify `PlayState.hx`'s `updateDpad` function; the values assigned to the four `dpad*` variables should be:

```
var dpadLeft = _gamePad.dpadLeft;
var dpadRight = _gamePad.dpadRight;
var dpadUp = _gamePad.dpadUp;
var dpadDown = _gamePad.dpadDown;
```

Run `lime test neko` and observe that all input displays as expected.
