---
title: Animate a Character using SpriteBuilder's Timeline
slug: animating-spritebuilder
---

Let's learn how to create animations with SpriteBuilder's timeline. For our game we want to add a taunting animation to a bear that will sit behind our catapult:

<!-- TODO: update with gif -->

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_animating_preview.png)

#Setting up a new .ccb file

The bear will have it's own .ccb file.

> [action]
> Create a new file called "Bear.ccb" (`File > New > File`) in your SpriteBuilder project, and select `Node`:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_BearCCB.png)

We need to combine two images to make the bear: the body without an arm, and a separate arm that will be animated.

> [action]
> Add *bearnoarms.png* and *beararm.png* to the root node:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_animated_bear.gif)

<!-- Make School -->

> [action]
> Place *bearnoarms.png* at position (0.0, 0.0).

<!-- Make School -->

> [action]
> Set the anchor point of *beararm.png* to `(0.0, 1.0)` and it's position to `(-5.0, 10.0)`.

We need to set the anchor point of the arm because we are going to rotate it. When we apply a rotation to a CCNode it rotates around its anchor point - for the arm, this should be somewhere near the shoulder, which is the top left corner of our image.

#Interlude: Keyframe based animations

Before you can create the actual animation you need a basic understanding of keyframe based animations. When you create animations with keyframes, you define how a value changes over a certain time. You define these value changes by defining the final value and a timestamp. For example, for the rotation of our arm we could define three time-value pairs:

<!-- TODO: change to MarkDown table if parser supports it -->

<table>
  <tr>
    <th>Seconds</th>
    <th>Rotation in Degrees</th>
  </tr>
  <tr>
    <td style="text-align:center">0</td>
    <td style="text-align:center">0</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td style="text-align:center">90</td>
  </tr>
  <tr>
    <td style="text-align:center">2</td>
    <td style="text-align:center">0</td>
  </tr>
</table>

The special thing about keyframe based animations is that the values between the defined timestamps are [tweened](http://en.wikipedia.org/wiki/Inbetweening). This means that every value between the ones you have defined is calculated by some mathematical function. Assuming linear tweening, which we use most of the time, this means that, according to the table above, the rotation value at 0.5 seconds would be...? *Correct*: 45 degrees.

Once you understand this basic concept the creation of most animations will be rather intuitive to you.


#Setting the animation up

Now we can animate the polar bear's arm. InSpriteBuilder each animation is defined on its own timeline. If you have multiple animations for one object, you can add multiple timelines to the .cbb file. For our bear we are starting with only one animation. It is good practice to name the timeline like the animation happening on it. We also need to set the duration of our animation.

> [action]
> Rename the timeline to "ArmAnimation" and set the duration to two seconds:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/SetTimelines.gif)


##Adding Keyframes

Now it's time to add the three key frames we have described earlier. Keyframes are created in the timeline at the bottom of the screen. Using the control on the top right of the pane, you can zoom in and out:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/SpritebuilderAnimation_Zoom.png)

> [action]
> Zoom in by dragging the slider on the left of the timecode a bit to the right. Select the arm of the bear and create three keyframes for *rotation* at 0, 1 and 2 seconds. Do this by pressing *R* (on your keyboard) when the time marker is set at the mentioned times. Update the rotation value at each to -15, 20, -15 respectively:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_SetKeyframes.gif)

Click the play button in the timeline pane to see how it looks.

> [action]
> Last, we need to chain this animation to itself so that it will repeat when playing in our game:
>
> ![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_ChainAnimations.png)

Congratulations! You have just created your first keyframe based animation with SpriteBuilder!
