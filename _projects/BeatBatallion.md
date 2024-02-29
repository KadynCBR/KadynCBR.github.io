---
layout: page
title: Beat Battalion
description: Unreal Online Multiplayer Rhythm RPG Roguelike
img: assets/img/BeatBatallion/bblanding.png
importance: 3
category: Game Dev
---

<!-- 
    Summary & Purpose
        

    noteable stuff:
        Online multiplayer rhythm game
        multiple characters and kits
        RPG elements
        enemy ai on beat
        Gameplay ability System
        Steam integration
        continued support and dev
 -->

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div class="embed-responsive embed-responsive-16by9">
        <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/NDbv8ityWig" allowfullscreen></iframe>
        </div>
    </div>
</div>
<div class="caption">
    First level video demo
</div>

## Summary & Purpose
Beat Battalion started out as a gamejam game, one week to do a rhythm game. I'm a big fan of rhythm games but not a fan of falling-note style games (ddr, fnf, etc.). Because of this my team and I decided to do a spin on an RPG by making it a rhythm game. Particularly we pulled a lot of inspiration from crypt of the necrodancer for movement, and then for mechanics we took inspiration from Final Fantasy XIV's raiding mechanics. The core premise was a game where you play with your friends as different RPG classes and need to coordinate to take down enemies, perform mechanics and dodge AOEs all while having to remain on the beat of the song playing. This game continued to be worked on and updated, and I hope to release it on steam when its in a more polished and stable state.

Discussion points:
- Gameplay Ability System
- RPG Elements
- Multiple Characters And Kits
- On The Beat
- Online Multiplayer Rhythm Game
- Steam Integration
- Continued Support And Dev

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/BeatBatallion/menu.jpg" title="Menu" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/BeatBatallion/steamintegration.jpg" title="Steam integration" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/BeatBatallion/gameplay.jpg" title="Gameplay" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    We accomplished a lot so far and are still adding more, by far the most advanced project ive worked on thus far learning a lot about networking, GAS, and unreal in general.
</div>

---

## Gameplay Ability System
The project makes heavy use of unreal's Gameplay Ability System (GAS) to handle abilities and overall gameplay flow as it relates to these abilities. I had learned GAS through [This Course by Stephen Ulibarri](https://www.udemy.com/course/unreal-engine-5-gas-top-down-rpg) which ended up being up being an incredibly valuable resource not just for GAS but best practices for Unreal development. I debated adding that Aura project to this portfolio but instead thought it would be better if I was able to apply what I learned without guardrails of a course. 

---

## RPG Elements
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/BeatBatallion/stats.png" title="Stats" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
With GAS, it makes RPG elements easier to implement, the game currently has few primary stats but from this a semi-complex effect pipeline can be created. The pipeline takes into account things like luck stats for crits and blocks, defensive stats for mitigation, and two different types of attack types for varying skill damage. This also takes into account any equipped items or other modifying buffs and effects. As impressive as it might sound it really is letting GAS do a lot of the heavy lifting.

---

## Multiple Characters And Kits
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/BeatBatallion/characters.png" title="Characters" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
Since a core selling point of this game was cooperation between players, there is a simple yet diverse cast of characters each capable of doing different things. On a technical level each character is initialized with a _skillset_ which is a basic data asset that once given to the character will initialize and apply relevant stats and active / passive abilities. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/BeatBatallion/skillset.png" title="skillset" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    For those that care, this is what the skillset looks like, when giving the ability, itll apply a dynamic tag to the ability, and then when pressing one of the input buttons the playercontroller class looks at which class has that input and activates it.
</div>


---

## On The Beat
So far I've discussed the RPG elements, and those are great but this is intended to be a rhythm game first. To that end this is achieved by enforcing each input and action is taken on the beat. After researching quite a few different ways to make fighting games, I decided to utilize unreal's Quartz subsystem. Quartz is a subsystem that provides sample accurate scheduling. It has many uses from what I found, but primarily it can be used to schedules sounds on specific time boundaries (1/4 of a beat, a beat, a bar) as well as broadcast when these time boundaries are happening back to the main thread. This broadcasting back is done via subscribing to metronome events.

Using the above as a single source of truth, I can have everything subscribe to those beats at varying boundaries to perform checks, tick enemy behavior trees, apply effects, display UI elements, grant mana.. etc. 


