# Creating stages
Stages are defined via XML files, similar to <a href="./characters/index.md">Creating characters</a>. By creating an XML file in ``./data/stages``, you officially created a stage.

The default stage XML looks something like this:
```xml
<stage zoom="0.9" name="stage" folder="stages/default/" startCamPosY="600" startCamPosX="1000">
    <sprite name="bg" x="-600" y="-200" sprite="stageback" scroll="0.9"/>
    <sprite name="stageFront" x="-600" y="600" sprite="stagefront" scroll="0.9"/>
    <girlfriend/>
    <dad/>
    <boyfriend/>
    <sprite name="stageCurtains" x="-500" y="-300" sprite="stagecurtains" scroll="1.3"/>
</stage>
```
There's a lot to pack but we will explain what's going on here.

First, to start with the "parent node", which is the <syntax lang="xml">&lt;stage&gt;</syntax> node, that is where stage options are being defined.<br>
```xml
<stage zoom="0.9" name="stage" folder="stages/default/" startCamPosY="600" startCamPosX="1000">
```
Noting options that you can set there:
- ``zoom`` which controls the default zoom of the stage.
- ``name``, name of the stage when you look for it in the Chart Editor.
- ``folder`` which changes the destination of where the sprites are taken from *(like ``images/stages/default/``)*.
- ``startCamPosX, startCamPosY`` set the destination of where the camera starts from *(before countdown)*

As for the other nodes, there's about *7 types* of them *(and more if you script them in, see <a href="./scripting/playstate-scripts/character-stage-scripts.md">Character/Stage Scripts</a>)*, we'll go through each of them.

