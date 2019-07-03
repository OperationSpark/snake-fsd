Note: These project instructions are written for Mac OSX

# Snake
Classic Snake Game

<a href = "https://jsbin.com/kumifavazi/1/edit?output" target="_blank"> Play it here:  (Right Click -> Open Link in New Tab)</a>

#### To download this project into your workspace:

First locate the folder in which you would like to download this code. If you need a lesson on how to navigate your file system using the Terminal, see the link below:

https://www.macworld.com/article/2042378/master-the-command-line-navigating-files-and-folders.html

Use the `cd` command to navigate to the folder you wish to use. Then:

1) Fork it (Click the "Fork" button in the top right corner of the github repository for this project and fork it to your
account
2) Click on the "Clone or Download" button and copy the github URL
3) Enter this command into your bash terminal:

```bash
git clone <paste copied URL here>
```

#### To run this project:

At this point your working directory should be the folder that holds your `snake` folder. `cd` into the `snake` directory:

```bash
cd snake
```

Then, start an HTTP server which should be already installed on your machine. Enter this command:

```bash
hs
```

Then follow the link that pops up. It should look like this:

  http://127.0.0.1:8080

## Learning Objectives
- Build an app from start to finish including writing HTML, CSS, and JavaScript
- Manipulate HTML elements using the jQuery JavaScript library
- Use keyboard inputs
- Organize code using Functions

# Part 1 - HTML & CSS

## Learning Objectives
- Create CSS rules for elements that you plan on adding dynamically using jQuery
- Learn how the various files in a program are linked together in an index.html file
- Set up HTML elements whose content will be filled dynamicalyly using jQuery

## TODO 1: Add the initial HTML elements

This Snake program is simple. It only has a few visual components:
- The board
- The score display
- The high score display
- The snake
- The apple

The board, score display, and high score display are all static elements that
don't move throughout the program while the snake and apple will constantly be
changing.

For now, we can set up the stationary elements first. When we get to the
JavaScript portion of this project we'll deal with the snake and apple.

Between the `<body> </body>` tags add the following elements:
- An empty `<div></div>` element with an `id = "board"` property
- An empty `<h1></h1>` element with an `id = "score"` property
- An empty `<h1></h1>` element with an `id = "highScore"` property

## TODO 2: Link the CSS and jQuery files to our index page
This program can be built using a single HTML file, a CSS file, and a JavaScript
file. While all three of these languages can be included in one file, it is
best practice to separate them into their own files. We'll also be using the 
library *jQuery* which has been downloaded for you in the file `jQuery.min.js`.

This, however, requires us to link them together within the `index.html` file, 
the _entry point_ of the program. We have provided the basic structure for your
project but you will need to add a few things. 

We'll start by plugging in the `index.css` file and the `jQuery.min.js` file. 
Inside the `<head>` element, add the following lines:

```html
<link rel="stylesheet" type="text/css" href="index.css">
<script src="jquery.min.js"></script>
```

When plugging in CSS files to our `index.html` page, we use the tag:

```html
<link rel="stylesheet" type="text/css" href="filepath.css">
```

When plugging in a JavaScript file to our `index.html` page we use the tag:

```html
<script src="filepath.js"></script>
```
Remember this last one...


## TODO 3: Link the index.js file

Now we want to link our `index.js` file. 

When the browser loads our `index.html` file, it starts at the top of the file
and reads each line one at a time until it reaches the bottom. When it gets to
an externally loaded file, it must first read through that entire file before
moving on to the next line in our HTML.

Since we will be using JavaScript and jQuery to manipulate the existing HTML 
elements on our page, we have to wait for those elements to be loaded _before_ 
we can load in the `index.js` file. So, we have to link this file at the bottom
of our page, _after_ the `<body></body>` tags.

Between the closing `</body>` tag and the closing `</html>` tag, use the `<src>`
tag to link the `index.js` file.

## TODO 4: Review your work

Nice job! Our HTML page is now set up with all necessary files plugged in! Often
times, before you even start programming, setting up a basic file structure is
the first step. You'll want to consider what various components you'll need and 
then link them all together in the `index.html` page. As you add new components
always remember to link them to the index page!

Double check that you have followed the instructions carefully. At this point,
your HTML file should look like this:

```html
<!DOCTYPE html>
<html>

  <head>
    <title> Snake </title>
    <link rel="stylesheet" type="text/css" href="index.css">
    <script src="jquery.min.js"></script>
  </head>
  
  <body>
    <div id='board'> </div>
    <h1 id="score"> Score: 0 </h1>
    <h1 id="highScore"> </h1>
  </body>
  
  <script src="index.js"></script>

</html>
```
# Part 2 - JavaScript Setup

## Input / Output

Every game provides some way for the user to have control. In this game, we will be using the keyboard to control our snake.

