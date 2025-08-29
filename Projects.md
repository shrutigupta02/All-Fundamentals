## Playhive:
##### Game logic: 
###### 2048:
created swipe functions for swipe left, right, up and down that facilitate the trigger of swipe like motion in game board with the use of key press and merge tiles accordingly

initialised a 2d array as a hook to simulate game grid and track changes in the grid at different events of swipes

wrote functions like 
- getScore which would compute the scores based on values in the grid
- initialise which starts a new game with a fresh new grid
- addNumber which adds a random number tile every swipe
- gameOver which checks if there are any possible merge available by any swipes, if not then it sets the gameOver state value to true which terminates the game
- swipe up, down, left, right that are called when up, down etc etc keys are pressed
