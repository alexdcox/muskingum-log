# Alex Devlog

## Batch One

```
Tuesday   13.09.2022  8h
Wednesday 14.09.2022  8h
Thursday  15.09.2022  4h
Friday    16.09.2022  8h
Saturday  17.09.2022 10h
Sunday    18.09.2022 10h
Monday    19.09.2022  8h
Tuesday   20.09.2022  8h
Wednesday 21.09.2022  6h
Monday    03.10.2022  8h
Tuesday   04.10.2022  1h
Total                79h
```

### Tuesday 13.09.2022 8h

Ash is working on a PoC print out of Illusion, based on a game he used to play
with Rich Cyber Wars on duelboard.

Chaos Rebord
Scrolls by Mojang

In Ash's version, you have a "Summoner" piece which you need to protect.

We messed about with some printed cards on a square grid on a whiteboard with bits of
red cardboard for damage counters.
Even the unbalanced thrown-together first iteration was pretty fun.
Seems like uni-directional combat is more thought provoking.
We've got a list of other things to experiment with.

I started reading up on hex grids and worked through the Red Blob Games write-up.
I currently have a fixed-size hex grid drawn using Vue and standard html svg components.
I thought eldudeio would be using d3 but it seems raw js/html + framework is good enough now.
I've managed to build my own grid using his library from scratch, along with labels and mouseover highlight.

For tomorrow I'd like to:
- get the grid we need drawn properly with dynamic sizing
- along with mouseover cube/doubled coords
- be able to draw card art and stats onto tiles
- start on a js game server

### Wednesday 14.09.2022 8h

So, getting the grid going...
We want 11x7 like seafarers.

ZeroTier network is: `6ab565387aba6994`
Need to get ash to join it at some point..

Trying to draw a cat to one of the hex tiles.

- multiple pages in vue

Well I managed to get images drawn to the tile grid, and am able to set single
hex tiles with unit details. Real basic so far.

I've got a js server running with auto-reload, and the game client running with hot-reload.

Made a start on the server as planned.
Will just write it really simple to start, basic json messages.
Just want to get v1 running ASAP.

### Thursday 15.09.2022 4h

Moving all these sporadic projects under `js` and renamed nonsense dirs to `server`, `client` and `engine`.

Switching client to use Typescript.

### Friday 16.09.2022 8h

Created the game on figma with a tile grid and the graphics from Battleforge,
using the stats Ash came up with.

After a few playthroughs it was obvious the unit speed was far too much for the
grid. Created a little program to read the json and print out stat curves.
Moved outliers with crazy movement speed back to max 3 and tweaked a few other
cards to make more sense.

Here were some notes:
**Clearly Broken**
- Movement speed
It's a small map, the avg piece should be probably 1.3 or something
Most cards 1
The dangerous fast cards maybe 3 or 4 at a push
probably 3
Perhaps there can be 1 card that is a 3 movement killer assasin which is a scary card for your oponent to spawn
(maybe not 1-shot on the commander, but perhaps 2/3 health)

**To Try**

- [x] discard for energy (round down)
  It just makes the high cards redundant and it's a shame because you kind of want to get to them
  J suggests actually kind of reducing the deck size which means these high cost cards come up less

- [ ] discard for energy (round up)

- [ ] start with energy


- [ ] map with more engery sources

- [ ] smaller draw pile / deck size

- [ ] change order of summon
  it's kind of annoying to have to summon before moving as a summoner
  might be more fun to move first, or whatever order you like
  even attacking
  it be fun to be able to attack then move back
  like each unit has 3 energy to spend: move, attack, (summon), do what you like

### Saturday 17.09.2022 10h

Going to try and actually make it playable today.

Still struggling hard with lerna + typescript.
Not sure if I need to use npm local file syntax or the syntax in the learna doc:
```
{
  "dependencies": {
    "engine": "../engine"
  }
}
```
VS
```
{
  "dependencies": {
    "engine": "*"
  }
}
```

Getting this error on `server`?
`SyntaxError: Cannot use import statement outside a module`
Set `"type": "module"` in `package.json`

Now getting:
`Named export 'Units' not found. The requested module 'engine' is a CommonJS module, which may not support all module.exports as named exports`

`TypeError [ERR_IMPORT_ASSERTION_TYPE_MISSING]: Module "file:///Users/adc/Documents/untitled-game/js/packages/engine/lib/units.json" needs an import assertion of type "json"
`

`TS2821: Import assertions are only supported when the '--module' option is set to 'esnext' or 'nodenext'.`

Managed to fight through a lot of the TS errors and inclusion problems.
Now I have the grid drawing all the details needed to see the game, without any choice syntantic sugar like:
- how far can this piece move if I select it
- how do I deselect this piece if I chose to move it

I'm not quite sure of the math, but I trail-and-errored my way to a symmetrical draw grid either side of the main board.
Now the hand is visible, and the entire deck each side, just as me and J were playing it messing about on figma.
The only remaining graphics needing to be coded is the amount of energy each person has.
Once that's done (and hopefully that will be easy) I can start programming player states.
Such as:
- board unit selected to either move/attack
- hand unit selected to summon


### Sunday 18.09.2022 10h

In building the grid into a component and having a hand and draw area, I've somehow lost the svg icons.
They seem to be somehow related to the parent svg.
After that I just need a few FE things:
- [x] Fix svg icons
- [ ] Fix hover state highlight
- [ ] Enter name
- [ ] Moving a piece (and cancelling)
- [ ] Summoning a piece (and cancelling)

Then it's time for the websocket and server shit!
Maybe I'll have a basic text version to work on the server.
Iron out the messages I need to send.

It's playable!
Took me all day until 2am to get there.

### Monday 19.09.2022 8h

Wrestling with svg.

For some reason, the sword/heart/shoe icons random in different places depending on the parent svg `viewBox`.
This means I cant make the hex a reusable component: `HexTile`.

It took me all day fucking around to realise it's because of aspect ratios being preserved by default.
So it tries to match the parent width/height and center by default.
Setting that to none positions it according to the defined width/height with the anchor at the top left of the image.

FINALLY

### Tuesday 20.09.2022 8h

Starting with css animations. Just the background tile fade. Something simple.

I REALLY need to keep a log of hours worked. I'm going a bit too hard atm. Going
to do another 3h and call it a day for today.

Already ignored my own advice.

Been trying to add animations all day and was fighting the dom constantly.
Decided to go the canvas route.

After a bit of research it seems webgl and wasm (web assembley) would be
slightly harder but much more performant. It's the route Figma took, and the
hex game zemeroth.

I'm going to learn how they did it...

### Wednesday 21.09.2022 6h

So even drawing fonts on the canvas is tricky.
They just come out looking so shit.

I'll try webgl.
Nope.

