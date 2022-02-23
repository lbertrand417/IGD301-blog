---
title: "First lab"
date: 2021-11-27
draft: false
description: "Setup Hugo and Unity"
tags: ["Hugo", "Unity", "Setup"]
thumbnail: https://upload.wikimedia.org/wikipedia/commons/thumb/a/af/Logo_of_Hugo_the_static_website_generator.svg/1024px-Logo_of_Hugo_the_static_website_generator.svg.png
---

# Lecture Homework

## Hyper reality

Mixed reality can improve a lot of tools, like google maps or, more generally, the way to present information. However, just as there are people who use technologies to do harm, 
mixed reality is also subject to abuse. The Hyper Reality video presents a dystopian vision of virtual reality and I will also present different problematic scenarios.

### Holo love

As it is already the case with existing technologies, social networks tend to transform relationships into something virtual. People don't talk to each other on the street anymore, 
they look at their phones and talk with people who may be on the other side of the country. Also, more and more people are meeting on dating apps and not in real life activities.
Now, imagine a world where dating apps are adapted to mixed reality. More than that, the user could create his own virtual partner and do all sorts of activities with him.  
There would be a huge problem in reproduction, as it already exists in Japan. People would close themselves in their world, with their virtual partner. 
Indeed, why bother building relationships when you can create your own ideal partner? People would then live in a world separated from reality and would only have relationships 
with avatars invented from scratch. 