As it would turn out rhythm games are incredibly difficult. In playtesting we found some people could nail perfect timings each time, some people couldn't get anywhere close, some had different monitor refresh rates which affected things. The game was intended to run at 60-90fps, but the metronome system only broadcasted an event for a single frame. A big problem was that the game originally required that the player be precisely on the beat or slightly after as it was easy to keep the door open, but only the metronome system could tell the game when to open that door in the first place. 

How was this solved? Quite simply by subscribing to a smaller beat increment and locking and unlocking the input windows. 
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/BeatBatallion/beattiming.png" title="timings" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    I had to learn what music was. Go figure. Green indicates gate open, red indicates gate closed.
</div>
By opening and closing input windows on the beat, and then keeping track of when a player had inputed or not, I could then open a wider window both before the beat and after. This resulted in a _MUCH_ nicer game feel. So much so that even the better and not-so-better rhythm game players agreed it felt better. Doing this also allowed a lot more flexibility in song choice as I could spead up and slow down the clock and this window would still allow players to adapt. 

This also allowed me to make failure states from the beat adding more excitement to the general moment to moment loop. If the player does an input off the beat, a MISS will occur and lock the player from action for a beat. They will also lose their combo and an potential build up to collecting mana to use abilities. A MISS will also occur when a player does not input at all. This pushes the player to make a decision at each beat, further solidifying the rhythm aspect



After all this working with the Quartz subsystem was pretty great and suited my needs well. My only gripe would be that the C++ documentation for the system is lacking but was easy enough to convert from the blueprint documentation to C++.

---

