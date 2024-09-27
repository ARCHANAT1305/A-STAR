<h1>ExpNo 4 : Implement A* search algorithm for a Graph</h1> 
<h3>Name: ARCHANA T       </h3>
<h3>Register Number: 212223240013          </h3>
<H3>Aim:</H3>
<p>To ImplementA * Search algorithm for a Graph using Python 3.</p>
<H3>Algorithm:</H3>

``````
// A* Search Algorithm
1.  Initialize the open list
2.  Initialize the closed list
    put the starting node on the open 
    list (you can leave its f at zero)

3.  while the open list is not empty
    a) find the node with the least f on 
       the open list, call it "q"

    b) pop q off the open list
  
    c) generate q's 8 successors and set their 
       parents to q
   
    d) for each successor
        i) if successor is the goal, stop search
        
        ii) else, compute both g and h for successor
          successor.g = q.g + distance between 
                              successor and q
          successor.h = distance from goal to 
          successor (This can be done using many 
          ways, we will discuss three heuristics- 
          Manhattan, Diagonal and Euclidean 
          Heuristics)
          
          successor.f = successor.g + successor.h

        iii) if a node with the same position as 
            successor is in the OPEN list which has a 
           lower f than successor, skip this successor

        iV) if a node with the same position as 
            successor  is in the CLOSED list which has
            a lower f than successor, skip this successor
            otherwise, add  the node to the open list
     end (for loop)
  
    e) push q on the closed list
    end (while loop)

``````



<h3>PROGRAM:</h3>

```    

 import heapq  # For the priority queue

class Node:
    def __init__(self, position, parent=None):
        self.position = position  # (x, y) tuple
        self.parent = parent
        self.g = 0  # Cost from start to node
        self.h = 0  # Heuristic cost to goal
        self.f = 0  # Total cost

    def __lt__(self, other):
        return self.f < other.f

def manhattan_heuristic(node, goal):
    return abs(node.position[0] - goal.position[0]) + abs(node.position[1] - goal.position[1])

def euclidean_heuristic(node, goal):
    return ((node.position[0] - goal.position[0]) ** 2 + (node.position[1] - goal.position[1]) ** 2) ** 0.5

def diagonal_heuristic(node, goal):
    dx = abs(node.position[0] - goal.position[0])
    dy = abs(node.position[1] - goal.position[1])
    return max(dx, dy)

def a_star_search(start, goal, grid):
    open_list = []
    closed_list = []

    heapq.heappush(open_list, start)

    while open_list:
        # Step a: find the node with the least f
        current_node = heapq.heappop(open_list)
        
        # Step b: pop q off the open list
        closed_list.append(current_node)

        # Step c: generate successors
        for new_position in [(0, -1), (0, 1), (-1, 0), (1, 0)]:  # Adjacent squares (up, down, left, right)
            node_position = (current_node.position[0] + new_position[0], current_node.position[1] + new_position[1])

            # Check if within grid bounds
            if node_position[0] > (len(grid) - 1) or node_position[0] < 0 or node_position[1] > (len(grid[len(grid)-1]) - 1) or node_position[1] < 0:
                continue
            
            # Check if the node is walkable
            if grid[node_position[0]][node_position[1]] != 0:
                continue

            # Create new node
            successor = Node(node_position, current_node)

            # Step i: if successor is the goal, stop search
            if successor.position == goal.position:
                path = []
                while successor:
                    path.append(successor.position)
                    successor = successor.parent
                return path[::-1]  # Return reversed path

            # Step ii: compute g, h, and f
            successor.g = current_node.g + 1  # Cost is assumed to be 1 for this example
            successor.h = manhattan_heuristic(successor, goal)  # Change to desired heuristic
            successor.f = successor.g + successor.h

            # Step iii: if a node with the same position is in the open list with a lower f, skip this successor
            if any(open_node.position == successor.position and open_node.f <= successor.f for open_node in open_list):
                continue

            # Step iv: if a node with the same position is in the closed list with a lower f, skip this successor
            if any(closed_node.position == successor.position and closed_node.f <= successor.f for closed_node in closed_list):
                continue

            # Add successor to open list
            heapq.heappush(open_list, successor)

    return []  # Return empty path if no path is found

# Example usage:
if __name__ == "__main__":
    grid = [
        [0, 0, 0, 0, 0],
        [0, 1, 1, 1, 0],
        [0, 0, 0, 0, 0],
        [0, 1, 1, 1, 0],
        [0, 0, 0, 0, 0],
    ]

    start_node = Node((0, 0))
    goal_node = Node((4, 4))

    path = a_star_search(start_node, goal_node, grid)
    print("Path found:", path)



```

            
<hr>
<h2>Sample Graph I</h2>
<hr>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/b1377c3f-011a-4c0f-a843-516842ae056a)

<hr>
<h2>Sample Input</h2>
<hr>
10 14 <br>
A B 6 <br>
A F 3 <br>
B D 2 <br>
B C 3 <br>
C D 1 <br>
C E 5 <br>
D E 8 <br>
E I 5 <br>
E J 5 <br>
F G 1 <br>
G I 3 <br>
I J 3 <br>
F H 7 <br>
I H 2 <br>
A 10 <br>
B 8 <br>
C 5 <br>
D 7 <br>
E 3 <br>
F 6 <br>
G 5 <br>
H 3 <br>
I 1 <br>
J 0 <br>
<hr>
<h2>Sample Output</h2>
<hr>
Path found: ['A', 'F', 'G', 'I', 'J']


<hr>
<h2>Sample Graph II</h2>
<hr>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/acbb09cb-ed39-48e5-a59b-2f8d61b978a3)


<hr>
<h2>Sample Input</h2>
<hr>
6 6 <br>
A B 2 <br>
B C 1 <br>
A E 3 <br>
B G 9 <br>
E D 6 <br>
D G 1 <br>
A 11 <br>
B 6 <br>
C 99 <br>
E 7 <br>
D 1 <br>
G 0 <br>
<hr>
<h2>Sample Output</h2>
<hr>
Path found: ['A', 'E', 'D', 'G']
