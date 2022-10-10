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

### Monday 10.10.2022

Need to put the game repo online.  
Have I not done that already?  
https://github.com/alexdcox/muskingum  
Yep.  
Perhaps I need this log up too.  
Okay done. Went with the random wiki page muskingum for the codename for now.  
https://github.com/alexdcox/muskingum-log
It sounds thrilling doesn't it?!  




