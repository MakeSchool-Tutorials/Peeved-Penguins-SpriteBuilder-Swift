---
title: Add Physics using Chipmunk, Part 2
slug: joints-cocos2d3
---

This section will take place fully in code, so pull up Xcode to get
started! We will only have to make a couple of small changes to get the
penguins going.

Start by setting up two new member variables, for the penguin we are
going to launch and for the joint which we will use to attach the
penguin to the catapult:

	var currentPenguin: Penguin?
	var penguinCatapultJoint: CCPhysicsJoint?

Spawn a penguin
===============

When a touch begins, we need to spawn a penguin and attach it to the
scoop of the catapult arm. Therefore add these lines to *touchBegan*
**inside the if-statement**:

	// create a penguin from the ccb-file
	currentPenguin = CCBReader.load("Penguin") as Penguin?
	if let currentPenguin = currentPenguin {
	    // initially position it on the scoop. 34,138 is the position in the node space of the _catapultArm
		let penguinPosition = catapultArm.convertToWorldSpace(CGPoint(x: 34, y: 138))
		// transform the world position to the node space to which the penguin will be added (_physicsNode)
		currentPenguin.position = physicsNode.convertToNodeSpace(penguinPosition)
		// add it to the physics world
		physicsNode.addChild(currentPenguin)
		// we don't want the penguin to rotate in the scoop
		currentPenguin.physicsBody.allowsRotation = false

		// create a joint to keep the penguin fixed to the scoop until the catapult is released
		penguinCatapultJoint = CCPhysicsJoint.connectedPivotJointWithBodyA(currentPenguin.physicsBody, bodyB: catapultArm.physicsBody, anchorA: currentPenguin.anchorPointInPoints)
	}

Most of this should be quite easy to understand by now. Note, that we
are first expressing the penguins position relative to the catapult arm.
Then we are translating this position to world coordinates, and finally
we convert it to the node space of our *physicsNode*, because that is
where the penguin is located in. Also note that we turn of rotation for
the penguin so it doesn't rotate in the scoop of the catapult. Let's
move on to releasing the penguin!

Let it fly
==========

Now we are going to extend the *releaseCatapult* method. We need to
destroy the joint between the penguin and the scoop, activate rotation
for the penguin and add the camera code to follow the penguin again (add
this code inside the if-statement):

		// releases the joint and lets the penguin fly
		penguinCatapultJoint?.invalidate()
		penguinCatapultJoint = nil

		// after snapping rotation is fine
		currentPenguin?.physicsBody.allowsRotation = true
		 
		// follow the flying penguin
		actionFollow = CCActionFollow(target: currentPenguin, worldBoundary: boundingBox())
		contentNode.runAction(actionFollow)

Tuning
======

Now run the game and shoot a couple of penguins! You'll probably realize
that the penguins fly very high. If you like you can do a little of
tuning now (an important part of building physics games). Spritebuilder
makes it very easy to change different physics properties, such as
density, friction, etc.:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_tuning.png)

Well done! Now you have a first fully playable prototype. In the next
chapter we will start turning this into an actual game.
