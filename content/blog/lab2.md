---
title: "Second lab"
date: 2021-12-28
draft: false
description: "Roll-a-ball"
tags: ["Unity", "VR", "Tutorial", "Roll-a-ball"]
thumbnail: https://connect-prd-cdn.unity.com/20200720/learn/images/98d62c5a-f856-4b1f-ae9f-d92fc780aa8a_MASTER.png
---


# Roll-a-ball

It's time to create the first Unity project! The teachers initially asked us to follow the Roll-a-ball tutorial but to test other more complex tutorials 
in case we were already familiar with Unity. I had already used Unity for previous projects but I found that I was still very new to the software so 
I preferred to follow the Roll-a-ball tutorial anyway to get a good start.

The tutorial is mostly about following the tutorial so there is not really much to explain. I'm going to tell you a little bit about the different steps to follow. 
I'm not going to explain the steps in the order in which I followed them but rather I'll separate the Scene part from the Scripts part.

## Create a Unity project

The first step is to create a Unity project. To do this we select the type of project (here, a 3D project), its name and its location.

![Project](http://lbertrand417.github.io/IGD301-blog/begin_project.png)


## Create the scene

Once the project is created, the first step is to create the scene. To do the ground we create a GameObject plane, 
which we resize to the desired size. Unity also allows you to create materials, which you can then add to GameObjects in order to 
give them particular properties (here a blue color to the ground). 

![Plane](http://lbertrand417.github.io/IGD301-blog/plane_setup2.png)

In the same way we create the ball (which represents the player here).

![Ball](http://lbertrand417.github.io/IGD301-blog/balle_setup.png)

For walls, the easiest way is to first create an Empty GameObject and then create GameObjects that will be children of it. 
This creates a hierarchy and makes both the code and the scene easier to understand. The wall itself is composed of 4 cubes that have been deformed and rotated. 
To go faster, just create one of the 4 walls and then duplicate it and move and rotate it.

![Walls](http://lbertrand417.github.io/IGD301-blog/walls_setup.png)

Finally, for the objects to catch we use a GameObject cube. Here, unlike the previous objects, we use a Prefab, 
which allows us to modify all the copies of this Prefab. Thus, all the pick-ups of the scene are only duplicates of this Prefab 
and therefore if we change a property (for example, the size or the color) all the instances will be modified too. 
This is a very powerful tool that should not be missed.

![Pick-ups](http://lbertrand417.github.io/IGD301-blog/pickups_setup.png)

Also, don't forget to adjust the light and camera parameters, so that the scene is well visible.

![Lights](http://lbertrand417.github.io/IGD301-blog/light_setup.png)

![Camera](http://lbertrand417.github.io/IGD301-blog/camera_setup.png)

One last thing that can be added is text allowing the user to track his score in real time and know when he wins. 
For this we use a canvas which will display/update the text depending on the state of the game.

![Canvas](http://lbertrand417.github.io/IGD301-blog/canvas_setup.png)

## Add scripts

Now that our scene is complete it's time add so functionalities to our GameObjects.

The ball is a solid object subject to physics. Therefore, it must have a Collider as well as a RigidBody. 
Moreover, we want the ball to move thanks to the user. For this we need to add an extra package, called Input System. 
This package has the PlayerInput script which allows to manage the user's input. Here it uses the arrows to move the ball.

![Package](http://lbertrand417.github.io/IGD301-blog/package.png)

![Player Input](http://lbertrand417.github.io/IGD301-blog/player_input.png)

Now we have to actually move the ball. For this we create a new PlayerController script. In it, 
we instantiate a function that retrieves the user's input in order to determine the movement of the ball, then we update the movement thanks to the RigibBody.

Now that the ball is moving, we have to manage the collision between it and the other objects. 
As it collides with all the other objects they must all contain a Collider (it is added by default when we create the GameObject). 
However the collision with the pickups is different. We want :

- The pickup disappears
- The counter increases

To do this we will add a tag to the pickup prefab, which will allow us to separate the pickups from the other objects. 
Then we just have to study the object in contact with our ball. If it has the tag "pickups" then we must make the object disappear, increment the counter and update 
the display on the canvas.

When the user has caught 30 or more objects, he/she has won and "You win!" is then displayed on the canvas.

You can find the final code of the PlayerController script below

![Player Controller](http://lbertrand417.github.io/IGD301-blog/player_controller3.png)

![Player Controller](http://lbertrand417.github.io/IGD301-blog/player_controller4.png)

Now we have to deal with the pick-ups. First of all, they are also RigidBody so we need to add the component. 
However, unlike the ball, they are not subject to gravity so we need to uncheck the associated box. 
We must also check the isTrigger box of the collider so that the ball can interact with the pick-ups without colliding, as well as the isKinematic box of RigidBody. 
This last box allows the objects not to react to collisions and forces.

We also want to make the pick-ups a little more dynamic. To do this, we create a new Rotator script that we attach to the Prefab (not to the objects themselves!). 
This script will only make a rotation at each update.

![Rotator](http://lbertrand417.github.io/IGD301-blog/rotator.png)

Finally we want the camera to follow the movement of the ball.

So we have to create a last script, attached to the camera. The function calculates the offset between the camera and the ball at the beginning of the game 
and makes sure that this offset is constant throughout the game.

![Camera Controller](http://lbertrand417.github.io/IGD301-blog/camera_controller.png)

The last step is to create the build and here we have our first working game! Here is a small demonstration of the result.


![Game result](https://raw.githubusercontent.com/ruthnaebeck/rollABall/master/readme/Demo.gif?raw=true)

