---
layout: page
title: Danmaku
description: Unity twinstick shooter
img: assets/img/dmku/im3.png
importance: 3
category: Game Dev
---

<!-- 
    Summary & Purpose
        

    noteable stuff:
        Fully playable end to end game
        behavior tree ai
        Smart pooling bullets
        Scriptable Object event architecture
        map tools from pixels
        proceedually Generated Animations
        Screen space effects and shaders (glitch on hit)
 -->

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div class="embed-responsive embed-responsive-16by9">
        <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/txsRaaYUU8o" allowfullscreen></iframe>
        </div>
    </div>
</div>
<div class="caption">
    First level video demo
</div>

## Summary & Purpose
Danmaku was originally a gamejam title that I felt compelled to finish up as it was simple but also had a lot of promise in terms technical challenge. I'll discuss the key interesting things as well as provide a brief video playthrough of the level. The game should be on itch.io soon for download.

Discussion points:
- Behavior Tree AI
- Smart pooling bullet objects
- Scritable Object Event Architecture
- Custom Tools for map creation
- Procedually Generated Animations
- World Transition Gimmick

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/dmku/im1.png" title="Ship Activation" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/dmku/im2.png" title="Enemies and bullets" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/dmku/im3.png" title="Boss 1" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    On the left, our ship playable character. Middle, combat encounter with enemy ships. Right, the introduction cutscene frame for the first boss fight.
</div>

---

<!-- Behavior Tree AI -->
## Behavior Tree AI
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div class="embed-responsive embed-responsive-16by9">
        <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/S4-dvw6AC48" allowfullscreen></iframe>
        </div>
    </div>
</div>
<div class="caption">
    The behavior tree for the first boss.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/dmku/im6.png" title="PixelMage AI Tree" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    An enemy AI Tree which teleports to avoid damage after fighting
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/dmku/im7.png" title="Action Combat AI" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    An unusued AI behavior tree designed for a more complex game I'm also doing.
</div>

For enemy AI, I wanted more practice with working with behavior trees. The above display the behavior trees I've created. A lot of these have simple nodes but there are also a few custom nodes that I made specifically for that enemy or that combat encounter.

---

<!-- Smart Pooling Bullet Objects -->
## Smart Pooling Bullet Objects
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div class="embed-responsive embed-responsive-16by9">
        <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/lZm9WcnWrLE" allowfullscreen></iframe>
        </div>
    </div>
</div>
<div class="caption">
    There's a lot of bullets to manage. They all pretty much manage themselves though, which is nice. I enjoy my create and forget objects. 
</div>
Since this game was intended to be a sort of bullet hell game, I wanted to manage the many bullets that were going to be on screen at the same time in an efficient manner. To this end I created a bullet manager to accomplish this. 

Anytime a bullet is required by an entity it asks this level-wide manager. The manager then checks to see if a bullet is available in the queue, if it is, it will enable it and set it up as required. If not, it will create a new bullet. 

Once a bullet has run its course or made impact, instead of destroying, it notifies the bullet manager, and places itself back into a queue ready to be used for the next time the bullet manager needs a new bullet. 

Doing this removes the constant overhead of creating and deleting bullets.

---

<!-- Scriptable Object Event Architecture -->
## Scriptable Object Event Architecture
This game was made with a strong lean into Scriptable Object Event Architecture.
This <a href="https://www.youtube.com/watch?v=raQ3iHhE_Kk">talk</a> was pretty inspiring to try using events to decouple a lot of the systems talked about here. I found I really enjoyed working with this paradigm, despite it not having as clear a paper trail as traditional dependency injection in unity. The benefits far outweight the cons in my head as a lot of these systems I made are easily portable to other projects. I extended the methods discussed in the talk quite a bit, adding generics and parameters to events, as well as making special Scriptable Object variables to house custom data types and the like. 

---

<!-- Custom Tools for map creation -->
## Custom Map Creation Tools
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/dmku/im5.png" title="Level" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/dmku/pixil-black.png" title="Pixel source" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left, level created. Right, pixel map used to generate left.
</div>
To make map generation easier, I implemented some map generation tooling which takes png images to create a level. The actual implementation is pretty simple with some unity editor tooling wrapped around it. We go through each pixel in the provided image, and then spaw the block or enemy at the calculated offset. This map is also automatically enrolled in the world shift gimmick discussed later. 

{% raw %}
```c#
public void CreateMap()
{
    foreach (Texture2D map in maps)
    {
        int mapwidth = map.width;
        int mapheight = map.height;
        int xoffset = -(map.width) / 2;
        int zoffset = -(map.height) / 2;
        Color[] pixels = map.GetPixels();
        float px, pz;
        for (int i = 0; i < pixels.Length; i++)
        {
            px = i % mapwidth * distanceBetweenCells + xoffset;
            pz = Mathf.Floor(i / mapwidth) * distanceBetweenCells + zoffset;
            if (pixels[i].a == 0)
                continue;
            if (pixels[i] == Color.white)
            {
                CreateBlock(blocktype.WHITE, px, pz);
            }
            if (pixels[i] == Color.black)
            {
                CreateBlock(blocktype.BLACK, px, pz);
            }
            if (pixels[i].r > 0 && pixels[i].b == 0)
            {
                createEnemy(px, pz);
            }
        }
    }
}
```
{% endraw %}

---

<!-- Procedually Generated Animations -->
## Procedually Generated Animations
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div class="embed-responsive embed-responsive-16by9">
        <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/36ifthpvv_0" allowfullscreen></iframe>
        </div>
    </div>
</div>
<div class="caption">
    To save time and allow for dynamic movement of this tripod character, I implemented inverse kinematics to procedually animate the character. This is easier done in game than in <a href="https://www.youtube.com/watch?v=xEQ1KPo9HM8">real life.</a>
</div>

---

## World Transition Gimmick
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/dmku/transition.gif" title="World Transition" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    World transition shifts color of the player character, but also shifts the world around it, allowing different obstacles or barriers to come into play.
</div>

This Gimmick was the whole purpose of the game initially, the theme for the gamejam was something like "split decisions". So the game is based around shifting between these worlds.
