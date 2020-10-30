## ACM AI 2020 (Energium) Python Docs

You may have noticed that people can submit code in Python, Javascript, or Java for this competition. In 2018 Battlecode achieved this by transpiling Python and Java code to Javascript, but the transpiling wasn't perfect and also slowed down the code.

This competition instead achieves this by doing something similar to what CodinGame does. It takes control of the IOStream and prints out the board every round. Then, using whichever language you prefer, you can read what was printed out and get a list of where units and resources are. Of course, this can be cumbersome to do, but fortunately the organizers included a "kit" that does the processing all for you. Unfortunately, there was no documentation for this kit, so I have made some here.

### Installation
Download the kit here:
https://github.com/acmucsd/energium-ai-2020/tree/main/kits/python

### Objects

- [Agent][1]
- [GAME_CONSTANTS][2]
- [Base][3]
- [Unit][4]
- [Player][5]
- [GameMap][6]
- [Tile][7]
- [Position][8]

### Agent

**`id`** - 0 for team red, 1 for team blue.

**`turn`** - Current match turn. Equal to [unit.match_turn][3]

**`mapWidth`** - Width of the map.

**`mapHeight`** - Height of the map.

**`map`** - Current match turn. Equal to [unit.match_turn][3]

**`players`** - List containing the two [Player][4] objects, indexed by their id (red first, then blue).

### GAME_CONSTANTS

This is dictionary containing several subdictionaries. The correct way to access a constant would be `GAME_CONSTANTS["<Group>"]["<Property>"]`. For example, to access the `UNIT_COST` you would write `GAME_CONSTANTS["PARAMETERS"]["UNIT_COST"]`.

#### `DIRECTIONS`

**`NORTH`** - `"n"` (moving north 1 unit is equivalent to a translation by the vector `[0, -1]`)

**`EAST`** - `"e"` (moving east 1 unit is equivalent to a translation by the vector `[1, 0]`)

**`SOUTH`** - `"s"` (moving south 1 unit is equivalent to a translation by the vector `[0, 1]`)

**`WEST`** - `"w"` (moving  1 west is equivalent to a translation by the vector `[-1, 0]`)

#### `Parameters`

**`UNIT_COST`** - Cost to build one collector.

**`MAX_TURNS`** - How many turns are in a match.

**`INITIAL_POINTS`** - Amount of energium you start with.

**`BREAKDOWN_TURNS`** - How many turns it takes for a unit to "breakdown" by 1.

**`BREAKDOWN_MAX`** - If a unit reaches this level of breakdown it dies.

### Base

**`team`** - 0 for team red, 1 for team blue.

**`pos`** - A [Position][8] object representing its location.

**`spawn_unit()`** - Returns a string to spawn a collector on the base. You should wait to print out the string until the end of your turn, where you should print out all your commands on one line. Look at the starter kit's `bot.py` file to see how they do it.

### Unit

**`team`** - 0 for team red, 1 for team blue.

**`id`** - A unique integer that identifies this unit. Currently the ids are set up so that the first collector to be created will get an id of 0, the next will get an id of 1, and it increases by 1 every time a new collector is spawned.

**`last_repair_turn`** - The last turn this collector was repaired. I.e. its breakdown level was equal to 0 on this turn.

**`match_turn`** - The current round in the match. It is equal to [agent.turn][1].

**`get_breakdown_level()`** - Returns the breakdown level of this unit. If it gets above [GAME_CONSTANTS["PARAMETERS"]["BREAKDOWN_MAX"]][2] the unit will die.

**`spawn_unit(direction)`** - Takes a single argument, [direction][2] (one of `n`, `e`, `s`, and `w`), and returns a string to move the unit in that direction. You should wait to print out the string until the end of your turn, where you should print out all your commands on one line. Look at the starter kit's `bot.py` file to see how they do it.

#### Player

**`team`** - 0 for team red, 1 for team blue.

**`energium`** - The amount of energium this player has.

**`units`** - A list of [units][4] this player controls.

**`bases`** - A list of [bases][3] this player controls.

#### GameMap

**`width`** - Width of the map.

**`height`** - Height of the map.

**`bases`** - List of [bases][3] from both teams on the map.

**`players`** - List containing the two [Player][4] objects, indexed by their id (red first, then blue).

**`get_tile_by_pos(position)`** - Takes one argument, position (a [Position][8] object), and returns the [Tile][7] object at that position.

**`get_tile(x, y)`** - Returns the [Tile][7] object at the location `(x, y)`.

#### Tile

**`base_team`** - If it is a base, it is equal to its team's id (0 for red, 1 for blue). If it is not a base, it is equal to `None`.

**`energium`** - The amount of energium (resources) at the tile. Energium values never change for a given tile. Also, make sure to note that the energium could be negative on a tile, in which case you would lose energium for having a unit on the tile. All base tiles should have 0 energium on them.

**`pos`** - A [Position][8] object representing the tile's location.

**`is_base()`** - Returns `True` if this tile has a base on it, otherwise it returns `False`.

#### Position

**`x`** - The x-coordinate.

**`y`** - The y-coordinate.

**`equals(other_position)`** - Checks if this position is equal to another position.

**`is_adjacent(other_position)`** - Checks if this position is adjacent to another position.

**`translate(direction, k)`** - Translates this position `k` units in the given direction.

**`distance_to(other_position)`** - DO NOT USE! It gives the Euclidean distance (distance as the crow flies) from this position to another position. However, units can only move in the four cardinal directions so this "distance" isn't a true reflection of how long it will actually take to get there. Plus, it has a nasty square root in there which makes it much much slower. I would recommend implementing your own distance function as follows:

```python
def taxicab_distance(position1, position2):
    return abs(position1.x - position2.x) + abs(position1.y - position2.y)
```

This finds the taxicab distance between the two points, basically how far away they are if you can't move diagonally but only along the cardinal directions.

**`direction_to(other_position)`** - This finds the direction that moves closest to the target position. I would recommend not using this function, as it relies on the above `distance_to` function which is quite slow. Instead, I would implement your own as follows:

```python
def direction_to(pos, target):
    closest_direction = None
    taxicab_dist = taxicab_distance(pos, target)
    bigger_side_length = max(abs(pos.x - target.x), abs(pos.y - target.y))
    for dir in ALL_DIRECTIONS: # ALL_DIRECTIONS is defined at the top of bot.py
        newpos = pos.translate(dir, 1)
        dist = taxicab_distance(newpos, target)
        if dist < taxicab_dist:
            taxicab_dist = dist
            bigger_side_length = max(abs(newpos.x - target.x), abs(newpos.y - target.y))
            closest_direction = dir
        elif dist == taxicab_dist:
            s = max(abs(newpos.x - target.x), abs(newpos.y - target.y))
            if s < bigger_side_length:
                bigger_side_length = s
                closest_direction = dir
    return closest_direction
```

[1]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#Agent
[2]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#GAME_CONSTANTS
[3]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#Base
[4]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#Unit
[5]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#Player
[6]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#GameMap
[7]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#Tile
[8]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#Position
