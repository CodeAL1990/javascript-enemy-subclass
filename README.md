# javascript-enemy-subclass

Add the boilerplate like previous previous projects, with the exception being the 3 image tags in the html
After setting up the variables for your canvas, it's time to create a Game class with the usual update and draw methods
Your Game Class will have a property of enemies with an empty array where we will push different enemy types to it
Add a new PRIVATE(#) method to the class called addNewEnemy
Now, create new Enemy class where you will call it and create new enemies with different types
The Enemy class will be in charge of the properties of only Enemy objects such as their position, movement patterns, sprite animation etc
Now, create a function called animate where it will handle all animations in the game frame by frame
As usual, clearRect at the start and requestAnimationFrame(animate) at the end in it, and call the function
Now, cut all you have written and put it in a load event listener for document so you will only trigger the functions and classes inside only when everything is loaded(including your stylesheets and other dependents)
requestAnimationFrame has a built in function of extracting the refresh rates on your current pc and using them
To make use of it, pass a timeStamp argument in animate function
To compare previous timeStamps and timeStamp for the current loop, create a constant variable in animate called deltaTime(another name for timeStamp) and assign it timeStamp deducted by lastTime(a variable that will hold the previous timeStamp)
lastTime will be set to timeStamp right after to store the current timeStamp and be used again when called, creating a loop of timeStamps
Declare/Initialize lastTime variable outside animate and set it to 0;
console.log deltaTime to check out what info it is storing
\\Video author says to use load event listener but DOMContentLoaded works but using load does not for timeStamps currently. Will see if changing it later makes the load event listener works or not
Pass 0 as an argument when calling animate because for the first call animate() does not have a timeStamp and will show NaN in the console.log
console.log that constantly generates output will cause performance issues, so remove them once you have ascertain the variable is working
Add x, y, width and height properties to Enemy class and hardcode some numbers to them
Decrement x in update(to move sprite from right to left)
Use fillRect method in draw with the 4 properties you just created for now
Now, we are going to push the Enemy class with experimental properties to the enemies array in the PRIVATE method you made in Game class
Use the push method and push new Enemy() to it
This will allow the PRIVATE method to track and find the Enemy class and use its properties inside and gain access to update and draw inside Enemy class
You will now 'extend' the PRIVATE method into the Game class by adding #addNewEnemy as a property and calling it in the same line
Now, create a constant game variable and assign it new Game() to create 1 object of Game class
You will now use the forEach method to loop through enemies array in the Game update and for each object in the array, run its update method
Do the same for draw
To test the code out, call the update and draw method on game in animate function just before requestAnimationFrame
It's good practice to not directly use global variables to prevent bugs so pass context, width and height to the Game constructor
Now, pass ctx, canvas width and height in the arguments of the game variable to acquire and use these arguments
To pass the global variables as class properties in Game class, add ctx, width and height and assign them to themselves, corresponding to the arguments you have created
Pass the this keyword as argument for the push method in the PRIVATE method in new Enemy
Now, pass game as argument in Enemy class constructor and set it as a property assigned to itself
console.log() the game property to see what it has access to
Change the x position in Enemy class to the width of the game since we now have access to Game class properties
This will spawn the object at the right side of the canvas
For y position, you can put a random number between 0 and the full height of the game so it can spawn anywhere along the y axis
Currently, every refresh only cause the game to spawn 1 'enemy'
You could move the PRIVATE addNewEnemy() property from Game class into its update method but it will spawn too many and crash your game
Move the PRIVATE property into update and comment it
Add two new properties that will determine how fast you want the enemy objects to spawn
Add enemyInterval and set it to 400
Add enemyTimer and set it to 0
You will use them to spawn an enemy object every 400 milliseconds
So, add an if condition where if enemyTimer is more than enemyInterval, call the PRIVATE method that you have commented before (move it into the if condition and uncomment it)
Within this if condition, set the enemyTimer back to 0 whenever the condition happens
Else, increment enemyTimer(when enemyTimer is not yet 400 milliseconds)
This will cause the spawn speed to be based on your enemyInterval value AND your pc power
To standardize it across all types of pc, slow or fast, you need to use deltaTime variable in your animate function so that your game will run the same on all pc types
Pass the deltaTime variable into the update call on game in animate
You now have access to deltaTime from animate, you should pass it as an argument in the update method in Game class
Now, instead of using a standard increment of +1 on enemyTimer in update, increment it by deltaTime which you have just gained access from animate
Change the enemyInterval value to 1000
You should now see roughly the same number of enemy objects spawning per 1000 milliseconds in any computer types, regardless of refresh rates
Again, you should try not to use global variable to reduce bug creation
Currently, you are still referencing from the global canvas variable by passing it to the new Game() class method in game variable
In Game class under draw method, pass the property ctx into each of the object's draw method
Then, pass ctx into the draw method in Enemy class so the fillRect method will be referencing from the class property ctx instead of the global variable ctx
Currently, if you console.log the if condition of enemyTimer > enemyInterval, you will see the array of enemies grow continually even after the objects have left the canvas
You will need a way to remove the array items that have left the canvas
To do this, you will set every object to have a named property with a default value of false
When this property has left the canvas, this property's value will change to true, allowing javascript to recognise it and delete it
Add a property in the Enemy class called markedForDeletion and set it to false
In Enemy class update, add an if condition where if x is less than 0 minus the width of the sprite, markedForDeletion becomes true
To make use of the above if condition, use a filter method on the enemies array in Game class update, and for every object in the array, when markedForDeletion is true(!object.markedForDeletion to inverse the false value in the property), take them out
\*To further improve performance, you could place the filter method in the if condition in Game class update which will mean filter will only trigger whenever a new enemy object is added
We will now touch the subject of extends
Create a new Worm class and use the extends keyword and link it to Enemy
This means that the Enemy class is now a parent of the Worm class
This will allow javascript to look for methods and properties in the Enemy class if it does not exist in the Worm class
Since javascript will always look for properties and methods in the child class first before looking in the parent class, unique properties pertaining to the child class should be placed in the child class only
For the Worm class, add an image property and assign worm as its value
Pass the game variable as argument in the constructor and in the constructor, call the super() method and pass game as argument as well
super() is a built in method to call for the parent's properties inside the child class
The above method should always be used to gain access to the parent's properties before adding additional properties in the child class
You only want shared properties in your parent class and individually unique properties in your child classes, so properties like positions, width and height should be unique to each child
As such, move the x, y, width, and height over from Enemy to Worm
In Game class under the PRIVATE method, instead of pushing Enemy class, push the child Worm instead
Notice that image is set to worm in the Worm class but there are no references to it
If you console.log image, you will see that it returns a format similar to getElementById from your html file in the img tag
This is because any elements created in the DOM with an id attribute is automatically added to javascript execution environment as a global variable(word for word taken from a commenter)
Now your canvas is set, so let's use real sprites instead of rectangles!
Remove fillRect from draw method in Enemy class and use the drawImage instead
Pass in the image, positions, width and height properties into it for now
This will push all the worm image and squeeze it inside the currently specified width and height of the properties
Start with the first frame of the worm sprite sheet, add a property called spriteWidth (total width of sprites divided by number of sprites) and a property called spriteHeight
Now you have the exact size of each sprite, you will need to specify the additional sx, sy, sw, sh parameters in drawImage to specify where you want to crop the image
To crop out the first sprite, pass 0 into sx and sy, pass the spriteWidth for sw and spriteHeight for sh
Since the current width and height in Worm class are hardcoded values that you put on a whim, it has no relations with the spriteWidth and spriteHeight and will look stretched
To make them correspond to the sprites' dimensions, you can assign width and height toe spriteWidth and spriteHeight respectively and divide them by a number to reduce their size appropriately (Remember to multiply them by the division instead (i.e multiply by 0.5 instead of dividing by 2) to improve performance)
This will keep their aspect ratio and not stretch the sprites in any way
Currently, your worms move at the same speed across the canvas to the left
To vary the speed, add a property called speed in the Worm class and give it a random number between 0.1 to 0.2 for now
Instead of decrementing x constantly by 1 in the update method in the parent class, decrement it by the speed you just created
You will see the worms moving at different speeds, albeit very slowly
Change the random number to lets say 4 to 6 to visually see different speeds
Again, we want to make sure all machines run this game at the same speed so we want to make use of deltaTime to standardize the speeds
In Game class under update, pass the deltaTime argument into the update method of each object in the forEach loop
Also, pass deltaTime in Enemy class in update
Multiply the speed in the decrement operation by deltaTime
Slower computers will have larger deltaTime so your animation interval will be larger and vice versa but the frame rate for all pc types should be similar
Now the worms should be zooming across the canvas, so switch the random number of speed back to 0.1 to 0.2
Since this is horizontal movement only, speed is too generic of a name and could apply to vertical or other types of movements as well, so rename speed property in Worm class to vx to signify velocity of x(or even better velocityX)
Remember to change the variables affected by the calculations using speed as well
If you reduce the value of enemyInterval (thereby increasing the spawn rate), you should see that the worms are stacked on top of each other regardless of spawn positions
You want the worms that overlap on the top side to appear as if it is 'behind' the worm at the front, giving it a 3d look in a 2d space
Currently, we are pushing array items from index 0 to the last index as the items are being added which means they are being added in their index order
To achieve that 3d effect, you want to tie the position on the y axis of the array to the index order they are being added
To do this, use the sort method when adding new enemies in the private method, as sort compares the array items with the elements you have added in the function (i.e a,b) and pushing it up or down depending on the conditions you are specify in the callback
Since we are comparing it on the y axis, the array items that appear higher on the y axis should be first and subsequent ones later (so in an ascending order)
This means you are returning a.y - b.y
Child classes are useful because they allow you to give them unique properties that only affect them as stated before
Now copy and paste the Worm class and change it to Ghost class
Change the properties accordingly (image = ghost, different sprite width and height etc)
Ghosts should 'typically' move faster than worms so adjust vx so they become faster
Now we need to make it so that we push worms or ghosts randomly into the enemies array
To do this, add a new property in Game called enemyTypes and give it an array with worm and ghost as array items for now
Now create a variable in the PRIVATE method called randomEnemy and assign enemyTypes with an array of a random enemy type based on its length using Math.random of the length and wrap it with Math.floor since array items do not have decimal places
Now add a new if condition to check if randomEnemy coincides with the enemy type, push the relevant enemy child class into it
It makes sense thematically for ghosts to 'fly' on the canvas and for worms to 'slide' across the ground
So for y in Worm class, you want to deduct the game.height by the height of the sprite to present this effect, removing random method for now
Now you want the ghosts to take up only the upper portion of the canvas and stay off the ground so you want the y axis to be a random number between the total game height multiplied by like 0.6(60% of the upper canvas)
Due to this change (worms at the bottom ghosts at the top), the sort method you created before is redundant for now so comment it out
Remember how javascript would prioritize finding properties and methods from the child class first? You can add a new draw method inside Ghost class which will allow javascript to prioritizing the draw method in Ghost instead of using the draw method in Game class
Remember how you use the super() method to call for the parent's properties? use it on the new draw method you just created to call for the methods present in the draw function in the parent class
In this case, since you are calling the draw method, you will employ super.draw() in the draw method
The original draw method requires an argument of ctx (in Game class under draw method you called for ctx for each object), so pass the required ctx the super.draw() method as well
You will currently run all the code in your parent class (due to super.draw(ctx)) and any additional methods in draw method inside Ghost should only affect the ghost object (technically)
To change opacity of an object, you can use the built in globalAlpha method on the canvas and set it to 0.5 for ghosts in draw
This will cause every object in the canvas to have the property(including the worms)
To prevent this, use the save and restore method you use previously in the previous projects to save the state of the canvas before you draw and restore it to the original state at the end
Now that we know we can target and change child classes behaviour, you can go further by changing movement patterns of ghosts
Create an update method with the same argument of deltaTime inside Ghost class and use super method on the parent update with an argument of deltaTime as well
\*\* Remember to pass ctx on draw method in Ghost class(I CAUGHT THIS)
Add a property called angle in Ghost class and set it to 0;
To create a wavy movement pattern(sine wave), you will increment y axis with the Math.sin method on the angle property;
Incrementing angle property will let you observe a 'jerky' up down movement of ghost object
Multiplying Math.sin value by a number will increase the angle
Incrementing angle by small amounts will let you see a more prominent sine wave pattern
Changing the values of the multiplied value and increment value of angle will result in different waves
To randomise this, add a property called curve and randomise from 0 to 3
Now remove the hardcoded multiplication value in the Math.sin method and use curve instead
We will now create the final child class, Spider
Copy and paste the Worm class and change the properties accordingly
To make it so that spiders spawn only from the top of the canvas, deduct 0 by the height of the sprite in the y axis
Spiders would only move up and down(no horizontal movement), so set vx(or in my case velocityX) to 0
Add a new property in Spider class called velocityY and set it to 1
Add an update method for Spider and super it(with the same arguments)
Inside this update method, incremennt y axis by velocityY for now
Add spider as the new array item in enemyTypes and add a new line for the if condition in the PRIVATE method so spider gets pushed into the enemies array randomly as well
You cannot see the spiders on the canvas because currently, the spiders only move down and they start spawning at the x axis
To fix this, Set their x position to be a random number between 0 and game width
To move the spiders up and down, add an if condition in update where if y is above a certain number, incrementally multiply velocityY by negative 1 to pull the spider in the opposite direction
To give each spider a random length in which it pulls back up, add a new property called maxLength and give it a random number from 0 to the full height of the game
velocityY is currently hardcoded to 1.
To give each spider different speed, give velocityY a random number between 0.1 to 0.2
Multiply the increment in y in update method by deltaTime to get consistency across machines
Now, we want to create threads for the spiders when dropping down and pulling themselves up with these threads
Create a draw method for Spider class and super it
Use the beginPath method on the canvas, moveTo(0, 0) for now, and lineTo() to the positions of the spiders, and stroke the canvas
Now you should see lines appear at top left corner(0, 0), following each spawned spider
To make it so that the lines spawn from the original position of the spiders, pass the x position instead of 0 but leave y at 0 in moveTo method
this.x position is at the top left end of the 'rectangle' of the class, so the lines would appear at the top left position of the spider sprite
To fix this, add half of the divided width to moveTo and lineTo in the x position
There is a slight gap between the line and the spider, you can add a number to the y position in lineTo to fill this gap
Adding properties to Enemy class will be shared across its child classes
We will now 'animate' the sprites by shuffling through the sprite sheets
Add a frameX property to Enemy class and set it to 0
Add a maxFrame property and set it to 5 in Enemy class
Add frameInterval property and set it to 100 in Enemy
Add frametimer and set it to 0 in Enemy to accumulate values till frameInterval
Set a condition in update which needs access to deltaTime to check if frameTimer is more than frameInterval, invoke another condition where if frameX is less than maxFrame, increment frameX, else set frameX and frameTimer back to 0
Else, increment frameTimer by deltaTime
To display this in your canvas, you will need to replace sx from 0 to frameX(indicating starting position of the sprite sheet) multiplied by the spriteWidth of the sprite
Currently the game accumulates spiders forever because they never move pass the x axis since they move up and down the y axis, so markedForDeletion is never true in the parent class
To fix this, copy that condition(removal of enemies) in Enemy's update and paste it in Spider's update
Since Spider class objects only move in the y axis, change the condition to if y is less than 0 minus the height multiplied by 2, delete them
This will make sure that spiders are deleted as they leave the canvas and do not cause the enemies array to keep storing spiders
Done!
\*\*Found out why load event listener is not working, because i was using document instead of window event listener
