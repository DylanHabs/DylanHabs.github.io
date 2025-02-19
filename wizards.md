# Wizards in Shorts [2025]
[Check it out on Steam!](https://store.steampowered.com/app/3173220/Wizards_in_Shorts/)
Wizards in Shorts is a fast-paced arena shooter I made by myself in Godot. 
You battle it out in quick rounds where the last player left alive wins the round. 
Each round is played on one of 15 different maps. All maps have unique features and contain a subset of the over 10 different weapons.
It was a blast making the game and within the first week it had 50 reviews and over 1600 unique players!
As far as I know this is one of only a couple 3D multiplayer games released in Godot.
I ran into a lot of issues that I could not find solutions to online so it forced me to be critical about my own code and read as much documentation as I can. 

<iframe src="https://www.youtube.com/embed/SDYWh0s3W5A?si=RIe7F3YNQ9EZblkR" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

## Multiplayer Overview
I used the Steam API to create the lobbies and initialize peer to peer connections.
This was great since I don't have a budget for dedicated servers and Steam allows anybody to host without port forwarding.
It also enabled several features in the game, such as:
- Steam achievements and stats
- Voice chat and text chat
- A server browser where I can easily estimate pings

The game is mostly peer to peer but it does use more traditional client server architecture to store the game state.
Things like round wins, what map should be played next and which players are kicked from the lobby—are all managed by the original host. Since the game allows anybody to host I decided p2p would be better than a full client server architecture that handles most all logic server side and then uses client side prediction.

Connections can also fail for a variety of reasons so having a timeout menu that lets you reconnect is important.
<iframe src="https://www.youtube.com/embed/vJpZklvWrkc?si=cnpUsHnDsElG9N3Q" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Player Controller
Players are made of two big components an Input handler and a Wizard. 
- Input handler: Manages player-specific abilities such as username, spectating, and forwarding inputs to the Wizard.
- Wizard: A custom character controller containing all possible spells that can be collected and all relevant movement code. 
    - Uses the same acceleration function as Quake, allowing for movement quirks like bunny-hopping.

Initially, the Wizard was destroyed and recreated between rounds, but this caused syncing issues with each connected player. Resetting the Wizard between rounds proved to be a better solution.

Player positions are sent to each connected client as often as possible. However, due to many connections, the updates aren't fast enough to ensure smooth motion. To address this, Wizard positions are interpolated between their current position and the last received packet containing their position, resulting in smooth motion even with slow connections.

```
#Runs at 60fps
if not get_parent().is_multiplayer_authority():
    if global_position.distance_to(targetPos) <= 3.0:
        global_position = global_position.slerp(targetPos, 0.5)
    else:
        global_position = targetPos
```
Each players also has an enum that determines which team they are
- Blue team
- Red team
- FFA (anybody can kill anybody)

## Weapons
Weapons fall into three main categories: hit-scan/raycast weapons, projectile weapons and area of effect weapons. All inherit from a base weapon class that takes care of variables like fire rate and mana cost.

Raycast Weapons
- These weapons detect collisions based on the client who is firing them. This ensures that if it looks like you hit the target on your end, it registers as a hit.

Projectile Weapons
- Unlike raycast weapons, projectiles require you to physically collide with them on your own client. If you visually dodge a projectile, you won’t be hit.
- Projectiles also utilize raycasts to detect collisions.
<iframe src="https://www.youtube.com/embed/_nDj0uVBefQ?si=WXKbmmEc5l05TcaW" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Area of effect Weapons
- These affect any enemy player within a bounded region

To improve performance and reduce networking bugs, I use object pooling for projectiles and impact effects created by weapons. This involves creating all objects at the start and moving them to desired positions as needed, rather than constantly creating and destroying them.

## Maps
Since maps alternate so fast they also need to load fast. Because of this most maps reuse a lot of the same textures and can be somewhat minimal on details.
I still tried to keep them visually interesting by putting the in unique places such as a lighthouse or a fast food restaurant.

![wizShorts1](/assets/wizShorts1.jpg){: width="75%"}

Godot's built in scene changer does not work well with multiplayer so I made my own that can load and unload maps for all players.
It also will sync the map for people who join after it has started.

## Bots
Making an online FPS game as a solo developer is a bit tricky (it turns out testing by yourself by running the game four times is a little tedious) but the addition of bots made it a lot easier to test maps. 
The bots use a finite state machine that around every .2 seconds will reassess the state of the game and transition to a different state. I learned a lot about making CPU performant AI code working on them such as using approximation algorithms.
- If the BOT takes damage or a player is within 4m it will agro towards them.
- After attacking a lot the BOT might be low on manna and retreat rather than staying in the current fight
- Depending on the distance between the player and the BOT they will select the ideal weapon for the range

## Customization
I allow for the player to customize their staff, colors, and robe patterns using currency they find on maps or win at the end of the game.
To make it at least a little harder to cheat unlocks I used Steam stats to store which unlocks they have and how much of the in-game currency they have.

<iframe width="560" height="315" src="https://www.youtube.com/embed/_XaminSjPr0?si=z-8L1e01NelO-61U" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Concluding thoughts
I learned so much from this project but am really happy with how it turned out!
- Multiplayer is really tricky and any band-aid fixes in your code will get torn apart by latency
- Large play testing sessions are so important for catching bugs. Testing locally by myself never unearthed nearly as many game breaking features
- I got so much better at reading documentation as carefully as possible

I tend to have a problem scoping games but this one ended up being reasonable for one person to make and a whole lot of fun to test with friends!
Seeing people's reaction to the game online has also been so surreal.
Having people from all over the world take time out of their day to play something you made is such a good feeling. 