<!DOCTYPE html>
<html>
<head>
    
    <center><h1>Powerd by= OPURBA NOBO</h1></center>

  <title>Tic Tac Toe</title>
  <style>
    .container {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      height: 100vh;
    }
  
    .board {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      grid-template-rows: repeat(3, 1fr);
      width: 300px;
      height: 300px;
    }
  
    .cell {
      display: flex;
      justify-content: center;
      align-items: center;
      border: 1px solid black;
      font-size: 48px;
      cursor: pointer;
    }
    
    .cell.x {
      color: blue;
    }
    
    .cell.o {
      color: red;
    }

    .counter-bar {
      width: 300px;
      height: 20px;
      background-color: lightgray;
      margin-top: 20px;
    }

    .counter-fill-x {
      height: 100%;
      background-color: blue;
      transition: width 0.3s ease;
    }

    .counter-fill-o {
      height: 100%;
      background-color: red;
      transition: width 0.3s ease;
    }

    .score-board {
      margin-top: 20px;
    }

    .champion-x {
      color: blue;
      font-weight: bold;
    }

    .champion-o {
      color: red;
      font-weight: bold;
    }
  </style>
</head>
<body style="background-color: chartreuse;">
  <div class="container">
    <div class="board">
      <div class="cell" onclick="makeMove(0, 0)"></div>
      <div class="cell" onclick="makeMove(0, 1)"></div>
      <div class="cell" onclick="makeMove(0, 2)"></div>
      <div class="cell" onclick="makeMove(1, 0)"></div>
      <div class="cell" onclick="makeMove(1, 1)"></div>
      <div class="cell" onclick="makeMove(1, 2)"></div>
      <div class="cell" onclick="makeMove(2, 0)"></div>
      <div class="cell" onclick="makeMove(2, 1)"></div>
      <div class="cell" onclick="makeMove(2, 2)"></div>
    </div>
    <div class="counter-bar">
      <div class="counter-fill-x" id="winBarX"></div>
    </div>
    <div class="counter-bar">
      <div class="counter-fill-o" id="winBarO"></div>
    </div>
    <div class="score-board">
      <div id="winCounterX">Player X wins: 0</div>
      <div id="winCounterO">Player O wins: 0</div>
    </div>
    <div id="champion"></div>
  </div>

  <script>
    let currentPlayer = "X";
    let gameEnded = false;
    let board = [
      ["", "", ""],
      ["", "", ""],
      ["", "", ""]
    ];
    let matchCount = 0;
    let winCountX = 0;
    let winCountO = 0;

    function makeMove(row, col) {
      if (gameEnded || board[row][col] !== "") {
        return;
      }

      board[row][col] = currentPlayer;
      let cell = document.getElementsByClassName("cell")[row * 3 + col];
      cell.innerText = currentPlayer;
      cell.classList.add(currentPlayer.toLowerCase());
      
      if (checkWin(currentPlayer)) {
        alert(currentPlayer + " wins this match!");
        resetBoard();
        if (currentPlayer === "X") {
          winCountX++;
        } else {
          winCountO++;
        }
        matchCount++;
        updateMatchCounter();
        updateWinCounter();
        checkMatchWinner();
        return;
      }

      if (checkDraw()) {
        alert("It's a draw!");
        resetBoard();
        matchCount++;
        updateMatchCounter();
        checkMatchWinner();
        return;
      }

      currentPlayer = currentPlayer === "X" ? "O" : "X";
    }

    function checkWin(player) {
      for (let i = 0; i < 3; i++) {
        if (
          board[i][0] === player &&
          board[i][1] === player &&
          board[i][2] === player
        ) {
          return true;
        }
  
        if (
          board[0][i] === player &&
          board[1][i] === player &&
          board[2][i] === player
        ) {
          return true;
        }
      }
  
      if (
        board[0][0] === player &&
        board[1][1] === player &&
        board[2][2] === player
      ) {
        return true;
      }
  
      if (
        board[2][0] === player &&
        board[1][1] === player &&
        board[0][2] === player
      ) {
        return true;
      }
  
      return false;
    }

    function checkDraw() {
      for (let row = 0; row < 3; row++) {
        for (let col = 0; col < 3; col++) {
          if (board[row][col] === "") {
            return false;
          }
        }
      }
      return true;
    }

    function resetBoard() {
      board = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""]
      ];
      for (let i = 0; i < 9; i++) {
        let cell = document.getElementsByClassName("cell")[i];
        cell.innerText = "";
        cell.className = "cell";
      }
      gameEnded = false;
    }

    function updateMatchCounter() {
      document.getElementById("matchCounter").innerText = "Match: " + matchCount;
      updateMatchProgress();
    }

    function updateWinCounter() {
      document.getElementById("winCounterX").innerText = "Player X wins: " + winCountX;
      document.getElementById("winCounterO").innerText = "Player O wins: " + winCountO;
      updateWinBars();
    }

    function updateMatchProgress() {
      let matchProgress = document.getElementById("matchProgress");
      matchProgress.style.width = (matchCount * 20) + "%";
    }

    function updateWinBars() {
      let winBarX = document.getElementById("winBarX");
      let winBarO = document.getElementById("winBarO");
      winBarX.style.width = (winCountX * 20) + "%";
      winBarO.style.width = (winCountO * 20) + "%";
    }

    function checkMatchWinner() {
      if (winCountX >= 3) {
        document.getElementById("champion").innerText = "Player X is the champion!";
        document.getElementById("champion").classList.add("champion-x");
        gameEnded = true;
      } else if (winCountO >= 3) {
        document.getElementById("champion").innerText = "Player O is the champion!";
        document.getElementById("champion").classList.add("champion-o");
        gameEnded = true;
      } else if (matchCount >= 5) {
        alert("The game ends in a draw!");
        resetGame();
      }
    }

    function resetGame() {
      matchCount = 0;
      winCountX = 0;
      winCountO = 0;
      updateMatchCounter();
      updateWinCounter();
      resetBoard();
      document.getElementById("champion").innerText = "";
      document.getElementById("champion").classList.remove("champion-x", "champion-o");
      gameEnded = false;
    }
  </script>
  <div class="container">
    <div class="board">
      <div class="cell" onclick="makeMove(0, 0)"></div>
      <div class="cell" onclick="makeMove(0, 1)"></div>
      <div class="cell" onclick="makeMove(0, 2)"></div>
      <div class="cell" onclick="makeMove(1, 0)"></div>
      <div class="cell" onclick="makeMove(1, 1)"></div>
      <div class="cell" onclick="makeMove(1, 2)"></div>
      <div class="cell" onclick="makeMove(2, 0)"></div>
      <div class="cell" onclick="makeMove(2, 1)"></div>
      <div class="cell" onclick="makeMove(2, 2)"></div>
    </div>
    <div class="counter-bar">
      <div class="counter-fill-x" id="winBarX"></div>
    </div>
    <div class="counter-bar">
      <div class="counter-fill-o" id="winBarO"></div>
    </div>
    <div class="score-board">
      <div id="winCounterX">Player X wins: 0</div>
      <div id="winCounterO">Player O wins: 0</div>
    </div>
    <div id="champion"></div>
  </div>
</body>
</html>
