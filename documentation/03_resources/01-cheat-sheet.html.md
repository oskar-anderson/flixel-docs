```
title: "Cheat Sheet"
```

## [FlxSprite](http://haxeflixel.com/documentation/flxsprite) (Base)

```haxe
package;

import flixel.FlxSprite;
import flixel.FlxG;

class MySprite extends FlxSprite
{
	public function new()
	{
		super();
	}

	override public function update(elapsed:Float):Void
	{
		super.update(elapsed);
	}
}
```


## [FlxState](http://haxeflixel.com/documentation/flxstate) (Base)

```haxe
package;

import flixel.FlxState;
import flixel.FlxG;

class MyState extends FlxState
{
    override public function create():Void
    {
    	super.create();
    }

    override public function update(elapsed:Float):Void
    {
    	super.update(elapsed);
    }
}
```


## Switch [FlxState](http://haxeflixel.com/documentation/flxstate)

```haxe
FlxG.switchState(new MyState());
```

## Load [FlxSprite](http://haxeflixel.com/documentation/flxsprite)

```haxe
loadGraphic("assets/my_sprite.png");

// OR dynamically create a rect
makeGraphic(100, 100, 0xFFFFFFFF); // width, height, color (AARRGGBB hexadecimal)

// to update bounding box by default (positioned on top left corner of the sprite)
updateHitbox(); // or offset.set(10, 5); to shift bounding box 10 pixels horizontally and 5 pixels vertically
```

## FlxText

* **setFormat**(font, size, color, alignment)
* **setBorderStyle**(style, color, size)

```haxe
import flixel.text.FlxText;
import flixel.util.FlxColor;
```

```haxe
myText = new FlxText(0, 0, 500); // x, y, width
myText.text = "Hello World";
myText.setFormat("assets/font.ttf", 20, FlxColor.WHITE, CENTER);
myText.setBorderStyle(OUTLINE, FlxColor.RED, 1);
```


## FlxButton

```haxe
import flixel.ui.FlxButton;
```

```haxe
myButton = new FlxButton(0, 0, "Label", myCallback);

// Custom graphics (sprite sheet with 3 frames for normal / highlight / pressed)
myButton.loadGraphic("assets/custom.png", true, width, height);
```

```haxe
function myCallback():Void
{
}
```

* **myButton.label** is a `FlxText`, use `setFormat()` and `setBorderStyle()` to customise.


## Sound Effects and Music
With the stock `Project.xml`, simply place them in your project's `assets/music` and `assets/sounds` subfolders and they're ready to use.

Sound effects are usually in WAV format (44.1 kHz source).

Music must be in MP3 format (44.1 kHz source) for Flash, and OGG for everything else.  To support both Flash and non-Flash platforms without bundling both formats in your output, you can replace the stock `<assets>` tag in your `Project.xml` with this:

```xml
<assets path="assets" exclude="*.ogg" if="flash"/>
<assets path="assets" exclude="*.mp3" unless="flash"/>
```
Play in your code:

```haxe
// Play sound effect using AssetPaths
FlxG.sound.play(AssetPaths.mySound__wav);
// Play sound effect without AssetPaths
FlxG.sound.play("assets/sounds/mySound.wav");

// Loop music, Flash only
FlxG.sound.playMusic(AssetPaths.myMusic__mp3);
// Loop music, non-Flash only
FlxG.sound.playMusic(AssetPaths.myMusic__ogg);
// Loop music, Flash or non (getSound() adds .mp3 on Flash and .ogg otherwise)
FlxG.sound.playMusic(FlxAssets.getSound("assets/music/myMusic"));
```

## Keyboard Input

```haxe
// 'A' key
if (FlxG.keys.justPressed.A) {}
if (FlxG.keys.pressed.A) {}
if (FlxG.keys.justReleased.A) {}

// Checking multiple keys:
if (FlxG.keys.anyPressed([RIGHT, D])) {}
```

#### Keys
`ANY`

`A`...`Z`

`UP` `DOWN` `LEFT` `RIGHT`

`SPACE` `ENTER` `ESCAPE`

`ZERO` `ONE` `TWO` `THREE`...`NINE`

`F1`...`F12`
 
