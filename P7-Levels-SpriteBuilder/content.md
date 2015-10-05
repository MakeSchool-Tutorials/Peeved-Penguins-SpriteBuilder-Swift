---
title: Create Levels with SpriteBuilder
slug: levels
---

Shooting penguins is fun. Colliding with seals and ice blocks is even more fun!

Time to learn how to create and load levels.

## Setup the ground

The levels we are going to load a few minutes from now need to stand on a stable ground in the Gameplay scene. So the first thing we will do is to add the *ground* to the *CCPhysicsNode* and setup a physics body for it.

> [action]
> Drag the *ground* node onto the *CCPhysicsNode* in the timeline. After that your arrangement should look like this:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_PhysicsNode.png)
>
> With the ground selected, open up the physics tab in the properties pane (third tab in right pane). Enable physics and set it to "static".

<!-- Make School -->

> [info]
> Static is used for objects that never move and are not affected by gravity or other forces.

Since our ground is not flat, we need to contract it’s bounds a bit. Otherwise we would have floating objects in some places.
> [action]
> Contract the ground's bounds by moving the pink corners down a few pixels.
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/ground.png)

Now we have a solid ground that will stop objects from falling into oblivion.

## Set up a level loading mechanism

Now we get to solve our first little puzzle. Problem: we want to be able to have an unlimited amount of levels in our game. Therefore we don't want the game mechanics (shooting, collision detection, etc.) to be part of the levels. Otherwise we would be in big trouble if we wanted to change them -- we'd need to apply the changes to every level.

This means we are not going to add the information about a level directly to our *Gameplay.ccb*. A better solution is to define an area in the *Gameplay.ccb* that is level specific and to load the level data into that area.

## Add a Level Node

> [action]
> Drag a new CCNode to your timeline; **add it as a child to the CCPhysicsNode** because the levels we will be loading will contain physics objects (every physics object in Cocos2D needs to be a child of a CCPhysicsNode).
>
> Apply the following dimensions: x:469, y:47, width:490, height:275:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_LevelNode.png)

This node will be the container for the levels we will be loading later on. The actual loading will happen in code, so we need tocreate a code connection to make the *levelNode* accessible from the *Gameplay* class.

> [action]
> Create a code connection from *levelNode* to the *Doc root var* "levelNode".

## Create a Level

> [action]
> In your SpriteBuilder project create a new folder called "Levels". Inside that Folder create a new CCB file. Call it "Level1", as SpriteBuilder document type choose "Layer" and set it's size to (490, 275).

Your first level and the folder structure should look like this:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_Levels.png)

Congratulations, this is your first level! Now let's add some content.

> [action]
> Pull multiple *Seal.ccb* files and different images of ice blocks (*tallblock.png*, etc) to this scene. Be sure to place some items at the very left edge of the level (this way they will be visible when we load the level - even before we have implemented a scrolling mechanism). Your scene could look somewhat like this:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_FirstLevel.png)

## Add level loading code

Now we are going to add some surprisingly simple level loading code.

> [action]
> Open *Gameplay.swift* in Xcode.
>
> Add a member variable after the opening curly brace:
>
>        weak var levelNode: CCNode!
>
> and add these lines to the end of the method *didLoadFromCCB*:
>
>        let level = CCBReader.load("Levels/Level1")
>        levelNode.addChild(level)

This will load *level1* and add it as a child to the levelNode. When you run your game you should see your level appear in the Gameplay scene:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_LevelLoadingWorks.png)

## Scrolling the scene

Now that we can shoot penguins, let's improve the experience by following the penguin while it flies across our level. Luckily, Cocos2D provides a very convenient way of doing this.

Have you used CCActions before? CCActions provide a lot of convenience methods to implement animations, movements, scaling, etc. in code. Performing a CCAction is always a two-step process:

- setup a CCAction with a couple of parameters (e.g scale, duration, etc)
- tell any CCNode to run that CCAction

To implement our camera that follows the penguin we will use an action called *CCActionFollow*. The usage is very simple. You define a CCNode that shall be followed; optionally you can also define world boundaries, so that the camera does not scroll outside of your background image.

