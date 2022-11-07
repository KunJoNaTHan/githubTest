# githubTest
Research for pac man game:

Required libraries: sfml


Logic and AI of pacman game:
pacman's only defense = "energizer" pellets located at the corners of the maze, which makes the ghosts retreat for a moment
Also, pacman can also eat the ghosts but the ghosts aren't completely eliminated such that they are returned to their starting position

Source of points: 1. the dots to eat (by the pacman)  2. fruits appear in the maze, which appear twice, in the middle (after 70 dots eaten) and end of the game (after 170 dots eaten) respectively


Start of the game: the Ghost House, both pacman and the left ghosts can't enter

The game board can be splitted into tiles of which a "tile" is a 8*8 pixel square (28*36 tiles maybe? screen resolution = 224*288)

every pellet in the maze is in the center of its own tile.

the mechanism of pacman being caught by the ghosts: they occupy the same tile

since the symbol or picture of Pac-Man and the ghosts are larger than one tile in size, they are not contained in a single tile. 
-> !!!!! the character is considered to occupy whichever tile contains its center point. !!!!! This is important to avoid ghosts, since Pac-Man will only be caught if a ghost manages to move its center point into the same tile as Pac-Man's.

4 Ghosts have different behaviours (do we need to do so?)
each of them has a specific tile it's trying to reach and its behaviour affect the way it reaches the tile (but they have identical methods travel downwards)

Ghost movement modes: CHASE (normal mode), SCATTER, FRIGHTENEDS

CHASE: use pacman's current position as factor in the selection of the target tile 

SCATTER: each ghost has fixed target tile, located at the diffrent corners of the board -> so the four ghosts will disperse to th corners under such mode

FRIGHTENED: no target tile, so the ghosts pseudorandomly decide which turns to make at every intersection. Also, they move much slower and can be eaten by the pacman

The mode will change periodically, for instance:
Scatter for 7 seconds, then Chase for 20 seconds.
Scatter for 7 seconds, then Chase for 20 seconds.
Scatter for 5 seconds, then Chase for 20 seconds.
Scatter for 5 seconds, then switch to Chase mode permanently.

The above is just an example, the seconds can be adjusted based on the difficulty of the game

How will the ghost move? (The AI logic of the movement of ghosts)

It only makes one decision on each tile, which is thinking about the move of the next tile
Restriction: the ghost cannot move in opposite direciton (reverse direction)
Exception: when the mode changed from CHASE or SCATTER to other mode, they should reverse direction as they enter the next tile -> overwrite the decision made previously (alsi notifies the player that the ghosts have changed modes) (FRIGHTENED mode will not change  direction)


INTERSECTION TILES: decisions are made when approachinig it
Which direction to turn? 
the tile adjoining the intersection tile which will put the ghost nearest to its target tile (by measuring the straight line distance)
Distance from every possibility to the target tile will be measured -> select the closest one
If there are multiple potential choices in equal distance from the target, the decision-making is in the order of: up > left > down
However, there could be mistakes: such that the ghost takes wrong direction (a shorter straight line distance to the target tile but longer path due to the walls)