**Change over to the console tab in the inspector** and start pressing arrow keys. You'll notice that the direction that you press will be printed! This is because of this line:

```javascript
$('body').on('keydown', setNextDirection);
```

which makes our webpage respond to key presses with the function
`setNextDirection`. This is known as an _Asynchronous Function Call_. This means that the function `setNextDirection` is not called in any sort of sequence, rather it is called in response to an `event` occuring.

The primary purpose of the `setNextDirection` function is to modify the `.direction` property of the `snake.head` Object. You can find the definition of this function in the *HelperFunctions* section at the bottom of the program. See if you can follow what it does!

##  Animation

Animation can be easily achieved by rapidly making changes to the appearance of our program. 

In the `init` function, look at the this line:

```javascript
updateInterval = setInterval(update, 100);
```

The function `setInterval` works by calling the `update` function every `100` milliseconds (10 frames / second). This will give us a way to modify the game over time, like a flipbook! We will animate our game by making slight adjustments on each frame. 

Let's look at the logic of this `update` function:

```javascript
function update() {
  moveSnake();
  
  if (hasHitWall() || hasCollidedWithSnake()) {
    endGame();
  }
  
  if (hasCollidedWithApple()) {
    handleAppleCollision();
  }
  
}
```    
    
On each tick of the timer we move the snake. We'll check if it has collided
with the wall, itself, or the apple. 

If it collides with the Apple, handle that collision (this includes lengthening the snake, adding a new apple, and increasing the score). 

If it collides with itself or the wall, end the game.

As you can see, we are using a lot of functions to write out the logic. This 
is a strategy to make the main logic of the program readable. We can
now dive into each function to get them working!

# Part 3) Data and Planning

Now that the setup has been taken care of, let's get into the fun stuff. To create this program, we need to do some planning:
1) What are the components of the game?
2) How can I visually render those components?
3) For each game component, what data will I need to track while my game is running?
4) For each piece of data, will that data change throughout the course of my game? If so, how?

**Before reading on, write down your answers to these questions**

### 1) What are the components of the game?

- Board (2D grid with 20 rows and 20 columns)
- Score Display
- High Score Display
- Snake
- Apple

### 2) How can I visually render those components?

Go ahead and run the `index.html` file. At this point your screen should 
be black with a white outline for the board, show the score at 0 and inside the board will be a single green square. This is the snake's head! The apple will be added later. 

This project will rely on HTML, CSS, and jQuery to render the components of the game. As we know, we can create visual elements on our screen using HTML tags, like so:

```html
<div id="board"></div>
```

And then style them using CSS like so:

```css
#board {
  width: 440px;
  height: 440px;
  border: 1px solid white;
  background-color: black;
}
```

This combination of HTML and CSS creates our board on the screen

Open up the `index.css` file and you'll see that we've created the CSS for this project for you. A few of the style rules will be applied to the HTML elements that we created earlier: `#score`, `#highScore` and `#board`. 

You'll also notice that we have CSS rules defined for the `.snake` and `.apple` classes. But those HTML elements don't exist yet!

Since the Snake and Apple are changing constantly throughout our program, we will be _procedurally generating_ the HTML for those elements using the JavaScript library **jQuery** which can be recognized by the all-in-one function `$` (equivalent to the function `jQuery`).

Open the `index.js` file and find the functions `makeSnakeSquare`. The first line is:

```javascript
// make the snakeSquare jQuery Object and append it to the board
var snakeSquare = $('<div>').addClass('snake').appendTo(board);
```

Let's break this down:

```javascript
$('<div>')  // create a new div HTML element using the jQuery $ function. Notice the <>
```

```javascript
.addClass('snake')  // add the .snake class to that Element
```

```javascript
.appendTo(board);  // insert the new Element as a child of the #board
```

This combination of jQuery method calls will also return a JavaScript Object that we can save in a variable to keep a reference of. This allows us to manipulate it further down the road. A reference to each new `snakeSquare` that we create is added to the `snake.body` Array. More on that later.

Additionally, since `snakeSquare` is just a JavaScript Object, we can easily add properties for each `snakeSquare`:
- `snakeSquare.row`
- `snakeSquare.column`
- `snakeSquare.direction`

NOTE: Everything above can be applied to the Apple as well.

### 3) For each game component, what data will I need to track while my game is running?

At the top of the program, all variables required for the lifetime of the program have been declared for you. By declaring them at the top of the program, we can easily see what data will be important to keep track of for the code to follow.

##### HTML DOM Elements

- `board`: jQuery Object to reference/manipulate the board element
- `scoreElement`: jQuery Object to reference/manipulate the score display element
- `highScoreElement`: jQuery Object to reference/manipulate the high score display element

