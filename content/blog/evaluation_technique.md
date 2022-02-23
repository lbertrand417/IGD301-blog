---
title: "Locomotion Evaluation"
date: 2022-01-31
draft: false
description: "Locomotion technique: Teleportation ball"
tags: ["Locomotion", "VR", "ball", "teleportation", "Evaluation"]
thumbnail: http://lbertrand417.github.io/IGD301-blog/thumbnail_project1.png
---

# Locomotion technique evaluation 

The last step is to evaluate my locomotion technique. For this purpose, I asked 3 people to test it and I myself took part in the evaluation. 
The test session went as follows: a first phase of discovery, during which the player tests the locomotion (I don't answer questions, they must discover by themselves). 
It lasts about 2 to 3 minutes. Then, follows a 10-minute test phase during which the player must complete the course, as many times as he wants, 
as quickly as possible and by catching as many coins as possible. Finally, he must answer a questionnaire with the following questions: 
"On a scale from 1 to 10, how much motion sickness do you perceive right now", "On a scale from 1 to 10, how present did you feel in the virtual world", 
"On a scale from 1 to 10, how much fun did you have during the task", 10 being the highest score. They also had to give in a few words their opinion on 
the locomotion technique as well as compare it to other locomotion techniques they had tested elsewhere. 

The graph below summarizes the scores given to the 3 questions:

![Ball locomotion results](http://lbertrand417.github.io/IGD301-blog/BallLocomotionResults.png)

Overall, players din't experience motion sickness at all. This result was predictable since we use teleportation. Concerning the presence, 
the results are rather low which was also predictable since it's the disadvantage of teleportation. However, the feedback is positive because most of them had 
a lot of fun, even if one of them didn't like it because he found the system a bit redundant. 

The graphs below show a comparison between my locomotion and that of the other students. The presence of my locomotion is lower than that of the other students, 
which again was predictable since I am the only one using teleportation, and we know that presence is diminished compared to continuous movement in the virtual world. 
The lack of presence is replaced by the absence of motion sickness, which is found in many of the other students' locomotion. 
This is one of the major assets of my locomotion technique. Especially when you look at the fun graph, the scores between my locomotion and theirs are similar. 
Overall, fun without motion sickness!

![Presence](http://lbertrand417.github.io/IGD301-blog/Presence.png)
![Motion sickness](http://lbertrand417.github.io/IGD301-blog/Sickness.png)
![Enjoyment](http://lbertrand417.github.io/IGD301-blog/Enjoyment.png)
![Round time](http://lbertrand417.github.io/IGD301-blog/RoundTime.png)
![Accuracy](http://lbertrand417.github.io/IGD301-blog/Accuracy.png)

Concerning the average travel time, we notice 3 categories: fast locomotion techniques, normal locomotion techniques, and slow locomotion techniques. 
I thought that mine would be part of the slow category because the balls do not allow big and fast movements but finally it is part of the normal 
category. Finally, the average accuracy is rather low compared to the other locomotions. While discussing with the participants I discovered that they didn't find 
the little trick that allows to catch the ball in mid-air. And it seems to me that it's quite complicated to go fast AND to be accurate without knowing this trick. 
10 minutes were probably not enough to discover it but I am sure that if the player plays it longer he will realize it by himself. 


Other feedback I got from my participants included:
- The presence is not as good as for the continuous locomotion techniques but the realism of the throws increases it
- The throws are realistic but the sticky ball seems heavier than the petanque ball and it is hard to aim with the bouncy ball 

I already talked about the fact that the sticky ball seemed heavier in my implementation. I'd have to fix that while keeping the petanque ball useful. 
Concerning the bouncy ball, I was trying to make it less precise to force the user to change ball for throws that require more precision but maybe it is not precise enough.

- This locomotion technique is not adapted to the race 

For this I already suspected it but I hadn't thought this technique to be used in racing games.

- Most of the players used the sticky ball only

I think it's mainly due to the ignorance of the trick because it's the most precise but it doesn't allow to go fast. 
It would be necessary to make the same test but by making the player aware of the trick to see if they still use the sticky ball only. Personally, knowing the trick, 
I found the petanque ball more optimal.

Overall, the participants were quite satisfied with the locomotion technique. There are still things to be adapted to make it even more realistic and funny 
(for example by adding objects to retrieve, to be able to use certain types of ball such as a golf ball) but this technique still seems viable.