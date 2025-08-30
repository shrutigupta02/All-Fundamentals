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

###### Tic Tac Toe:
created a board component and a main tictactoe component that handles game logic and display
created the following functions:
- generate squares that generates a 3x3 matrix with initial 0 value
- update square that uses setState for board every time a player makes a move
- check for winner that checks if any player has won using a while loop with conditions inside
- reset game that restarts a new game

##### Description:
What is PlayHive?
PlayHive is a web-based gaming platform that hosts classic games like 2048 and Tic-Tac-Toe. Think of it as a mini-arcade where users can play games, register accounts, and have their progress tracked.

Technical Stack Deep Dive

###### Frontend: React.js + Tailwind CSS

What You Built:
Games Page: Main landing page that displays available games in categories
Individual Game Components: Separate React components for 2048 and Tic-Tac-Toe
Common Components: NavBar, Footer, Background, CardSlider, GameDeck

Key React Concepts You Used:
Functional Components: All your components use modern function syntax
Hooks: useState for game state, useEffect for API calls
Props: Passing game data down to child components
Component Composition: Breaking UI into reusable pieces

###### Backend: Node.js + Express.js

What Your server.js Does:
Express Concepts You Implemented:
Middleware: Functions that run before route handlers (cors, json parsing)
Route Handlers: Functions that respond to specific HTTP requests
Error Handling: Try-catch blocks with appropriate status codes
Database Integration: Using Mongoose models to interact with MongoDB

###### Database: MongoDB

Your Data Models:
Games Model: Stores metadata about available games (name, description, etc.)
User Model: Stores user registration data (firstName, lastName, email, password, phone)

MongoDB Concepts:

Collections: Like tables in SQL (you have Games and User collections)
Documents: Individual records (each user is a document)
Schemas: Structure defined by your Mongoose models
CRUD Operations: Create (POST /user), Read (GET /games)

###### Authentication: Firebase

What You Integrated:
Firebase Authentication for user login/registration
Though your current backend shows basic user storage in MongoDB
Firebase handles the complex auth logic (password hashing, sessions, etc.)


Game Logic Breakdown
2048 Game Implementation
**Game Flow:**
1. User presses arrow key or swipes
2. Corresponding swipe function called
3. Board state updated with new positions
4. addNumber() adds new tile
5. getScore() calculates new score
6. gameOver() checks if game should end
7. React re-renders with new state

#### **How to Answer Common Questions**

#### **"Explain your component structure"**

_"I organized my React app with a clear hierarchy. The Games page is the main container that fetches data and renders game categories. Each game (2048, Tic-Tac-Toe) is a separate component with its own logic. I have shared components like NavBar and Footer that are reused across pages. The GameDeck component handles the layout of game cards."_

#### **"How do you handle API calls?"**

_"I use axios to make HTTP requests. In my Games component, I have a useEffect hook that runs on component mount to fetch available games from my backend. I handle both success and error cases with .then() and .catch() methods."_

#### **"Explain your Express.js setup"**

_"My Express server is straightforward - I set up middleware for JSON parsing and CORS, connect to MongoDB, and define two main routes. The GET /games route fetches all games from the database, and POST /user handles user registration with proper error handling."_

#### **"How do you store game state?"**

_"For both games, I use React's useState hook to manage state locally. The 2048 game uses a 2D array to represent the grid, while Tic-Tac-Toe uses a 3x3 matrix. State updates trigger re-renders to show the current game state."_

#### **"What about form validation?"**

_"I integrated Formik for form state management and Yup for validation schemas. This handles user input validation for registration forms, including email format, required fields, and password requirements."_

