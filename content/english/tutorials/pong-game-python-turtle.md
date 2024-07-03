---
title: Pong Game with Python Turtle
meta_title: Pong Game with Python Turtle
description: Pong Game with Python Turtle
summary: Pong is a two-player game, where each player controls a paddle and tries to score points by hitting the ball into his opponent's side. Pong is one of the first popular games created.
date: 2024-06-28T19:25:50.981Z
image: /images/pong-tutorial.svg
categories:
  - Python Tutorials
author: Vladimiras Malyskinas
tags:
  - python
---

{{< toc >}}

### Introduction

Pong is a fast-paced arcade game that is made for two players. Each player controls a paddle and tries to score points by hitting the never-stopping ball into his opponent's side. The game is very similar to table tennis and we will recreate it using the `Turtle` module in Python!

### Planning

First, we need to plan our program. Let's think about everything that exists in a game of Pong. Thankfully, there aren't too many moving parts so the plan will be simple.

{{< tabs >}}
{{< tab "Players" >}}

**Remember, in this part, we are only planning - not programming!**

Firstly, we would need to create our players. The players must:
- Look like a paddle.
- Be placed on opposite sides of the playing field.
- Only be able to move up and down.

{{< /tab >}}

{{< tab "The ball" >}}

In Pong, the ball does the following:
- Moves continuously forward in one direction.
- If the ball touches the paddle, it bounces (changes the direction of travel).
- If the ball touches the top or the bottom side of the screen, it bounces.

{{< notice "tip" >}}
Notice how these cases are `if-statements`, this will be important later!
{{< /notice >}}

{{< /tab >}}

{{< tab "Scoring system" >}}

The scoring system in Pong is very straightforward - if a ball touches the left side, a point is awarded to the player on the right, if the ball touches the right side, then a point is awarded to the player on the left.

{{< /tab >}}
{{< /tabs >}}

And that is it! If you have gone through the steps, you should know the plan already. But you can open the menu below to look at it in full.

{{< accordion "Our plan for the Tron game" >}}

1. Create the players.
2. Create the ball.
3. Implement the scoring system.
**As easy as 1, 2, 3!**

{{< /accordion >}}

### Coding

Now that we have a plan, we can start coding.

#### Setting up the window

Our game cannot exist nowhere - it requires a window, which will display our game and will allow us to click. Let's review which commands we will need to use to create it:

{{< accordion "Setting up the window" >}}

{{< notice "note" >}}
All code will be hidden in these unfolding menus. Before opening them, try to complete the code yourself!

This one doesn't count :)
{{< /notice >}}

```python
import turtle  # Import the turtle library

# Set up the game window
window = turtle.Screen()  # Create a Screen, where our turtles live
window.setup(600, 400)  # Set the size of our window
```

{{< /accordion >}}

This gives us a blank screen, a canvas we can play our game on.

#### Setting up the players

Now that we have created a window, we can start setting up our players. Our players should be rectangular (which is a stretched-out square), we can change the colour of the players to one we like and put our pen up so that the players don't draw anything as they move around. We need to place the two turtles in opposite positions on the left and right sides of our screen.

{{< accordion "Creating the players" >}}

```python
...

left_paddle = turtle.Turtle()
left_paddle.shape("square")
left_paddle.shapesize(2, 0.5)
left_paddle.penup()
left_paddle.setx(-270)

right_paddle = turtle.Turtle()
right_paddle.shape("square")
right_paddle.shapesize(2, 0.5)
right_paddle.penup()
right_paddle.setx(270)
```

{{< notice "tip" >}}
To make sure the game window doesn't close, you can add a `turtle.done()` command at the **very end of the code**. This command should always be at the very end!
{{< /notice >}}

{{< /accordion >}}

##### Moving the players

Now that we have our players, it is time to make them move! When we are moving the player, we only want them to move up and down and we don't want them to rotate. Let's break down which commands we will need before revealing the answer. 

Using `turtle.sety(coordinate)` we can set our turtles y coordinate (change how high or low our turtle is on the screen). Using `turtle.ycor()` we can find out where our turtle is on the y coordinate. Using these commands let's translate our logic to Python: set our turtles y coordinate to wherever it is currently plus (higher) or minus (lower) 3 steps. Try to come up with an answer and open the unfolding menu for the complete code.

{{< accordion "Controlling the players" >}}

