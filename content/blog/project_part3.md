---
title: "Project Part 3"
date: 2022-02-04
draft: false
description: "Locomotion technique: Teleportation ball"
tags: ["Locomotion", "VR", "ball", "teleportation"]
thumbnail: https://l3apq3bncl82o596k2d1ydn1-wpengine.netdna-ssl.com/wp-content/uploads/2019/06/KATloco_5.jpg
---

Today my goal is to solve the problems with the coins and banners and to add the bouncing ball. As a reminder, currently the balls collide with the coins.
This is a problem because the coins are not supposed to be solid, and also because when you use the sticky ball you teleport onto the coin.
Since the locomotion is done with the ball, I decided that the ball represents the movement of the user. Therefore, the player must be able to catch coins 
with the balls and the balls themselves must trigger the passage under the banners. For that, I took the part of the code already present initially for the management 
of the coins and banner, and I added it to my script Ball.cs.

```c++
void OnTriggerEnter(Collider other)
{
	if (other.CompareTag("banner"))
	{
		this.GetComponent<AudioSource>().Play();
		parkourCounter.locomotionTech.stage = other.gameObject.name;
		parkourCounter.isStageChange = true;
	} else if (other.CompareTag("coin"))
	{
		parkourCounter.coinCount += 1;
		this.GetComponent<AudioSource>().Play();
		other.gameObject.SetActive(false);
	}
	// These are for the game mechanism.
}
```

Then, I used the same process as what we had done in the TP Roll-a-ball so that there is no collision but a trigger. 
To do this, I added a rigibody to the coins, deactivated the gravity, and activated the kinematics. I also checked isTrigger in their collider in order to 
activate the OnTriggerEnter() function of the ball. After some trial and error due to carelessness I finally got a working result. 
Finally I used the same concept for the banner. I removed the Uncollide and Ball layers that I had initially created and I added a RigidBody to the objects 
containing the banners by putting the same parameters as for the coins. Now, we can start the steps by throwing the ball as well as retrieving coins.

I didn't do much more today. Next time I will correct other issues that I detected : when you throw the ball and use the trigger again during the throwing, 
the ball comes back to your hand. Also, if you just click on the trigger (without doing the throwing move) the ball is still thrown (well, it's more falling than 
being thrown but you get the idea). Same when you try to change the ball when it's already been thrown. I will also add the mechanics for the bounciness of the bouncy ball
because right now it doesn't bounce. Finally, I will code the part to change hands (for lefthanded players).