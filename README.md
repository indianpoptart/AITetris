-------
Objective
-------
Your objective is to create an AI that will solve tetris for as long as possible. We will grade you on number of moves and the amount of time the game lasts. Below is information on the file structure and things youíll need.

Materials
	Readme.txt -- this introduction
	JTetris.jar -- a functioning Tetris
	
If you have any questions feel free to reach out to Ryan Miller (ryan.miller2@ge.com) and Kyle Dillon (kyle.dillon@ge.com) as they wrote the project and will be doing the grading.
-------
Running
-------

The provided JTetris.jar contains a runnable Java version of tetris
which supports the brain features (below). The program needs Java 1.2
or later -- the free download from Sun is http://java.sun.com/j2se/1.3/jre/

Youíll also need an IDE to develop and run the program; we recommend netbeans as that is what we used to make the project.

Once you get the IDE feel free to open the project and run it to get a sense of how it works. 
The program includes a little slider that controls the speed.

You will edit the brain file and add an AI algorithm that is better than the one there. The file is called ITLPAI and is located under source packages/tetris.

--------
Projects
--------

1. Fun: Brain

A fun project that's not too hard is designing a tetris brain.
The code to implement a brain is surprisingly short.
The default brain is called ITLPAI, and it's
game playing logic is less than a page long.
Writing a brain requires just basic Java and OOP skills.
Weíve provided you with a basic AI to begin with: ITLPAI, start there and try to improve upon it. Write your own subclass or to replace the features of the ITLPAI class.

Running the program will use the AI to solve, we will grade it at max speed and time to finish.
The classes that make up Tetris are...
ï	Piece -- a single tetris piece
ï	Board -- the tetris board
ï	AI-- simple heuristic logic that knows how to play tetris
ï	ITLPAI -- a subclass of AI that uses a brain to play the game without a human player
As an additional feature, the tetris classes implement the logic for tetris in a way that runs quickly. 
Piece
The Piece class defines a tetris piece in a particular rotation. Each piece is defined by a number of blocks known as its "body". The body is represented by the (x, y) coordinates of its blocks, with the origin in the lower-left corner. 
  
So the body of this piece is defined by the (x, y) points :  (0, 0), (1, 0), (1, 1), (2, 1).
The Piece class and the Board class (below) both measure things in this way -- block by block with the origin in the lower-left corner. As a design decision, the Piece and Board classes do not know about pixels -- they measure everything block by block.
Each piece responds to the messages like getWidth(), getHeight(), and getBody() that allow the client to see the state of the piece. The getSkirt() message returns an array that shows the lowest y value for each x value across the piece ({0, 0, 1} for the piece above). The skirt makes it easier to compute how a piece will come to land in the board. There are no messages that change a piece -- it is "immutable". To allow the client to see the various piece rotations that are available, the Piece class builds an array of the standard tetris pieces internally -- available through the Piece.getPieces(). This array contains the first rotation of each of the standard pieces. Starting with any piece, the nextRotation() message returns the "next" piece object that represents the next counter-clockwise rotation. Enough calls to nextRotation() gets the caller back to the original piece.
Board
The Board class stores the 2-d state of the tetris board. The client uses the place() message to add the blocks of a piece into the board. Once the blocks are in the board, they are not connected to each other as a piece any more; they are just 4 adjacent blocks that will eventually get separated by row-clearing.
ï	place(piece, x, y) -- add a piece into the board with its lower-left corner at the given (x, y)
ï	clearRows() -- compact the board downwards by clearing any filled rows
Board has many methods that allow the client to look at the Board state. These all run in constant time -- makes things easy and fast for the client, but harder for the board implementation...
ï	int getWidth() -- how many blocks wide is the board
ï	int getHeight() -- how many blocks high is the board
ï	int getRowWidth(y) -- the number of filled blocks in the given horizontal row
ï	int getColumnHeight(x) -- the height the board is filled in the given column. This is 1 + the y value of the highest filled block
ï	int getMaximumHeight() -- the max of the getColumnHeight() values
ï	int dropHeight(piece, x) -- the y value where the origin (lower left corner) of the given piece would come to rest if the piece dropped straight down at the given x
Board Undo
The Board also supports a 1-deep undo() facility that allows the client to undo the most recent placement and/or row clearing. The undo facility is rather limited -- the client can do a single place() and a single clearRows(), and then use undo() to return to the original state. Although simple, the undo facility is fast. It turns out that the Brain (below) needs exactly this facility of a simple but fast undo. Here's how undo works...
ï	We'll designate the  state of the board as "committed"  -- a state that can be returned to.
ï	From a committed state, the client can do a place() operation that modifies the board. An undo() will remove the change so the board is back at the committed state.
ï	Then the client can do a clearRows() operation which further modifies the board. An undo() will remove all the changes so the board is back at the committed state.
ï	Finally, the client can do a commit() operation which marks the current state as the new committed state. The previous committed state can no longer be reached via undo().
A client can use the undo facility to animate a piece moving in the board like this: place() piece with y=15, pause, undo(), place() with y=14, pause, undo(), place() with y=13, ...


