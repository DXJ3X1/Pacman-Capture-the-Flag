# Pacman Capture the Flag AI Design
## Introduction
![Map](http://ai.berkeley.edu/projects/release/contest/v1/002/capture_the_flag.png)
The game is a multi-player capture-the-flag variant of Pacman, where AI agents control both Pacman and ghosts in coordinated team-based strategies. Our team will try to eat the food on the far side of the map, while defending the food on our home side. For more info, click [here](http://ai.berkeley.edu/contest.html)

Our team (Level-256-UniMel18) won Finalist Award at [Year 2018 Sem 2 Pacman Capture the Flag Inter University Contest](https://sites.google.com/view/pacman-capture-hall-fame/2018)
## AI Design
#### Techniques used
1. Heuristic Search
2. Monte Carlo Tree Search
#### Our team of two agents include
1. one offensive agent
2. one defensive agent 
## Heuristic Search
#### Heuristic Search is the core technique for our solution
Heuristic Search is an AI technique that use heuristic function to generate information based on the current state and the ranking for each actions of a given state
## Offensive Agent
### Feature-Weight Model
Our team leverages multiple heuristic functions to generate output as feature, and calculate overall ranking for the each actions
### Features
* isPacman: whether the agent has become Pacman
* ghostDistance: Distance to nearest ghost
* distanceToAvailableFood : Distance to nearest food, with no ghost on the way
* distanceToHome: Distance to the nearest point within our camp
* distanceToStart: Distance to starting point
### Two Modes
#### Eating mode
* Eating food as much as possible
* Trying to eat the capsule and become more aggressive while opponent’s ghosts are scared 
#### Going-home mode
* Pacman has eaten a given amount of food and starts to go home in order to make sure the scores are firmly obtained
* The game is coming to the end (steps left are less than a given amount). Pacman starts to go home in order to boost the final score 
### Decision Tree
![DecisionTree1](https://raw.githubusercontent.com/DXJ3X1/Pacman-Capture-the-Flag/master/img/decisionTree.png)
## Defensive Agent
### Three Modes
#### Patrol mode
* If the agent detects some of our food are eaten, the agent will go to the position of eaten food to search for enemy
#### Chasing mode
* If the agent detects the incoming enemy, the agent will hunt the invading enemy
#### Offensive mode
* If no enemy in sight and no food is being eaten, eat the food at the other side
### Decision Tree
![DecisionTree2](https://raw.githubusercontent.com/DXJ3X1/Pacman-Capture-the-Flag/master/img/decisionTreeD.png)
## Monte Carlo Tree Search
#### Leverage the simulation part of Monte Carlo Tree
Monte Carlo Tree Search allows us to simulate the future game states after taking a series of actions under a certain policy
#### This serves as our solution to “Food in Block Area” situation
When our Pacman's nearest food is inside a blocked area that has only one entry, we use simulation to see how many steps it would take for our Pacman to enter the blocked area, eat a certain amount of food and leave. Then the Pacman can decide how many food it would eat in the blocked area, without being blocked or eaten by ghosts. We simulated maximum 20 steps since a blocked area that takes more than 20 steps to walk around is dangerous enough for the Pacman to decide not to enter
## Challenges
* The provided function getmazedistance(pos1, pos2) only returns the length of the shortest path from pos1 to pos2, ignoring all other paths. However, in order to avoid enemies, our Pacman needs to take a longer path to get to its target. The goAround() function is designed to solve the problem by conducting a BFS from the current position to find a middle point whose shortest path to the target is clear
![goAround](https://raw.githubusercontent.com/DXJ3X1/Pacman-Capture-the-Flag/master/img/goAround.png)
* Our defensive agent used to waste time chasing the opponent’s defensive agent that never eats food. To solve the problem, we keep a record of indexes of the opponent’s agents who has ever carried food. So that our defensive agent would only spend time defending those agents that would offend
## Further Improvements
1. Defensive agent can perform better if it could predict the food the invading Pacman is going after and try to get in the way
2. Eat capsules and opponents more wisely to maximize scare time
3. Offensive agent’s path can be planned better to eat as many as food per step instead of always eating the closest food first
4. After eating an opponent, its distance can be estimated using the knowledge of the opponents’ start point
5. Adding the concept of time -> So the pacman knows how to choose more wisely when time is almost up
6. Apply game theory to auto-switch between offender and defender.
## Demo
#### Blocked Area (Our AI Agent: Blue Pacman)
![Demo1](https://raw.githubusercontent.com/DXJ3X1/Pacman-Capture-the-Flag/master/gif/eat.gif)
#### Bypassing Ghost (Our AI Agent: Blue Pacman)
![Demo2](https://raw.githubusercontent.com/DXJ3X1/Pacman-Capture-the-Flag/master/gif/around.gif)
#### Trapping Enemy (Our AI Agent: Red Ghost)
![Demo3](https://raw.githubusercontent.com/DXJ3X1/Pacman-Capture-the-Flag/master/gif/trap.gif)
