---
title: "Project Part 1"
date: 2022-02-04
draft: false
description: "Locomotion technique: Teleportation ball"
tags: ["Locomotion", "VR", "ball", "teleportation"]
thumbnail: https://l3apq3bncl82o596k2d1ydn1-wpengine.netdna-ssl.com/wp-content/uploads/2019/06/KATloco_5.jpg
---

It's time to start the project. As described previously in lab 3, I have chosen to implement the teleportation ball as my locomotion technique. 
Even if I know that this locomotion technique is not the most adapted for the parkour provided by the teachers I still chose to make my tests by using it. 

The first step was to import the project. I didn't encounter any problems, I simply followed the teacher's tutorial. As for the Roll-a-ball project, 
I use my phone cable to connect the headset with my computer.

Initially, I wanted to not use any buttons when throwing the ball. But I had no idea how to handle the break point of contact between the hand and the ball. 
That's why I changed my plans and decided to use the trigger. The idea is to hold the trigger down when you start the throwing motion and then release the trigger 
at the point of contact break. I could have opted for a movement where the user just presses the trigger at the breakpoint but I found the method 
I opted for much more natural and easy to use (even if it seems more restrictive at first). 

The main goal was to be able to throw a ball. For this, I used the Unity Grabber and Grabbable scripts. Thanks to this, I could catch a ball and throw it. 
However, this was not exactly what I needed because I wanted the ball to be already in my hand, to be thrown and then to come back automatically to my hand. 
That's why I removed the Grabber and Grabbable and instead used something similar to MySelect.cs that we created in the Roll-a-ball TP. 

Initially, I wanted to reuse the MySelect.cs script as is because the principle was the same, but with this code the ball appeared well below the controller. 
I thought it might have been a problem with the anchor points or something similar so I adapted the OVRGrabber code to my problem instead. I got the parts I was interested in 
(attaching and detaching the ball) and integrated it into my MySelect.cs. Here is the code I used to attach the ball to the Controller.

```c++
private Vector3 m_anchorOffsetPosition;
private Quaternion m_anchorOffsetRotation;

private Vector3 m_grabbedObjectPosOff;

protected virtual void Awake()
{
	m_anchorOffsetPosition = transform.localPosition;
	m_anchorOffsetRotation = transform.localRotation;
}

public void AttachBall()
{
	ball.transform.parent = this.transform;

	Vector3 relPos = ball.transform.position - transform.position;
	relPos = Quaternion.Inverse(transform.rotation) * relPos;
	m_grabbedObjectPosOff = relPos;
	Vector3 grabbablePosition = transform.position + transform.rotation * m_grabbedObjectPosOff;

	ball.transform.localPosition = new Vector3(0.0f, 0.0f, 0.0f);
	Rigidbody rb = ball.GetComponent<Rigidbody>();
	rb.position = grabbablePosition;
	rb.isKinematic = true;
	rb.useGravity = false;
	rb.velocity = Vector3.zero;
	rb.angularVelocity = Vector3.zero;
}
```

And here is the code used to detach the ball to the Controller when throwing the ball.

```c++
ball.transform.parent = null;
Rigidbody rb = ball.GetComponent<Rigidbody>();
rb.useGravity = true;
rb.isKinematic = false;

OVRPose localPose = new OVRPose { position = OVRInput.GetLocalControllerPosition(controller), orientation = OVRInput.GetLocalControllerRotation(controller) };
OVRPose offsetPose = new OVRPose { position = m_anchorOffsetPosition, orientation = m_anchorOffsetRotation };
localPose = localPose * offsetPose;

OVRPose trackingSpace = transform.ToOVRPose() * localPose.Inverse();
Vector3 linearVelocity = trackingSpace.orientation * OVRInput.GetLocalControllerVelocity(controller);
Vector3 angularVelocity = trackingSpace.orientation * OVRInput.GetLocalControllerAngularVelocity(controller);
rb.velocity = linearVelocity;
rb.angularVelocity = angularVelocity;
```
As you can see, it's still similar to the original MySelect.cs script.

Now, the ball appears immediately in your hand and you can throw it. However you can't teleport yet. To add it, it's quite simple since you just have to move the user's 
camera to the current location of the ball. However, I encountered a problem with the y-axis component. Indeed, initially I had written 

```c++
this.transform.position = new Vector3(currentBall.transform.position.x, this.transform.position.y, currentBall.transform.position.z);
```

but the problem is that the ground is not always flat, however in my current formula the height remains constant. So I changed it to the following line 

```c++
this.transform.position = new Vector3(currentBall.transform.position.x, currentBall.transform.position.y + this.transform.position.y, currentBall.transform.position.z);
```

This way, the change in height of the ground is taken into account in the y-axis component of the ball. Then, you just have to attach the ball to the controller 
using the same code as before.

I explained earlier that it's enough to teleport the user to the arrival point of the ball, but when is the ball considered to have arrived? 
Indeed, I noticed that the ball never stops completely. So I added a threshold at 0.8. When the ball is not attached to the controller and its speed is lower than this threshold,
I consider that it has reached its arrival point and then I teleport the user. Note that this is only valid for the petanque ball since the sticky ball will have 
to stop at the point of contact directly.

Well, now we have a good base since we can throw a ball and teleport. However, while testing I noticed that the ball collides with the box Collider which surrounds 
the start panel (and consequently the stages and end panels). To prevent this from happening, I created an Uncollide layer that I associated to the GameObject that contains the banner 
as well as a Ball layer associated to my ball, and I disabled the interactions between these 2 layers. Now the ball can go through.

![Ball first draft](http://lbertrand417.github.io/IGD301-blog/throw_ball_1st_draft.gif)

This concludes my first session on the project. In the next session I plan to solve other problems I've noticed: sometimes the ball goes through the ground 
(whereas the ground has a collider). 

![Mesh collider issue](http://lbertrand417.github.io/IGD301-blog/mesh_collider_issue.gif)


Furthermore, it's very hard to start the course because the user has to collide with the banner, and my locomotion technique is a teleportation so 
you don't necessiraly go through the collider.

![Start issue](http://lbertrand417.github.io/IGD301-blog/start_issue.gif)
