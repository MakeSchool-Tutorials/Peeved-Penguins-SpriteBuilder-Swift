---
title: Getting Started with Peeved Penguins!
slug: getting-started
---

In this tutorial we will build Peeved Penguins -- a clone of the popular mobile game [Angry Birds](https://itunes.apple.com/us/app/angry-birds/id343200656?mt=8). We will use Apple's new Swift programming language together with SpriteBuilder and Cocos2D.

<!-- If you aren't familiar with SpriteBuilder you should read our [SpriteBuilder beginner tutorial](https://www.makeschool.com/tutorials/getting-started-with-spritebuilder-and-swift/installing-spritebuilder) first since this tutorial assumes that you are familiar with basic SpriteBuilder tasks. Make sure you have both SpriteBuilder and Xcode installed! -->

#What you will learn

Throughout this tutorial you will learn many concepts including how to:

- Switch scenes from main menu to gameplay
- Use the Cocos2D physics engine
- Load levels from CCB files
- Use the timeline for sprite frame and key frame animations
- Make a game that is compatible on both iPhone and iPad

#The finished product

We'll be making a basic clone of Angry Birds with a catapult launcher and seal enemies. The final game will look like this:

<!-- TODO: update with gif -->

![](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/SpriteBuilder_iPad_improved.png)

Let's get started with a blank SpriteBuilder project!

#Create a new project

> [action]
> Create a new project in SpriteBuilder named `PeevedPenguins` and change the Primary Language to `Swift`.

Now download the [Peeved Penguins art pack](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/PeevedPenguinsAssets.zip) we created for you. Once the download is complete, unpack the folder.

#Importing resources

> [action]
> Drag the PeevedPenguinsAssets folder into the Resources pane (the empty space under MainScene.ccb). This will copy the assets to the correct location:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_Import_Resources.png)

#Adjust the autoscaling settings

If you look at the resources folder you will notice that each image is only provided in one resolution instead of providing separate assets for retina and non-retina devices. This is possible because SpriteBuilder supports *autoscaling*.

Thanks to SpriteBuilder's autoscaling you only need to provide the image with the highest resolution and the lower resolution images are generated automatically. If you've worked with Cocos2D before this means **no more regular and -hd files!**

> [info]
> SpriteBuilder is set up by default to downsize assets from a 4x resolution (double resolution of retina images).

The Peeved Penguins assets are provided as 2x assets (retina resolution) so we have to change this setting for our project.

> [action]
> Go to `File > Project Settings` and change *Default scaling* to `2x (phonehd)`:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_Autosizing.png)

Now, when you publish through SpriteBuilder, it will auto-generate non-retina iPhone assets.

#Enabling Smart Sprite Sheet

SpriteBuilder has another nice feature you should use in your games: *Smart Sprite Sheets*.

> [info]
> When you use smart sprite sheets, SpriteBuilder will automatically generate one large image out of all of your assets. This allows the device to load all your assets into memory at once and will speed things up when your game is running.

> [action]
> To transform your Peeved Penguin Assets into a spritesheet, you need to right-click onto the folder and select *Make Smart Sprite Sheet*:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_SmartSpriteSheet1.png)

After this your folder's icon should become pink.

> [action]
> Now hit the publish button. That will generate your smart sprite sheet. If everything worked out you will see a nice preview of your sprite sheet when you select it in the resource pane:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_SmartSpriteSheet2.png)

Now you know how to add resources to your game and use some of the neat optimizations that come with SpriteBuilder.

Let's move on and learn how to create animations with SpriteBuilder!
