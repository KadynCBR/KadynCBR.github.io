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

## Summary & Purpose
Danmaku was originally a gamejam title that I felt compelled to finish up as it was simple but also had a lot of promise in terms technical challenge. I'll discuss the key interesting things as well as provide a brief video playthrough of the level. The game should be on itch.io for download. [ITCHLINK]

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

<!-- Behavior Tree AI -->
### Behavior Tree AI

<!-- Smart Pooling Bullet Objects -->
### Smart Pooling Bullet Objects

<!-- Scriptable Object Event Architecture -->
### Scriptable Object Event Architecture

<!-- Custom Tools for map creation -->
### Custom Map Creation Tools
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



<!-- Procedually Generated Animations -->
### Procedually Generated Animations
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

### World Transition Gimmick
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/dmku/transition.gif" title="World Transition" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    World transition shifts color of the player character, but also shifts the world around it, allowing different obstacles or barriers to come into play.
</div>

This Gimmick was the whole purpose of the game initially, the theme for the gamejam was something like "split decisions". So the game is based around shifting between these worlds.
