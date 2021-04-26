# Solving problems by Searching

## Uninformed Search

### Breadth First Search

```python
def BreadthFirstSearch (NodeList, Goal) :
    NewNodes = set()
    for Node in NodeList:
        if GoalReached(Node, Goal) :
            return ("Solution Found", Node)
        NewNodes.add(Successors(Node))
    
    if len(NewNodes) != 0:
        return BreadthFirstSearch(NewNodes, Goal)
    else:
        return("No Solution")
```

Time and Memory complexity = $O(b^d)$ for a tree with a constant branching factor b and depth d.

### Depth First Search

```python
def DepthFirstSearch (Node, Goal) :
    if GoalReached(Node, Goal) :
        return ("Solution Found")
    NewNodes = Successors(Node)
    
    for Node in NewNodes:
        Result = DepthFirstSearch(Node, Goal)
        if Result = "Solution Found":
             return "Solution Found"
    
    Return("No Solution")
```

The search is incomplete in DFS.

### Iterative Deepening

```python
def DepthFirstSearch_B (Node, Goal, Depth, Limit) :
    if GoalReached(Node, Goal) :
        return ("Solution Found")
    NewNodes = Successors(Node)
    
    for Node in NewNodes and Depth < Limit:
        Result = DepthFirstSearch(Node, Goal, Depth+1, Limit)
        if Result = "Solution Found":
             return "Solution Found"
    
    Return("No Solution")

def IterativeDeepening(Node, Goal) :
    DepthLimit = 0
    Result = "No Solution"

    while Result != "Solution Found" :
        Result = DepthFirstSearch_B(Node, Goal, 0, DepthLimit)
        DepthLimit = DepthLimit + 1
```

## Heuristic Search

```python
def HeuristicSearch(Start, Goal) :
    NodeList = [Start]
    while True:
        if len(NodeList) == 0:
            return "No Solution"
        Node = NodeList.pop(0)
        
        if GoalReached(Node, Goal):
            return "Solution Found"
        
        NodeList = SortedInsert(Successors(Node), NodeList)
```

Here SortedInsert inserts Successor Nodes in the sorted list NodeList according to the value return a Heuristic Evaluation Function.

### Greedy Search

Here, the evaluation function $f(s) = h(s)$, where $h(s)$ is the heuristic value of a node in state s.

### $A^*$ Search

In this technique, we also conside the costs accrued during the search up to current node s. We define,

$g(s)$ = Sum of accrued costs from the start to the current node. 

Evaluation function $f(s) = g(s) + h(s)$

The heuristic cost estimate function $h(s)$ should never overestimate the actual cost from state s to goal.

$A^*$ is complete and optimal. It always finds the shortest solution for every solvable search problem.

### $IDA^*$ Search

Here, we use an upper limit for the heuristic evaluation function $f(s)$.

```python
def ida_star(root) :
    bound = h(root)
    path = [root]
    while True:
        t = search(path, 0, bound)
        if t == FOUND:
            return (path, bound)
        elif t == inf :
            return NOT_FOUND
        else:
            bound = t

def search(path, g, bound) :
    node = path.last
    f = g + h(node)

    if f > bound :
        return f
    elif is_goal(node):
        return FOUND
    
    tmpmin = inf

    for successor in successors(node):
        if successor not in path:
            path.push(successor)
            t = search(path, g + cost(node, successor), bound)
            if t == FOUND:
                return FOUND
            elif  t < tmpmin :
                tmpmin = t

            path.pop()

    return tmpmin
```

# Advanced Intelligent Search Techniques

## Hill Climbing

```python
def hill_climbing(init_state):
    current_state = init_state
    while true:
        neighbor_state = max_valued(successors(current_state))

        if neighbor_state <= current_state :
            return current_state
        
        current_state = neighbor_state
```

## Simulated Annealing

Source: http://www.cse.iitm.ac.in/~vplab/courses/optimization/SA_SEL_SLIDES.pdf

```python
y = random_solution()
e_old = cost(y)

temp = temp_max
while temp >= temp_min :
    for i in range(0, i_max):
        # successor function should be randomised
        successor_function(y)
        e_new = cost(y)
        delta = e_new - e_old
        if delta > 0 :
            # random returns value between 0 and 1
            if random() >= exp(-delta / K * temp) :
                undo_function(y) 
            else :
                # Accept the bad move
                e_old = e_new
        else:
            # Always accept good moves
            e_old = e_new
    temp = next_temp(temp) 
```

## Tabu Search

```python
def tabu_search(init_state):
    sBest = init_state
    bestCandidate = init_state
    tabuList = [init_state]

    while stop() == False :
        sNeighborhood = getNeighbors(bestCandidate)
        bestCandidate = sNeighborhood[0]
        for sCandidate in sNeighborhood :
            if ! tabuList.contains(sCandidate) and fitness(sCandidate) > fitness(bestCandidate) :
                bestCandidate = sCandidate

        if fitness(bestCandidate) > fitness(sBest) :
            sBest = sCandidate
        
        tabuList.push(bestCandidate)
        if len(tabuList) > maxTabuSize :
            tabuList.removeFirst()
        
    return sBest
```

## Genetic Algorithm

```python
def genetic_algorithm(population, fitness_fn) :
    heapify(population, fitness_fn)
    while fitness(population[0]) < desired_val :
        new_population = []
        for i in range(0, len(population)) :
            x = random_selection_based_on_fitness(population)
            y = random_selection_based_on_fitness(population)
            child = reproduce(x, y)
            # random returns values between 0 and 1
            if random() < some_small_value :
                mutate(child)
            
            new_population.append(child)

        heapify(new_population, fitness_fn)
        population = new_population

def reproduce(x, y) :
    n = len(x)
    c = random(0, n)
    return x[0:c] + y [c:n]
```


