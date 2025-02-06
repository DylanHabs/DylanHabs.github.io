## Third-person fighting game prototype [2024]
[Github page](https://github.com/DylanHabs/ThirdPersonSmashDemo)\
I created a third person online fighting game that is inspired by Super Smash Bros. This demo helped me learn about how multiplayer authority works in Godot.
![smashScreenshot](/assets/smash.png){: width="75%"}\
I also created a circular input queue that will save the last .125 seconds of inputs as a buffer for understanding which move the player preformed. This allows differentiating between an attack where the player tapped forward and left clicked vs and attack where they held forward and left clicked similar to how 2D fighting games work.

![monkey](/assets/monkey.png){: width="50%"}\
I also created a customization system
- Allows players to change the colors and proportions of their character
- Upload a face that can be used for the character and sent over the network so other players can view it