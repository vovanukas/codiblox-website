---
title: Noughts and Crosses / Tic-Tac-Toe with Python Turtle
meta_title: ""
description: Noughts and Crosses / Tic-Tac-Toe with Python Turtle
date: 2024-04-15T03:55:40.656Z
image: /images/noughts-and-crosses.svg
categories:
  - Python Tutorials
author: Vladimiras Malyskinas
tags:
  - python
draft: false
---

{{< toc >}}

### Introduction

{{< notice "note" >}}
This tutorial was adapted from [Free Python Games by Grant Jenks.](https://grantjenks.com/docs/freegames/tictactoe.html)
{{< /notice >}}


You probably know how to play Noughts and Crosses (or Tic-Tac-Toe). It is a two-player game, which is simple on paper - two players take turns marking a three-by-three grid with X's and O's. The first player to put three of their markings in a horizontal, vertical or diagonal row - wins! As I have said before, the game might seem simple on paper but let's try to recreate it using the `Turtle` module in Python!

### Planning

First, we need to plan our program. We can do that by playing Noughts and Crosses in our imagination - let's start!

{{< tabs >}}
{{< tab "Step 0" >}}

#### Let's plan our game...

**Remember, in this part, we are only planning - not programming!**

What is the first thing we do on a piece of paper when we want to play Noughts and Crosses? 

Try to come up with your own answer and then head to **Step 1**!

{{< /tab >}}

{{< tab "Step 1" >}}

#### We draw a grid!

{{< notice "tip" >}}
In programming, we need to simplify. A grid can be seen as nine squares or only four lines!
{{< /notice >}}

Now that we have a grid, what is our next step?

{{< /tab >}}

{{< tab "Step 2" >}}

#### Draw the X and the O!

Inside the grid, X's and the O's are drawn. But let's think in detail, how should they be placed?

{{< /tab >}}

{{< tab "Step 3" >}}

#### Each mark should be placed one after another, where the player clicks.

In Noughts and Crosses the players take turns to draw their marks. Although we cannot draw on our computer screen, we will track where each player clicks.

{{< /tab >}}
{{< /tabs >}}

And that is it! If you have gone through the steps, you should know the plan already. But you can open the menu below to look at it in full.

{{< accordion "Our plan for Noughts and Crosses" >}}

1. Create the playing grid.
2. Draw an X and draw an O.
3. Create code to place each mark one after another, wherever the player clicks.

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

This gives us a blank screen, a canvas we can paint our grid on.

#### Drawing the grid

Now that we have created a window, we can start drawing the playing grid.

{{< notice "tip" >}}
In programming, we need to simplify. A grid can be seen as nine squares or, even more simply, only four lines!
{{< /notice >}}

Let's follow the tip and make a reusable function (custom command), which will create a line - let's call the command `draw_line`. It will have four parameters (promises we make to the function) `(start_x, start_y, end_x, end_y)`.

But who will create that line? Before creating the function we will need to create a turtle that will draw the grid, let's name the turtle `grid`. Try to complete this task and check your answer!

{{< accordion "Creating draw_line(start_x, start_y, end_x, end_y) function" >}}

```python
grid = turtle.Turtle()  # Create the grid turtle

def draw_line(start_x, start_y, end_x, end_y):
    grid.penup()  # Make sure the turtle doesn't draw going to the starting position
    grid.setposition(start_x, start_y)
    grid.pendown()
    grid.setposition(end_x, end_y)
```

{{< /accordion >}}

Now that we have a function that draws a line, we can use it to draw out the grid! You can use the image below to help with `start` and `end` coordinates.

{{< image src="images/coordinates-grid.png" caption="Coordinates where each line should start and end" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title"  webp="false" >}}

{{< accordion "Drawing the grid" >}}

```python
grid = turtle.Turtle()  # Create the grid turtle

def draw_line(start_x, start_y, end_x, end_y):
    grid.penup()  # Make sure the turtle doesn't draw going to the starting position
    grid.setposition(start_x, start_y)
    grid.pendown()
    grid.setposition(end_x, end_y)

def draw_grid():
    draw_line(-300, 100, 300, 100)
    draw_line(-300, -100, 300, -100)
    draw_line(-100, 300, -100, -300)
    draw_line(100, 300, 100, -300)

draw_grid()
```

{{< notice "tip" >}}
You can always personalise your grid by changing the `grid` turtle. For example, you can change the colour of the grid to red with `grid.color('red')` 
{{< /notice >}}

{{< /accordion >}}

#### Drawing the X and the O

We have a grid and we need to add X's and O's to it. We will start slow by having a new turtle, called `player` draw the symbols in the middle of the screen. Let's start with the `draw_x()` and `draw_o()` functions. A few things to note:
- The `player` turtle needs to draw the symbol in the very middle of the screen.
- After drawing the symbol, our `player` turtle should face the same direction when it started drawing it.
- Remember to not repeat yourself - try to use loops when possible.

{{< accordion "draw_x()" >}}

```python
player = turtle.Turtle()  # Create the player turtle

def draw_x():
    player.color("black")

    player.left(45)
    for _ in range(4):
        player.forward(140)
        player.forward(-140)
        player.left(90)

    player.right(45)

draw_x()  # Ask Python to run the command to test it out
```

{{< /accordion >}}

{{< accordion "draw_o()" >}}

```python
def draw_o():
    player.color("black")
    player.dot(150)
    player.color("white")
    player.dot(130)

draw_o()  # Ask Python to run the command to test it out
```

{{< /accordion >}}

##### Putting a symbol where the player clicks

By now, we should have the following:
- A playing grid
- A function to draw an X
- A function to draw an O

The symbols are being drawn in the middle of the screen, where the `player` turtle starts its adventure:

{{< gallery dir="images/noughts-and-crosses-gallery" class="" height="400" width="400" webp="true" command="Fit" option="" zoomable="true" >}}

Now it's time to make the game work. First, we must figure out how to use the [`window.onclick()`](https://docs.python.org/3/library/turtle.html#turtle.onclick). `window.onclick()` binds a command to our mouse button.

{{< notice "info" >}}
`window.onclick(draw_x)` won't work. The function `draw_x()` doesn't have the required 2 parameters - `x` and `y`. A new function needs to be created, which contains these 2 new parameters. For example, this code moves the turtle to the place on the screen where we clicked:

```python
import turtle

window = turtle.Screen()

def move_to(x, y):
    turtle.setposition(x, y)

window.onclick(move_to)
turtle.done()
```
`x` and `y` parameters tell the turtle where to move/where the player has clicked!

{{< /notice >}}

{{< accordion "Code to place the X, wherever the player clicks" >}}

```python
def draw(x, y):
    # Make sure that the player turtle is not drawing on its way to where the player clicked! 
    player.penup()
    player.setposition(x, y)
    player.pendown()

    draw_x()

window.onclick(draw)
```

{{< /accordion >}}

##### Switch between the X and the O

We have created the code to only draw the X in the place where the player clicks on the screen. But how do we switch between the two symbols? We know of the `if` statements, which allow us to check whether something is true and we have `else`, which will run the code if the `if` statement is not true (false). We can use that in our `draw` function. What do we check against? We will need to create a new variable `is_crosses` and it's going to be a part of `window`. `window.is_crosses = True` since our game starts with the crosses going first, we are going to set it to `True`. Once an X has been placed, we set the variable to `False`. Once an O has been placed, we set it back to `True`. It behaves like a switch!

{{< notice "tip" >}}
These two snippets will need to be added somewhere and the comments (lines with the `#`) will need to be turned into code. Try to do it yourself and then check the solution!

{{< /notice >}}

{{< accordion "Code to switch between placing X and O" >}}

```python
window.is_crosses = True

...

def draw(x, y):
    player.penup()
    player.setposition(x, y)
    player.pendown()

    if window.is_crosses:
        draw_x()
        window.is_crosses = False
    else:
        draw_o()
        window.is_crosses = True

window.onclick(draw)
```

{{< /accordion >}}

#### Final Result

After working hard through the tutorial, below is the complete code for the game. Although it works, there are plenty of things that we can improve. Stick around and scroll past the code for some **Extension Tasks**.

<iframe src="https://trinket.io/embed/python/08f2d2ebe6?showInstructions=true" width="100%" height="600" frameborder="0" marginwidth="0" marginheight="0" allowfullscreen></iframe>

### Extension Tasks

#### Make the game faster

Using `window.tracer(0)` and `window.update()` we can control when the screen refreshes. This makes our turtle games faster and feel more snappy.

{{< notice "tip" >}}

`window.tracer(0)` must be added right after we create our `window`.

`window.update()` must be added inside of the `draw()` function.

{{< /notice >}}

{{< accordion "Code with improvements" >}}

```python { hl_lines=[2, 18] }
window = turtle.Screen()
window.tracer(0)

...

def draw(x, y):
    player.penup()
    player.setposition(x, y)
    player.pendown()

    if window.is_crosses:
        draw_x()
        window.is_crosses = False
    else:
        draw_o()
        window.is_crosses = True

    window.update()

window.onclick(draw)
```

{{< notice "warning" >}}
If you are using `window.tracer(0)` the window won't update until the code reaches `window.update()`, which might make it seem like your game is not working. Always make sure that you have `window.update()` added somewhere.
{{< /notice >}}

{{< /accordion >}}

#### Make sure the symbols are centered

To make sure the symbols are centered on the grid, we will need to round our `x` and `y` positions to the center. To not focus on the maths side of programming, this is the function that will need to be added:

```python
def floor(value):
    return ((value + 700) // 200) * 200 - 600
```

This function rounds a given `value` to the center of the nearest slot.

{{< accordion "Code with improvements" >}}

```python { hl_lines=[5, 6] }
def floor(value):
    return ((value + 700) // 200) * 200 - 600

def draw(x, y):
    x = floor(x)
    y = floor(y)

    player.penup()
    player.setposition(x, y)
    player.pendown()

    if window.is_crosses:
        draw_x()
        window.is_crosses = False
    else:
        draw_o()
        window.is_crosses = True

    window.update()

window.onclick(draw)
```

{{< /accordion >}}

#### Don't allow the player to put symbols on top of each other

Currently, the X and the O can be placed anywhere - even on top of one another. This is not how the game of Noughts and Crosses goes, let's fix that.

Once again, let's remember what we can use to check something in Python - the `if` statement. The question is: what do we check? We need to check which slots are already filled with a symbol but to do that, we must first record where the player has put a symbol. For that, we will use a list.

Firstly, let's create an empty list that is part of window - `window.previous_positions = []`. This list will be empty at the start of the game since there are no Noughts or Crosses in the beginning.

Once we have created this empty list, we need to fill it. `window.previous_positions.append(item)` is the command to append an `item` to this list. Try to come up with how we could use this line of code.

{{< accordion "Code with the list added" >}}

```python { hl_lines=[3, 11] }
window = turtle.Screen()
...
window.previous_positions = []

...

def draw(x, y):
    x = floor(x)
    y = floor(y)

    window.previous_positions.append([x, y])  # We append a list of x and y coordinate clicked on into the previous_positions list - think of it as organising a big box using smaller boxes. 

    player.penup()
    player.setposition(x, y)
    player.pendown()

    if window.is_crosses:
        draw_x()
        window.is_crosses = False
    else:
        draw_o()
        window.is_crosses = True

    window.update()

window.onclick(draw)
```

{{< /accordion >}}

We have a list that stores where we have put an X or an O but that list does not block us from placing more. That is because we have not added the `if` statement, which would check whether the current spot we are trying to place a symbol on is occupied, or not. We discussed how to check that before, try to think of a way to add it!

{{< accordion "Code with the if statement added" >}}

```python { hl_lines=[5] }
def draw(x, y):
    x = floor(x)
    y = floor(y)

    if [x, y] not in window.previous_positions:
        window.previous_positions.append([x, y])

        player.penup()
        player.setposition(x, y)
        player.pendown()

        if window.is_crosses:
            draw_x()
            window.is_crosses = False
        else:
            draw_o()
            window.is_crosses = True

        window.update()

window.onclick(draw)
```

{{< notice "tip" >}}
The `if [x, y] not in previous_positions:` statements, checks whether the `x` and `y` coordinate pair exists in the `window.previous_positions` list. If not - we add it to the list and draw an X or an O in that place. After that, the position is occupied.

If we add an `else`, we will be able to display something to the player to tell that the position is occupied!
{{< /notice >}}

{{< /accordion >}}

#### Add some colour!

Using the `.color()`, `.pensize()` commands we can change the colour and the line width of a turtle. For example `grid.color("red")` would change the grid to red and `grid.pensize(10)` would make the grid lines thicker. Try to explore the complete game with the extensions and add some colour to it.

### Final Code with Extension Tasks

<iframe src="https://trinket.io/embed/python/0b1cdc5c26" width="100%" height="600" frameborder="0" marginwidth="0" marginheight="0" allowfullscreen></iframe>

### Conclusion

In conclusion, Noughts and Crosses might be a simple game that can be played with just a pen and paper but creating it in Python using the `Turtle` is not that simple. In this tutorial, we have successfully created a basic Noughts and Crosses game using Python, which can be played by clicking on the screen. The final code still has plenty of things that can be added - you just need to think about it.

I hope you had fun tagging along and learned something new!
