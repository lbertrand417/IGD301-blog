---
title: "First lab"
date: 2021-11-27
draft: false
description: "Setup Hugo and Unity"
tags: ["Hugo", "Unity", "Setup"]
thumbnail: https://avatars.githubusercontent.com/u/29385237?s=280&v=4
---

# Lecture Homework

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

## Setup Unity

This part is easier because I already used Unity for old projects. Wait, did you say easier? Well, I didn't count on the fact that I don't have any more 
space on my computer and that the version we have to use is not the same as in the other courses! Good good good.... I leave you I must go to free up some space on my computer.