It seems even drawing fonts is a master-thesis level topic. Chris Green, a
former value employee who worked on the Source engine, developed a technique he
calls in his whitepaper: "Improved Alpha-Tested Magnification for Vector
Textures and Special Effects".

https://github.com/Michaelangel007/game_dev_pdfs/blob/master/graphics/signed_distance_field/SIGGRAPH2007_AlphaTestedMagnification.pdf

It's fair to say I'm a little out of my depth already:
- Alpha-blending
- Alpha-testing
- alpha-thresholding
- bilinear filtering
- distance field
- custom shader
- texture map
- quadtree
- octree
- interpolation
- texel sample
- implpicit cubic curve
- vector texture
- Voroni Region
- Pixel shader
- subtexel
- bilinear texture interpolation
- piecewise-linear

Even after reading through and understanding as much as possible, there isn't an implementation guide.

According to [this SO post](overhttps://stackoverflow.com/questions/25956272/better-quality-text-in-webgl) Chlumsky Viktor's [master thesis](https://dspace.cvut.cz/bitstream/handle/10467/62770/F8-DP-2015-Chlumsky-Viktor-thesis.pdf) covers an open-source method.


### Wednesday 28.09.2022

So I'm trying to use opengl/webgl to render the grid today.
That's first up...
It really doesn't seem worth it.
Just to make a line spin requires a ridiculous amount of code.
If I'm going to actually get anywhere, might be a better idea to just learn Unity.

### Monday 03.10.2022 8h

I'm going to have to learn Unity to make this work.
Here goes...

https://catlikecoding.com/unity/tutorials/basics/game-objects-and-scripts/

A day of mostly research.

[Daniel Mullins - Inscryption Game Design](https://www.youtube.com/watch?v=DOZZSVjcn_c)
[Hearthstone - Parallax Effect in Unity](https://www.youtube.com/watch?v=1_pBJK2vNQw)
[Dicko - Card Game Animations using DoTween & Mirror](https://www.youtube.com/watch?v=i0omT5dG6w4)
[Dicko - Grid](https://www.youtube.com/watch?v=OJtHek_sRpo)
[Dicko - Multiplayer](https://www.youtube.com/watch?v=E70cymHuI90)
[Brackeys - 2D Glow](https://www.youtube.com/watch?v=WiDVoj5VQ4c)
[Brackeys - Card Scriptable Object](https://www.youtube.com/watch?v=aPXvoWVabPY)
[GothicVania Church Pack Free 2D Pixel Art](https://ansimuz.itch.io/gothicvania-church-pack)

[Brackeys - How to make a 2D Game in Unity](https://www.youtube.com/watch?v=on9nwbZngyw)

[How to make Simple 2D Glow with Outline in Unity - 2D Shader Graph](https://www.youtube.com/watch?v=eSHM45ogT7g)

This is the closest one to the MTGA style glow I've found so far:
[How to make Unity GLOW!](https://www.youtube.com/watch?v=bkPe1hxOmbI)

[Game VFX Tutorial - Arrow with FX](https://www.youtube.com/watch?v=r96T5QPzw9Q)

This is the kind of VFX artist we need:
https://www.youtube.com/c/HovlStudio/videos

[Brackeys - Basics of Shader Graph](https://www.youtube.com/watch?v=Ar9eIn4z6XE)

URP
Universal Render Pipeline

### Tuesday 04.10.2022 1h

[Brackeys - Shader Graph Force Field](https://www.youtube.com/watch?v=NiOGWZXBg4Y)

[Brackeys - 2D Outline](https://www.youtube.com/watch?v=MqpyXhBIRSw)

## Batch Two

```
10.10.2022 Monday      4h
11.10.2022 Tuesday     8h
12.10.2022 Wednesday   6h
13.10.2022 Thursday    6h
14.10.2022 Friday      6h
17.10.2022 Monday      6h
18.10.2022 Tuesday     1h
26.10.2022 Wednesday   4h 30m
27.10.2022 Thursday    6h
28.10.2022 Friday      6h
01.11.2022 Tuesday     6h
02.11.2022 Wednesday   6h
04.11.2022 Friday      6h
09.11.2022 Wednesday   6h
10.11.2022 Thursday    7h
11.11.2022 Friday      6h
14.11.2022 Monday      6h
16.11.2022 Wednesday   6h
18.11.2022 Friday      8h
Total                110h 30m
```

### 10.10.2022 Monday 4h

Need to put the game repo online.  
Have I not done that already?  
https://github.com/alexdcox/muskingum  
Yep.  
Perhaps I need this log up too.  
Okay done. Went with the random wiki page muskingum for the codename for now.  
https://github.com/alexdcox/muskingum-log
It sounds thrilling doesn't it?!  

Diving into the Catlike procedural grid:
https://catlikecoding.com/unity/tutorials/procedural-grid/

Might be worth going through this:
https://blog.unity.com/technology/isometric-2d-environments-with-tilemap

More unknown terms:
- degenerate triangle
- normals

Counter-clockwise trianges are never drawn.

Now onto the Hex grid tutorial.

I'm feeling my lack of mathmatic ability even with this basic equation:
![](/uploads/10314c71-dc0b-4cd2-9ff4-4375783c6f36.png)

### 11.10.2022 Tuesday 8h

It's going to be a long one. Have 8 hours to do today.

Starting with shader graph. Wanting to make something suitable for a mouseover and selected effect.

Here's a little unity tip note thingy that I may start to use more:
<div class="unity-note">
    The Lightweight Render Pipeline (LRP) has been superseded by the Universal Render Pipeline (URP). See the <a href="https://docs.unity.cn/Packages/com.unity.render-pipelines.universal@11.0/manual/upgrade-lwrp-to-urp.html#:~:text=Open%20your%20Project%20in%20Unity,Lightweight%20RP%20and%20select%20it.">upgrade guide</a> for migration steps.
</div>

Installed A. Learning how to export 3D objects and import into unity.
Using their 3d primitive Suzanne, as does Brackeys.
`Shift+A→Mesh→Monkey`
`X` is delete.

<div class="unity-note">
    Hold right click to enter flythrough mode, then WASD QE and mouse move to navigate.
</div>

https://devassets.com/browse/
https://brackeys.com/

<div class="unity-note">
    PBR (Physically Based Rendering) Graph has been superseded by 
</div>

Right! We have an egg waffle:

![](/uploads/b96bad9e-3dc8-415f-b0f6-9a136c8240b1.png)

Doesn't it look delicious? This is being rendered using the hex grid and a tutorial tile set.

Covered a lot today.
Still getting used to the editor.
Blender.
Exporting 3D assets to Unity.
Tilesets.
Shader Graphs.
Sprite animation.

I think the best bet moving forwards is to essentially do everything with images and use sprite animation, or the shader graph.

Consider a stroke animation on a hex square such as (with css)
`border: 1px solid black` to `border: 3px solid red`, producing a glowing red thicker line and back to a thinner solid black line (horrrible style choices but they illustrate the idea) setup with keyframe transitions so it eases in and out repeatedly. I don't know how to actually program that with unity. But now I know ways to achieve that using the editor.

Writing custom shaders is beyond my day 2 unity ability, but it looks like the powerful shader graph can provide what we need.

I dare say a combination of those two approaches will suffice.

### 12.10.2022 Wednesday 6h

Going to just dive straight into a new project and see if I can replicate what we had in html with unity using what I've learnt so far.

Looking back at the figma boards, I have no idea how I created the hex svg used.

First thing I'm trying to do is, get the tile at 0,0,0 and create an empty game object in that position.

`public Sprite spriteA;`

> Tile(s) are only meant for rendering & physics, not data

64x55
64x55

55x64

103/116

https://docs.unity3d.com/Manual/Tilemap-Hexagonal.html
I just want to be able to create the xyz coords as in the link above.

default x 0.8659766
defauly y 1

120

If I change it to 0.86 I can do 86x100 tiles.
174x200 is really close to the figma tiles, I'll go with that.

Looks blury, not nice.

There is a note about width/height needing to be a multiple of 4.
256

Getting gaps, both resolutions suck.
Do I just need big sprite assets?
https://www.youtube.com/watch?v=QW53YIjhQsA

One guy suggests using a hex calculator online, and using transparency in the png so it exactly matches 100x100.

Ahh my hex math is wrong.
Looking at this website:
https://hexagoncalculator.apphb.com/

A width of 64px requires a height of 55.4256px.
Which is why there are vertical gaps in my 174x200 image but the horizontal tiles nicely.

According to this SO post:
https://gamedev.stackexchange.com/questions/86098/are-there-any-hex-tile-sizes-where-both-width-and-height-are-integers

100x173 would be suitable.

Having a lovely time yak shaving.
One solution would be to introduce a space between tiles on the grid.

I unchecked "Low Resolution Aspect Ratios" and it sharpened up a lot:
![](/uploads/7ffaa842-6868-4fdb-a886-409365fc7126.png)

Okay that looks good enough for now. I'll use 173x200.

57x66 is also good if we want to go more towards pixel art sizing, perhaps with spacing. Might be the better plan here.

STOP. Need to keep focused. Just trying to re-create the html game at the moment. Stay on track.

Converting unit images from webp to jpg using imagemagick:
`brew install imagemagick`
`magick image.webp image.jpg`

To add a unity package not on the list:
Package Manager > Plus Icon > Add From Github > com.unity.vectorgraphics

I'm at a point where I'm making the unit prefab, and I'm trying to render the image. So. How. The fuck. Do I do that? I'm thinking perhaps I need a hex shaped gameobject that I can just render the image on.

I've already programatically created those as part of the catlike tutorial. Maybe I can create a unity script to generate me one and have it persist after playing. Or, have it run as a oneoff script in the editor while not playing.

Oh FUCKYEA:
![](/uploads/c1aa19c0-0773-44b9-94b1-a727f007d293.png)

A custom context menu is pretty sweet and very easy.

Or you can add a script to the component dropdown:
![](/uploads/45433cd7-3cc2-43b2-897a-f60f45cae3c6.png)

You can even augment existing components with:
`[MenuItem("CONTEXT/Rigidbody/MyThingaling"]`

Looking at how to use with git/github:
https://www.youtube.com/watch?v=qpXxcvS-g3g

https://github.com/github/gitignore/blob/main/Unity.gitignore

Managed to tweak things to generate a persistent hex mesh! 💥

Finished the day trying to apply the summoner image to the mesh with stretching (it's on there, just full size):
![](/uploads/2a3370f4-9a75-47f1-a495-ebbfe1c76eec.png)

O shit, just after writing that and trying to save the scene I realised I was in play mode, now everything has gone.

### 13.10.2022 Thursday 6hrs

Right, starting with trying to get my mesh back.

Back to where I was, but it's not drawing correctly on the mesh.
I added a square, but it uses the sprite renderer.
A cube works fine.
So does a sphere.
Tried using the default URP material and an unlit mat.
I think it's either to do with tiling or the fact that the mesh was generated and doesn't have everything it should have.
How can I break this down? I'll try saving my messh.
Perhaps UV mapping is the issue.
Not getting anywhere here.
Going to try and export a hex from blender.

To move around blender, use two fingers and drag to change camera angle and with shift to pan, pinch gestures work for zoom.

Research segway into Slay the Spire:
https://www.youtube.com/watch?v=r_BPJzNPF6M

They say be as transparent as possible. Get a discord. Go into Steam open beta asap.

Slay the Spire
Inscryption
Dominion
Catan

2yrs of unity dev progression:
https://www.youtube.com/watch?v=8fcy0_FQ_OA

Don't waste too much time with graphics
Brackeys tower defence
Tutorial series: How to make dark souls in unity
Netcode RPC multiplayer
sebastian lague - terrain generation
Keep hearing that multiplayer should be a first consideration and addressed early and throughout
sebastian graves

Managed to export a hex from blender and it did the exact same thing as my humble procedural hex. UV mapping??

https://www.youtube.com/watch?v=lZHbzjFYNIQ
UV Maping
Blender Add-Ons
Node Wrangler
Shift A - Add Something
Tab - Switch Edit Mode
Ctrl T - Break Node into Subcomponents
Preferences > Input > Emulate Numberpad
Shift Fn 1 - Front Orthogonal View

Ctrl R - Edge Loop
A - select all
Ctrl A - Apply Modifier

To split windows drag from the corners
To remerge windows: wait for 7s then drag in the direction you want to close

I think there might be a bug in my version of Blender where the mouse cursor doesn't actually change to reflect what action you're doing. I had to trial-and-error until I counted 7s for the delete and at least 3s for create new (otherwise the old gets resized). Suprised there's no modifier key that makes it faster.

I've followed multiple tutorials now and have exported two UV mapped spheres earth/moon. Both fail to display properly in Unity. I've tried multiple projects to be sure it's not my 2D URP that's causing the issue.

So essentially I'm not baking the texture properly. The shader nodes need to be baked into a single image, I think.tbc.

There's so much to unpack here:
https://www.youtube.com/watch?v=x4mySebugl0
The diffusion map seem fine but the normal map looks plain. Suspiciously plain.

### 14.10.2022 Friday 6h

Watching Brackeys tutorial on melee attacks:
https://www.youtube.com/watch?v=sPiVz1k-fEs&t=424s

```
animator.SetTrigger("Attack");
Physics2D.OverlapCircleAll(position, range, layers);
OnDrawGizmos() {
  Gizmos.DrawWireSphere(pos, rad);
}
```

Inspector has a debug mode.

![](/uploads/a3eda75d-171c-4202-aed1-8fc36967c955.png)

Back to UV/shader baking/cooking whatever the hell I'm supposed to do to get a hex with an image show up...

I could use an image mask and use a square, but that's not really the point. The point is to learn how to UV map properly as this seems essential.


[YT - Daniel Bystedt - Uv mapping tutorial for Blender 2.80](https://www.youtube.com/watch?v=L3654VGZObg)
[YT - Flipped Normals - UV Mapping Explained](https://www.youtube.com/watch?v=Yx2JNbv8Kpg)
[YT - Flipped Noramls - UV Mapping for Games](https://www.youtube.com/watch?v=Xm_Ibg-bqD4)

Just bought the Flipped Normals tutorial. It's going to take a few hours to go through apparently.

[Here's the Unity Netcode intro video](https://unity.com/products/netcode)
They recommend going through the docs as well as downloading their demo games.

How to make a hex in blender recap:
```
Cmd + N > G > D
A > X > Enter
Shift + A > M > R
Open the "adjust last operation" window in the bottom left or by pressing F9
Change verticies to 6, make sure to hit return if you entered with the keyboard
Tab
F
```

To create a new UV map for that hex:
Click the `UV Editing` tab p
In the left `UV Editor` pane
Click `+ New`, name: uv, generated type: color grid
In the right `3D Viewport` pane
Tab (to enter object mode)
With the hexagon still selected in object mode
`Properties` (right pane) > `Material Properties` (tab near the bottom) > `New`
This will create a new "Material.001"
Click the yellow dot on the `Base Color` control and set to `Image Texture`
Click the image select dropdown and choose the new "uv" texture
Back in the 3D Viewport, tab to edit mode, `A > U > U`
Switch to `Material` or `Rendered` view to actually see it.
To export to unity: `F3 > "export fbx" > Enter`

Remember, if you're not in edit mode you won't see the outline in the UV Editor pane.

Bake requires "render engine"

https://www.youtube.com/watch?v=5UZ-niuRWz8

Texel Density
Consistent texel density allows artwork to maintain a high level of rendering accuracy accross a model.

[YT - The Many Worlds of Nate Purkeypile](https://www.youtube.com/watch?v=0rsu66FNUGk)
Worked for Bethesda on Skyrim, Fallout 3, 4 and 79 now doing his own Unreal 5 project Axis Unseen
He studied art for games and became a light engineer
Blender goto for 3D
Maya might be better for animation
He uses medium for basic sculpting with a VR headset and then ZBrush for the fine details like pores
9/10 shadow casters caused performance issues
ISIS
Pelican
Soulfly
KMFDM
High Lung - Primitive Music?
Houdini for procedural things such as forests
Houdini is built from the ground up to be a procedural system that empowers artists to work freely.
He mentions a Perforce Depot
Never put something in you don't want to ship i.e. "Untitled Goose Game"

-----

Voxel art vs pixel art?
With voxel art you could do some pokemon stadium style attacks and cameras moving around the board.
Is it worth the time to implement? I have no idea.
https://www.mixamo.com/

This is a thing of beauty:
![](/uploads/93a67384-0d16-4e35-a36d-bbf7e2eb3958.png)

I now have a hex with the proper UV mapping. Wow.

Now to use the google font.
That was easy.
Just found a Unity polygon input for the Shader Graph.
The next problem is replicating the stroke that Figma applied to the text.
It accentuates the sharp edges of the letters creating these wild spikes that I think look great.
I probably shouldn't spend too much time on this.
Let's say an hour and then I'll just move on without it...

An TMP atlas resolution of 4096 gets the text as close to figma as I've seen so far.
That's step one.
Back to the stroke...
I'm thinking either a custom shader graph OR perhaps a shadow, or double

Now I'm seeing a white square overlay over each character which almost gives it a transparent feel, but it's very, exceedingly, broken looking.
Ah it happens when zooming out, perhaps I'm using too high quality now?

Feels like I'm back to where I was with just learning how to use opengl/webgl and doing everything from scratch.
Looks like I might need to write a custom shader to handle this outline.

https://forum.unity.com/threads/overlapping-outline-shaders.578485/

Messing around with "TMP_SDF.shader" and I think I crashed unity.

---------- 6hrs here ----------

Looks like there are some massive game studios in Van:
https://gamejobhunter.com/local-video-game-companies-vancouver/

https://thebookofshaders.com
https://thebookofshaders.com

https://www.shadertoy.com/

[YT - The Art of Code - Live Coding and Alien Orb - Modeling & Shadows](https://www.youtube.com/watch?v=b0AayhCO7s8&list=PLGmrMu-IwbguuF_XwpsU9Teo72_19olII)

[This guy](https://www.youtube.com/watch?v=8--5LwHRhjk) is a better artist than me with nothing but math! Insane.


### 17.10.2022 Monday 6h

Trying a fresnel effect around the hex.
No dice with that approach. Fresnel is for increasing reflectivity at lower angles, i.e. the flatter you view water the more reflective it is, viewing it straight down reduces this effect the most. So for a 2D shape such as my hex, fresnel only comes into play when you're looking at the side profile, which of course is useless to me.
Brackeys in his previous tutorial achieved the border by duplicating a sprite 4 times in each compass direction, then applying a color override to it. I'm not using a sprite here so I can't apply that.
A similar approach should work though. I'll just duplicate the mesh, scale it up and place that behind.

Having a bit of a confusing time trying to change the outline color for units owned by the other player. Despite chosing a prefab variant, changing the color kept updating both. Then I realised, both are still referencing the same underlying material behind the scenes which is referencing the same shader. Back to shaders. Should I just dupe?

So, can I have a variable on the prefab root that controls that? Or should I just have two materials?
The latter approach is easier, I'll start there.

What do you call data for unity objects?
ScriptableObjects.
And the demo is super relevant:
https://github.com/ciro-unity/UnityRoyale-Public?utm_source=YouTube&utm_medium=social&utm_campaign=evangelism_global_generalpromo_2020-09-21_better-data-scriptable-unity-royale

The [UnityRoyale](https://github.com/ciro-unity/UnityRoyale-Public) demo is amazing.
- [New Addressable Assets system for speed and performance](https://www.youtube.com/watch?v=U8-yh5nC1Mg)
- [Timeline and Signals talk at GDC](https://www.youtube.com/watch?v=SP3LvN-Q4Rw)
- [Creating a Stylised Toon Shader with Shader Graph](https://youtu.be/DOLE4nrK97g)

[YT - Tarodev - DOTWEEN is the BEST Unity asset in the WORLD and I'll fight anybody who disagrees](https://www.youtube.com/watch?v=Y8cv-rF5j6c)

It suddenly just came to me.
How to setup the shader graph to create the hex stroke outline effect I was after originally.
I've managed to add a subtle pulse effect too.
Awesome.

- All the UI including game menus is under the same canvas with 1080x1920 res 100ppu.
- Uses `RectTransform` within empty GOs 
- Uses empty game objects as subtitles in the hierarchy
- Demos addressable assets (albeit with local paths instead of a remote server)
- Demos using Cinemachine to fly the camera around for different events
- Uses DOTween `DOAnchorPos` method to move cards from deck to hand
- Uses `UnityAction` event system
- Has a `Managers` GameObject at the root of the scene with `CardManager`, `UIManager`, `InputManager`, `GameManager`, `CPUOpponent` custom scripts.
- Uses a quad mesh and box collider without the mesh renderer (disabled) to facilitate drag and drop
- I think the models were created with Maya (they wont even open in blender unfortunately)
- Custom Gizmos (not really sure why)

There's so much to learn from this demo it's brilliant.
Will dive into the scripts a bit more tomorrow and hopefully have time to rip out their card ui and add to the game.
Or perhaps I'll just move on to networking first, I'm not sure.
I kinda wanted to make it playable with the original style, replicating what I have in html/js.
Okay I'll come back to this...

----- Post Work Notes -----

[YT - The Art of Code - How to convert a shader from ShaderToy to Unity](https://www.youtube.com/watch?v=CzORVWFvZ28)
HLSL VS GLSL

He does it by hand, but perhaps you can just use:
https://zz85.github.io/hlslparser/

At some point I'll watch this but I think it's a bit too low level for me at the moment:
https://www.youtube.com/watch?v=Cfe5UQ-1L9Q

Starting this tut too:
https://www.youtube.com/watch?v=HKMo3pczQyc&list=PLD_vBJjpCwJtrHIW1SS5_BNRk6KZJZ7_d



### 18.10.2022 Tuesday 1h

I feel like today, I have JUST about enough knowledge to scrape together (with heavy googling and thieving):
- [x] single hex prefab
- [x] unit definition

Upgrading from Unity 2021 to 2022 (think the Jan version has only just come out of prerelease state into stable)

Just got past the unit definition and hex stage. There's now a unit script applied to the hex, which you can drag a unit definition into, and the name/stats/image is set correctly. The player colors are also applied to variables exposed on the shaders. I've made that a hex prefab. Wooooo.

Now I'm thinking, wouldn't it be nice to just import all the unit definitions from the JSON file I've already made?
Can Unity do that?

### 26.10.2022 Wednesday 4.5h

- [x] Draw current mousepos x,y to screen
- [x] Convert to hex coord
- [x] Move P1 summoner to next coords on click

[YT - samyam - Mouse Click Movement in Isometric Tilemap - Unity Tutorial](https://www.youtube.com/watch?v=b0AQg5ZTpac)

Add UI canvas
Scale tilemap to fit new canvas range
Move and scaled camera too
Scale summoner prefab to match
Add unity input system package (required restart)

[YT - Code Monkey - How to use NEW Input System Package! (Unity Tutorial - Keyboard, Mouse, Touch, Gamepad)](https://www.youtube.com/watch?v=Yjee_e4fICc)

Alright. So that's input, coords, moving game objects and tweening tackled.
What to tackle next?

- [ ] Spawn P2 summoner after 2s
- [ ] single card prefab
- [ ] show card, tween to hand loation

Also worth noting it's using axial coords.
Also, the middle is 0, 0.
I find doubled much easier to reason with.
Might be worth converting.

I also wonder if it's possible to generate CV maps for generated meshes.

### 27.10.2022 Thursday 6h

Copied these down from yesterday along with the ones before:
- [x] Spawn P2 summoner after 2s
- [x] Damage to health indicator

Started on [Unity Code Snippets](/wBoO0r77TiCdYCC4xdZ3UA) page.

Add unit images again. For some reason all but summoner had cleared.
Moved xy to coords logic into new game controller.
Add summon function.
Rework hex shader/material/code to reduce errors - unity likes the materials to be instantiated by code rather than modified.
Add damage indicator and logic.

What now? Actual game code?????
No.

If I can get all this done with the rest of the day that'll be HUGE:
- [ ] draw the old style unit selection left-rigt
- [ ] draw the old style player hands bottom-left/right
- [ ] show energy
- [ ] show player turn

I could use [UABE (Unity Asset Bundle Extractor)](github.com/SeriousCache/UABE) to try and pull images out of Inscryption to see if I can learn anything. Not yet though.

Yeah that was wildly ambitious.
The good news is, I've gone back to the generate hex mesh code and am able to create a new mesh as I intended way back in the beginning and apply the unit hex image with border shader to it.
In doing so and trying to position within the rect representing the P1 deck area, I realised the `Transform` component references the parent. So the scale of game objects changes with the parent. See the screenshot:

<img src="/uploads/92835a5d-47ca-4235-b023-637bd6f9be23.png" width="400px"/></br></br>
The far left and bottom two summoner hexes have scale 1:1 set with their parent.
I'm now wondering how to keep sizes consistent between parent transforms of different sizes/scales.

When you duplicate a prefab and drag from one parent to another, unity automatically adjusts the scale to retain the same dimensions, but in relation to the new parent. Does this happen when using instantiate in code?

Shit. It froze. Will have lost some changes to the scene.

So I'm thinking about how to layout the hexagons.
I could continue down this road of rendering everything using the hex layout.
If I could make my prefabs match the unity hex size exactly, that'd be another option.
I'm thinking the best way to position everything is to have parent empty game objects in the middle of rects set to specific sizes calculated as a % of the 1920x1080 res.

Right I'm going to create a new project and mess around with how to lay this out...

### 28.10.2022 Friday 6h

Learning these very essential components:
- Canvas
- Panel
- Horizontal/Vertical Layout Group
- Grid Layout
- Layout

Unity Shortcuts / Keyboard Bindings:
- `Shift Option A` - Toggle Enabled/Disabled
- `Shift Option N` - Create Game Object (as child, I made/stole this one)
- `Shift Cmd A` - Add Component

Bonus tip: If you just want a 2D plane with color, use image. You can set the color without having to create a material.

Awesome, I now have the main grid centered within the layout and a flexbox style grid which works in the ipad/mobile simulator.
That's a big improvement.

Back to the illusive perfect hex grid layout with prefabs and perfect scaling.
Don't worry, it will happen.

First thing I'd like to do is draw some gizmo lines on the center line.

With layout groups, I think, the `transform.position` for children is the bottom left corner of the parent.

Wow that was a struggle.
Started by just drawing sphere gizmos in places to see what these pivot points are.
They didn't make any sense. Most just landed in the middle.
They seem to be referencing local space too.
Gizmos are global space so I was trying to find a way to convert this.

The secret method is: `rectTransform.GetWorldCorners(corners);` (yes, you actually have to instantiate the array and pass it by reference)
That's something I haven't really considered in c#. Pass by value vs reference.
Looks like all `class`es are passed by reference.
`struct`s are passed by reference, I'm going to assume that's the case with primitives too, except arrays apparently?.
> All method parameters are passed by value unless you specifically see ref or out

### 01.11.2022 Tuesday 6h+

Back to it.
So I think I need to convert my own coordinates into world space.
The goal is to be able to ask something along the following lines:
Draw me a hex at xy:00 with a grid of 3x3 and a margin of 3%.
The hexes will be automatically sized to take advantage of the full available width/height.
I could introduce a "units per x" along the lines of unity.
Or I could just say the bottom left is 0, 0 and the top right is 1, 1 and calculate everything else from that.

So I suppose the first thing is to be able to draw the margins.
Say I want a 3% margin.
Lets try drawing that.
Oh right, you have to find the smallest/largest margin between horizontal/vertical and stick to that for both or it looks odd.

So using the old js hex lib code, the height of hex (pointy) is greater than the value I'm supplying.

Rewrote the code from https://hexagoncalculator.apphb.com/ in c# using get/set.
It's quite nice to use.
Now I'm able to calculate these hex sizes easily.
The "width" of a hex in "pointy" layout actually corresponds to the distance from vertex to vertex rather than side to side, as would make more sense with a pointy hexagon. To compensate, we need to convert our side to side/2apothem distance into vertex to vertex.

So now I'll try layout out 3 hexagons next to each other dynamically sizing within the box...

Yeslawd! I have a dynamically resizing hex grid.
But... I need spacing between the hexagons. Not much, but enough to be required.

YES. Hex, drawn with math, with spacing.
Oh timer has been hit but my laptop is muted.

I'm going to continue on...

### 02.11.2022 Wednesday 6h

So `Mesh` has a property called `triangles` which is a `int[]`. Why?
Still doesn't really make sense to me as it's just an array of `[0, 1, 2, 3]` etc. equal to the number of verticies.

> vertex count must == uv count

Wow, so close to being done with this.

<img src="/uploads/b3386cac-2de4-4c0f-afb7-81dc30e2a140.png" width="400px"/></br></br>

The shader is not centered.
The whole hex game object doesn't rotate around the center.

Shader aside for now, no matter what I do, how I organise things in the unity hierarchy, the hex seems to rotate around a point a little beneath the expected center.

Okay I was missing a very crucial concept. Everything in 3D begins at 0,0,0. That's not just for unity but seemingly every 3D program in existence. It makes sense. But what that meant for my strangely spinning hex was that I was translating the verticies for the mesh so the result matched where the game board was in global space. That was the issue. Because my center point was actually -x pixels above the middle, everything rotated around that point below. All I had to do was NOT translate the hex, then translate the `transform.position` to where I wanted it AFTER creating it in the center of the universe.

Then for some reason I had to rotate on the z axis.
Then I reworked the shader graph because it was off too. Not central. I think it's because I was rotating by 30deg but the shaders also use the bottom-left as the 0,0 point, so the hex was rotating up and off the shader to the top left. I had to set the center point to 0.5, 0.5, it was 0, 0. I probably didn't realise when creating the shader in the first place that the center coordinates are in UV space.

Hell. Fucking. Yes.

<img src="/uploads/6e59c023-2976-47a4-957a-c3b2e21358cb.png" width="400px"/></br></br>

Look at that precision... Gawdamn.

Now onto the different layouts. I have board, I need hand, left deck, right deck. Then I need to update the auto layout to work with each one (just defaults to board calculation atm)

The next challenge might be scaling and positioning game objects but I think with this foundation that will be a lot easier.

### 04.11.2022 Friday 6h

Remember: rect corners start at [0] bottom-left and work clockwise to [3] bottom-right.

Spent the day on hex positioning. Getting constantly confused between world, local and hexgrid vector3 coordinates which is making this more difficult than it should be. Almost there though.

<img src="/uploads/e9f4e13c-0de3-45a1-8572-b77b0cf6304a.png" width="400px"/></br></br>


### 09.11.2022 Wednesday 6h

*sigh* Back to hex positioning...

Sweet! I now have the hex grid layout done. Sort-of.
The canvas stretches with my screen and while I want the inner components to stretch and auto-layout, I was hoping the canvas itself would be fixed. Maybe there's a way to do that?

<img src="/uploads/f6e324a6-fef2-4a24-aca9-208da9a05f2b.png" width="400px"/></br></br>

Can I now spawn a proper unit hex game object rather than a custom tile.
And resize and position it properly?

Going to refactor this hex gizmo craziness so that the layout is only recalculated when it needs to be, and the hex meshes are actually created when the game starts (if desired that way, will test that first then switch to using the game object prefab)

Okay finished the refactor.
Using the Unity Rect class instead of 4 variables top/left/right/bottom every time I wanted a rectangle really brought the line count down and helped with readability.

Getting:
```
uvmidpoint (NaN, NaN) uvfrom (NaN, NaN) uvto (NaN, NaN)
UnityEngine.Debug:Log (object)
HexLayout2:CreateHexMesh (HT.Hex) (at Assets/Scripts/HexLayout2.cs:282)
HexLayout2:Update () (at Assets/Scripts/HexLayout2.cs:69)
```
Spamming the same code on `Update` does eventually do what I want but... yeah it's not the way.

Protip: You can use `cmd k` to open the search and then use more complicated queries such as `h: tag:board` to search the hierarchy for game objects with the tag board, then shift down arrow to select them all then return to select them in the hierarcht then `shift option a` - for example - to toggle them all.

So despite everything seeming great in the editor, when playing the game the layout values don't seem to be aligning with the gizmos:

<img src="/uploads/7a3d1411-c992-45ee-b215-ab308469f3c5.png" width="600px"/></br></br>

<img src="/uploads/08a6c8d4-4eb9-48ab-ad1b-0960112b6098.png" width="600px"/></br></br>
NOICE :grinning: Just needed to reference the `_centerTranslate` in the old create hex mesh logic.

Now the prefab?

<img src="/uploads/ffb36ef0-1326-4492-afec-bfd5e53ab312.png" width="200px"/></br></br>
Close, but not scaling correctly.


### 10.11.2022 Thursday 7h

UnityEngine.UI
The grid doesn't seem to ever change size and scales with the resolution.

```
Not allowed to access vertices on mesh 'Circle Instance' (isReadable is false; Read/Write must be enabled in import settings)
UnityEngine.Mesh:GetVertices (System.Collections.Generic.List`1<UnityEngine.Vector3>)
HexLayout2:Update () (at Assets/Scripts/HexLayout2.cs:96)
```

Massive breakthrough.
So I was trying to find the bounds using the `MeshFilter.mesh.bounds` which always returned confusingly low numbers. I created a cube primitive and the bounds for the cube were smaller than the hex despite the hex being larger in the scene. Yes, I did check the z axis to see if distance was the culprit. It turns out the MeshFilter only works in local space, so it completely ignores scale. I had to use `MeshRenderer.bounds`. Here's a screenshot:

<img src="/uploads/e3131b6c-4113-4b3c-9d0c-94e192810a51.png" width="600px"/></br></br>

This effect is pretty sick:
https://www.youtube.com/watch?v=1jlLDHHReHk

Now I'm getting a different issue, see these black bands either side of the hex:
![](/uploads/37a4b47c-fc7a-4ca0-9a26-38ba51eed199.png)

Actually, that was caused by a bugfix. I was originally returning the side-to-side width of a hex as `side x 2`. That's not correct, it's actually the `apothem x 2`. That said, using `side x 2` for the uv calculations seems to be exactly what I need. What a lucky accident, and I'm damn lucky I noticed too. Here's the corrected code snippet:
```c#
public float SideToSide {
            get => Apothem * 2;
            set => Apothem = value / 2;
//          get => _derivedSideLength * 2;
//          set => _derivedSideLength = value / 2;
}
```

Here's a bit of a frustration: when you generate a mesh and drag it into the project tab to save as an asset, unity doesn't save the mesh.

Apparently this guy has solved that:
https://forum.unity.com/threads/solution-save-a-prefab-with-a-generated-mesh-es-without-creating-assets.463496/

He also hints that you can "save the mesh as an asset". Interesting...

Lots of loose ends are falling into place at the moment. I realised the summoner is facing the opposite way to the image so did some digging. Found out my UV map numbers were inverted which is why I had to rotate on the z axis to have her head at the top (still facing the wrong way) I've corrected the UV map and removed the default 180 degree rotation.

I think I already found a mesh saver that added the option to save it as an asset to the MeshFilter context menu. I just used that. Now I can remake the unit prefab with the known perfect hex background.

Even that involved quite a bit of juggling:
- Save the new hex mesh as an asset
- Drag that back onto scene
- Move both old prefab and new hex into parent game object (I used board so I could use the grid gizmo overlay)
- Align both to 0,0,0
- Delete the old hex
- Unpack the asset completely
- Group the new hex
- Drag that back to create a new prefab
- Switch the renderer in Unit to use new shader graph
- Resave the prefab

I think that's done it.

I still have a bit of a confusing line:
`unitGameObject.transform.localScale *= 0.94f;`
Which is required to keep the border of the hex within the grid.

There's a fair amount of UI/GFX stuff I still need including:
- energy indicators
- turn indicator
- "selected" unit state
- tile backgrounds
- greyed out style for "used" units

I might also have to draw some gizmo debug text to tell me what the axial coords are

Damn. Don't make the mistake of creating an infinte loop unless you want to restart Unity and lose your updates. I wanted to go from 0 to -27 in a for loop, I incremented up, game over. Grrr.

Having this weird bug where loading p1 deck shows just summoner, but with the correct unit info. Reloading the scripts causes the images to change. Dragging another unit definition into the inspector slot triggers the change. Calling the same code in Update doesn't work, nor from the GameController. I'll have to tackle that tomorrow already an hour over.

### 11.11.2022 Friday 6h

Getting stuck into porting the core game logic from js to c#. This should be speedy.

I'm battling with vscode csharp auto-format. Keeps putting braces on new lines. I'd rather jam my little toe in a door than continue like this. Putting an `omnisharp.json` configuration file with this content:
```
{
  "FormattingOptions": {
    "NewLinesForBracesInLambdaExpressionBody": false,
    "NewLinesForBracesInAnonymousMethods": false,
    "NewLinesForBracesInAnonymousTypes": false,
    "NewLinesForBracesInControlBlocks": false,
    "NewLinesForBracesInTypes": false,
    "NewLinesForBracesInMethods": false,
    "NewLinesForBracesInProperties": false,
    "NewLinesForBracesInObjectCollectionArrayInitializers": false,
    "NewLinesForBracesInAccessors": false,
    "NewLineForElse": false,
    "NewLineForCatch": false,
    "NewLineForFinally": false
  }
}
```
does ABSOLUTELY NOTHING. I've tried:
- restarting omnisharp within vscode
- restarted vscode itself
- reinstalling all c# extensions
- adding to `~/.omnisharp//omnisharp.json`
- adding to `./omnisharp.json`
- adding to `./.vscode/omnisharp.json`
- my `view > output > OmniSharp Log` looks like:
```
  Starting OmniSharp server at 11/11/2022, 1:12:35 PM
    Target: /Users/adc/Unity/ADC Hex Grid/ADC Hex Grid.sln

OmniSharp server started with .NET 7.0.100
.
    Path: /Users/adc/.vscode/extensions/ms-dotnettools.csharp-1.25.2-darwin-arm64/.omnisharp/1.39.2-net6.0/OmniSharp.dll
    PID: 13782

[info]: OmniSharp.Stdio.Host
        Starting OmniSharp on Unknown 0.0 (Unknown)
[info]: OmniSharp.Services.DotNetCliService
        Checking the 'DOTNET_ROOT' environment variable to find a .NET SDK
[info]: OmniSharp.Services.DotNetCliService
        Using the 'dotnet' on the PATH.
[info]: OmniSharp.Services.DotNetCliService
        DotNetPath set to dotnet
[info]: OmniSharp.MSBuild.Discovery.MSBuildLocator
        Located 1 MSBuild instance(s)
            1: .NET Core SDK 7.0.100 17.4.0 - "/usr/local/share/dotnet/sdk/7.0.100/"
[info]: OmniSharp.MSBuild.Discovery.MSBuildLocator
        Registered MSBuild instance: .NET Core SDK 7.0.100 17.4.0 - "/usr/local/share/dotnet/sdk/7.0.100/"
[info]: OmniSharp.WorkspaceInitializer
        Invoking Workspace Options Provider: OmniSharp.Roslyn.CSharp.Services.CSharpFormattingWorkspaceOptionsProvider, Order: 0
[info]: OmniSharp.Cake.CakeProjectSystem
        Detecting Cake files in '/Users/adc/Unity/ADC Hex Grid'.
[info]: OmniSharp.Cake.CakeProjectSystem
        Did not find any Cake files
[info]: OmniSharp.MSBuild.ProjectSystem
        Detecting projects in '/Users/adc/Unity/ADC Hex Grid/ADC Hex Grid.sln'.
[info]: OmniSharp.MSBuild.ProjectManager
        Queue project update for '/Users/adc/Unity/ADC Hex Grid/Assembly-CSharp.csproj'
[info]: OmniSharp.MSBuild.ProjectManager
        Queue project update for '/Users/adc/Unity/ADC Hex Grid/Assembly-CSharp-Editor.csproj'
[info]: OmniSharp.Script.ScriptProjectSystem
        Detecting CSX files in '/Users/adc/Unity/ADC Hex Grid'.
[info]: OmniSharp.MSBuild.ProjectManager
        Loading project: /Users/adc/Unity/ADC Hex Grid/Assembly-CSharp.csproj
[info]: OmniSharp.Script.ScriptProjectSystem
        Did not find any CSX files
[info]: OmniSharp.WorkspaceInitializer
        Configuration finished.
[info]: OmniSharp.Stdio.Host
        Omnisharp server running using Stdio at location '/Users/adc/Unity/ADC Hex Grid' on host 13373.
[info]: OmniSharp.MSBuild.ProjectManager
        Successfully loaded project file '/Users/adc/Unity/ADC Hex Grid/Assembly-CSharp.csproj'.
[info]: OmniSharp.MSBuild.ProjectManager
        Adding project '/Users/adc/Unity/ADC Hex Grid/Assembly-CSharp.csproj'
[info]: OmniSharp.MSBuild.ProjectManager
        Loading project: /Users/adc/Unity/ADC Hex Grid/Assembly-CSharp-Editor.csproj
[info]: OmniSharp.MSBuild.ProjectManager
        Successfully loaded project file '/Users/adc/Unity/ADC Hex Grid/Assembly-CSharp-Editor.csproj'.
[info]: OmniSharp.MSBuild.ProjectManager
        Adding project '/Users/adc/Unity/ADC Hex Grid/Assembly-CSharp-Editor.csproj'
[info]: OmniSharp.MSBuild.ProjectManager
        Update project: Assembly-CSharp
[info]: OmniSharp.MSBuild.ProjectManager
        Update project: Assembly-CSharp-Editor
[warn]: OmniSharp.Roslyn.CSharp.Services.Navigation.FindUsagesService
        No symbol found. File: /Users/adc/Unity/ADC Hex Grid/Assets/Scripts/Game.cs, Line: 4, Column: 0.
```

There was another hoop I had to go through, in more recent omnisharp versions it reads configuration from vscode. I had to set:
`"omnisharp.enableEditorConfigSupport": false` in `./.vscode/settings.json`.
I added a comment to [this stack overflow post](https://stackoverflow.com/questions/49136015/vscode-format-curly-brackets-on-the-same-line-c-sharp) to help others in future.

CodeLens was really grinding my gears too.
It was adding `x references` to EVERY GODDAM DECLARATION.
Christ on a leash.
This is the calming sauce: `"editor.codeLens": false`

![](/uploads/02537626-228e-4c1f-a8ad-d3557644850c.png)

Also, the autocomplete would trigger when hitting space, or a comma. I had to hit escape to disable the autocomplete to avoid having the first irrelevant suggestion thrusted into my code. Here's a github issue with people complaining about it.
https://github.com/OmniSharp/omnisharp-vscode/issues/3647
Disabled the option "Accept Suggestion On Commit Character"
Why is that a thing? And why in the name of Hades (great game btw) is it enabled by default??

### 14.11.2022 Monday 6h

Noticing some inconsistencies that need to be cleaned up at some point:
- I'm always assuming hex -> doubled uses `2 * r` but that will not work with the flat top orientation. All these hard-coded `RDoubled...` calls need to be refactored.

Awesome, refactored the board to render from the game state, (and finished porting all that javascript to c#. So now I'd like to render some blank background hexagons so I can actually see the board and also the hands and draw piles again.

### 16.11.2022 Wednesday 6h

Should be playable really soon. Just need to:
- work out the update loop
- tie in the remaining ui elements
- handle input mousemove/mousedown

Okay so I've got the mousemove working for the main hex board. I also need that to work for each hand. I've managed to get the coords working again with axial and doubled figures. The p2 draw is still a bit mis-aligned. I suppose we actually need mouse in/out callbacks for the hexagons, as well as mouse down.

Board is fully setup now:
![](/uploads/a4e7cf0a-bdef-48b5-b182-88d889953df8.png)

All loading from underlying game engine emitting a state update event. Coords in top loading on mouseover for all hexagons across all layouts.

Now to handle mouse click.
Done.

Now to connect up input with the game code.
- [x] current player hand mouseover

![](/uploads/ee1fed6f-52e0-4ed2-b25b-d48f4110a1a4.png)

Got mouseover hand highlights working for the active player. Took a bit longer than I expected. For some reason the input controller can't find the unit renderer on mouse in to hex. There's also this flicker where the hex you're moving to highlights then unhighlights then re-highlights. Maybe unity does some mouse prediction and smoothing which is causing issues? It might be worth having a separate invisible game object which is bigger than the hex background (which has a collider for mouse move detection with ray casting) to close up the gaps so the in-out-in just becomes in-in when moving within the grid. It's perfectly usable in the current state though so I'm going to move on for now and just add an extra check to make sure the unit is not null.

Just to clarify the obvious here: the mouse in event for a hex should always return a hex, and for the hand, always return a unit if there's a unit in that spot.

- [ ] highlight summonable areas on hand unit select

### 18.11.2022 Friday 8h

Had a thought. It might be a good idea to refactor the hex layout into drawable groups:
- background hexagons
- active states (summon highlight, move highlight etc)
- units

When you highlight a hand hex to select for summoning, the image changes to the summoner image. Hmmmm.
Ahh a new Material is created when the highlight color, which also means a new shader, which means the image returns to the default.

Occasionally getting an error where the hex to the right of the summoner is not highlighted when attempting to move:

<img src="/uploads/245b7c4f-a335-4844-8edd-574f21ffa2aa.png" width="200px"/></br></br>

- [x] fix board unit not deselecting
- [x] fix summoner sometimes summoned
- [x] fix hand duplicates
- [x] energy indicators on board

- [x] grey out played units from deck

My idea for a simple grey out style to the draw hexagons was to just have an overlay mesh the same dimensions as the hex and cost circle with a semi transparent black fill.

It ends up like this. The problem is they are both transparent which leads to a darker area around the cost circle:

<img src="/uploads/2a138b8e-b953-4bf7-a465-0b720c4e9637.png" width="200px"/></br></br>

Looking into learning probuilder. So I can combine these meshes somehow. Seems like it might provide the tools to do so. Unfortunately, the blender export isn't working.

Had to enable experimental features to access subtract. Then it crashed unity :( Might have to resort to blender for this. But how do I even get the meshes into blender?

Worked it out. Did it by hand moving points around, combining them, and reworking the way the circle is subdivided. Seems ok.

- [x] unable to summon vulkan
- [x] unable to summon sun reaver

Awesome. It's playable!

- [ ] player energy UI
- [ ] summoning sickness indicator

Need to look at network code now so it can be played multiplayer.
Would be good to have a couch co-op AND multiplayer mode.
Although couching wouldn't work with cards/hands which are supposed to be hidden.
There's the card art and animations to tackle as well.





