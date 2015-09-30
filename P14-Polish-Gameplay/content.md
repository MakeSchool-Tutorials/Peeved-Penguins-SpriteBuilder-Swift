---
title: Polish the Gameplay
slug: improve-gameplay
---

Currently a player can only shoot once. The camera will follow the flying penguin, but won't scroll back to the catapult when the attempt is completed. We will change that in this chapter. The rules will be the following: If a penguin leaves the left or right bound of the level, or if it moves at very slow speed, we will trigger the next attempt to start.

We will have to check if any of these conditions become *true* on a regular basis - we can use Cocos2D's update method to do so.

#Implementing the update method

Before we actually implement the update method - add a constant to *Gameplay.swift* that defines the minimum speed which we will use to check if a penguin is slow enough for the round to end.

> [action]
> Add minSpeed to the `Gameplay` class:
>
>   let minSpeed = CGFloat(5)

The value '5' works well for our physics setting. If you have changed the physics properties of your objects you might want to experiment a little with this value.

Now we can implement the *update* method. This method is automatically called by Cocos2D every frame.

> [action]
> Add the update method to `Gameplay`:
>
>        override func update(delta: CCTime) {
>            if let currentPenguin = currentPenguin {
>                // if speed is below minimum speed, assume this attempt is over
>                if ccpLength(currentPenguin.physicsBody.velocity) < minSpeed {
>                    nextAttempt()
>                    return
>                }
>     
>                let xMin = currentPenguin.boundingBox().origin.x
>                if (xMin < boundingBox().origin.x) {
>                    nextAttempt()
>                    return
>                }
>     
>                let xMax = xMin + currentPenguin.boundingBox().size.width
>                if xMax > (boundingBox().origin.x + boundingBox().size.width) {
>                    nextAttempt()
>                    return
>                }
>            }
>        }

While this may look a little complicated at first, what actually is going on is only a tiny bit of math. We check whether the speed is below our defined limit. Therefore we use the *ccpLength* function that calculates the square length of our velocity (basically the x- and y-component of the speed combined). Further we check if the penguin has exited the level through the left or right boundary. If anything of this happens, we call the *nextAttempt* method and *return* immediately (to avoid that *nextAttempt* is called multiple times).

#Implementing the *nextAttempt* method

The most important thing we need to do in the *nextAttempt* method is scrolling back to the catapult. However, since we already are running an action to follow the penguin, we need to stop this action before we start another scrolling action (otherwise Cocos2D would understandably be confused about these two conflicting instructions).

Cocos2D provides a method called *stopAction:* that can be called on any CCNode. However we need a reference (a variable) for the action we want to stop.

> [action]
> Create a new member variable:
>
>        var actionFollow: CCActionFollow?
>
> Then modify the code in *releaseCatapult* to assign the scrolling action to this newly created variable:
>
>         // follow the flying penguin
>         actionFollow = CCActionFollow(target: currentPenguin, worldBoundary: boundingBox())
>         contentNode.runAction(actionFollow)

Now we are ready to implement the *nextAttempt* method!

> [action]
> Add this method to *Gameplay.swift*:
>
>        func nextAttempt() {
>            currentPenguin = nil
>            contentNode.stopAction(actionFollow)
>     
>            let actionMoveTo = CCActionMoveTo(duration: 1, position: CGPoint.zeroPoint)
>            contentNode.runAction(actionMoveTo)
>        }

First we reset the reference to the *currentPenguin*, because once an attempt is completed we consider none of the penguins as current one. Then we stop the scrolling action in the second line. Finally we create a new action to scroll back to the catapult.

#Just one more optimization

You may already have realized that there is one potential problem with the current solution. Assuming the player pulls back the catapult very slow a *next attempt* can be triggered **before** the penguin even launched.

To avoid this happening we will add a flag to the Penguin that stores if it has been launched already or not.

> [action]
> Open *Penguin.swift* and add this line:
>
>        var launched = false

Now that we have a *launched* property one the penguin, we can add a check to the update method, so that everything is only executed if the penguin exists and has already launched.

> [action]
> Change the update method to check if a penguin has been launched:
>
>        override func update(delta: CCTime) {
>            if let currentPenguin = currentPenguin {
>                    if currentPenguin.launched {
>                        ... // <-previous content of this method belongs inside of this if-statement
>                    }
>            }
>         }

Now all the checks will only be performed after firing the penguin.

As a final step we actually need to set the *launched* flag to *TRUE*, once a penguin is fired.

> [action]
> Change the currentPenguin's launched flag to true in the *releaseCatapult* method:
>
>        currentPenguin.launched = true

With this important optimization our *next attempt* mechanism is completed for now! You just have improved the game quite a bit. Run your game and confirm that everything works as expected.

You are ready to move on to the next chapter where you will learn how you can make this game iPad compatible!
