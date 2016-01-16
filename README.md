# Awale

This library provide a PHP implementation of the [Awale (or Oware) game](https://en.wikipedia.org/wiki/Oware).


## Installation


### Download

Using Composer:

``` js
{
    "require": {
        "alcalyn/awale": "1.0.x"
    }
}
```

Update your composer.

``` bash
composer update
```

Not using Composer ? [Install it directly](https://github.com/alcalyn/awale/archive/master.zip).


## Usage

### Creating an instance

Create an instance of Awale, which is an instance of an Awale board, with seeds.

``` php
use Alcalyn\Awale\Awale;

$awale = new Awale();

// Players start with 4 seeds in each container.
$awale->setSeedsPerContainer(4);

// The first player starts.
$awale->setCurrentPlayer(Awale::PLAYER_0);

// Needs to explicitly init the grid (containers seeds number, and attics).
$awale->initGrid();
```

Or to do it shorter:

``` php
use Alcalyn\Awale\Awale;

$awale = new Awale::createWithSeedsPerContainer(4);
```


### The grid

The Awale grid represents the containers and attics.

``` php
// Retrieve the grid array from the Awale instance
$grid = $awale->getGrid();

print_r($grid);

/* Outputs:
Array
(
    [0] => Array                // Player 0, or player top.
        (
            [seeds] => Array
                (
                    [0] => 4    // Player 0 seeds, he has 4 seeds in each containers.
                    [1] => 4    // The first container is the top left container.
                    [2] => 4
                    [3] => 4
                    [4] => 4
                    [5] => 4    // The top right container.
                )

            [attic] => 0        // Player 0 has no seeds in his attic.
        )

    [1] => Array                // Player 1, or player bottom.
        (
            [seeds] => Array
                (
                    [0] => 4    // The bottom left container.
                    [1] => 4
                    [2] => 4
                    [3] => 4
                    [4] => 4
                    [5] => 4    // The bottom right container.
                )

            [attic] => 0        // No seeds in player 1 attic.
        )

)
*/

Or to get a graphical representation:

echo $awale;

/* Outputs:

Awale game instance.
     4  4  4  4  4  4
  0                    0
     4  4  4  4  4  4
seeds per container: 4
current player: PLAYER_0
last move: null

*/
```


### Play moves

Once you have an instance of a game, you can play move.

``` php
// Top player plays his third container (from left)
$awale->play(Awale::PLAYER_0, 2);

// Bottom player plays his first container (from left)
$awale->play(Awale::PLAYER_1, 0);
```

The `play` method throws an `Alcalyn\Awale\Exception\AwaleException` on invalid move.


### Game usefull checks

``` php
// Last played move
// An array with keys 'players' and 'move', example: {player:1, move:5}, Player 1 played his 5th container
$awale->getLastMove();

// Player's turn to play
$awale->getCurrentPlayer(); // Awale::PLAYER_0 or Awale::PLAYER_1

// Get amount of seeds needed to exceed, depending on seedsPerContainer
$awale->getSeedsNeededToWin(); // 24 if seedsPerContainer = 4

// Check is game is over (a player has more than 24 seeds, or game is looping, or player cannot feeds his opponent)
$awale->isGameOver(); // true or false

// Get winner when game is finished
$awale->getWinner(); // Awale::PLAYER_0 or Awale::PLAYER_1 or Awale::DRAW or null

// Get seeds in a player attic
$awale->getScore(Awale::PLAYER_1);

// Whether a loop is detected (a same state of the game will appear again and again)
$awale->isGameLooping();
```

There is some other methods in the [Awale class](src/Awale.php).


## Run unit tests

First, update your composer to get phpunit, then run:

``` php
vendor/bin/phpunit -c .
```


## License

This library is under the [MIT License](LICENSE).
