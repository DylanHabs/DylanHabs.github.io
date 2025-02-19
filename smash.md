# Third-person fighting game prototype [2024]
[Github page](https://github.com/DylanHabs/ThirdPersonSmashDemo)\
This project is a third-person online fighting game inspired by Super Smash Bros, developed using Godot. Through this demo, I gained valuable insights into multiplayer authority within the Godot engine.

![smashScreenshot](/assets/smash.png){: width="100%"}

## Fighting game design
The fighting gameplay had two steps.
1. Determining what move was inputted
1. Executing that move with all relevant data

I created a circular input queue that will save the last .125 seconds of inputs as a buffer for understanding which move the player performed. 
This allows differentiating between an attack where the player tapped forward and left clicked vs an attack where they held forward and left clicked similar to how 2D fighting games work.

![smashIntro](/assets/smashINtro.png){: width="75%"}

That input data is used to select the closest move to execute.
Each attack has an associated data file that contains relevant information.
- Frame data
    - Startup
    - Active 
    - Cooldown
- Attack sound
- Animation
- Damage
- Hit-stun
- Hit-boxes to activate

I used timers in Godot to use the frame data to determine when the colliders should be activated checking for enemy collision. If players do get hit the hit-stun determines how long it takes before they can do a different action.
By playing around with an attacks hit stun and active frames players can also do basic combos. 

## Customization  
![monkey](/assets/monkey.png){: width="50%"}\
I also created a customization system
- Allows players to change the colors and proportions of their character
- Upload a face that can be used for the character and sent over the network so other players can view it

The proportions used blend shapes that allow for simple deformations to be applied to the model. Each deformation's strength can be changed with a single float. Then it's sent to other players to sync everything visually.
Godot has resource files that can save all custom character data. I sent all bytes of these resource files to all connecting players to make the customizations synchronize.