```python
...

def left_paddle_up():
    left_paddle.sety(left_paddle.ycor() + 3)

def left_paddle_down():
    left_paddle.sety(left_paddle.ycor() - 3)

def right_paddle_up():
    right_paddle.sety(right_paddle.ycor() + 3)

def right_paddle_down():
    right_paddle.sety(right_paddle.ycor() - 3)

window.onkey(left_paddle_up, "w")
window.onkey(left_paddle_down, "s")
window.onkey(right_paddle_up, "Up")
window.onkey(right_paddle_down, "Down")
window.listen()

turtle.done()
```

{{< /accordion >}}

#### Creating the ball

Now that we are able to control the players, we should focus on the ball. First, let's start easy by just creating the ball in the middle of the screen. Feel free to choose how the ball looks - its shape and colour.

{{< accordion "Creating the ball" >}}

```python { hl_lines=[6, 7, 8] }
...

window = turtle.Screen()
window.setup(600, 400)

ball = turtle.Turtle()
ball.shape("circle")
ball.penup()

left_paddle = turtle.Turtle()

...
```

{{< /accordion >}}

##### Moving the ball

Now that we have a ball, let's make it move forward.

Our ball will need to do a lot of things - move forward, check whether it needs to bounce, check whether it hit the left or the right screen (and some player has scored). To make everything work, our ball will need to take small steps forward and every step it takes check whether any of the conditions above have been fulfilled. We will write those conditions later but let's focus on the moving part.

{{< notice "tip" >}}
  Sine we don't know when our game will end, we will need to use the infinite `while True:` loop. Don't forget that any code we put **after** the `while True:` loop will be **unreachable**!
{{< /notice >}}

{{< accordion "Code to move the ball forward" >}}

```python
... 
# At the very end of the code:
while True:
    ball.forward(1)

# turtle.done() - Since we have a while True loop, turtle.done() is no longer necessary.
```

{{< notice "tip" >}}
  If you find that the ball moves a bit too fast, change the 1 in `ball.forward(1)` to a smaller number. 
{{< /notice >}}

{{< /accordion >}}

##### Detecting collision with the paddles

If you press play now, you will notice that the ball just moves through the paddle - that's not good. Let's add some code to reverse where the ball is heading when it hits a paddle.

To measure the distance between 2 turtles (our paddles and the ball are all turtles), we can use the `turtle.distance(another_turtle)` command. This command gives us a number, if that number becomes smaller than *something* (30 works well), bounce the ball back using `ball.setheading(180 - ball.heading())`.

{{< accordion "Bounce the ball from paddles" >}}

```python
...

while True:
    ball.forward(1)

    if ball.distance(right_paddle) < 30:
        ball.setheading(180 - ball.heading())

    if ball.distance(left_paddle) < 30:
        ball.setheading(180 - ball.heading())
```

{{< /accordion >}}

##### Bouncing off of the top and bottom of the screen

If we are working on bouncing, let's also make the ball bounce when it touches the top or bottom of the screen. To make sure we are able to work on this problem, we will need to rotate our ball using `ball.left(90)` at the beginning of our code, to see the results.

Since the screen borders are not a turtle, we cannot use the `turtle.distance()` command. Instead, we will need to use an `if-statement` to see whether the ball is above or below a certain y coordinate.

We can get the y coordinate for the top of the screen using `window.window_height() / 2` and the y coordinate for the bottom of the screen using `-window.window_height() / 2`. So if the coordinate of our ball becomes bigger than the top or lower than the bottom, we need to bounce the ball using `ball.setheading(-ball.heading())`.

{{< accordion "Bounce the ball from the edges of the screen" >}}

```python
ball = turtle.Turtle()
ball.left(90)

...

while True:
    ball.forward(1)

    ...

    if ball.ycor() > window.window_height() / 2:
        ball.setheading(-ball.heading())

    if ball.ycor() < -window.window_height() / 2:
        ball.setheading(-ball.heading())
```

{{< /accordion >}}

##### Randomizing the ball's starting direction

Currently, our ball does not start going in a random direction but in the game of Pong, it does. Thankfully, to fix that we would only need to add one line of code when creating our ball. Remember just a step before we added `ball.left(90)`? Instead of rotating it 90 degrees to the left, it should rotate a random amount between 1 and 360 degrees.

{{< accordion "Start in a random direction" >}}

```python { hl_lines=[2, 7] }
import turtle
import random

...

ball = turtle.Turtle()
ball.left(random.randint(0, 360))

...
```

