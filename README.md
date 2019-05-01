# ConnectFour-
A version of the popular British 1976 game written and developed using python and pygame.  

This project will teach us how to develop and engineer a simple connect four mini-game using python 3 and its respective multimedia library pygame. This is a great starting off protect for users who have a beginner to intermediate level of skill who want to show some project work to go with their development. The way this is going to work is that we will try to create a command line-based game, and work our way up to the graphical game later 

# Step 1: Creating the CMD Template
The first thing you need to think about is what is the best way to represent a connect four board. If you are familiar with the game, you’ll know that the board is 6X7 grid. I believe the perfect way to represent this is a python matrix so that’s were we’ll start of with. 

So we define a function called ```create_board()``` that will take no inputs. Now we need to import a package called Numpy that will help us in creating our matrix. Now if you don’t already have this installed you can open your command line and type ```pip install numpy```. We start by making a matrix with all zeros using ```board = np.zeros((6,7))``` our inputs are the dimensions of our board as discussed earlier, simply return the board for a print out to test . Having all zeros is good as it’s our initial state of the game. When you print out the return board you should see the following on your CMD.

![ConnectFour(Part One)](https://user-images.githubusercontent.com/22968181/56972053-3c0d9180-6b62-11e9-8e4f-e542c2f7bf38.PNG)


Now we need to create our main game loop this is where all the logic of our program will be placed. So, when the game is not in a game over state do the following actions. The game will run forever if this game_over variable is false and it’s only ever true when we have a connect four, so we need to create this variable. We have two players in this game player 1 and player 2 so we need to create another variable to tell us who’s turn is who’s. So, back in our loop when the turn is a certain value it’s a player turn.  When it’s player ones turn, I ask for their input now seeing how we are starting off with a command line based we’ll be taking in numbers from the keyboard. Now we need to convert the col variable to an int as its currently a string which won’t do. Now make the same changes for player two. We then increment our turn variable and mod divide by 2 in order to alternate our player turns. Run it to see if it works.

![ConnectFour(1 2)](https://user-images.githubusercontent.com/22968181/56972215-80992d00-6b62-11e9-990a-633c9be1eeb5.PNG)


Now what we want is to make a selection and drop our piece into the board, so we create a number of functions starting with ```drop_piece():``` which does that it says on the tin. We also create a second method called ```is_valid_location()``` which checks to make sure the user input is a correct value, accounting for minus numbers and numbers that exceed the range of our matrix. Finally we finish with ```get_next_open_row():``` Now all these methods will work together to create our game. We start by passing our board and col into our ```is_valid_location()``` method.

We need to figure out if a selected row chosen by the user is free or not so if the top column is free then our program knows to drop a token in there, within our ```is_valid_location()``` we return the board and the col location the user has picked and if it’s equal to 0 then we know that action is permitted. Moving on to the ```get_next_open_row()``` method when it comes to the our program we need to know which of the rows has or hasn’t gotten a token and to do this we are going to write a quick little loop. We loop through the values of everything in our ROW_COUNT variable and if it’s equal to 0 then we know that drop token action can be permitted.  Now we have some numbers in a project that appear to have to come out of nowhere this is what’s known in programming as magic numbers as is consider bad practice, so we are going to have these numbers represented as global variables.

Next we have to actually drop the piece and this of course will all take place within our ```drop_piece()``` method this need to take in the board, row, col, and piece arguments and all we do is that we get the position that is valid and drop the piece in. To get this all to work fluently we need to start using these methods within our main game loop  (while loop) so where we left off if ```is_valid_location(board, col)``` is true and valid then we create our row that is intilaised to 0 ```get_next_open_row():``` (which checks for valid rows) and once those checks have been done we use the ```drop_piece():``` method to well drop the piece. Now we repeat for player two.


After we done with the player turn, we need to print out the board to show the changes. Run and check for errors. Now one issue you may have noticed is that our piece are at the time of the board are working our way down we know that’s not how gravity works so to fix this we need to add an additional function called ```print_board():``` and all that’s going to do is change the orientation of the board.

![Connectfour(1 3)](https://user-images.githubusercontent.com/22968181/56972308-ad4d4480-6b62-11e9-84e9-6a4bee63f696.PNG)

Code so far 
```python
import numpy as np

ROW_COUNT = 6
COLUMN_COUNT = 7


def create_board():
    board = np.zeros((6, 7))
    return board


def drop_piece(board, row, col, piece):
    board[row][col] = piece


def is_valid_location(board, col):
    return board[5][col] == 0


def get_open_row(board, col):
    for i in range(ROW_COUNT):
        if board[i][col] == 0:
            return i


def print_board(board):
    print(np.flip(board, 0))


board = create_board()
print_board(board)
game_over = False
turn = 0

while not game_over:
    # Ask for player 1 input
    if turn == 0:
        col = int(input("Player 1 make your selection (0-6):"))

        if is_valid_location(board,col):
            row = get_open_row(board, col)
            drop_piece(board, row, col, 1)

    # Ask for player 2 input
    else:
        col = int(input("Player 2 make your selection (0-6):"))

        if is_valid_location(board,col):
            row = get_open_row(board, col)
            drop_piece(board, row, col, 2)

    print_board(board)

    turn += 1
    turn = turn % 2
```
# Step 2: Winning the game

The problem we have now is that there is no way for the game to tell us that it’s won so we need to program that aspect next. To do this we need to add a function called ```winning_move():``` and have that take in the board and piece as an argument. There is a number of different ways to implement this. The way we are going to do this is not the most efficient way out there but bear in mind this is for users who only have a beginners knowledge in python, what I’m going to do is to manually check each individual position(verticals, horizontals and diagonals ) and check to see if there’s a winning combination.

First, we are going to check all the horizontals, and to do that we need a loop that iterates over the columns and another loop within it that will loop over the rows. Now when you look at the board you can see that you can only win a game in the middle positions vertically speaking so we need to subtract 3 from our COLUMN_COUNT in the loop. Now to see if we can win the game vertically we need to get the current 2 loop positions column and row and see if they = a piece and increment the n loop by 1,2,3 to check the vertical positions for a token and return true if they all contain a token. So when the user wins a game we need to go to the main game loop and say when the ```winning_move():``` method is called  (meaning a game has been won) print player has won and set the game_over variable to True and the main game loop will terminate and the program will finish.

![ConnectFour(1 4)](https://user-images.githubusercontent.com/22968181/56983992-e72b4480-6b7c-11e9-9b10-5d2f6ec936f9.PNG)


Next, we do a similar process for vertical wins this essentially the opposite of what we did for the horizontal wins. I recommend you copy and past the previous section and make the following adjustments. All that’s needed is to make row count – 3 and add to the n loop instead of the i test and run to see if it works.


Now we have to check for a diagonal win, which once again is similar to before instead we add + 1 to the column count the row count and two loops because we are checking for the positively shaped slopes.

![ConnectFour(1 5)](https://user-images.githubusercontent.com/22968181/56984126-37a2a200-6b7d-11e9-82ef-293549d44cfa.PNG)


```python
def winning_move(board, piece):
    # check horizontal locations for win

    for i in range(COLUMN_COUNT - 3):
        for n in range(ROW_COUNT):
            if board[n][i] == piece and board[n][i + 1] == piece and board[n][i +2] and board[n][i+ 3] == piece:
                return True

    # check vertical locations for wins
    for i in range(COLUMN_COUNT):
        for n in range(ROW_COUNT - 3):
            if board[n][i] == piece and board[n + 1][i] == piece and board[n + 2][i] and board[n + 3][i] == piece:
                return True

    # Check positive slopped diagonal locations for wins
    for i in range(COLUMN_COUNT - 3):
        for n in range(ROW_COUNT - 3):
            if board[n][i] == piece and board[n + 1][i + 1] == piece and board[n + 2][i + 2] and board[n + 3][i +3] == piece:
                return True


    # Check negatively sloped diaganols
    for c in range(COLUMN_COUNT-3):
        for r in range(3, ROW_COUNT):
            if board[r][c] == piece and board[r-1][c+1] == piece and board[r-2][c+2] == piece and board[r-3][c+3] == piece:
                return True


```

# Step 3: Building the graphics
This part we are going to implement the graphics of the game and to do this we will need to import the package pygame and if you don’t have it installed do the same what you did for numpy with ```pip install pygame```. We start off with initialising pygame and by configuring our screen size. The screen can be broken up of squares sizes of 100 pixels each. The width of the screen is going to be the column count by the square size, and height will be row count by square size. When we run the game there is an additional row count that will allow the token to glide over the columns it adds a nice fluency to the game. We add + 1 to the row size to create room at the top. We create a size variable to store these parameters as a tuple. We develop a screen object and using pygame we take in our tuple to display the screen of our game.  Test to see if our screen is working. You can get these pygame methods from the documentation at   https://www.pygame.org/docs/ 

The screen pops out for a second which is what is expected at his point so what we do is that we need to create another method called ```draw_board():``` which will essentially print our board to the screen but with pygame graphics. The way pygame works is that its event based so this would be actions such as mouse clicks and mouse motion this what we need to do next. Starting in our main game loop, we create another loop that loops over all motion events. We need to allow our player to exit out of the game at any moment  and we allow this like most applications allow exits by enabling the x in the upper right part of our window so we create a conditional statements that allows for ```if event type equals pygame.Quit:``` we then preform a system exit and to do that we need to import sys which comes with every python installation.

![ConnectFour(1 6)](https://user-images.githubusercontent.com/22968181/57017948-463e9700-6c19-11e9-8ba6-a24a4b1e3ece.PNG)


The first event we are going to focus on is Mouse button down which drops a certain piece on the screen. So instead of reading in the column we want to drop in the piece like we did with our CMD version no it’s going to be populated by where we click instead. First place all of code for player 1 and 2 input into that if statement. Now when we run this you can see that our screen remains. Now that we have that working we need to start working on our ```draw_board():```  method and make our graphics.  We are going to draw a rectangle as a background for our board and then colour the rectangle blue. Then we need to create our piece that we drop in alternating between the colours to distinguish between player 1 and player 2.

To start we are going to iterate through the board with a loop for rows and columns. We draw a blue rectangle that will be the background in our board. We need to create a blue Global variable to represent the colour of this rectangle this take in an RGB value. We need to create the rectangle location so we multiple i by the square size which gives us the top right-hand corner of the shape on the x axis and to get the y axis we multiple r by the squaresize. Next, we specified the length and width of our rectangle as squaresize by squaresize (100, 100). Call the method with the board and to up date pygame you need ```pygame.display.update():``` Now we need to cut out the holes in the blue rectangle to represent our slots. We do this similar to how we create the rectangle shape in pygame, defining black as a global for the holes the first two parameters for the circle method are the same as rectangle with surface and colour and location, but we need to define a Radius in our global variables. Our radius is going to be a little less than our squaresize/2 - 5 you kind of play around these numbers but this is what works for me. The location of our holes we’ll need to add are squaresize plus squaresize and divide by 2 and we slot in our radius as the last argument. Run the application and you should have the board up and running. 


![connectFour(1 7)](https://user-images.githubusercontent.com/22968181/57017977-653d2900-6c19-11e9-9b9b-673d954f8ddf.PNG)
```python
BLUE = (0, 0, 255)
BLACK = (0, 0, 0)

def draw_board(board):
    for i in range(COLUMN_COUNT):
        for n in range(ROW_COUNT):
            pygame.draw.rect(screen, BLUE, (i * SQUARESIZE, n * SQUARESIZE+SQUARESIZE, SQUARESIZE, SQUARESIZE))
            pygame.draw.circle(screen, BLACK, (int(i * SQUARESIZE + SQUARESIZE / 2), int(n * SQUARESIZE + SQUARESIZE + SQUARESIZE / 2)), RADIUS)
            
            
while not game_over:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()

        if event.type == pygame.MOUSEBUTTONDOWN:
            continue
            
            
```  