`ALT`
`BACKSLASH`
`BACKSPACE`
`CAPSLOCK`
`CONTROL`
`DELETE`
`HOME`
`INSERT`
`QUOTE`
`PERIOD`
`PLUS`
`MINUS`
`PAGEUP`
`PAGEDOWN`
`RBRACKET`
`GRAVEACCENT`
`TAB`
`SLASH`
`SEMICOLON`

`NUMPADZERO` `NUMPADONE` `NUMPADTWO`...`NUMPADNINE`


## [Mouse](http://haxeflixel.com/documentation/mouse) Input

```haxe
if (FlxG.mouse.pressed) {}
if (FlxG.mouse.justPressed) {}
if (FlxG.mouse.justReleased) {}
```

#### Positional Data
```haxe
// Relative to world space
FlxG.mouse.x;
FlxG.mouse.y;

// Relative to screen
FlxG.mouse.screenX;
FlxG.mouse.screenY;
```

#### Wheel (mouse scroll)
Current "delta" value of mouse wheel. If the wheel was just scrolled up, it will have a positive value. If it was just scrolled down, it will have a negative value. If it wasn't just scroll this frame, it will be 0.

```haxe
FlxG.mouse.wheel;
```

#### Up, Down, Over, Out Callbacks per Object

```haxe
var clickableSprite:FlxSprite;

// ...

// register plugin in PlayState.create()
FlxMouseEventManager.init();

// ...

// register callbacks
var pixelPerfect = false;
FlxMouseEventManager.add(clickableSprite, mousePressedCallback, mouseReleasedCallback, null, null, false, true, pixelPerfect, [FlxMouseButtonID.LEFT, FlxMouseButtonID.RIGHT]);

// ...

function mousePressedCallback(sprite:FlxSprite)
{
	if (FlxG.mouse.justPressed)
	{
		// left button was pressed
	}
	else if (FlxG.mouse.justPressedRight)
	{
		// right button was pressed
	}
}

function mouseReleasedCallback(sprite:FlxSprite)
{
}
```

## Touch Input

```haxe
for (touch in FlxG.touches.list)
{
	if (touch.justPressed) {}
	if (touch.pressed) {}
	if (touch.justReleased) {}
}
```

#### Positional Data
```haxe
// Relative to world space
touch.x;
touch.y;
        
// Relative to screen
touch.screenX;
touch.screenY;
```

* `touchPointID`: The unique ID of this touch.

* `overlaps(objectOrGroup)`: Checks for overlap between this touch and another `FlxObject` or `FlxGroup`.


## Swipes (Input)

"Swipes" from both mouse and touch input that have just been released:

```haxe
for (swipe in FlxG.swipes)
{
    // swipe.startPosition (FlxPoint)
    // swipe.endPosition (FlxPoint)
    
    // swipe.id (Int)

    // swipe.distance (Float)
    // swipe.angle (Float)
    // swipe.duration (Float)
}
```


## FlxSignal

```haxe
import flixel.util.FlxSignal;
```

```haxe
// for signals that don't need data, use FlxSignal
var signal = new FlxSignal();
// for signals that need data, use FlxTypedSignal with the correct function type
var stringSignal = new FlxTypedSignal<String->Void>();
```

Note: `FlxSignal` is nothing but a convenient shortcut for `FlxTypedSignal<Void->Void>`

```haxe
signal.add(voidCallback); // type must be Void->Void
stringSignal.add(stringCallback); // type must be String->Void
```

```haxe
function voidCallback()
{
	trace("Hello");
}

function stringCallback(text:String)
{
	trace(text);
}
```

```haxe
// this will print "Hello World"
signal.dispatch();
stringSignal.dispatch("World");
```

You can have up to 4 parameters in your signal:
```haxe
var collisionNotify = new FlxTypedSignal<FlxObject->FlxObject->Bool->Bool->Void>();
collisionNotify.add(collisionCallback);

function collisionCallback(source:FlxObject, target:FlxObject, shouldKillSource:Bool, shouldKillTarget:Bool):Void (...)
```

## FlxTimer

```haxe
import flixel.util.FlxTimer;
```

```haxe
// time (seconds), callback, loops
new FlxTimer().start(10.0, myCallback, 3);
```

```haxe
function myCallback(timer:FlxTimer):Void
{
}
```
* Setting `loops` to `0` results in an endless loop.
* `reset(?NewTime)` restarts the timer, optionally with a new duration.
* `cancel()` stops the timer and removes it from the timer manager.


