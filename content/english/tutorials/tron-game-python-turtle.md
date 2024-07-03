---
title: TRON Game with Python Turtle
meta_title: TRON Game with Python Turtle
description: TRON Game with Python Turtle
summary: Tron is an arcade game that combines aspects of the iconic snake game with the Tron movies, you must control your very own Tron-style snake and try to box in your opponent.
date: 2024-06-05T10:54:53.238Z
image: /images/tron-tutorial.svg
categories:
  - Python Tutorials
author: Vladimiras Malyskinas
tags:
  - python
---

{{< toc >}}

### Introduction

{{< notice "note" >}}
This tutorial was adapted from [Free Python Games by Grant Jenks.](https://grantjenks.com/docs/freegames/tron.html)
{{< /notice >}}


Tron is an arcade game that combines aspects of the iconic snake game with the Tron movies, you must control your very own Tron-style snake and try to box in your opponent. Making such a game using the `Turtle` module in Python is an interesting challenge, which we will try to complete in this tutorial.

### Planning

First, we need to plan our program. Let's think about everything that happens in a game of Tron.

{{< tabs >}}
{{< tab "Player" >}}

**Remember, in this part, we are only planning - not programming!**

Firstly, we would need to create our player. The player must:
- Move continuously.
- Not be able to rotate in the opposite direction of where he is moving (if the player is moving to the right, he can't suddenly go to the left).
- As he moves, leave a line behind him.

{{< /tab >}}

{{< tab "Winning/Losing" >}}

In Tron, the player loses in the following cases:
- If the player hits the border of the window.
- If the player hits his own line.
- If the player hits the line of his opponent.

{{< notice "tip" >}}
Notice how these cases are `if-statements`, this will be important later!
{{< /notice >}}

{{< /tab >}}

{{< tab "[ADVANCED] Adding more players" >}}
Although at first, we will complete the game with only 1 Turtle, we will use functions and some smart programming to add a second player without repeating ourselves too much.

{{< /tab >}}
{{< /tabs >}}

And that is it! If you have gone through the steps, you should know the plan already. But you can open the menu below to look at it in full.

{{< accordion "Our plan for the Tron game" >}}

1. Create the player.
2. Make the game playable with 1 player.
3. Use functions to add the second player.
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
window.setup(600, 600)  # Set the size of our window
```

{{< /accordion >}}

This gives us a blank screen, a canvas we can play our game on.

#### Setting up the player

Now that we have created a window, we can start setting up our player. Our player should be a square, we can change the colour to one we like and put our pen up so that the player doesn't draw anything as we move around (we will do the line a bit differently). In the next step, we are going to work on rotating the player, to properly see where the player is facing we will need to comment out the `player.shape('square')` command for now.

{{< accordion "Creating the player" >}}

```python
player = turtle.Turtle()  # Create the player turtle
player.penup()
# player.shape('square') - To test our keybinds for rotating the player, we will comment this out for now.
player.color('red')
```

{{< notice "tip" >}}
To make sure the game window doesn't close, you can add a `turtle.done()` command at the **very end of the code**. This command should always be at the very end!
{{< /notice >}}

{{< /accordion >}}

##### Rotating the player

Now that we have a player, it is time to make him rotate! Let's first figure out how to make him rotate and then we will add the relevant `if-statements` to block him from rotating into the opposite directions. For our rotation, we will be using the `player.setheading()` command. This allows us to set where the turtle is heading to a specific angle:

{{< image src="images/setheading.gif" caption="Possible turtle.setheading() angles." alt="turtle.setheading() angles" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="turtle.setheading() agnles"  webp="false" >}}

Now let's use the `player.setheading()` command with `window.onkeypress()` to make the player rotate when we press the corresponding keys.

{{< accordion "Controlling the player" >}}

```python { hl_lines=["6-26"] }
player = turtle.Turtle()  # Create the player turtle
player.penup()
# player.shape('square') - To test our keybinds for rotating the player, we will comment this out for now.
player.color('red')

def turn_right():
    player.setheading(0)

def turn_left():
    player.setheading(180)

def turn_up():
    player.setheading(270)

def turn_down():
    player.setheading(90)

window.onkeypress(turn_right, "Right")
window.onkeypress(turn_left, "Left")
window.onkeypress(turn_up, "Up")
window.onkeypress(turn_down, "Down")
window.listen()

turtle.done()
```

{{< /accordion >}}

##### Limiting player rotation

Now that we are able to control the player, we should restrict him from rotating to the opposite of where he is looking. Just like in the real Tron game.

For that, we need to ask ourselves, in what case are we able to rotate in a direction? The answer is fairly straightforward - if we are not looking in the opposite direction. Now let's translate this English if-statement into a Python `if-statement`. Let's approach it word by word.

**If we are not** - For this part of the sentence, we would need to use the `if` in Python. But how do we say not? We can check whether something is **not** in Python by using `!=` in an `if-statement`.

**Looking in the opposite direction** - To see which direction we are looking towards, we would need to use the `player.heading()` command.

{{< notice "tip" >}}
`player.setheading()` - sets where the player is heading, and `player.heading()` gives us a number (which is an angle), where the player is heading.
{{< /notice >}}

Now let's put these two parts together! Note: We will need to replace `{OPPOSITE DIRECTION}` with a number.

`if player.heading() != {OPPOSITE DIRECTION}`

Try to make this fit into our functions for turning.

{{< accordion "Restricted movement" >}}

```python { hl_lines=[4, 8, 12, 16] }
...

def turn_right():
    if player.heading() != 180:
        player.setheading(0)

def turn_left():
    if player.heading() != 0:
        player.setheading(180)

def turn_up():
    if player.heading() != 90:
        player.setheading(270)

def turn_down():
    if player.heading() != 270:
        player.setheading(90)

...
```

{{< /accordion >}}

##### Making the player move

By now, we should have the following:
- A player
- The player is able to turn
- But not in the opposite directions

But the player is not moving yet, so let's do that!

We have two loops in Python - finite `for` loops and infinite `while True:` loops. Since we don't know when exactly our game needs to end, we will have to use the infinite loop - `while True:`. Since it's a loop, let's imagine we are stepping to a new tile - let's digest what will need to happen each time we take a step:

1. Check whether we have been to this tile before or if we have hit the edge of the screen.
    1. If we have - we will need to end the game, as we have lost. Otherwise, continue on.
    
2. Record that we have visited this tile.
3. Move the player forward.

These are 3 crucial steps to our game, let's start with the easier one - moving the player forward.

{{< notice "info" >}}
Our square-shaped turtle is 20 pixels wide, which means that each loop, we will need to move our turtle 20 steps forward.
{{< /notice >}}



{{< accordion "Code to move the player forward" >}}

```python
... 
# At the very end of the code:
while True:
    player.forward(20)
```

Now this is too fast for our game, so let us ask Python to wait a little before starting a new `while True` loop.

```python { hl_lines=[2, 9] }
import turtle
import time

...

while True:
    player.forward(20)

    time.sleep(0.1)
```

{{< /accordion >}}

##### Drawing a line after the player

Now that we have made the player move forward, we should start drawing the line behind him as well. Let's break down what we will need to do and what command we can use to make it happen.

In a game of Tron, before moving forward to the other tile, we should stamp the shape of our turtle (which is a square), to mark that we have already been to this square. The command to do that is straightforward - `player.stamp()`.

{{< accordion "Leave a line as we go" >}}

```python { hl_lines=[4] }
...

while True:
    player.stamp()
    player.forward(20)

    time.sleep(0.1)
```

{{< /accordion >}}

##### Checking whether the player has touched his own line

Now we have come to what is probably the trickiest part of this game - how do we check whether a player has touched his own line? The problem with turtle is that we can't just say `if turtle is touching a colour different from the background`, instead we will need to split this task in two:

1. Record every position that we have been to as our turtle travels
2. Check whether we are standing on a tile that we have been on before

These two parts put together will allow us to make a game-over condition for our game.

###### Recording previous turtle positions

Let's start with the first part - recording our previous positions. We need to remember which type of variable can we use, that holds a lot of items in Python. That type of variable would be a `list`. This list we will use to store all previous positions that our turtle has been to, so as the name of the list can be quite straightforward - `previous_positions`.

To add positions to this list we can use the `previous_positions.append()` command. In brackets, we would put what we want to append - previous positions, right? But to get the position of our turtle, we would need to use yet another command `player.pos()`. Combining these two commands together we get the following `previous_positions.append(player.pos())`.

Let's add it. How often do we need to record our position of the player? Each time before we take a step. So this commands needs to go just above the `player.forward(20)` command:

{{< accordion "Leave a line as we go" >}}

```python { hl_lines=[4, 9, 11] }
import turtle
import time

previous_positions = []

...

while True:
    print(previous_positions) # To see what previous_positions stores, we will print it out, we will remove the command later on.
    player.stamp()
    previous_positions.append(player.pos())
    player.forward(20)

    time.sleep(0.1)
```

{{< /accordion >}}

###### Checking whether we are in a previous position

As you can see, to record all of our previous positions all we had to do was add one line of code. But to check whether we have been to a previous position, will be a little bit harder. If we take a look at what the `print()` command prints out, we will see that as our game progresses it populates a list with all of the positions where our turtle has been.

So to see whether we are standing on a tile that we have been to before, we would need to loop through the entire list and if the distance from the player to previous position is too small, then that would mean that we are standing on top of a tile we have already been to and the game should end.

That sounds like a complex task to do, so let's break it down into pieces:

**Looping through the entire list** - to loop through a list, we can use a `for` loop. We should be familiar with the `for side in range(4):` loop, but did you know that we can replace the `range(4)` command with any list? If we do `for side in previous_positions:` Python will loop through every position that is present in previous_positions and we will be able to compare whether we are close to any of them using the `side` variable:

```python
...

while True:
    # Loop through `previous_positions` list
    for side in previous_positions:
        # print the dinstance from our player to each position in previous_positions:
        print(player.distance(side))

    player.stamp()
    previous_positions.append(player.pos())
    player.forward(20)

    time.sleep(0.1)
```

{{< notice "warning" >}}
The variable `side` holds one set of coordinates at a time, which allows us to compare our players' coordinates to every position that we have visited previously (which are stored in this `previous_positions` list). This means that `side` is not a good name for this variable, it would be better to rename it to `every_position`.

```python {hl_lines=[5, 7]}
...

while True:
    # Loop through `previous_positions` list
    for every_position in previous_positions:
        # print the dinstance from our player to each position in previous_positions:
        print(player.distance(every_position))

...
```
{{< /notice >}}

**Checking if we hit a line** - from the previous code snippet, you may have noticed that we are using `print(player.distance(every_position))`. This command prints out the distance between the player and every position that Python can see in `previous_positions` list. Now we need to check if that distance is smaller than our step. We will use an `if-statement` for that, so try to see if you can figure out how to translate this into Python. 

{{< accordion "Code for line detection" >}}

```python
...

while True:
    # Loop through `previous_positions` list
    for every_position in previous_positions:
        # If our distance to any coordinate in `previous_positions` is less than 5...
        if player.distance(every_position) < 5:
            #  ...change our title to tell us that we have lost the game.
            window.title("Game Over - You have hit your own line")

    player.stamp()
    previous_positions.append(player.pos())
    player.forward(20)

    time.sleep(0.1)
```

We could simplify this to the following code, but I have found it unreliable and sometimes it doesn't work properly:
```python
# Checks whether players position is present in previous_positions list
if player.position() in previous_positions:
    window.title("Game Over - You have hit your own line")

```

{{< /accordion >}}


{{< image src="images/tron-line-detection.gif" caption="Working line detection." alt="Tron working line detection." height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="Working line detection."  webp="false" >}}

Now that we have created the system for detecting when we hit a line, it is time to make the game stop when we actually hit the line. Let's think about it - how can we stop a `while True:` loop? By changing `True` to `False`. But that is not possible without a variable, so let's create a variable that is going to control whether our game is running:

{{< accordion "Code for line detection" >}}

```python {hl_lines=[4, 8, 15]}
...

previous_positions = []
game_is_running = True

...

while game_is_running:
    # Loop through `previous_positions` list
    for side in previous_positions:
        # If our distance to any coordinate in `previous_positions` is less than 5...
        if player.distance(side) < 5:
            #  ...change our title to tell us that we have lost the game.
            window.title("Game Over - You have hit your own line")
            game_is_running = False

...
```

{{< /accordion >}}

As you can see, if we hit a line we change the `game_is_running` variable to `False`, which ends our game and closes the window. As an extension task you can think of a way to not close the game window when the game ends but just stop the game.

##### Detecting border collision

Great job so far, we are very close to completing our Tron game! Now let's add another important part of the game - border collisions. For the border collisions, we will once again have to perform a check, checking whether the player is within the playing field. How often will we need to perform this check? Every time we take a step or every `while` loop.

Remember how we have used `window.setup(600, 600)`? That means that we already know where our borders are:
- Top border = 300y
- Bottom border = -300y
- Right border = 300x
- Left border = -300x

As our first example, let's make it so that our player is not able to escape from the side borders, and based on this example you will be able to complete the top and bottom borders yourself.

We need to tell Python that if our players' x coordinate becomes bigger than (>) 300 (the player goes too far to the right), or becomes smaller than (<) -300 (the player goes too far to the right), we should stop the game as the player has hit the border. Let's translate this into Python language.

{{< accordion "Code for left and right border collision" >}}

```python {hl_lines=[4, 5, 6]}
...

while game_is_running:
  if player.xcor() < -300 or player.xcor() > 300:
    window.title("Game Over - You have hit the border")
    game_is_running = False

  ...
```

{{< /accordion >}}

Now that you see an example of how we can do it for the left and right borders, which use x coordinates, try to complete the same exercise for top and bottom borders.

{{< accordion "Code for all border collisions" >}}

```python
...

while game_is_running:
  if player.xcor() < -300 or player.xcor() > 300:
    window.title("Game Over - You have hit the border")
    game_is_running = False

  if player.ycor() < -300 or player.ycor() > 300:
    window.title("Game Over - You have hit the border")
    game_is_running = False
    
  ...
```

{{< /accordion >}}

#### Final Result with One Player

After working hard through the tutorial, below is the complete code for the game. Although it works, as you will see there are plenty of things that we can improve. Stick around and scroll past the code to learn how we can add the second player and some extension tasks.

<iframe src="https://trinket.io/embed/python/4b8c4d6a3f" width="100%" height="600" frameborder="0" marginwidth="0" marginheight="0" allowfullscreen></iframe>

### Extension Tasks

#### Make the turtle rotate instantly

If you have played the game above, you will have noticed that when we rotate our turtle keeps moving as it turns around. Using `window.tracer(0)` and `window.update()` we can control when the screen refreshes. This makes our turtle games faster and feel more snappy.

{{< notice "tip" >}}

`window.tracer(0)` must be added right after we create our `window`.

`window.update()` must be added inside of the `draw()` function.

{{< /notice >}}

{{< accordion "Code with window.tracer(0)" >}}

```python { hl_lines=[2, 9] }
window = turtle.Screen()
window.tracer(0)

...

while True:
  ...

  window.update()
  time.sleep(0.1)
```

{{< notice "warning" >}}
If you are using `window.tracer(0)` the window won't update until the code reaches `window.update()`, which might make it seem like your game is not working. Always make sure that you have `window.update()` added somewhere.
{{< /notice >}}

{{< /accordion >}}

#### Tidy the code up by adding functions

Currently, the code doesn't look too messy but it could be improved by adding functions with descriptive names on what they are doing. Look around the code and try to determine, which parts can be "promoted" to functions. Here is a list of functions that we can have:

- `def move_player():`
- `def player_is_alive():` - Advanced, requires `return`.

{{< accordion "Code with move_player() added." >}}

```python {hl_lines=[3, 4, 5, 6, 7, 12]}
...

# New function to move the player
def move_player():
    player.stamp()
    previous_positions.append(player.pos())
    player.forward(20)

...

while True:
    move_player()

    window.update()
    time.sleep(0.1)
```

{{< /accordion >}}

{{< notice "tip" >}}
As you can see from the name in this function we are asking a question: is the player still alive? If he is not (one of the `if-statements` gets triggered) we use `return False` to return the answer to that question. If the player is still alive, we return True.

By putting the function directly in the `while` loop, we can use these returns to either continue with our game loop or make it stop.
{{< /notice >}}

{{< accordion "Code with player_is_alive() added." >}}

```python
...
# New function, which tells whether the plays is still alive
def player_is_alive():
    if player.xcor() < -300 or player.xcor() > 300:
        window.title("Game Over - You have hit the border")
        return False

    if player.ycor() < -300 or player.ycor() > 300:
        window.title("Game Over - You have hit the border")
        return False


    for side in previous_positions:
        # If our distance to any coordinate in `previous_positions` is less than 5...
        if player.distance(side) < 5:
            #  ...change our title to tell us that we have lost the game.
            window.title("Game Over - You have hit your own line")
            return False
            
    return True

while player_is_alive():
    ...
```
{{< /accordion >}}

### Final Code with One Player and Extension Tasks

<iframe src="https://trinket.io/embed/python/14c1d47abc" width="100%" height="600" frameborder="0" marginwidth="0" marginheight="0" allowfullscreen></iframe>

### Adding a Second Player

Adding a second player is a more complex task than making this game work with one player. To add the second player, we could copy and paste the code that creates the second turtle and binds the movement keys to it, but that would go against an important programming principle `DRY - Don't Repeat Yourself`. So instead, let's think about what can we use to re-use commands in Python. That's right, we can use functions. For this next part, we are going to go off of the previous improved code with one player.

#### Creating a `setup_player()` function

To create the second player, instead of copying and pasting the same lines of code twice, we will create a function `setup_player()`, which will act like a 'factory' in which we create our turtle and ship it out (using `return`) to the main code. To start, let's put every line of code that creates the player into a function:

```python
def setup_player():
    player = turtle.Turtle()
    player.penup()
    player.shape("square")

    def turn_left():
        if player.heading() != 0:
            player.setheading(180)

    def turn_right():
        if player.heading() != 180:
            player.setheading(0)

    def turn_up():
        if player.heading() != 270:
            player.setheading(90)

    def turn_down():
        if player.heading() != 90:
            player.setheading(270)

    window.onkey(turn_left, "Left")
    window.onkey(turn_right, "Right")
    window.onkey(turn_up, "Up")
    window.onkey(turn_down, "Down")
    window.listen()
```

But if we run the same function twice, it will create the same turtle, in the same place with the same colour twice. Somehow, we need to fix that and Python allows us to give 'promises' to a function, that we are going to provide certain information to it when we run the function. These are called `parameters`. Let's add them and use these parameters in our function. What do we need to customize for each player?

- Starting position
- Colour (optional)
- Keys we press to go up, down, left, right

```python {hl_lines=[1, 4, 6, 24, 25, 26, 27]}
def setup_player(starting_position, player_colour, key_up, key_down, key_left, key_right):
    player = turtle.Turtle()
    player.penup()
    player.setposition(starting_position)
    player.shape("square")
    player.color(player_colour)

    def turn_left():
        if player.heading() != 0:
            player.setheading(180)

    def turn_right():
        if player.heading() != 180:
            player.setheading(0)

    def turn_up():
        if player.heading() != 270:
            player.setheading(90)

    def turn_down():
        if player.heading() != 90:
            player.setheading(270)

    window.onkey(turn_left, key_left)
    window.onkey(turn_right, key_right)
    window.onkey(turn_up, key_up)
    window.onkey(turn_down, key_down)
    window.listen()
```

Great! Now what we need to do is return the player that we have created into our main code. In this function `player` is just a temporary name for our turtle. Let's add a `return player` to the very end of our function:

```python {hl_lines = [12]}
def setup_player(starting_position, player_colour, key_up, key_down, key_left, key_right):
    player = turtle.Turtle()

    ...

    window.onkey(turn_left, key_left)
    window.onkey(turn_right, key_right)
    window.onkey(turn_up, key_up)
    window.onkey(turn_down, key_down)
    window.listen()

    return player
```

But we need something to accept that `return` of the player - we will need another variable when we call the function, we will call the function and assign the return to a new variable right before the `while` loop:

```python {hl_lines=[3, 4]}
...

player_1 = setup_player((-250, 200), "blue", "Up", "Down", "Left", "Right")
player_2 = setup_player((-250, -200), "red", "w", "s", "a", "d")
while player_is_alive():
    ...
```

But now within our code, we get an error saying `player` is not defined. To fix that, we will need to pass which player we want to move or check which player is alive as a parameter again!

```python {hl_lines=[2, 5, 10, 11, 12]}
...
def move_player(player):
    ...

def player_is_alive(player):
    ...

player_1 = setup_player((-250, 200), "blue", "Up", "Down", "Left", "Right")
player_2 = setup_player((-250, -200), "red", "w", "s", "a", "d")
while player_is_alive(player_1) and player_is_alive(player_2):
    move_player(player_1)
    move_player(player_2)

    window.update()
    time.sleep(0.1)
```

And that is it! Our game is now working with two players!

### Final Code with Two Players

<iframe src="https://trinket.io/embed/python/7fb70c6d4b" width="100%" height="600" frameborder="0" marginwidth="0" marginheight="0" allowfullscreen></iframe>

### Conclusion

Congratulations! You've successfully created a basic TRON game using Python's Turtle module. Through this tutorial, you've learned essential programming concepts such as planning a project, handling player movements, detecting collisions, and managing game loops. This game serves as a fun and educational project that introduces you to the fundamentals of game development in Python. While our game is quite basic, there are numerous ways you can expand and improve it, such as adding more players, enhancing the graphics, or implementing more complex game mechanics.
