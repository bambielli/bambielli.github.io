---
title:  "Weekend Hacks: Game of Life"
date:   2017-07-16 21:30:00
category: post
tags: [weekend-hacks, javascript]
---

No, this is not the board game we all played as kids...

[Connway's Game of Life][gol]{:target="_blank"} is a simulation often covered in biology and math courses. It was created by mathematician John Conway in 1970 in response to a challenge posed by the great John Von Neuman.

The game is played on a 2d grid of cells (ideally infinite), and begins with the player selecting an initial seed of "living" cells from which all future generations are calculated. A simple set of rules dictate whether a cell lives or dies in the next generation (see Rules section below).

To see a **deployed version** of the client I wrote that plays the game, [click here.][brian]{:target="_blank"}

**Client code** can be found [in this repo.][code]{:target="_blank"}

## The Rules of Life

Cells live or die in the next generation by following a simple set of rules:

1. **underpopulation** - Any live cell with fewer than two live neighbors dies.
2. **overpopulation** - Any live cell with more than three live neighbors dies.
3. **stabilization** - Any live cell with two or three live neighbors lives on to the next generation.
4. **reproduction** - Any dead cell with exactly three live neighbors becomes a live cell.


## Why Should We Care?

Even though the rules themselves are simple, the Game of Life has surprising depth.

Certain patterns of cells are mobile in the 2d space. In other words, they can translate themselves through the space in a predictable manner. An example of such a pattern is the **glider**, which translates down and to the right in a diagonal pattern.

<figure>
  <img src="https://upload.wikimedia.org/wikipedia/commons/f/f2/Game_of_life_animated_glider.gif" style="width:225px; height:225; margin:0px auto;">
  <figcaption>Source: https://upload.wikimedia.org/wikipedia/commons/f/f2/Game_of_life_animated_glider.gif </figcaption>
</figure>


To take it one step further, there are patterns that can produce these gilders, like **Gosper's glider gun**.

<figure>
  <img src="https://upload.wikimedia.org/wikipedia/commons/e/e5/Gospers_glider_gun.gif" style="width:312px; height:225; margin:0px auto;">
  <figcaption>Source: https://upload.wikimedia.org/wikipedia/commons/e/e5/Gospers_glider_gun.gif</figcaption>
</figure>

These mobile patterns and their producers can be used to **perform calculations** during an active game of life, meaning the system has [Turing Machine][turing]{:target="_blank"}-like capabilities! There is an entire realm of mathematics devoted to the study of **cellular automata** like the game of life. This realm has implications across many different branches of science, math, and computation.

And it's cool to look at, too :)

## My Implementation

My implementation uses javascript, HTML, and CSS. Its only dependency is a [Coordinate][coord]{:target="_blank"} class that I published to npm, which I extend for my game of life implementation. I used webpack to leverage some ES6 javascript features (like imports) in my code.

The bulk of the algorithm exists in the [Generation class.][generation]{:target="_blank"} `Generation` keeps track of the current state of the game. It accepts an integer in its constructor, and generates a new NxN matrix of dead cells based on that integer. This allows the game to have a flexibly sized window in which to run.

The `Generation.next` function is used to calculate the set of living cells for the next generation of the game. Each cell is of type [LifeCoordinate,][life]{:target="_blank"} which stores the x and y coordinates of a cell, as well as the alive-ness.

Each new generation is calculated from a snapshot of the last, so during a `next` call I construct a new dead set of `LifeCoordinate`s and update its coordinates' 'isAlive' properties in correspondance with the rules applied to cells in `Generation.state`. Once the new generation is fully calculated, I replace `Generation.state` with it.

To speed up rendering the new generation, I keep track of the `diff` of cells that swapped from live to dead (and vice-versa). I return this diff from the `next` call, and only toggle cells from this list in the display grid. **This reduces the number of DOM manipulations that need to be performed between generations.**

There is a [main.js][main]{:target="_blank"} file which leverages the `Generation` class to run the game. It manages the state of the view using `document` APIs, and defines event listeners for the interactive elements on the page.

## Room for Improvement

If I were to improve upon this client, I would try to refactor to make the borders of the grid "infinite". This would more closely emulate Conway's game as it was intended to be played. To do this, I would need to change the way I `findSurroundingCoordinates` for a cell: my `Coordinate` package only returns coordinates inside the scope of the provided matrix and does not consider wrapping around to the other side of the matrix for edge coords.

I would also try to use an ES6/ES7 [generator][generator]{:target="_blank"} to replace my `Generation.next` function. This would likely involve refactoring my entire `Generation` class to **just be a generator**, instead of a stateful class. The stateful-ness isn't *really* necessary, as the only piece of state being stored is a snapshot of the current Generation's grid. **This chunk of state could easily be initiated inside of the generator function scope upon creation of an instance of the generator.**

It would also be cool to have some sort of **time travel** capability. If you wanted to advance N generations ahead, it would be cool to just jump a user right to that generation. Going back in time would be more difficult... and would likely involve storing previously computed generations in memory.


[gol]: https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life
[brian]: http://www.bambielli.com/thats-life-bro/
[code]: https://github.com/bambielli/thats-life-bro
[glider]: https://upload.wikimedia.org/wikipedia/commons/f/f2/Game_of_life_animated_glider.gif
[produce]: https://upload.wikimedia.org/wikipedia/commons/e/e5/Gospers_glider_gun.gif
[turing]: https://en.wikipedia.org/wiki/Turing_machine
[coord]: https://github.com/bambielli/Coordinate
[life]: https://github.com/bambielli/thats-life-bro/blob/master/assets/classes/LifeCoordinate.js
[generation]: https://github.com/bambielli/thats-life-bro/blob/master/assets/classes/Generation.js
[main]: https://github.com/bambielli/thats-life-bro/blob/master/assets/main.js
[generator]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*
