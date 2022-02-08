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

```c++
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.InputSystem;

using TMPro;

public class PlayerController : MonoBehaviour
{

    public float speed = 0;

    private Rigidbody rb;
    private float movementX;
    private float movementY;
    private int count;

    public TextMeshProUGUI countText;
    public GameObject winTextObject;

    // Start is called before the first frame update
    void Start()
    {
        rb = this.GetComponent<Rigidbody>();
        count = 0;
        SetCountText();
        winTextObject.SetActive(false);
    }

    // Update is called once per frame
    void Update()
    {
       
    }

    void OnMove(InputValue movementValue)
    {
        Vector2 movementVector = movementValue.Get<Vector2>();

        movementX = movementVector.x;
        movementY = movementVector.y;
    }

    private void FixedUpdate()
    {
        Vector3 movement = new Vector3(movementX, 0.0f, movementY);

        rb.AddForce(movement * speed);
    }

    private void OnTriggerEnter(Collider other)
    {
        if (other.gameObject.CompareTag("pickups"))
        {
            other.gameObject.SetActive(false);
            count += 1;
            SetCountText();
        }
    }

    void SetCountText()
    {
        countText.text = "Count: " + count.ToString();
        if (count >= 30)
        {
            winTextObject.SetActive(true);
        }
    }
}
```

Now we have to deal with the pick-ups. First of all, they are also RigidBody so we need to add the component. 
However, unlike the ball, they are not subject to gravity so we need to uncheck the associated box. 
We must also check the isTrigger box of the collider so that the ball can interact with the pick-ups without colliding, as well as the isKinematic box of RigidBody. 
This last box allows the objects not to react to collisions and forces.

We also want to make the pick-ups a little more dynamic. To do this, we create a new Rotator script that we attach to the Prefab (not to the objects themselves!). 
This script will only make a rotation at each update.

```c++
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Rotator : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        transform.Rotate(new Vector3(15, 30, 45) * Time.deltaTime);
    }
}
```


Finally we want the camera to follow the movement of the ball.

So we have to create a last script, attached to the camera. The function calculates the offset between the camera and the ball at the beginning of the game 
and makes sure that this offset is constant throughout the game.

```c++
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraController : MonoBehaviour
{

    public GameObject player;

    private Vector3 offset;

    // Start is called before the first frame update
    void Start()
    {
        offset = transform.position - player.transform.position;
    }

    // Update is called once per frame
    void LateUpdate()
    {
        transform.position = player.transform.position + offset;
    }
}
```

The last step is to create the build and here we have our first working game! Here is a small demonstration of the result.