## FlxRandom

```haxe
// (Int) between 0 and 10
FlxG.random.int(0, 10);

// (Float) between 0.0 and 10.0
FlxG.random.float(0.0, 10.0);

// (Bool) Chance by percent
FlxG.random.bool(50); // 50% chance to return 'true'
FlxG.random.bool(10); // 10% chance to return 'true'
```

## [FlxTween](http://haxeflixel.com/documentation/flxtween/)

[Check the demo](http://haxeflixel.com/demos/FlxTween/) to visualize all `FlxTween` types.
* **tween**(Object, Values, Duration, ?Options)

```haxe
import flixel.tweens.FlxTween;
import flixel.tweens.FlxEase;
```

```haxe
// Moves sprite to position (100, 200) in 3 seconds
FlxTween.tween(sprite, {x: 100, y: 200}, 3.0, {ease: FlxEase.quadInOut, complete: myCallback});
```

```haxe
function myCallback(tween:FlxTween):Void
{
}
```

## FlxTween Options

#### ease
```haxe
{ease: FlxEase.quadInOut}
```

#### complete
```haxe
{complete: callbackFunction}
```

```haxe
function callbackFunction(tween:FlxTween):Void
{
}
```

#### type
```haxe
{type: FlxTweenType.PINGPONG}
```
* **FlxTweenType.BACKWARD:** plays tween in reverse direction
* **FlxTweenType.LOOPING:** restarts immediately when it finishes.
* **FlxTweenType.ONESHOT:** stops and remove itself from its core container when it finishes.
* **FlxTweenType.PERSIST:** stops when it finishes.
* **FlxTweenType.PINGPONG:** plays tween hither and thither

#### loopDelay
```haxe
{loopDelay: 1.0} // 1 second
```

#### startDelay
```haxe
{startDelay: 2.0} // 2 seconds
```

## FlxEase List

[Check the demo](http://haxeflixel.com/demos/FlxTween/) to visualize all `FlxEase` types.

* `backIn`, `backInOut`, `backOut`


* `bounceIn`, `bounceInOut`, `bounceOut`


* `circIn`, `circInOut`, `circOut`


* `cubeIn`, `cubeInOut`, `cubeOut`


* `elasticIn`, `elasticInOut`, `elasticOut`


* `expoIn`, `expoInOut`, `expoOut`


* `quadIn`, `quadInOut`, `quadOut`


* `quartIn`, `quartInOut`, `quartOut`


* `quintIn`, `quintInOut`, `quintOut`


* `sineIn`, `sineInOut`, `sineOut`


## Containers ([FlxGroup](http://haxeflixel.com/documentation/flxgroup))

`FlxGroup` is a shortcut for `FlxTypedGroup<FlxBasic>`. Use `FlxTypedGroup<MyOwnClass>` if you need to access your own variables and functions when iterating over the container.

### Iteration
```haxe
for (member in myGroup)
{
	member.x += 10;
	member.mySpecificFunction(); // myGroup = FlxTypedGroup<MyOwnClass>
}
```


## Collision

```haxe
FlxG.overlap(objectOrGroup1, objectOrGroup2, myCallback);
```

```haxe
function myCallback(object1:FlxObject, object2:FlxObject):Void
{
}
```

Or use `FlxG.collide()` which calls `FlxG.overlap()` and presets the `processCallback` parameter to `FlxObject.separate()`.

### Setting World Bounds

```haxe
// collision won't work outside the bounds, and by default they are only size of one screen
FlxG.worldBounds.set(tilemap.x, tilemap.y, tilemap.width, tilemap.height);
```

### Quick check whether there was a collision

```haxe
// sets the touching flags
FlxG.collide(player, level);

if (player.isTouching(DOWN))
{
	// player stands on the ground and can jump 
}

// will reset touching flags when called
super.update(elapsed);
```

### Teleport sprite

```haxe
// after setting the sprite's new position
setPosition(10, 100);
// don't forget to update 'last' variable if you don't want overlap callbacks for objects between old and new positions of the sprite
last.set(x, y);
```

### Pixel Perfect Collision

```haxe
var overlapping = FlxG.pixelPerfectOverlap(sprite1, sprite2);
```

## Drawing Shapes

**Dynamically draw:** circle, ellipse, line, polygon, triangle, rect, round rect and rect complex.

```haxe
using flixel.util.FlxSpriteUtil;
```

Haxe docs about the `using` keyword: [haxe.org/manual/lf-static-extension.html](https://haxe.org/manual/lf-static-extension.html).

```haxe
var canvas = new FlxSprite();
canvas.makeGraphic(FlxG.width, FlxG.height, FlxColor.TRANSPARENT, true);
add(canvas);
```

The last argument of `makeGraphic()` is `Unique`, whether the graphic should be an unique instance in the graphics cache, if you create multiple graphics like this, set it to `true` to avoid conflicts.

```haxe
var lineStyle:LineStyle = {color: FlxColor.RED, thickness: 1};
var drawStyle:DrawStyle = {smoothing: true};
```

```haxe
// Circle
canvas.drawCircle(x, y, radius, color, lineStyle, drawStyle);

// Ellipse
canvas.drawEllipse(x, y, width, height, color, lineStyle, drawStyle);

// Line
canvas.drawLine(startX, startY, endX, endY, lineStyle);

// Polygon
var vertices = new Array<FlxPoint>();
vertices[0] = new FlxPoint(0, 0);
vertices[1] = new FlxPoint(100, 0);
vertices[2] = new FlxPoint(100, 300);
vertices[3] = new FlxPoint(0, 100);
canvas.drawPolygon(vertices, color, lineStyle, drawStyle);

// Triangle
canvas.drawTriangle(x, y, height, color, lineStyle, drawStyle);

// Rect
canvas.drawRect(x, y, width, height, color, lineStyle, drawStyle);

// Round Rect
canvas.drawRoundRect(x, y, width, height, ellipseWidth, ellipseHeight, color, lineStyle, drawStyle);

// Rect Complex
canvas.drawRoundRectComplex(x, y, width, height, topLeftRadius, topRightRadius, bottomLeftRadius, bottomRightRadius, color, lineStyle, drawStyle);
```

Use `canvas.fill(FlxColor.TRANSPARENT);` to clear the canvas.


## HUD

```haxe
// prevents the sprite to scroll with the camera
scrollFactor.set(0, 0);
```

### Zooming game while leaving HUD intact

```haxe
// create a new camera for the HUD
uiCamera = new FlxCamera(0, 0, screenWidth, screenHeight);
uiCamera.bgColor = FlxColor.TRANSPARENT;

// add camera to list and set 'DefaultDrawTarget' to false
FlxG.cameras.add(uiCamera, false);

// add element to the camera
hudElement.cameras = [uiCamera];

FlxG.camera.bgColor = 0xff626a71;
FlxG.camera.zoom = 0.5; // zoom only on the default camera
```


## Debugger

Press `~ key` to open it during runtime, or open by code with `FlxG.debugger.visible = true`.

```haxe
// Log
FlxG.log.add("My var: " + myVar);
// or
FlxG.log.redirectTraces = true;
trace("My var: ", myVar);

// Watch
FlxG.watch.add(object, "property");

// Add world space mouse position to watch list
FlxG.watch.addMouse();

// Create a tracker window for player (of class Player) showing "x", "y" and custom fields "jumping" and "ladder"
FlxG.debugger.addTrackerProfile(new TrackerProfile(Player, ["x", "y", "jumping", "ladder"], []));
FlxG.debugger.track(player, "Hero");
```

## Hiding Cursor

```haxe
FlxG.mouse.visible = false;
```

## Adding Gravity

```haxe
acceleration.y = 600;
```

## Sort objects in FlxGroup

```haxe
// sort by Y for top-down game
group.sort(FlxSort.byY, FlxSort.ASCENDING);

// sort with custom function (here: by Z)
var group = new FlxTypedGroup<ZSprite>();
group.sort(
	function(order:Int, sprite1:ZSprite, sprite2:ZSprite):Int
	{
		return FlxSort.byValues(order, sprite1.z, sprite2.z);
	},
	FlxSort.ASCENDING
);
```

## FlxPoint Pool

```haxe
// get from FlxPoint pool
var tileSize = FlxPoint.get(16, 16);

var actionTileset = FlxTileFrames.fromGraphic(FlxG.bitmap.add("assets/images/ui/actions.png"), tileSize);

// release it back in pool to reuse
tileSize.put();
```
