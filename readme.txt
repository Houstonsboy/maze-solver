HERE IS A DETAILED EXPLANATION OF THE IMPLEMENTATION OF A MAZE 
USING DEPTH FIRST SEARCH AND BREADTH FIRST SEARCH

the class node has 3 parameter features, the state which represents the position in our maze, action is the movement and parent is the previous position we were at
class Node():
    def __init__(self, state, parent, action):
        self.state=state
        self.parent=parent
        self.action=action
  
this is a class function with a list of frontier(already explored nodes), it returns the most recently accesed node        
class StackFrontier():
    def __init__(self):
        self.frontier=[]
        
    def add(self, node):
        self.frontier.append(node)
        
    def contains_state(self, state):
        return any(node.state== state for node in self.frontier)
    
    def empty(self):
        return len(self.frontier)==0
    
    def remove(self):
        if self.empty():
            raise Exception("empty frontier")
        else:
            node= self.frontier[-1]
            self.frontier= self.frontier[:-1]
            return node

the queuefrontier inherits stackfrontier properties but it returns the first node of the frontier list rather than last
class QueueFrontier(StackFrontier):
    def remove(self):
        if self.empty():
            raise Exception("Empty frontier")
        else:
            node=self.frontier[0]
            self.frontier=self.frontier[1:]
            return node
 this is a the maze which inputs a file with the maze, it converts the file to a 2D array with width and height and it iterates through it replacing walls with True and spaces with False, it also keeps track of the self.start(A) and self.goal(B) 
class Maze():
    
    def __init__(self, filename):
        #read file and set height and width of maze
        with open(filename) as f:
            contents=f.read()      
            
        # Validate start and goal"
        if contents.count("A")!=1:
            raise Exception("maze must have exactly one start point")    
        if contents.count("B")!=1:
            raise Exception("maze mus have exactly one goal")
        
        # get the height and width of the maze
        contents=contents.splitlines()
        self.height= len(contents)
        self.width= max(len(line)for line in contents)

        #Keep track of walls
        self.walls=[]
        for i in range(self.height):
            row=[]
            for j in range(self.width):
                try:
                    if contents[i][j]=="A":
                        self.start=(i,j)
                        row.append(False)
                    elif contents[i][j]=="B":
                        self.goal=(i,j)
                        row.append(False)
                    elif contents [i][j]==" ":
                        row.append(False)
                    else:
                        row.append(True)
                except IndexError:
                    row.append(False)
            self.walls.append(row)
        self.solution=None 
          
   this iterates through the walls and prints the values on the screen but it equates the walls to spaces and the free space as * . the solution initially looks like this
      (['right', 'right', 'down', 'down'], [(0, 0), (0, 1), (0, 2), (1, 2), (2, 2), (3, 2)])
    storing the action and the coordinates
def print(self):
        solution = self.solution[1] if self.solution is not None else None
        print() 
        for i, row in enumerate(self.walls):
            for j, col in enumerate(row):
                if col:
                    print(" ", end ="")
                elif(i,j)==self.start:
                    print("A", end="")
                elif(i, j)== self.goal:
                    print("B", end="")
                elif solution is not None and (i,j) in solution:
                    print("*", end="")
                else:
                    print (" ", end="")
            print()
        print()         