![Hologram partner](http://lbertrand417.github.io/IGD301-blog/holo_love.jpg "Hologram partner")

The worst part is that this scenario is already happening to some extent. I talked earlier about social networks and relationships becoming virtual, 
but this is still an interaction with existing people. However, there are already holograms with the purpose of keeping company with people who feel lonely, like Gatebox. 
The technology is not yet transportable but it is only a matter of time. These apps have positive aspects as it can help people who need it, but the risk is that they remain locked in this 
utopia because they feel reassured and comfortable. If the technology is transportable, these people will be completely cut off from the rest of the world.

![Gatebox](https://helios-i.mashable.com/imagery/articles/04K1LWDkuwBfojQt0CO1SUL/hero-image.fill.size_1248x702.v1611614804.png "Gatebox")

In order to prevent the world from taking this turn, we could consider prohibiting specific applications in certain places. 
For example, entering a workplace would close all social applications (except those needed for work), to encourage real interactions between colleagues. 

### Differentiate real from virtual

Let's imagine again a world where everyone uses mixed reality. It's very practical for everyday life, especially for classes. 
No more need to carry heavy textbooks, no more need to come to class, everything can be done remotely. We live with mixed reality from an early age. 
Do you see the problem coming? A child, whose brain is still developing, has mixed reality as part of his reality. Unlike adults who can distinguish 
between "real" reality and mixed reality, a child bathed in this world may not be able to distinguish the two! He will no longer be able to differentiate 
fantasy from reality, which is a real problem for his future development in the real world. One solution could be to limit or even prohibit the use of 
mixed reality under a certain age. A kind of computer majority in a way. 

![Children and mixed reality](http://lbertrand417.github.io/IGD301-blog/children_mixed_reality.jpg "Children and mixed reality")

## Ultimate display

![Children and mixed reality](https://images.prismic.io/add2020/684383c1-6459-4d0d-a22e-460db5e611c4_Sutherland-Cover.png?auto=compress,format "Children and mixed reality")


Ivan Sutherland begins his article by describing technologies that we have nowadays, namely a computer and a graphics tablet. 
Then he talks about the technology that he considers to be the Ultimate Display. A computer that can create a chair on which one can sit, 
handcuffs that can really constrain, or a bullet that can really kill. It is interesting to think about where his Ultimate Display fits into 
Milgram's Reality-Virtuality Continuum. Let's start with reality. The ultimate display cannot be reality in the sense that it is the computer that creates the object. 
In this sense, we have a virtual object. It is not virtual reality either because the object created is placed in a real environment of which the user is aware. 
It remains therefore augmented reality and augmented virtuality. I personally think that his Ultimate Display is an area between these two points. 
Indeed, as he describes it, the virtual looks as real as the reality itself. Therefore it is a perfect mix between virtual and real. We can say at the same time 
that it is augmented reality (we make appear an extremely real virtual object in a real environment) or augmented virtuality (the objects are virtual but the user can
interact with them and the sensations and the effects on him are real. He can really sit on the chair, he can really die from the virtual bullet,...). 


# Lab Homework

## Setup blog

The teachers recommended Hugo to write our blog but we were free to choose another tool if we wanted. 
I had already created a sort of eportfolio on wix and could have continued to use that but Hugo seems to be a much more complete tool, albeit more complex. 
That's why I decided to learn how to use Hugo. The installation was quite difficult but finally I managed to get the result I wanted. 
At the beginning I wanted to be able to have all the updates of Hugo easily, so I installed it using github and not with the .zip file. 
But I quickly realized that this would mean installing a lot of other software like Chocolatey and I don't have enough space on my computer for that. :sob:
So I used the .zip file, as recommended by the teachers. 

The second step was to choose a theme so that I wouldn't have to do the layout from scratch. 
I chose the Blist theme which I think is very elegant and adapted to the blog format. However, I had a hard time understanding how to use it. 
Even now, I don't know if I have the right files or if some are useless. At least it works! :laughing: You must be curious about the problems I had so, just for you, I will explain. 
I followed the teachers tutorial to include a theme, which basically consists of adding a line in the configuration file. 
But when I ran the command to start the server I got an error telling me that I had problems with postCSS. :fearful:
Looking on the internet they advised to install the Hugo extended version instead of the classic version. 
So I started from scratch again (including my blog because I didn't know if it would remain compatible) with the Hugo extended version. 
But it didn't solve the problem. :disappointed: In the console they told me to type the command "npm install postcss-cli" to solve the problem but npm was not recognized. 
So I had to install Node.js and npm (not sure if I needed Node.js but it installed both so...), then I could type the command and it solved my problem! 
Well, this was actually just the beginning of all my problems... Indeed, I could open the Blist theme example page but not my own page. 
When I entered the command in the console I had layout errors. I was told that some layouts were missing. 
I looked for a long time but couldn't find the files so I used the ultimate technique.... copy all the folders from the example theme into my own project! :laughing:
And it miraculously worked! That's why I might have some extra files in my folders but the main thing is that it works, right? :blush:
Finally I just had to adapt the files : for instance, the example was provided in several languages but I only use English. Now here I am, writing my first article. 

But Hugo's setup work isn't over yet. We must succeed in publishing it now! For that, once again, I tried to follow the teachers' tutorial. 
The idea is to create a git in which we put only the html files created at the launching of the server. 
Well, I created the git including all the folders so I ended up adding everything instead of only the html files...so I had to delete my git repository and start again. :expressionless:
The problem is that I can't find the folder where the html pages are supposed to be. I added the line publishDir = "docs" but no file appears in the folder. 
I must have made a mistake somewhere but at the time of writing I have not yet solved the problem.

Update: Well I was a little bit stupid, I was trying to create the files using the "hugo server -D" command instead of the "hugo" command. Nevermind, now it works. I created a git with
the docs folder and now you can admire this wonderful blog!

Update 2: I'm coming back here just to talk about a little problem I had when adding images from my computer. 
The theme said to write "/image.png" to add an image but it didn't work for me. The solution I found was to write the full path, e.g  
http://localhost:1313/IGD301-blog/image.png. The problem is that this command works when I run my blog locally but not when I send it to github. 
Finally, I solved this problem by replacing with http://lbertrand417.github.io/IGD301-blog/image.png. It requires that I push my site on Github to get the 
images but once it's done they are available both locally and on Github.

## Setup Unity

This part is easier because I already used Unity for old projects so I already have an account. Wait, did you say easier? Well, I didn't count on the fact that I don't have any more 
space on my computer and that the version we have to use is not the same as in the other courses! Good good good.... I leave you I must go to free up some space on my computer.

[1 week later]

I'm back! Finally I have enough space (and an internet connection) so I managed to download the correct Unity version. Nothing much to say about it so let's meet again in the next post
to create our first project!

