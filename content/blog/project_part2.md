---
title: "Project Part 2"
date: 2022-01-19
draft: false
description: "Locomotion technique: Teleportation ball"
tags: ["Locomotion", "VR", "ball", "teleportation"]
thumbnail: http://lbertrand417.github.io/IGD301-blog/thumbnail_project1.png
---


It's time to get back to the project. As described in the previous post there are two major issues to address: 
- The ball sometimes crosses the ground when it has a collider
- It is difficult to start the game. In fact, the user does not collide with the collider of the banner because of teleportation.

Today I will focus on the balls and leave the problem of starting the game for another time. After some tests and research, 
I noticed that the problem of the ball crossing the ground was linked to the collider type we use, the mesh collider. Indeed, the mesh collider has no thickness 
and the movement of the ball must be too fast. Therefore, it never touches the collider. The solution I found is to select isConvex in the collider parameters. 
This solved the problem for most tiles. However, the problem with this method is that as the sidewalk is slightly higher than the road, the collider was also 
higher than the road and so it looked like the ball was floating. For these tiles, I replaced the mesh collider by a box collider and adjusted the height to the road. 
Now, the ball goes slightly into the ground on the sidewalks but lands correctly on the road. It could be further improved by combining 3 box colliders 
(1 for each sidewalk and 1 for the road) but as the user is not really supposed to go out of the road I considered that it was not necessary.

Now that this problem is solved, it's time to create new balls. The idea is to change the ball using the thumbstick of the controller in which the ball is located 
(here the right hand). To do this, I created in the MySelect.cs (where the ball change will be managed) an array "balls[]" containing the different possible 
balls as well as an index "currentBall" indicating the ball held in hand. I also added, in this same script, a function allowing to change the ball. For this part, 
I met several problems:
- Even if the MySelect.cs script is attached to the right controller, I had to use the thumbstick of the left controller to change the ball. I think the mistake was that 
I wrote 
```c++
Vector2 primaryAxis = OVRInput.Get(OVRInput.Axis2D.PrimaryThumbstick);
```
instead of
```c++
Vector2 primaryAxis = OVRInput.Get(OVRInput.Axis2D.PrimaryThumbstick, controller);
```
- The ball change to the right worked but not to the left. The reason was that I was changing the index with this command 

```c++
currentBall = (currentBall + increment) % balls.Length;
```
but I was actually getting negative indices. 
So instead of calling balls[currentBall] I replaced it with balls[Mathf.Abs(currentBall)].

- The change was happening way too fast. Indeed, my code was in the Update() function and so the ball was changing 3 to 4 times in the interval when the thumbstick 
was moved. 

![Change ball issue](http://lbertrand417.github.io/IGD301-blog/change_ball_issue.gif)

To solve this problem I used a coroutine, in order to wait a certain time before continuing the Update(). This gives the user time to let go of the 
thumbstick before the function restarts.

So the code to change the ball looks like this:

```c++
if (primaryAxis.x > 0.95f && !isThrowing && changeBall)
{
	StartCoroutine(ChangeBall(1));
                        
}

if (primaryAxis.x < -0.95f && !isThrowing && changeBall)
{
	StartCoroutine(ChangeBall(-1));
}

private IEnumerator ChangeBall(int increment)
{
	changeBall = false;
	balls[Mathf.Abs(currentBall)].SetActive(false);
	currentBall = (currentBall + increment) % balls.Length;
	balls[Mathf.Abs(currentBall)].SetActive(true);
	locomotion.currentBall = balls[Mathf.Abs(currentBall)];
	AttachBall();
	yield return new WaitForSeconds(0.5f);
	changeBall = true;
}
```

![Change ball issue corrected](http://lbertrand417.github.io/IGD301-blog/change_ball_issue_corrected.gif)

Note that I also changed the asset of the petanque ball to make it more realistic. For this I took a file of a petanque ball for 3D impression that I converted 
into a .obj file. Then I imported it in Unity and added a metallic material to make it looks like a real petanque ball. 
However the anchor point of the object was different from the other balls so It wasn't attached properlyto the controller. My solution was to create an EmptyObject which
will contain my petanque prefab. Then, I moved the ball inside such that the anchor point of the empty object was in the middle of the ball.
This way the anchor point was back to its normal position.

![Demo petanque ball](http://lbertrand417.github.io/IGD301-blog/demo_petanque_ball.gif)

Now that I have different balls, I also have to deal with the properties of the different balls. For this, I created a new Ball.cs script attached to each ball, 
which stores their properties. A ball has a type (sticky, petanque, bouncy), a speedFactor and a velocityThreshold. The speed factor is used to determine 
the speed of the ball when it is thrown. Indeed, I noticed that when throwing a ball with only the speed of the controller, the ball didn't go far enough 
(even when decreasing the mass). The speed factor increases the speed that's why I replaced 

```c++
rb.velocity = linearVelocity;
```

by 

```c++
rb.velocity = balls[Mathf.Abs(currentBall)].GetComponent<Ball>().speedFactor * linearVelocity;
```

in the code to throw the ball. Beyond that, 
the speedFactor is useful because each ball has different properties: we must be able to throw the bouncy ball very far unlike the petanque ball.


I talked about it in the previous post but the sticky ball should induce teleportation with the user upon collision with an object. 
For that, in the Ball.cs script I added an OnCollideEnter() which checks that the ball is of type Sticky and which puts the speed at 0 in this case.


```c++
private void OnCollisionEnter(Collision collision)
{
	Rigidbody rb = this.GetComponent<Rigidbody>();
	if (type == Type.sticky || (type == Type.bouncy && rb.velocity.magnitude < velocityThreshold))
	{
		rb.velocity = Vector3.zero;
	}
}
```

![First draft sticky ball](http://lbertrand417.github.io/IGD301-blog/sticky_ball_1st_draft.gif)

Finally, I noticed that there were still problems with my teleportation formula when we were on a hill. As I said last time, I put 

```c++
this.transform.position = new Vector3(currentBall.transform.position.x, currentBall.transform.position.y + this.transform.position.y, currentBall.transform.position.z);
```
 
But in reality the this.transform.position.y part, which is supposed to represent the user's height, increases as you go up, which is not right. 
To fix this problem I created a constantHeight variable that takes the value of this.transform.position.y when the game starts and I use this one instead. 
So I end up with 

```c++
this.transform.position = new Vector3(currentBall.transform.position.x, currentBall.transform.position.y + constantHeight, currentBall.transform.position.z);
```

These are all the changes made during this session. There is still the banner problem to solve and another problem I found during my tests today: 
the ball sticks to the coins, and when it sticks the user teleports onto the coin.

![Coin issue](http://lbertrand417.github.io/IGD301-blog/collide_coins.gif)

![Coin issue](http://lbertrand417.github.io/IGD301-blog/sticky_coin_issue.gif)