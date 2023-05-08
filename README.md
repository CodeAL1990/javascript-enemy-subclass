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
//TBC
