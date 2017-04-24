# maze

1. HOW TO COMPILE & RUN
The first part of this file describes how to:
1.1 Compile this file into a java class structure
1.2. How to run the main class, once it's compiled
1.3. How to jar the structure for portability
1.4. How to run the jar, once it's been created.

2. PROGRAM & PROJECT INFORMATION
The second part of this file has:
2.1. An overview of the project requirements and program.
2.2. Maze Search Algorithm choice information
2.3. Structure of the program, including data storage choices.
2.4. Information about hardcoded elements.

3. ADDITIONAL DOCUMENTATION
At the end of this document is information on how to access the JavaDocs for this project.

************************
1. HOW TO COMPILE AND RUN
1.1 Compile this file into a java class structure
1.2. How to run the main class, once it's compiled
1.3. How to jar the structure for portability
1.4. How to run the jar, once it's been created.

from using ThTo run the jar file, download and extract the zip.

Change into the directory it creates.
There is a subdirectory called files/mazes.txt in that directory, along with a file called Maze.jar.

You must have Java 1.8 installed.

At a command line, type:

java -jar Maze.jar

The source files are in the Maze_full.zip file.  They won't compile as is, because the org.jetbrains libraries are not included.  I will try to find those tomorrow and upload a new version of the source.  Those libraries were part of my IDE.
The program requires Java 1.8 to run.
All imported classes are included in the project, so none have to be natively on the user's machine.
The maze file is also embedded in the java class structure.  More about that in the ABOUT section.

ABOUT THIS PROGRAM:
These are some details about the goal of the program, the structure of the program, reasons for some choices made during its creation, and possible future improvements.
**********
2.1. An overview of the project requirements and program.
2.2. Maze Search Algorithm choice information
2.3. Structure of the program, including data storage choices.
2.4. Information about hardcoded elements.

2. PROGRAM & PROJECT INFORMATION

2.1 OVERVIEW OF REQUIREMENTS & PROGRAM
This is a java program that uses bitwise operators to determine the characteristics of each cell of a maze. The goal is to create the maze, based on information given in a file.  The characteristics of each cell can be decoded using bitwise operators against each element of data in the file.

The fastest path through the maze created by these bitwise operators has to take the possibility of mines into consideration. The maximum allowed mines per path is 2.  Once a third mine was hit, that path is dead, and can't be considered viable.

2.2 MAZE SEARCH ALGORITHM
I chose a breadth first search to find the fastest path. I chose this, rather than a depth first search, because only the fastest path was needed, not all paths.  Because it processes each level before moving onto the next level, it's guaranteed to find the fastest path.

2.3 PROGRAM STRUCTURE
2.3.2. PROCESSING QUEUE
It uses a PriorityQueue to cycle through the elements in the maze matrix.  It uses a HashMap of the cell address and the cell information for tracking which cells have been visted.  

The program doesn't use the PriorityQueue's ability to change how to determine who's next in line, because the default behavior of the Queue is fine - FIFO - First In First Out.

2.3.2 MAZE NODES
All of the information about a cell is tracked in an object called a Maze Node.  This includes the direction traveled to arrive in it, the traveler's location, and where it can go next.  The node also knows if it's is a start or an end, and if it contains a mine.

2.3.3 LINKED LIST OF MAZE NODES
These Maze Nodes are stored in a Linked List
It also has a LinkedList of each cell's information. This list is where the queue gets the cell's details from.  As each element goes through the queue, from the starting point until the end is found, the relevant Maze Nodes are updated with the path traveled to reach them, and at what level they were found.  A HashMap is also created to track when a cell has been visited.  Before any updating is done on a MazeNode, the visit status of the maze node is checked, along with how many mines were found on the current path. 

NOTE: The program does NOT currently check to see if a cell was visited as part of what turned out to be a dead end path. In that case, the program should backtrack to unmark the cells as visited, if they were visited as part of a bad path.  This may not be an issue because of the way the algorithm was written, but it is something to test with external data to verify.

2.3.4 WHY A LINKEDLIST INSTEAD OF A HASH MAP FOR STORING MAZE NODES
It would seem that a HashMap is a natural choice. It features easy retrieval based on the key. The natural key would have been the cell's address.

However, a LinkedList storage structure was chosen over a HashMap.  Although the HashMap is faster for searching, because it would have been based on the cell address for quick retrieval, the effort to update the HashMap was more difficult than it was for the LinkedList. 

In the case of this program, as the path is traveled, the information about each cell is updated in its Maze Node.  Each time that object is updated in the HashMap, it would have invalidated the hashcode used to initially store that object.  So intead of a simple put, a coder would have to remember to get the information, change the object, and then purposefully put it back in the map.  This would have been necessary in order to relocate the information on a later search.

One possible improvement would be to create an update procedure that included those tasks each time someone wanted to change the HashMap, however another coder would always have to remember to use this non-standard update method.

Another possible improvement to negate the overhead of searching through an entire LinkedList to find a desired element would be to create a HashMap that stored the MazeNode's address as its key, and as the value of that key, store the index of the LinkedList to which it was added.  Then the search, instead of having to go through the whole linked list, could quickly retrieve the index from the HashMap, and pull the corresponding record from the LinkedList.

This could be embedded in the search functions of the program, and would mean two steps for getting a MazeNode, which is better than the iterative LinkedList loop, and avoids the hashcode corruption caused by changing the MazeNode.

2.3.5 WHY A LINKEDLIST INSTEAD OF AN ARRAYLIST
A LinkedList was chosen instead of an ArrayList because when an item is removed from an ArrayList, it readjusts all the indexes of the array.  If the HashMap Reference object is put in place, the HashMap of MazeNode addresses would now be invalid.  While the program doesn't currently need to remove anything from the LinkedList, it's a subtle issue that would be hard to track down if the requirement to remove nodes from the list was added later.

2.4. HARDCODING
2.4.1 NUMBER OF MINES
I hardcoded the number of mines allowed into a class called GlobalConstants, however another feature might be to allow this to be parsed from the mazes.txt file, or be read from a command line or interface.

2.4.2 MAZE INPUT FILE
The mazes.txt file is included in the package, so the person running it doesn't need to make sure it's in the correct directory path.  This, however, prevents the user from submitting their own mazes file.  An enhancement could be to ask for an input file from the command line, or to ask a user to upload it from a user interface.

3. ADDITIONAL DOCUMENTATION

The JavaDocs for this program can be reached by:
