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
