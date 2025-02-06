## Wizards in Shorts [2025]
[Check it out on Steam!](https://store.steampowered.com/app/3173220/Wizards_in_Shorts/)
Wizards in Shorts is a fast paced arena shooter I made by my self in Godot. You battle it out in quick rounds where the last player left alive wins the round.
It alternates between 15 different maps for each round. All maps have unique features and contain a subset of the over 10 different weapons. The weapons mostly consist of hitscan weapons using raycasts and projectile weapons. 
<iframe src="https://www.youtube.com/embed/SDYWh0s3W5A?si=RIe7F3YNQ9EZblkR" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

I used the Steam API to create the lobbies and initialize peer to peer connections.
This was great since I don't have a budget for dedicated servers and Steam allows anybody to host without port forwarding.
It also allowed for a bunch of other features in the game such as 
- Steam achievements and stats
- Voice chat and text chat
- A server browser where I can easily estimate pings

The game is mostly pier to pier but it does use more traditional client server architecture to store the game state. Things like round wins, what map should be played next and which players are kicked from the lobby is all stored by the original host. Since the game allows anybody to host I decided p2p would be better than a full client server architecture that handles most all logic server side and then uses client side prediction.

![wizShorts1](/assets/wizShorts1.jpg){: width="75%"}

Making an online FPS game as a solo developer is a bit tricky (it turns out testing by yourself by running the game four times is a little tedious) but the addition of bots made it a lot easier to test maps. 
The bots use a finite state machine that around ever .2 seconds will reassess the state of the game and transition to a different state. I learned a lot about making CPU performant AI code working on them such as using approximation algorithms.

I learned so much from this project but am really happy with how it turned out! I tend to have a problem scoping games but this one ended up being reasonable for one person to make and a whole lot of fun to test with friends!