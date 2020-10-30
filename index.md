## ACM AI 2020 (Energium) Python Docs

You may have noticed that people can submit code in Python, Javascript, or Java for this competition. In 2018 Battlecode achieved this by transpiling Python and Java code to Javascript, but the transpiling wasn't perfect and also slowed down the code.

This competition instead achieves this by doing something similar to what CodinGame does. It takes control of the IOStream and prints out the board every round. Then, using whichever language you prefer, you can read what was printed out and get a list of where units and resources are. Of course, this can be cumbersome to do, but fortunately the organizers included a "kit" that does the processing all for you. Unfortunately, there was no documentation for this kit, so I have made some here.

### Installation
Download the kit here:
https://github.com/acmucsd/energium-ai-2020/tree/main/kits/python

- [Agent][1]
- [GAME_CONSTANTS][2]
- [Unit][3]
- [Player][4]
- [GameMap][5]
- [Tile][6]
- [Position][7]

### Agent
#### `agent.id`

0 for team red, 1 for team blue.

`turn`

Current match turn. Equal to [unit.match_turn][35]

[1]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#Agent
[2]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#GAME_CONSTANTS
[3]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#Unit
[4]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#Player
[5]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#GameMap
[6]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#Tile
[7]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#Position

[35]: https://github.com/programjames/acmai2020docs/blob/gh-pages/index.md#agent.id
