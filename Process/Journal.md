# Exploration Prototype 3 - 12.02.26

For this week’s exploration prototype, I wanted, just like last week, to build on top of the prototype we saw in class. 
This week, it was Breakout. I wanted to test what it would feel like if one of the bricks was not red but another color, and if that brick was special in the fact that once it gets destroyed, it gives +5 points to the player and spawns a few more balls on the level. 

First, I had to make it so that one of the bricks had to randomly be a different color. In order to do so, I made changes in the BrickLayerManager script, created a random Index integer, and a count integer set to 0.
Within the for loop, if the brick instantiated is the number brick that equals the random index number, that brick will be colored purple. I chose a randomized Index this way every time you play, it will be a different block.

![start Image](Media/Start1)


Now, in addition to having a different color, that special block needs to be assigned a boolean that is true, while all the other blocks need to have a boolean that is false.

I created the boolean Special, set it to false in BrickScript, and within BrickLayerManager I added:
```csharp
if (count == Index){

   BrickX.GetComponent<SpriteRenderer>().color = Color.purple;
   
   BrickScript Nx = BrickX.GetComponent<BrickScript>();
   
   Nx.Special = true;
}
```
Now it is time to make it so that my special brick is worth more points and spawns more balls.
I realized that the points attributed were already high, and randomized between 10 and 20. So I want my special brick to give 35 points.
So in the if loop that checks if we hit a brick in the BreakoutBall script, I added:
```csharp
BrickScript isBrick = other.gameObject.GetComponent<BrickScript>();

if (isBrick != null && isBrick.Special){
          GameManager.S.AddPoint(35); }
```
And it is within that if loop that I will instantiate the different balls.

I started by creating a prefab of the Ball so that I could duplicate it.

I then added this script to the previous loop in order to instantiate three balls when the special brick is destroyed:

```csharp
if (isBrick != null && isBrick.Special) {

            GameManager.S.AddPoint(35);
            
            for (int i = 0; i < 3; i++) {
            
            GameObject newBall = Instantiate(prefabBall, transform.position, Quaternion.identity);
            
            BreakoutBall rd = newBall.GetComponent<BreakoutBall>();
            
            rd.ballSpeed = Random.Range(8f,14f);}
            
}

```

Once the special bloc is destroyed, three balls appear at the same starting position where the main ball is when you start the game, and they fall when you press the spacebar or if the ball pushes them, because they have physical components.

![second Image](Media/Start2)

The speed didn’t work like if I set it up like this, I need to play with the velocity.

Having a brick worth so many points basically means that the goal of the game becomes to break that one in priority. The other balls appearing seem to add a bit of messiness/fun?? Not sure, will definitely need more iterations.

Next steps:
- fixing the speed issue
- making the new balls random colors
- iterating to see how many new balls add more fun without adding too much messiness
- experimenting with singletons


# Exploration Prototype 2 - 05.02.26

Reflection on last week:

Last week I was too ambitious, this week I want to focus on testing ideas that only need me to manipulate the pre existing code we have for Pawng.

Pong is a classic game with a very simple mechanic, leaving room for changes.

First, I wanted to test how the game feel would be impacted if every time the ball touched a paddle, it went a little faster.

At first I added 2 lines in the BallScript script:

*`if (wall.gameObject.name == "paddleLeft" || wall.gameObject.name == "paddleRight") {`*

   *`blip.pitch = 1f;`*
   
   *`blip.Play();`*
   
   ***`ballSpeed += 0.5f;`***
   
   ***`rb.linearVelocity = rb.linearVelocity.normalized * ballSpeed;`***
   
*`}`*

I thought ballSpeed was in charge of how fast the ball was going but realised that changing ballSpeed  didn't lead to any change. I realised that the velocity was what needed to be changed for the speed to change.

So I replaced what I added with:

*`if (wall.gameObject.name == "paddleLeft" || wall.gameObject.name == "paddleRight") {`*

   *`blip.pitch = 1f;`*
   
   *`blip.Play();`*

   ***`rb.linearVelocity *= 1.5f;`***
   
*`}`*

1.5f was too high, the game was getting way too crazy too fast, so after some changes I settled with ***`*1.1f`*** so that the game still became challenging quickly, but it didn’t feel too overwhelming straight away.

I then wondered how it would affect the player if the blip sound had a higher pitch every time it hit a wall.

So I changed the script under:

*`if (wall.gameObject.name == "topWall" || wall.gameObject.name == "bottomWall")`*

and:

*`if (wall.gameObject.name == "paddleLeft" || wall.gameObject.name == "paddleRight")`*

to:

***`blip.pitch += 0.05f;`***

***`blip.Play();`***

In Reset() I also had to add: 

***`blip.pitch = 1;`***

to make sure that the pitch would reset when the game restarts.

