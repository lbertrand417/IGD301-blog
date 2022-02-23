---
title: "Third lab"
date: 2021-12-28
draft: false
description: "VR Locomotion"
tags: ["Locomotion", "VR"]
thumbnail: https://l3apq3bncl82o596k2d1ydn1-wpengine.netdna-ssl.com/wp-content/uploads/2019/06/KATloco_5.jpg
---

# Lecture Homework


The goal of this homework is to propose different types of locomotion techniques to naviguate in the virtual environment.

## Swimming locomotion

![Ocean](https://cdn.akamai.steamstatic.com/steam/apps/422760/ss_0bf5988da6f464d136e1f1e92506f8a71907f9d7.1920x1080.jpg?t=1517860245 "Ocean")


I want to design a locomotion technique that could be used in an aquatic environment. 
The purpose of my locomotion is to move freely in the water and enjoy the environment. 
It is therefore more suitable for underwater exploration games, without stress or constraints, 
even if we could imagine more complex scenarios (speed game where you have to escape from a shark, underwater escape game,...). 
And what better movement than the breaststroke to explore the sea bed? No need to move your feet, you just have to use your hands and draw circles, as for a breaststroke movement. 
We could use the tracking of the remote controls to obtain the position of the hands and the speed of the movement and thus deduce the speed of the movement in the virtual environment. 
The disadvantage of this locomotion technique is that the user does not really move in the real world, which can lead to motion sickness. 
However, Minority Media discovered that moving in water doesn't cause as much motion sickness as walking on the ground. Hence, we can restrict the motion to only arm movements. 
Furthermore, we could even adapt this locomotion technique to other environments such as flying in the sky, a bit like Peter Pan. 

Below, an example of breaststroke motion in a VR environment:

{{< youtube C4eu0saLiOQ >}}

To evaluate the quality of the locomotion technique, we could compare the motion sickness felt during a standard movement on the ground and our breaststroke movement in water. 
The metrics to collect are speed, ease of learning, ease of use and presence. Indeed, the movement made by the user must be coherent with the generated displacement so we must study the speed. 
Moreover, I focused on exploration games, so it is important that the locomotion technique is instinctive and easy to use. 
In this sense, the fact that the movement is inspired by a breaststroke movement makes it easier to learn. 
Finally, the presence is a very important criterion since the main goal of exploration games is to be completely immersed in the environment.


## Paddle locomotion

![Paddle](https://www.realite-virtuelle.com/wp-content/uploads/2021/07/Kayak_VR_image3-1024x576-1.jpg "Paddle")

In this scenario we try to find an adequate locomotion technique to play in small spaces. Indeed, many users (myself included) don't have enough space in their house to move around. 
So we have to find a way of locomotion in the virtual environment that leaves the user static in the real environment. To stay in the aquatic theme, we will try to move on water. 
If we imagine the user in a boat, a canoe, a kayak,... we can use the hand controllers to imitate the movement that we make when we row, which we add to the "automatic" motion due to the current. 
The movement uses the controlers as well and the motion path depends on the type of oar used. 

- One oar with one flat blade

If the user has only one oar with both hands are on the same oar, the movement should be straight on the side of the oar. 

![One oar](https://cdn.dribbble.com/users/230073/screenshots/3780396/kayak8.gif "One oar")

- Two oars

If there are two oars (one per hand), the user must do a circular movement on the desired side or on both sides to go straight. 

![Two oars](https://mir-s3-cdn-cf.behance.net/project_modules/fs/3ae4f472574477.5bec032ec5810.gif "Two oars")

- One oar with two flat blades

If there is a large oar with two flat blades, the user must do a 8 move to go straight. If we wants to turn he can do the same motion like for the one oar with one flat blade one. 

![One oar with 2 flat blades](https://i.pinimg.com/originals/ef/d8/16/efd816c1af96ef7b6e0196e3eecaba89.gif "One oar with 2 flat blades")

Note that this motion can be used in other cases than rowing. In skiing, the movement to push on the poles is very similar and we could therefore adapt our locomotion technique to this activity.

The concern with this move is that it can make you sick, as the user remains static. Especially if there are rapids in the game and he moves quickly in the game. 
If it were possible, the best solution to solve this problem would be to use a fake boat, which vibrates and moves according to the movements in the game like the example below.

![Kayak simulator](https://www.vr-show.fr/wp-content/uploads/2020/01/Animation-simulateur-Kayak-VR.jpg "Kayak simulator")

For places that have enough space we could further improve the movement by allowing the user to use a handle (a broom handle for example) and hang the controlers on it so that the movement seems more realistic. 

{{< youtube UZqK1qzkWUI >}}

The metrics to evaluate would be accuracy as the direction should be coherent with the motion of the user, the spatial awareness because we use a smooth locomotion technique,
information gathering and presence. Presence is a really important metric as it is the reason why researchers develop smooth locomotion technique even if it induces 
motion sickness. Thus, we also need to evaluate motion sickness as the user doesn't move as much in the real world as in the virtual world.


## Teleportation ball

The locomotion technique is different from the two previous ones because it is inspired by teleportation. 
As you know, moving in the virtual environment without moving in the real world can cause nausea and headaches. 
What could be more unpleasant than being sick while playing! I myself have experienced this kind of symptoms while playing and it is really annoying.
The teleportation allows to suppress this motion sickness since you don't move in the virtual world. However, the presence is less good since it doesn't represent the reality. 
In order to make up for the lack of presence I looked for a way to make teleportation more attractive. For that I tried to make teleportation a game in its own. 
I was inspired by the game spell 'n' stuff which uses objects to move. 


![Spell 'n' stuff](http://lbertrand417.github.io/IGD301-blog/spell_n_stuff.gif)


The idea is to throw an object and the player will be teleported to the place where the object lands. 
However, the game also uses smooth locomotion techniques to fly, which can cause motion sickness. Thus for my project, I restricted myself to teleportation only. 
The user has access to different balls with different properties: a petanque ball, a bouncing ball and a sticky ball. Each ball being different, 
it is better to use a specific ball according to the context. The petanque ball does not allow to go far but allows to be quite precise, 
the bouncing ball allows to move very far but has a bad precision and the sticky ball can be used for hard-to-reach places but does not allow to go far. 
This idea is quite extensible because we can imagine many other kinds of balls, even balls requiring other objects like a golf ball or a pistol ball. 

- Petanque ball

![Petanque ball](http://lbertrand417.github.io/IGD301-blog/petanque_ball.gif)

- Bouncing ball

![Bouncing ball](http://lbertrand417.github.io/IGD301-blog/bouncing_ball_compresse.gif)

- Sticky ball

![Sticky ball](http://lbertrand417.github.io/IGD301-blog/sticky_ball_compresse.gif)

To change balls I have two ideas in mind. The first one would be that the user has all the balls available on his belt and can take the one he wants to use. 
The other idea would be to use a button to change the ball, with a temporary display on a canvas in front of the user to indicate which ball he has in his hand. 
The first idea seems better in terms of presence because the user could interact with a virtual body but the second one allows to create an unlimited number of ball types. 
The user should also be able to exchange the hand ball according to his preference using a button.
Finally, I imagined that the user has to press a button to actually teleport at the ball's position so that if the throw is bad he can try again.

Regarding the movement that the user has to do, I imagined that he has to simulate a ball throw, without having to press any buttons. 
However, due to the limitations of the headset camera field, this seems complicated. That's why I think that the user will have to press a button 
(the trigger for example) at the moment when the ball should be thrown. Also, to make the game easier it could be interesting to show the future trajectory of the ball before the throw. 
In this case, instead of using the acceleration of the controller at the moment of the throw, we would use only the position of the controller and we would deduce the force of the throw. 
Thus, whatever the way the user throws the ball, it will go to the indicated point.

![Throw ball](http://lbertrand417.github.io/IGD301-blog/throw_ball.gif)

![Bowling ball](http://lbertrand417.github.io/IGD301-blog/bowling.gif)

Concerning the evaluation of the locomotion method, I would like to evaluate the fun of the user through a questionnaire and compare it to the fun of a simple teleportation. 
I also want to study the ease of learning and ease of use, which are two criteria related to fun. Indeed, if the tool is too hard or too easy to learn or use it becomes less funny. 
Finally, I want to study the precision since the goal is to have more or less precise balls according to their type. 
For this, I could ask the user to try to aim at an area and evaluate the success rate according to the type of ball.

Finally I chose to implement the last studied locomotion technique because it is the most adapted to the given Parkour. 
If I had taken on of the 2 others I would have had to create myself an aquatic environment and being a beginner in VR it seemed to me a bit ambitious.

