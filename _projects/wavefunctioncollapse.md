---
layout: page
title: Wave Function Collapse
description: procedural generation (c#)
img: assets/img/wfc/im4.png
importance: 3
category: Game Dev
---

<!-- 

https://youtu.be/Cvgw9HQMHIg - unity wave function collapse driving

https://youtu.be/Wm6Zpu6kttw - unity wave function collapse preprocessing

https://youtu.be/zTKVSR4AcPs - unity wave function collapse (not preprocessed)

 -->

## Summary & Purpose
 I'm a big fan of procedual generation and randomness in video games, to that end I wanted to make a package for doing procedual map generation. I stumbled upon wave function collapse and it seemed to accomplish what we needed to do. I found implementations in python and C, which were for small bitmaps or images which generated the image below.

 <div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/wfc/im4.png" title="bitmap WFC" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This is cool, but not immediately applicaple to what I need.
</div>

I ported a combination of these implementations to unity, while adding in the ability to use gameobjects and 3d models of "blocks" as cells. 

 <div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/wfc/im1.png" title="blocks" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    These are examples of simple modular "blocks" which I quickly mocked up in blender. From left to right, the tiles are: blank, intersection, straightaway, turn
</div>

Minimal setup in unity thanks to some custom editor tools allows me to quickly generate maps. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div class="embed-responsive embed-responsive-16by9">
        <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/zTKVSR4AcPs" allowfullscreen></iframe>
        </div>
    </div>
</div>
<div class="caption">
    20x20 map generation (slowed down with artificial delete between iterations to help visualization)
</div>

A good start, but we can get even closer to an 'objective' level by making this a maze. The way we can do this is by preprocessing cells allowing for one "entrance" and one "exit". 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div class="embed-responsive embed-responsive-16by9">
        <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/Wm6Zpu6kttw" allowfullscreen></iframe>
        </div>
    </div>
</div>
<div class="caption">
    20x20 map generation with preprocessing of cells (slowed down with artificial delete between iterations to help visualization)
</div>

This is cool and all but adding in a bit of playability goes a long way.
(<a href="https://assetstore.unity.com/packages/tools/physics/prometeo-car-controller-209444"> Car controller scripts</a> not made by me, just a quick asset drop while using my procedually generated roads as a surface) 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div class="embed-responsive embed-responsive-16by9">
        <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/Cvgw9HQMHIg" allowfullscreen></iframe>
        </div>
    </div>
</div>
<div class="caption">
    Procedually generated roads to drift away on. :)
</div>


This system is currently being used as is with different blocks in a roguelike action game I'm making where it generates a procedual maze for certain levels.

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/wfc/im2.png" title="maze generated" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/wfc/im3.png" title="maze path" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left the generated level the player is expected to traverse. Right the path through the generated maze. 
</div>