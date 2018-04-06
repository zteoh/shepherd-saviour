# :sheep: Shepherd // Savior :sheep:
> tdlr: A strategy game using breadth first search algorithm, markov chain and file i/o

Check out the game demo [here](https://youtu.be/B1rUa_udPgs)

## :rocket: Introduction

**Objective:** 
As a shepherd, the player is to ensure all sheeps are in the safe zone before the wolf reaches the sheep

**Board:** 
1. Safe Zone 
2. Hedges 
3. Portals

**Features:**
1. Ability to create and play on your own board
2. Two different difficulty levels
3. Ability for multiplayer
4. High scores are saved

## :pencil2: Grid Organisation

**What is Grid Orgaisation?**
Instead of moving a certain number of pixels in a certain direction, characters move in rows and cols. This means that each 'step' taken would be equal distance for each character.

**Problems:**
It is less appealing as it makes everything seem very rigid. 
*However*, it aids in the function of the program because:
1. I will be able to keep track of the value of each cell, making DIY-ing a board possible
2. Being able to control the speed of each character better

## :memo: Markov's Chain

**What is Markov's Chain?**
It is a stochastic model describing a *sequence of possible events* in which the probability of each event depends only on the state attained in the previous event.

**What am I using Markov's Chain for?**
Mainly the movements of the sheeps.

**Explaination**

The probability table I came out is shown below:

```
[	[0.2, 0.2, 0.2, 0.2, 0.2],
	[0.2, 0.3, 0.1, 0.2, 0.2],
	[0.2, 0.1, 0,3, 0.2, 0.2],
	[0.2, 0.2, 0.2, 0.3, 0,1],
	[0.2, 0.2, 0.2, 0.1, 0,3],	]
```

The row and column, which represents the next direction and the initial direction respectively, follows this list:
```
[(0,0), (0,1), (0,-1), (1,0), (-1,0)]
```

Which can be translated as:
```
[‘None’, ‘Right’, ‘Left’, ‘Up’, ‘Down’]
```

In general,
- P(moving in the same direction) is highest
- P(moving in the opporite direction) is lowest

Edge case: if the animal was stationary
- *Problems:* There is no opposite direction and sheeps might be stationary for a longer period
- *Solution:* Allow P(every direction) = 0.20

**Implementation**
I converted the probability 2D list into one that stores the next direction taken, given the initial direction as the row index and the probability as the column index, where a 0.09 probability will land on column 0 and a 0.91 probability will land on column 9

## :book: Simple Algorithm

I decided to make the wolf smarter than the sheep, by having it go to the closest sheep

**How?**
- Keep track of all the values
  1. Distance from the wolf to all the sheep using Manhattan Distance
  2. The number of sheeps on the board
  3. The minimum distance from the wolf to any sheep
- Have the wolf move towards the row of the nearest sheep, then move towards the col of the nearest sheep

- *Initial Problem:* The wolf might get stuck sometimes
- *Solution:* Constantly move towards the row of the nearest sheep unless obstructed, then move towards the column of the sheep. Repeat these steps until the wolf reaches the sheep.

- *More problem:* Depending on the obstacles created, the wolf might get stuck. Also, manhattan distance finds the displacement between the wolf and the sheep, which is inaccurate to describe the distance
- *Final Solution:* Creating a smarter algorithm!

## :mortar_board: Breadth First Search

**Why BFS?**
*Other Searching algorithm and why they are not the best fit*
1. Backtracking
    - Inefficient
    - Sheep would be constantly moving and this algorithm would have to constantly check the board everytime the sheeps move
2. A*/ Dijkstra's Algorithm
    - Excessive and unneccessary
    - Wolves are not allowed to move diagonally, hence, nodes are equallt weighted

**How the Algorithm works**
1. Keep track of cells visited in an array and cells to visit in a queue
2. Check the next cell I need to visit
3. Check what are the neighbours/ child of the checked cell and store it in the list
4. Add the pair of neighbouring cells into a dictionary, a child to parent pair
5. Keep checking cells until we reach a sheep
6. Backtrack the dictionary to find how to get from the sheep to the wolf

## :vertical_traffic_light: TODO

- [ ] Improve the UI
- [ ] Work with a non- grid system