> [action]
> Add these lines to the end of the *launchPenguin* method in *Gameplay.swift*:
>
>        // ensure followed object is in visible are when starting
>        position = CGPoint.zeroPoint
>        let actionFollow = CCActionFollow(target: penguin, worldBoundary: boundingBox())
>        runAction(actionFollow)

We are telling the Gameplay scene to act as a camera following the penguin. We also say that the camera shall not leave our scene by using the bounding box of the scene to define the world boundaries.

Run your game again and you'll see the camera following the penguin:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Scrolling.gif)

As you can see the penguin does not collide with the ice blocks yet. That's because they aren't physics objects at the moment. Let's change that in our next step!

## Whack the ice blocks

> [action]
> Open your SpriteBuilder project and open *Level1.ccb*.
>
> Select each ice block and enable physics for it.

<!-- Make School -->

> [info]
> Remember that you can drag the pink dots to change the form of the physics body. You should do this if you use the tapered block in your level:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_TaperedBlocks.png)

Once you're done; publish your SpriteBuilder project and run it again. You now should be able to collide with the ice blocks:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_Collision.png)

## Adding a level reset

Now that we can destroy the level, it also would be great to be able to restart easily. Let's add a button to reset the level.

> [action]
> Open *Gameplay.ccb* in SpriteBuilder. Drag a button to the left top of the screen. Set a title and a code connection for the button.
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_Retry_Button.png)

The implementation is pretty easy. We are just going to reload the entire scene utilizing the *CCDirector*.

> [action]
> Add this implementation to *Gameplay.swift*:
>
>       func retry() {
>           let gameplayScene= CCBReader.loadAsScene("Gameplay")
>           CCDirector.sharedDirector().presentScene(gameplayScene)
>       }

Great! Now run the app and make use of the new "Retry" button. Can you foresee what problem is going to come up? Right! The button is part of the Gameplay scene that is scrolling to follow the penguin. This means that the button scrolls off the screen. Yikes!

**Before reading on, try to think of possible solutions for this problem. It is important to come up with some ideas when you face this kind of hurdle.**

## Fixing the level reset

Ok. I hope you've spent some time and came up with some ideas. Here's our approach. We're going to restructure the Gameplay scene by adding a content Node. All gameplay action will be added to the content Node, all UI elements will be added on top of that content Node, directly to the Gameplay scene.

> [action]
> Open *Gameplay.ccb* and add a CCNode at position (0,0). This will be our content node.

Now restructure the scene in the timeline.

> [action]
> Drag *all Nodes except the button* onto the content node to make them a child of the content node. When you're done your scene hierarchy should look like this:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_rearrangeContentNode.png)

OK, you're halfway through this. We've restructured the scene but this doesn't change anything yet. We're still scrolling past the restart button when we scroll through the Gameplay scene.

**What is the second change we need to put in place to keep the button on the top left, while following the penguin? Once again, try to come up with your own solution.**

## Fixing the level reset for real

Figured it out? Right, we need to scroll the contentNode instead of the Gameplay scene!

> [action]
> Add a code connection for the contentNode, so that we can access it from *Gameplay.swift*.

When the code connection is set up, open *Gameplay.swift* in Xcode.

> [action]
> Add the member variable you need for the code connection:
>
>   weak var contentNode: CCNode!

Then modify the actual scrolling code. Make the new content node perform the action instead of the Gameplay Scene.

> [action]
> Your code for setting up and performing the action should now look like this:
>
>        // ensure followed object is in visible are when starting
>        position = CGPoint.zeroPoint
>        let actionFollow = CCActionFollow(target: penguin, worldBoundary: boundingBox())
>        contentNode.runAction(actionFollow)

Now the content Node is scrolling and not the complete Gameplay scene. Since the content Node is only used to structure the scene and doesn't have a content size, we still use the bounding box of the Gameplay scene to define the world boundaries.

If you run the game now, the button should stay in the top left corner and allow you to easily restart the level at any time.

Congrats! This is starting to look like an actual game. In the next chapter you are going to learn how to implement a great, physics based shooting mechanism, so stay tuned!
