<script lang="ts">
  let gameOver: boolean = false;
  let draw: boolean = false;
  let xTurn: boolean = true;
  let winner: string = "";
  let magicSquare: number[] = [4, 9, 2, 3, 5, 7, 8, 1, 6];
  let board: string[] = [" ", " ", " ", " ", " ", " ", " ", " ", " "];

  let convertIndex = (rNum: number, cNum: number) => {
    return rNum * 3 + cNum;
  };
  let handleTurn = (index: number) => {
    console.log(
      `${xTurn ? "X" : "O"} Plays ${index % 3} : ${Math.floor(index / 3)}`
    );
    board[index] = xTurn ? "X" : "O";
    checkWinner();
    xTurn = !xTurn;
  };
  let checkWinner = () => {
    if (hasWon("X")) {
      console.log("X WINNER");
      winner = "X";
      gameOver = true;
    } else if (hasWon("O")) {
      console.log("O WINNER");
      winner = "O";
      gameOver = true;
    } else if (hasDraw()) {
      console.log("Draw!");
      gameOver = true;
    } else {
      console.log("No Winner Yet");
    }
  };
  let hasWon = (player: string) => {
    for (let i = 0; i < 9; i++) {
      for (let j = 0; j < 9; j++) {
        for (let k = 0; k < 9; k++) {
          if (i != j && i != k && j != k) {
            if (
              board[i] == player &&
              board[j] == player &&
              board[k] == player
            ) {
              if (magicSquare[i] + magicSquare[j] + magicSquare[k] == 15)
                return true;
            }
          }
        }
      }
    }
    return false;
  };
  let hasDraw = () => {
    draw = !board.includes(" ");
    return draw;
  };
  let reset = () => {
    gameOver = false;
    xTurn = true;
    draw = false;
    winner = "";
    board = [" ", " ", " ", " ", " ", " ", " ", " ", " "];
  };
</script>

<main>
  {#each { length: 3 } as row, rNum}
    <div class="row">
      {#each { length: 3 } as col, cNum}
        <button
          class="square"
          disabled={board[convertIndex(rNum, cNum)] != " " || gameOver}
          on:click={() => handleTurn(convertIndex(rNum, cNum))}
          >{board[convertIndex(rNum, cNum)]}</button
        >
      {/each}
    </div>
    <br />
  {/each}
  {#if gameOver}
    {#if winner != ""}
      <h1>Winner is {winner}!</h1>
    {:else}
      <h1>No Winner, Its a Draw!</h1>
    {/if}
    <button on:click={reset}>Reset Game</button>
  {:else}
    <h1>It is {xTurn ? "X" : "O"}'s Turn</h1>
  {/if}
</main>

<style>
  main {
    max-width: 960px;
    text-align: center;
    margin: 0 auto;
    padding-top: 100px;
  }
  .row {
    display: inline-block;
    margin: 0;
    padding: 0;
  }
  .square {
    min-width: 100px;
    min-height: 100px;
    vertical-align: top;
    text-align: center;
    margin: 0;
    padding: 0;
    cursor: pointer;
    transition: 0.2s;
  }
  .square .x {
    background: #fcecc9;
  }
  .square .y {
    background: #788aa3;
  }
</style>