##### Game Data
- `snake`: An Object to manage the snake with a head, tail, and body properties. 
The snake will be made up of individual `snakeSquare` jQuery Objects that each 
will have their own `.row`, `.column`, and `.direction` properties.
    - `snake.body`: An Array containing all `snakeSquare` jQuery Objects. 
    - `snake.head`: Reference to the jQuery `snakeSquare` Object at the head of the snake. Same as `snake.body[0]`
    - `snake.tail`: Reference to the jQuery `snakeSquare` Object at the end of the snake. Same as `snake.body[snake.body.length - 1]`
- `apple`: Reference to the jQuery `apple` Object
- `score`: The number of apples eaten

#### Interval Variable
- `updateInterval`: the interval that is running the update function 
- (See https://www.w3schools.com/jsref/met_win_setinterval.asp)

##### Constant Data
- `ROWS`: number of rows in the board
- `COLUMNS`: number of columns in the board
- `SQUARE_SIZE`: the pixel dimensions of each square
- `KEY`: map for keycodes of the arrow keys (see https://keycode.info/)
  - It's common to create a keycode map when dealing with key commands.

### 4) For each piece of data, will that data change throughout the course of my game? If so, how? 

TODO

# Part 4) Completing the Game's Logic

## TODO 5: Make the head move

**Set up**

The first step in the `update` function is to actually move the snake. Find the `moveSnake` function. We'll start by moving the `head` of the snake. 

We'll start by modifying the `snake.head.direction` property, add this line below
`TODO 5`:

```javascript   
snake.head.direction = snake.head.nextDirection;
```

`snake.head.nextDirection` will either be `"left"`, `"right"`, `"down"`, or `"down"`, and is set based on keyboard input. 

Now that we know what direction our snake's head is moving in, we can determine which row and column it should move to on the next frame. We'll do this changing the `snake.head.row` or the `snake.head.column` property.

Below the line of code you just added, add these lines of code:

```javascript        
// determine how to change the value of snake.head.row and snake.head.column based on snake.head.direction
snake.head.row = snake.head.row + 0;
snake.head.column = snake.head.column + 0;

repositionSquare(snake.head);
```

Once we determine the new row and column for the head of the snake, we have to actually reposition that HTML element on the screen. The `repositionSquare` function accepts as input a jQuery Object (which can be any part of the snake's body or the apple) and then places that
Object correctly on the screen. 

**Goal: Determine the snake head's next row and column position based on its current row, column, and direction.**

Use the following pieces of data to calculate the values of `nextRow` and 
`nextColumn`

```javascript  
snake.head.row          // the current row of snake.head
snake.head.column       // the current column of snake.head
snake.head.direction    // the direction that the head should move towards
```

HINT: The top row in the board is row `0` and row numbers increase as you move down. The left-most column in the board is column `0` and column numbers increase as you move to the right.

## TODO 6: Check for collisions with the wall

Now that our snake can move freely, we need to put some constraints on it. We 
don't want our snake to leave the confines of the board (sorry snake).

The next step in our games `update` logic is to check if the snake has either
collided with the walls or with itself. Let's start with the walls.

Find the function `hasHitWall`.

**Goal: This function should return true if the snake's head has collided with 
any of the four walls of the board, false otherwise.**
  
Use the following pieces of data to determine if the snake's head has collided
with one of the walls.

```javascript
ROWS                // the total number of ROWS in the board
COLUMNS             // the total number of COLUMNS in the board
snake.head.row      // the current row of snake.head
snake.head.column   // the current column of snake.head 
```

## TODO 7: Add the apple

To create an apple we can call the function `makeApple` which is defined in the
*Helper Functions* section. We want to create an apple when the game starts so
in the `init` function below `TODO 7` add this line of code:

```javascript
apple = makeApple();
```

If you take a look at the definition of the `makeApple` function you'll see that
it finds a random position for the apple by calling the function
`getRandomAvailablePosition()`. More on that later.

**Refresh your game and try swallowing the apple with the snake**

## TODO 8: Check for collisions with the apple

Now that our `apple` has been added to the board, we need to train our snake to 
actually eat it!

Within the `update` function we can see the logic for doing this:

```javascript
if (hasCollidedWithApple()) {
    handleAppleCollision();
}
```

If the snake `hasCollidedWithApple` then we can `handleAppleCollision`. Makes 
sense! Let's start with detecting collisions with the apple.

Find the definition for the function `hasCollidedWithApple`.

**Goal: This function hould return true if the snake's head has collided with 
the apple, false otherwise**
  
Use the following pieces of data to determine if the snake's head has collided
with the apple.

```javascript
apple.row           // the current row of the apple
apple.column        // the current column of the apple
snake.head.row      // the current row of snake.head
snake.head.column   // the current column of snake.head 
```

**Save your code, refresh your game, and observe the changes!** If you did this
step properly then your snake should be able to eat the Apple.

### TODO 9: Handle Apple Collisions

You may notice that our apple eating behavior isn't perfect. Find the function 
`handleAppleCollision`. At this point it should have the following logic:

```javascript
// increase the score and update the score DOM element
score++;
scoreElement.text("Score: " + score);

// Remove existing Apple and create a new one
apple.remove();
makeApple();
```

Some things are working fine - the score is increasing, the eaten apple 
disappears and a new one is created - however our snake isn't getting any 
bigger! Instead a random green square is being added in the top left corner of 
the screen. What gives?!?

At the bottom of the function you can find this logic:

```javascript
var row = snake.tail.row + 0;
var column = snake.tail.row + 0;

// code to determine the row and column of the snakeSquare to add to the snake

makeSnakeSquare(row, column);
```

As we can see, right now we are creating a new snakeSquare at position (0, 0).

**Goal: determine the `row` and `column` where the next snakeSquare should be 
placed so that it is added on to the tail of the snake**

Use the following pieces of data to determine if the snake's head has collided
with the apple.

```javascript
snake.tail.direction    // "left" or "right" or "up" or "down"
snake.tail.row          // the current row of snake.tail
snake.tail.column       // the current column of snake.tail
```

HINT: If the snake's tail is moving right, the next snakeSquare should be one
column to the left. If the column is moving up, the next snakeSquare should be
one row below.

**NOTE: Completing this TODO will not make your snake grow properly. It will 
simply create each new snakeSquare at the point that the snake ate its first 
apple. Complete the next TODO to make your snake properly grow.**

## TODO 10: Move the body

Find the function definition for `moveSnake`.

Our program is still not working properly. When our snake eats an apple, a new
snakeSquare is added to the board in the correct location. However, each new
snakeSquare does not follow the snake! 

Add this code below the comment for `TODO: 10`:

```javascript
for ( /* code to loop through the indexes of the snake.body Array*/ ) {
    var snakeSquare = "???";
    
    var nextSnakeSquare = "???";
    var nextRow = "???";
    var nextColumn = "???";
    var nextDirection = "???";
    
    snakeSquare.direction = nextDirection;
    snakeSquare.row = nextRow;
    snakeSquare.column = nextColumn;
    repositionSquare(snakeSquare);
}
```

In order for the snake to follow the head, each snakeSquare must learn 
the position and direction of the snakeSquare that is in front of it. Since we 
want to apply this same logic to every snakeSquare in the `snake.body` Array,
iteration using a `for` loop will be very helpful!

**Goal: Reposition each snakeSquare in the `snake.body` Array and update the
direction for each snakeSquare**.

HINT 1: Before you start coding, think through how this process will work. How
can you access each `snakeSquare` in the `snake.body` Array? How do you know 
what the `nextSnakeSquare` should be?

Hint 2: The `for` loop will need to be set up in a particular way to make sure 
that each snakeSquare can follow the snake that comes before it without any data 
being prematurely overwritten.

Hint 3: Remember that the snake's head is the first entry in `snake.body` so 
make sure that your loop doesn't include index `0`!

## TODO 11: Check for snake collisions with itself

After eating a few apples our snake will be long enough to potentially run into
its own body! Try doing this and you'll notice that our snake will just breeze
right through. This is not good...

According to our logic in the `update` function, when this happens, the game is 
supposed to end! We need to fill out the `hasCollidedWithSnake` function so that
it properly detects this collision.

Find the `hasCollidedWithSnake` function. 

**Goal: This function should return true if the `snake.head` has overlapped with
any snakeSquare in `snake.body`.**

What data will you need to use to solve this problem?

Hint: Remember that the snake's head is the first entry in `snake.body` so 
make sure that you aren't comparing `snake.head` with `snake.body[0]`!


## TODO 12: Make sure our Apple is placed only in available positions

Our game is complete! Almost complete anyway...

One tiny detail, and quite an interesting problem to solve, is how to make sure
that when our apple is regenerated, it is positioned in a square on our screen
that is actually available - that is, in a position not occupied by the snake.

Find the function `getRandomAvailablePosition`.

Currently, this is its logic:
    
```javascript
var spaceIsAvailable;
var randomPosition = {};

while (!spaceIsAvailable) {
    randomPosition.column = Math.floor(Math.random() * COLUMNS);
    randomPosition.row = Math.floor(Math.random() * ROWS);
    spaceIsAvailable = true;
}

return randomPosition;
```

It seems a little weird that we have a while loop that relies on 
`spaceIsAvailable` to be `true` and part of what it does is to set it to be 
`true` every time. 

While this code may generate a random location within the confines of our board,
it does not guarantee that the space is unoccupied by the snake.

**Goal: Modify the code block in the `while` loop so that if the randomly 
generated position is occupied by any part of the snake's body, it loops again**