*(note that it's important which order you put them in because they will take the same order in-game too)*

## Sprite node (<syntax lang="xml">&lt;sprite&gt;</syntax>, <syntax lang="xml">&lt;spr&gt;</syntax>, <syntax lang="xml">&lt;sparrow&gt;</syntax>)
The sprite node defines the basic sprites in a stage.

```xml
<sprite name="bg" x="-600" y="-200" sprite="stageback" scroll="0.9"/>
```
The options are:
- ``name`` which defines the internal name used for scripting.
- ``x`` and ``y`` define it's position in-game.
- ``sprite`` is the name of the sprite in the files. *(keep in mind if you set this to point to a sparrow spritesheet it will automatically make an animation of all the frames in the spritesheet)*
- ``scroll``, ``scrollx`` and ``scrolly`` determine the scrollFactor of the sprite *(how much it moves with the camera)*

Other options include:
- ``skewx`` and ``skewy`` skewes the sprite.
- ``antialiasing`` determines whether or not the sprite has antialiasing. *(true by default)*
- ``width`` and ``height`` determine the width and height of the sprite's hitbox. *(does not affect the sprite itself)*
- ``scale``, ``scalex`` and ``scaley`` are size multipliers for the sprite's width and height. *(setting it to 2 means double the size)*
- ``graphicSize``, ``graphicSizex`` and ``graphicSizey`` determine the width and height of the sprite.
- ``updateHitbox`` determines whether or not to update the sprite's hitbox.
- ``zoomFactor`` defines how much the sprite should scale with the camera's zoom. *(setting it to 0 will make it stay to the original size no matter what)*
- ``alpha`` is the "opacity" of the sprite
- ``color`` tints the sprite in the color you set it to *(example #FF0000 will tint the sprite in full red)*
- ``playOnCountdown`` tells the sprite whether or not to play it's animation during countdown or not.
- ``beatInterval, beatOffset``, honestly, I don't even know what this shit is okay.

You can also define animations by giving it a child node called <syntax lang="xml">&lt;anim&gt;</syntax>. A sprite node with an animation node looks something like this:
```xml
<sprite name="dancer1" sprite="limoDancer" type="onbeat">
    <anim name="danceLeft" anim="bg dancer sketch PINK" loop="false"/>
    <anim name="danceRight" anim="bg dancer sketch PINK" loop="false"/>
</sprite>
```
An <syntax lang="xml">&lt;anim&gt;</syntax> node can define these options:
- ``name`` defines the internal name of the animation *(used for scripting)*
- ``anim`` is the prefix for finding the animation in the spritesheet.
- ``indices`` indicates which frames to play from the previously found frames.
- ``type`` determines the type of the animation. either ``beat`` or ``loop``.
- ``fps`` is the framerate of your animation.
- ``x`` and ``y`` are the animation offset. *(offset is only applied when the animation is played)*
- ``forced`` whether or not to force animation playback while a different one is already playing.
- ``loop`` to make your animation loop.

Defining advanced properties is also possible. Like this:
```xml
<sprite name="clouds" sprite="clouds">
    <property name="moves" type="bool" value="true">
    <property name="velocity.x" type="float" value="40.0">
</sprite>
```

## Solid node (<syntax lang="xml">&lt;solid&gt;</syntax>, <syntax lang="xml">&lt;box&gt;</syntax>)
Solid nodes are sprites that is, unlike a Sprite Node, composed of only one select color. This node has less parameters since it doesn't use images. <br>
Creating one of these looks something like this:

```xml
<solid name="void" color="#FFFFFF" width="1280" height="720">
```
*(there's a difference between using <syntax lang="xml">&lt;solid&gt;</syntax> and <syntax lang="xml">&lt;box&gt;</syntax>, while <syntax lang="xml">&lt;solid&gt;</syntax> offers more performance, it might break with shaders and other stuff, so if that happens, use <syntax lang="xml">&lt;box&gt;</syntax> instead)*

Accepted parameters:
- `name` internal name.
- `color` which colors the solid/box.
- `x` and `y` determine the position in the stage.
- `width` and `height` defines the size of the solid/box.

## Character nodes (<syntax lang="xml">&lt;boyfriend&gt;</syntax>, <syntax lang="xml">&lt;bf&gt;</syntax>, <syntax lang="xml">&lt;player&gt;</syntax>, <syntax lang="xml">&lt;girlfriend&gt;</syntax>, <syntax lang="xml">&lt;gf&gt;</syntax>, <syntax lang="xml">&lt;dad&gt;</syntax>, <syntax lang="xml">&lt;opponent&gt;</syntax>, <syntax lang="xml">&lt;character&gt;</syntax>, <syntax lang="xml">&lt;char&gt;</syntax>)
Character nodes are used to position characters in the stage. A Character node looks something like this:
```xml
<boyfriend x="200" y="400"/>
```
Accepted parameters are:
- ``x`` and ``y`` for positioning.
- ``camxoffset`` and ``camyoffset`` for changing the character's camera point.
- ``skewx`` and ``skewy`` for skewing.
- ``alpha`` which determines the opacity of the character sprite.
- ``flip`` for if you want to flip the character for specifically for the stage.
- ``scroll``, ``scrollx`` and ``scrolly`` to change the scrollFactor of the character.

For the <syntax lang="xml">&lt;character&gt;</syntax> and <syntax lang="xml">&lt;char&gt;</syntax> nodes only use them if you want to change a specific character *(ex. if you want to change pico's positions)*

```xml
<character name="pico" x="400" y="200">
```

Keep in mind that where you put these nodes is important as the character will be layered depending on that.
```xml
<boyfriend/>
<sprite name="stageCurtains" x="-500" y="-300" sprite="stagecurtains" scroll="1.3"/>
```
*(this will put boyfriend behind the <code class="hljs-string">stageCurtains</code> sprite)*

## Ratings sprites node (<syntax lang="xml">&lt;ratings&gt;</syntax>, <syntax lang="xml">&lt;combo&gt;</syntax>)
This node is only used to position the rating sprites and does nothing else *(atm)*. Positioning in nodes doesn't matter here.
```xml
<ratings x="200" y="400">
```
The only parameters are ``x`` and ``y``.

## Stage Extensions (<syntax lang="xml">&lt;use-extension&gt;</syntax>, <syntax lang="xml">&lt;extension&gt;</syntax>, <syntax lang="xml">&lt;ext&gt;</syntax>)

```xml
<use-extension script="example" />
<extension script="example" />
<ext script="example" />

<ext script="example" folder="data/scripts/" isShortLived="true" importStageSprites="true" />
```

Accepted parameters:
- ``script`` which is the name of the script to load.
- ``folder`` which is the folder to load the script from. (defaults to ``data/scripts/``)
- ``isShortLived`` which determines whether or not the script is destroyed after the stage has finished loading. (defaults to ``false``)
- ``importStageSprites`` which determines whether or not to import the stage sprites into the script. (defaults to ``false``)

This allows you to load custom scripts for your stages, that allow for adding custom stage nodes, or whatever else you can think of.

Check out the <a href="./stage-extensions.md">Stage Extensions</a> page for more information.