{{< /accordion >}}

#### Adding the scoring system

So far we should have a working prototype of our game - it might seem slow for now but don't worry - we will fix it after this step.

Let's add the scoring system! As we have discussed before, the scoring system is simple:
- If the ball touches the **left** side, then the **right** player gains a point.
- If the ball touches the **right** side, then the **left** player gains a point.

We already have `if-statements` for checking when the ball touches the top or bottom, so all we need to do is re-use them.

**Additionally** to adding a point to the score for left and right players, we should:
- Reset the ball back to the middle of the screen.
- Display the score.

{{< accordion "Code for line detection" >}}

```python { hl_lines=[3, 4, 6, 10, 11, 12, 14, 15, 16] }
...

left_score = 0
right_score = 0
while True:
    window.title(f"{left_score} : {right_score}")

    ...

    if ball.xcor() > window.window_width() / 2:
        ball.goto(0, 0)
        left_score += 1

    if ball.xcor() < -window.window_width() / 2:
        ball.goto(0, 0)
        right_score += 1
```

{{< notice "tip" >}}

You will notice `window.title()` exists within the `while True:` loop. That is because we need Python to update the score each time we score OR `score` variables increase.

{{< /notice >}}

{{< /accordion >}}

#### Making the game faster

The game looks great! All that is left is to make the game faster and remove the "loading" of the game (animations of turtles getting into place). We can do that by controlling when the game redraws what is seen on the screen. First, we need to turn off all screen updates using `window.tracer(0)` and then manually update the screen by using `window.update()` every `while True:` loop.

{{< accordion "Code for line detection" >}}

```python {hl_lines=[4, 9]}
...

window = turtle.Screen()
window.setup(600, 400)
window.tracer(0)

...

while True:
    window.update()

...
```

{{< /accordion >}}

#### Final Result!

Great work working through this Pong tutorial using Python Turtle! If you have followed along, you should have a nice game that you can play with two players, but if not - don't worry here is a demo and the source code:

{{< notice "warn" >}}

Since Trinket (the online coding platform) is limited, some commands had to be commented out.

{{< /notice >}}

<iframe src="https://trinket.io/embed/python/f4fbfb0d4b" width="100%" height="600" frameborder="0" marginwidth="0" marginheight="0" allowfullscreen></iframe>

### Extension Tasks

#### Speed up the ball when it bounces off a padle

In a game of Pong, the ball speeds up every time it bounces off of a paddle. Let's make it the same in our game. For this we will need to introduce a variable called `ball_speed` and set it to a lower number to start. Whenever we hit a ball, increase the `ball_speed` by a bit.

{{< accordion "Code with ball speed up" >}}

```python { hl_lines=[3, 6, 10, 14, 21, 26] }
...

ball_speed = 1

while True:
    ball.forward(ball_speed)
    
    if ball.distance(right_paddle) < 30:
        ball.setheading(180 - ball.heading())
        ball_speed += 1

    if ball.distance(left_paddle) < 30:
        ball.setheading(180 - ball.heading())
        ball_speed += 1
        
    ...
        
    if ball.xcor() > window.window_width() / 2:
        ball.goto(0, 0)
        left_score += 1
        ball_speed = 1

    if ball.xcor() < -window.window_width() / 2:
        ball.goto(0, 0)
        right_score += 1
        ball_speed = 1
```

{{< /accordion >}}

#### Randomize the bounce direction from the paddle

In addition to gaining speed, the ball also changes its course a little bit in a true game of Pong. We can add it by changing the `setheading` command of our ball:

{{< accordion "Random bounce direction" >}}

```python {hl_lines=[4, 8]}
...

    if ball.distance(right_paddle) < 30:
        ball.setheading(random.randint(160, 200) - ball.heading())
        ball_speed += 1

    if ball.distance(left_paddle) < 30:
        ball.setheading(random.randint(160, 200) - ball.heading())
        ball_speed += 1

...
```

{{< /accordion >}}

### Final Code with Extension Tasks

<iframe src="https://trinket.io/embed/python/cacb02ce57" width="100%" height="600" frameborder="0" marginwidth="0" marginheight="0" allowfullscreen></iframe>

### Conclusion

As I hope we have learned from this project - recreating Pong using Python Turtle isn't that hard! It is a fun small project and in the end you have a small game that you can play together with your friends.

I hope you had fun tagging along and learned something new!