![Game result](https://raw.githubusercontent.com/ruthnaebeck/rollABall/master/readme/Demo.gif?raw=true)


## VR version

Now that we have a working game in Unity, it's time to adapt it to VR. To do this, let's start by creating another 3D project. 
Then we need to import the plug-in needed for VR Oculus XR Plugin, add the necessary assets (Oculus integration found in the Asset Store) 
and change the project properties (remove Vulkan). Finally we have to change the Build platform to Android. All these operations were done without any problem. 
The only thing that happened was that when I downloaded the Oculus integration, Unity told me that some plugins had to be upgraded and I simply did OK and restarted Unity. 
As for the setup on the headset itself, I didn't need to do anything contrary to what was written in the tutorial. 
I just had to plug the headset to my computer and everything was already working. Speaking of connecting the headset to the computer though, 
I couldn't use the cable that came with the headset because my computer doesn't have a USB-C port and I don't have an adapter either. 
So I used the cable from my phone charger which has a USB-C on one side and a USB on the other side.

![Unity setup](http://lbertrand417.github.io/IGD301-blog/setup_VR_roll.png)

Now that the project is set up, it is necessary to import the work already done in the previous steps. 
To do this, we return to the first Unity project and export it as a .unitypackage. Then we import this file into our new project. 
When we do this, some errors appear because we have a script called PlayerController but this name is already used in the Oculus Integration scripts. 
So I simply changed the name of my script to MyPlayerController. It is also necessary to remove the lines of code relative to InputSystem 
because here we will use the controllers.

We can now create a new scene in which we will add a floor (the floor on which the player will walk), as well as a table where our roll-a-ball game will be. 
To be able to move in the environment we need a special camera, provided by the Oculus integration asset. So, we add in our scene OVRCameraRig and we change 
the tracking origin to "Floor level". In the GameObjects that correspond to the hands, we add as a child the Prefab of the controllers in order to make them appear 
in the scene. We can already test our result to have a preview of the environment. To test, I just connected the headset with the cable and did a build with my desired scene. 

![VR environment](http://lbertrand417.github.io/IGD301-blog/player_environment.gif)

Now, we can add our roll-a-ball game to the environment. To do this, we create a new EmptyObject in which we put all the elements of the game 
(the board, the pick-us, the ball,...). We add to this object a Box collider so that it can interact with the external environment. 
However, we don't want the objects inside to interact with this collider. To solve this problem we create two layers: "selection" which represents 
the whole game and "roll-a-ball" which represents all the elements of the game. Then, we disable the interaction between the "selection" object and the 
"roll-a-ball" objects. Finally, we add a RigidBody with gravity so that the board has a realistic behavior.
Concerning the controllers, we add sphereCollider with trigger and rigidBody without gravity but with Kinematic.

![VR with roll-a-ball](http://lbertrand417.github.io/IGD301-blog/roll_a_ball_environment.gif)

As for the version without VR, we also add a canvas to display the score (without forgetting to update the parameters of the script MyPlayerController).

Finally, we only have to add the interaction between the controllers and the game board. For this, we create a script attached to the 
controllers which will allow to grab the board. The idea is simple: when the controller is in the collider, if the trigger is pressed then we catch the object, 
otherwise we let it go. To catch the object you just have to put the controller as a parent (so the game will follow the controller's movement), 
disable gravity and set the movement speeds to 0. To drop it, it is the opposite. We remove the parent, we put back the gravity and we add a speed depending 
on the speed of the controller.


When I tested the game I made some mistakes so the interaction did not work. First, instead of writing 
```c++
other.gameObject.name == "Roll-a-ball"  
```
I wrote 
```c++
other.gameObject.name == "roll-a-ball" 
```
so it never entered the loop. Then I forgot to change the size of the box collider I added in the roll-a-ball 
object so there was never a trigger.

Our final code for MySelect.cs is:

```c++
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MySelect : MonoBehaviour
{

    public OVRInput.Controller controller;
    private float triggerValue;
    [SerializeField] private bool isInCollider;
    [SerializeField] private bool isSelected;
    private GameObject selectedObj;

    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        triggerValue = OVRInput.Get(
			OVRInput.Axis1D.PrimaryIndexTrigger, 
			controller);

        if(isInCollider)
        {
            if(!isSelected && triggerValue > 0.95f)
            {
                isSelected = true;
                selectedObj.transform.parent = this.transform;
                Rigidbody rb = selectedObj.GetComponent<Rigidbody>();
                rb.isKinematic = true;
                rb.useGravity = false;
                rb.velocity = Vector3.zero;
                rb.angularVelocity = Vector3.zero;
            }

            else if (isSelected && triggerValue < 0.95f)
            {
                isSelected = false;
                selectedObj.transform.parent = null;
                Rigidbody rb = selectedObj.GetComponent<Rigidbody>();
                rb.useGravity = true;
                rb.isKinematic = false;
                rb.velocity = OVRInput.GetLocalControllerVelocity(controller);
                rb.angularVelocity = OVRInput.GetLocalControllerAngularVelocity(controller);
            }
        }
    }

    void OnTriggerEnter(Collider other)
    {
        if(other.gameObject.name == "Roll-a-ball")
        {
            isInCollider = true;
            selectedObj = other.gameObject;
        }
    }

    void OnTriggerExit(Collider other)
    {
        if(other.gameObject.name == "Roll-a-ball")
        {
            isInCollider = false;
            selectedObj = null;
        }
    }
}
```



Here is the final result of the Roll-a-ball VR game:


![Final game](http://lbertrand417.github.io/IGD301-blog/final_game1.gif)

![Final game](http://lbertrand417.github.io/IGD301-blog/final_game2.gif)