## Online Multiplayer Rhythm Game
I've discussed RPG, I've talked about the rhythm elements. Our next topic is online.
I want to say that the course I mentioned earlier in regards to GAS replication and the [Multiplayer Network Compendium](https://cedric-neukirchen.net/docs/category/multiplayer-network-compendium/) came in handy so much. I had already abided by the gameplay framework for most of my games thankfully but never did I appreciate it as much as I did until doing this. Aside from the regular learning experience that I'm sure everyone has I had some special cases that I think might be relevant to this game in particular but also helpful for the weary traveler. 

### Moves like arthie
Early in development I had experienced a pretty nasty movement but that seemed to only be on clients. while it didn't explicitly prevent any problems with gameplay, it would cause a massive stutter in player movement. In general movement is gridbased and when a player wishes to move, a timeline performs a _set actor location_ from grid location A to grid location B. 
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div class="embed-responsive embed-responsive-16by9">
        <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/oFWiWmZCHrw" allowfullscreen></iframe>
        </div>
    </div>
</div>
<div class="caption">
    This was going to bug me forever
</div>
What this ended up being was a few things. 
1. The movement was not done on the server, and so the player was constantly fighting with the server in the timeline lerps.
2. The movement was being predicted.

I didn't have free movement, I was confined to a grid and very strict rigid movement, but still unreals defaults were seemingly trying to add momentum and prediction into the move actions. Turning off replicated movement ended up fixing this for the most part, but this took a while to pin down as any game with free movement this behavior is probably welcome. 

A similar bug would resurface later in development in the form of extra movement on clients. Each movement caused the client to become offset by 7 units or so. while this wasn't game breaking because of the way set actor location works, it set the player at the start of the routine but at the end it would always be a bit off. This in turn also caused some unwanted camera shake which was frustrating as it felt clumsy. This ended up being the Character movement component that comes with character objects in unreal. This component was presumably trying to course correct my rigid movement. 

After really thinking about it, I really didn't need to derive from character objects as I didn't use any of the functionality of that component. A simple pawn would have been fine. Lessons learned.
Once these issues were wrapped up. Buttery smooth movement on client and server with very little latency amongst our playtesters across North America.

### Where do I put that?
Throughout my time developing this game, I was met with a constant fear that I was developing pieces in the wrong place. "Should this be a custom subsystem? Should this go in the player controller? Game mode? Game base?" These questions are normally pretty easy to answer, perhaps my inexperience comes through here, but my game also had changing maps where players needed to maintain state across maps. 

Clocks needed to go where it made sense the most. Placing them on the playerstate or playercontroller made the most sense, but I opted for the playerstate as at the time this made the most sense to me. In retrospect, getting the playercontroller for the local player is easier than getting the playerstate for an online session (playercontroller @ 0 = local player, playerstate @ 0 = host). While this made it a bit more of a hassle it was still workable. Though definitely any other place would be a no go.

Then persistant state for clients across maps. Anything I needed to persist in the game between maps I kept on the hosts' gameinstance. I'm still unsure if this is best practice, but it allowed me to do things like keep track of client HP, keep track of client items, allow clients to select a character and actually give them that character. The next steps for this, yet to be implemented until I need it, would be to make a structure of required state, and then serialize the state of the game into this structure, then deserialize it as I need it for the next map. That way the server still remains a single source of truth, but also easier to work with. 

### Syncronize clocks
The biggest issue on this project by and large was the syncronizing of clocks. A cooperative rhythm game which didn't work was not going to be much fun. I'm familiar with networking and enough to know that getting a quartz accurate scheduling across a network was not going to be a fun nor easy task. So I thought why does the server need to be authoritative over the beat? Especially with timing windows and allowing player bias, all of these mechanics would be best left up to the player. Even doing something like online-turn-based would be weird because you're still relying on the server to distribute results on the beat. 

So what if, each player had their own personal rhythm game.

Each client would create their own quartz clock, manage their own input, and only when all these gates were passed, would it then report to the server the command or in the event of player failure, a failure state. This game was intended to be a coop experience, so I wasn't concerned with cheating.

Initial renditions of this system were mediocre at best. Starting a song and clock when all players entered the new map did allow each player to play their own rhythm game and input movement. There was a glaring issue, you were playing at a different beat than your allies, and even worse, your enemies (server clock).

**We can do better.**

What if when clients entered the new map, instead of immediately starting their clock, they waited, and the gamemode would tell each client when to start their clock.
From the lobby, I know how many players we left the lobby with. I'll wait until each of those players are connected, delay some time to fully load the level, and then send out a multicast to each client to start their clocks on receiving that multicast. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div class="embed-responsive embed-responsive-16by9">
        <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/LE4pfjjEv6Q" allowfullscreen></iframe>
        </div>
    </div>
</div>
<div class="caption">
We did infact, do better.
</div>

Great! This is starting to feel like a coop rhythm game now! One could say this is local, and that'd be correct. Even that client window would take time to join the session and load the map causing it to go off sync with the server window. How did this fare in actual gameplay with someone across a contient? It failed pretty horrifically. If the client wasn't loaded in time, the event to start your clocks wasn't rebroadcasted. Their clock would never start, and eventually an AOE would just kill that client as they couldn't move. The solution to this? Just have a larger delay time! isntead of waiting 3 seconds for all clients, wait 12. Everyone loads in and theres a weird awkward break in pacing because everyone is now waiting, even if all clients are ready to go. This does ensure everything starts properly. 

**We can do better.**

The solution to this was to let players themselves tell the server when they were ready. Since there are a lot of systems which utilize these events, and it'd be a bit difficult to centralize what is intentially decentralized for the purpose of starting the clock, I instead put a ready toggle. This also gives players a chance to ready themselves for the battle sequences of the game. This is so far the best solution I have that does what I need it to and maintains all players in sync.  
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div class="embed-responsive embed-responsive-16by9">
        <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/q3jAocY_uXs" allowfullscreen></iframe>
        </div>
    </div>
</div>
<div class="caption">
We did infact, do 'mo betta'.
</div>

---

## Steam Integration
We have RPG. We have rhythm. We have online. We now need a way to put everything together. Online only works if people can join. NAT punching, port forwarding, LAN, hamachi, on and on the list goes for methods. I instead opted to go for steam integration as this was my original target platform. Once the player is in from steam, the connection stays and is persistant until disconnect, even between levels. To test this we used the steam developer appid, and since we were only doing friend invites, we didn't need to handle matchmaking. This left the project in a pretty good place.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/BeatBatallion/steamintegration.jpg" title="steam" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Look mom! I'm on steam!
</div>

---

## Continued support And Dev
With all that, this is by far my most advanced push into the unreal engine and game development in general. This project is still being actively worked on and I hope to formally release it on steam for free to gauge interest on if I should continue this kind of game, or if this should just be a fun one off experience to be had among friends. 

I'd like to thank [Kronoshark (Programmer / UI)](https://twitter.com/RuinLightStudio) and [JeruTea (3D Modeler + Animator / Artist)](https://www.artstation.com/jerutea) for helping with this project and continuing to provide their support! 
