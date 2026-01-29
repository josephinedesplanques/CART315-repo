
# Exploration Prototype 1 - 29.01.26

Idea:

Since we explored in class the Unity components movement, colliders and instantiation, I wanted to create a prototype to play with those functionalities and see what I could use as code that I have read in the textbook.

I wanted to try to create a prototype where the player controls a cube and every time that cube touches a wall the wall randomly changes colors.

First I started by creating the walls of the room and giving them all a boxcollider so that the player can touch them and it can trigger an action.

I then created the 2d object square that was going to be my player. I added a BoxCollider2d as well as a Rigidbody2d with a gravity of 0 since I don’t want it to fall and I want to move it in the box freely, in all directions.

I took inspiration from the script for GottaCatchaMall to create the script for my player box. It took me a few tries to understand how to make my player move depending on the arrow input.
Since in GottaCatchaMall the basket only moves from left to right and I wanted my box to move left right up and down, I had to create a variable box_x and box_y.

My script at first wasn’t working, so I had to go and manually set the input manager to old like we had to do in class.

Now the speed felt too fast so I manually changed it to 0.01f, it was too slow now so I settled on 0.08.


Ok so at this point I was also lost because my square was walking though the walls even though they all had a box collider. And then I realized I forgot to add a RigidBody2d to my walls. So I did and set their body type as static. But I was still encountering this issue. So I manually changed the limits of the movement from the GottaCatchaMall script to make it look like my square couldn’t leave the box, even if it was manually and not through RigidBody


Then I realized I didn’t know how to execute my idea that if the box touches a wall that wall changes color given the fact that I was having trouble accessing my player’s Rigidbody. 


So I decided to follow along the 2d Essentials video tutorials from Unity Essentials to see if I could experiment with it and find an answer to my problem.

In the Pathway Unity Essentials, I looked at the programming essentials mission to look at the script from their example.







# Make-a-Thing, design reflection 22.01

I got the character sprite from itch.io, creator is Arkno:
https://project-arkno.itch.io/kayas-character-sprite


It was my first time using GB Studio. When I started looking at it, I realized the most time consuming part of making a game was going to be creating the scenes and the art for the background. 
Therefore I chose to make a labyrinth game where the player can only move from left to right.


Since I wanted the player to only move from left to right, I made a labyrinth of identical rooms made of one narrow hallway. 



To make sure the player was only going to be able to walk in a straight line, I added collisions and made sure that in the play view it looked like the character was walking on the “floor”, not flying or walking below it (it took a few tries).


My game goes as:

You walk to the right end of the room, get asked a question on which room you want to go to next. You first choose between rooms Star and Oval, then between Round and Field, then between Cats and Dogs. If you go through the right ones (example: Star then Field then Dogs), you win. If not, you lose and can restart. You need to remember which rooms you chose.



To keep track of whether the player chose the right room I had to create a Math function inside of some of my loops.
I created the global variable Victory_Path, and if the right room was picked, before getting teleported to it, +1 was added to Victory_Path.



Therefore, the trigger in the last room checks the value of Victory_Path to know which room to teleport the player to (Victory or Game Over)



If the player wants to start from the top, Victory_Path is set to 0.


Comments on things that didn’t work::

At first I wanted each room (7 of them) to look different, that's why I created 7 different scenes. But since I didn’t end up having the time to change the way they looked, I could have only had one room and changed the dialogue that asks which room to go to and still keep track of the global variable.
Instead I lost efficiency by creating all these different scenes.

When it comes to the triggers at the left and right of the room, I couldn't find a way to make it so the movement stops when the player is on the trigger and it doesn’t look like the player is walking into the wall.

Also I didn’t know how to change the key that had to be pressed to select so it is still Z, which i wanted to change.
