---
layout: page
title: Space Defense
description: Android mobile tower defense game with ad integration
img: assets/img/Space_Tower_Defence2.png
importance: 3
category: Game Dev
---
<!-- 

    Summary & purpose

    noteable stuff:
        Actually publish on android store
        full ad integration
        multiple levels and unlockables
        custom json serialization and persistant saving
        Challenges (Phone limitations)

        https://play.google.com/store/apps/details?id=com.CherryTeaGames.TowerDefense
 -->
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div class="embed-responsive embed-responsive-16by9">
        <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/Y7zkL7fyaLc" allowfullscreen></iframe>
        </div>
    </div>
</div>
<div class="caption">
    First level video demo
</div>

## Summary & Purpose
Space Defense was born of a collaboration between my wife and I to quickly make a game that would be capable of publishing on an appstore. We wanted the full loop experience of getting something to 'market'. Because of this I'm happy to say you can currently find the game available on the google play store 

    https://play.google.com/store/apps/details?id=com.CherryTeaGames.TowerDefense

Discussion points:
- Actually published on google play
- Full ad integration
- multiple levels and Persistant saving with custom json serialization
- Mobile limitations

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/spdf/im1.png" title="" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/spdf/im3.png" title="gameplay" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left, Main menu of space defense. Right, Gameplay screenshot. As you can see only intense gameplay here.
</div>

---

## Published on App Store
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/spdf/im6.png" title="App store Listing" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This was more painful than I thought it would be. 
</div>
Since the core point of this was to publish with monetization, A lot of work was done dealing with appstore launching and deploying. This point encompasses many things from release keys, to versioning for the platform, to even having to make a privacy policy and terms of service for this game. There was also setting up and hooking up an admob account and making sure those are connected properly. 

---

## Full Ad Integration
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/spdf/im4.png" title="Advertisement" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    If you die during a level, you can watch a short Ad to continue the game at that stage.
</div>
A core tenant to this project was exploring ad integration. To this end I setup google admob and their ads solution. This involved connecting and hooking up my play developer account and setting up API keys, on top of programming in where ads would be served and how to reward / trigger events on successful serving. 

I had thought about going with unity ads, but from what I hear their mediation wasn't quite up to par and I wanted to at least experience getting ads served to clients.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/spdf/im5.png" title="moneymoneymoney" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    I don't mean to brag but, from this project I'm already making stacks on stacks.
</div>


---

## Level progression & Serialization
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/spdf/im2.png" title="Levels" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Levels are unlocked as you progress through the game. This information persists even after close.
</div>
Since this was meant to be a mobile game with long running progression, I opted to make a custom serialization script to save persistant data using JSON. It's saved as purely plain text as I wasn't too concerned over security but obfuscation is possible with the method used. 

Saving and loading happens during important events (level select screen, winning a level, etc.) to create a seamless experience. 

---

## Challenges & Mobile limitations.
This was my first mobile only game, and during this process I picked up how to live debug on mobile as well as overcome a few mobile only limitations.

The build process was more involved, requiring specific android versions and setting up android build paths. Debugging required setting up android livelink to see console. 

During the process there was a nasty bug where what was performed on the unity "simulation" (which I later found out is just changing the resolution for the editor) was not the same as what happened on my phone. Not only this but the behavior was different on each phone I had to test with. 5 different phones, 5 different behaviors. 

After extensive debugging I found that I didn't explicitly set any of the project settings, as this was never something I needed to do for PC games as my games reasonably haven't reached the level where most computers can't play it. For mobile, however, despite this being a relatively simple game, update rate was crucial. Even when set to 60fps, some phones refused to adapt, I figure this could be individual phone settings. Solution? Explicitly set project settings and lower update rate to the lowest common denominator (30FPS). After this, the game performed more consistantly across phones.

Another issue was screen resultion scaling. Canvas has the option to scale with screen size, but with so many varying screen sizes I had to find a common denominator once again to adapt. The art assets I had my artist create also required a lot of revising when it came to this varying requirement. 