This change in pitch, in addition to the acceleration of the ball, adds a stress level to the game, making the player more tense and playing with a sense of urgency.

Now I wanted to make the game a little more unpredictable and wanted to test how it would feel if every time the ball touched a wall, it turned into a different shape.

I added the square and triangle sprites to my sprites folder in my assets and created an array **ShapeArray** of 3 that had the ball sprite assigned to 0, the square sprite assigned to 1 and the triangle sprite assigned to 2.

I then added in these two lines to my script under
*`if (wall.gameObject.name == "topWall" || wall.gameObject.name == "bottomWall") {`* :

***`int randomNumber = Random.Range(0, 3);`***

***`GetComponent<SpriteRenderer>().sprite = ShapeArray[randomNumber];`***



And here I have it! The shape of the ball now changes when it touches a wall, on top of the speed increasing when it touches a paddle and the pitch becoming higher when a wall or a paddle is touched.

![Image proto2](Media/Proto2.png)

These were not super complicated add ons but they helped me understand better how to manipulate a script using velocity, arrays, sprites etc. I also got to see how even the smallest changes can affect a game's feel.

It also helped me see how I could improve my Exploration Prototype 2 if I wanted to.

Next steps:

to make the game even more interesting, in the future I could:
- add color changes (the ball could change colors, the walls could change colors...)
- make it so the speed of the ball after it hits a paddle is random (could get faster, slower etc) to add even more unknown factors
- add 2 paddles, make it a 4-player game


# Exploration Prototype 1 - 29.01.26

Idea:

Since we explored in class the Unity components movement, colliders and instantiation, I wanted to create a prototype to play with those functionalities and see what code I could use that was in the textbook or in the GottaCatchaMall or in the Unity Essentials projects.

I wanted to try to create a prototype where the player controls a cube and every time that cube touches a wall that wall randomly changes colors.

First I started by creating the walls of the room and giving them all a boxcollider so that the player could enter in contact with them.

I then created the 2d object square that was going to be my player. I added a BoxCollider2d as well as a Rigidbody2d with a gravity of 0 since I don’t want it to fall and I want to move it in the box freely, in all directions.


I took inspiration from the script for GottaCatchaMall to create the script for my player box. It took me a few tries to understand how to make my player move depending on the arrow input.
Since in GottaCatchaMall the basket only moves from left to right and I wanted my box to move left right up and down, I had to create a variable box_x and box_y.

My script at first wasn’t working, so I had to go and manually set the input manager to old like we had to do in class.


Ok so at this point I was also lost because my square was walking though the walls even though they all had a box collider. And then I realized I forgot to add a RigidBody2d to my walls. I met that change and set their body type as static. But I was still encountering the same issue. So I manually changed the limits of the movement from the GottaCatchaMall script to make it look like my square couldn’t leave the box, even if it was manually and not through RigidBody collision.

Then I realized I didn’t know how to execute my idea that if the box touches a wall that wall changes color given the fact that I was having trouble accessing my player’s Rigidbody. 

So I decided to follow along the 2d Essentials video tutorials from Unity Essentials to see if I could experiment with it and find an answer to my problem.

In the Pathway Unity Essentials, I looked at the programming essentials mission and took a look at the script from their example. In the mission the goal is to control a robot vacuum that can move and rotate in a room. 
In the robot vacuum tutorial they had
private Rigidbody rb; // Reference to player's Rigidbody.
And 
rb = GetComponent<Rigidbody>(); // Access player's Rigidbody.
to access the object’s Rigidbody which seemed like a good start to fix my problem.

But I decided to turn my focus to using the rest of the tutorial to implement a collectible in my prototype. I created a script and wanted to add it to my collectible object but then I kept getting a “Can't add script” error message that I couldn’t fix.

So, once again, I decided to pivot and change direction. I couldn’t figure out how to implement rigidbody, I didn’t know how I was going to go about changing the color of walls on impact and I didn’t understand why my new script wouldn’t attach itself to a new object. I wanted to end on a victory :) so I simply wanted to add a random cube falling.

I looked at the script from GottaCatchaMall BuildingDropperScript and took inspiration from it.
I used this line of code:

GameObject currentBuilding = Instantiate(building, buildingPos, transform.rotation);

Renaming it, and using a if (Input.GetKeyDown(randomKey)); function, I managde to make it so that when I press the spacebar, an object falls.

I assigned this object a Rigidbody and box collider and funny enough, it does get blocked by the walls and can be moved by the player square. I’m sure the answer to why my player square can move past the walls is right in front of me.

So now I have a prototype where the player controls a square, and that square can push around a small yellow ball within a rectangular room.

Definitely a lot of trial with errors, I’m still confused on how the rigidbody works.

Next steps:
- fixing the rigidbody issue on the walls
- making more balls fall down
- making the walls change colors when they are interacted with, maybe do the same with the balls?








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
