---
title: Add Custom Objects Using SpriteBuilder
slug: penguins-seals
---

Seals are the bad guys in Peeved Penguins. The goal is to get rid of all of them! You shoot penguins at them to get rid of them.

Next, we want to create the Seal and Penguin objects in SpriteBuilder.

#Penguins

Let's start with the penguins. We want penguins and seals to each get their own .ccb files because they will be mapped to different Swift classes. That will allow us to distinguish between them later on when we add the shooting mechanism to our game.

> [action]
> Create a new CCB file (`File > New > File`) and make sure that you create it at the root level of the project. Choose Sprite as the root node:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_Penguin.png)

Now you need to set an image for the penguin. The easiest way is to select the root node (our CCSprite) is to select it from the timeline at the bottom of the screen.

> [action]
> Once you have selected the CCSprite, go to the right pane and set the "Sprite Frame" property to the *flyingpenguin.png* image:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_SetSpriteFrame.png)

Now you should see the penguin image on the center of the stage.

Later, when we implement the gameplay, we want to shoot these penguins from our catapult. This means we want to turn these penguins into physics objects as well so that they can interact with the other physics bodies in the scene.

> [action]
> Select the penguin, open the third tab and check the box "Enable physics". Once you check the box, you will see 4 pink dots forming a rectangle. This shape represents the shape of your physics body:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_enablePhysics.png)

By default this body is a square. For our flying penguin a *circle* seems like a better choice.

> [action] Change the physics shape in the dropdown accordingly:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_changePhysicsBody.png)

<!-- Make School -->

> [info]
> Note, that you can drag the pink points around to adjust the size and position of the physics body.

Now we have a CCSprite with the correct image and physics body set up. Finally, you need to set the custom class property of this CCB file. Once again, this custom class property links a Swift class to this CCB file (more on this later). We want the penguin to be linked to a Swift class called "Penguin".

> [action]
> Go the right pane, open the second tab, and set the custom class property:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_CustomClass.png)

That's it for now - our penguin is ready! You will learn how to initialize this custom CCSprite subclass very soon!

#Seals

You should be able to create the Seal.ccb file on your own now. Repeat all the steps you did for the penguins:

- Create a new CCB file (CCSprite as root node)
- Select the correct sprite image (seal.png)
- Setup a physics body (a circle as shape will do again)
- Setup the custom class

#Create the classes in Xcode

It is worth repeating that the "custom class" property of a .ccb file creates a link between the .ccb file in your SpriteBuilder project and an Swift class in your Xcode project.

For our example this means that whenever someone loads the *Seal.ccb* or *Penguin.ccb* file, SpriteBuilder will initialize the custom classes we have linked (Seal and Penguin).

To make this work, we need to create the Seal and Penguin class in Xcode.

> [action]
> Publish your SpriteBuilder project.
>
> Open the PeevedPenguins.xcodeproj (`File > Open Project in Xcode`). Create two new classes (`File > New > File > iOS > Source > Cocoa Touch Class`) called "Penguin" and "Seal" and make them subclasses of CCSprite (because the root nodes of Penguin.ccb and Seal.ccb are CCSprites). Set the language is set to Swift:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_Penguin_Xcode_Swift.png)
>
> Please create the new file in the *Source* Folder of your project to keep a consistent structure:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_Classes_Folder_Location.png)
>
> **Don't forget to repeat this step for the Seal class.**

#Testing that everything worked out

Since we have made a bunch of changes it would be nice to test if everything worked out as expected before we move on. Most importantly we want to check if our code connections are working.

> [action]
> Open "Penguin.swift" and add these this to the Penguin class:
>
>        func didLoadFromCCB() {
>            println("Penguin created!")
>        }

Now, everytime the Penguin.ccb file is loaded, "Penguin created!" should appear in the console. Let's do the same for the Seal.swift file. You remembered to create it, right? If not, go ahead and create a new Seal class in Xcode like you did for Penguin.

> [action]
> Open Seal.swift and add didLoadFromCCB:
>
>        func didLoadFromCCB() {
>            println("Seal created!")
>        }

So far, so good. Now for testing purposes we should try to load both files (Penguin.ccb and Seal.ccb) from our source code manually to invoke our custom didLoadFromCCB methods.

> [action]
> Open MainScene.swift and add the following code:
>
>        func didLoadFromCCB() {
>            CCBReader.load("Penguin")
>            CCBReader.load("Seal")
>        }

Now, when our app starts, the Penguin.ccb and Seal.ccb files should be loaded immediately, causing a Seal and Penguin object to be initialized. Two log messages should appear in your console log.

> [action]
> Run the app and check the console for the correct output:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_CodeConnectionTest.png)
>
> *Note: if the console does not appear, check if you have set the two options highlighted in the screenshot correctly*

If everything worked out, you should see a "Penguin created!" and "Seal created!" message in your console log. **Congratulations!**.

If you don't see the message, please go back and check if you have performed every step on this page.

#Cleaning up & moving on

You have learned a lot in this chapter. You've set up your first code connection and now have a basic understanding of how and where the use of SpriteBuilder and Xcode will overlap in your projects.

Now before we move on to the next chapter, let's clean up our test methods.

> [action]
> **Remove the didLoadFromCCB methods we have added to MainScene.swift, Seal.swift and Penguin.swift**.
