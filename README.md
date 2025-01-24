# -<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>9x9 老虎機遊戲</title>
    <style>
        body { font-family: Arial, sans-serif; display: flex; justify-content: center; align-items: center; height: 100vh; background-color: #f0f0f0; }
        #slot-machine { display: grid; grid-template-columns: repeat(9, 50px); grid-template-rows: repeat(9, 50px); gap: 5px; }
        .slot { width: 50px; height: 50px; display: flex; justify-content: center; align-items: center; border: 1px solid #ccc; background-color: #fff; font-size: 24px; }
        .highlight { animation: highlight 0.5s; background-color: #ff0 !important; }
        @keyframes highlight { from { background-color: #fff; } to { background-color: #ff0; } }
    </style>
</head>
<body>
    <div>
        <div id="slot-machine"></div>
        <div>
            <label for="bet-amount">下注金額:</label>
            <input type="number" id="bet-amount" min="1" max="10000" value="1">
            <button onclick="spin()">旋轉</button>
        </div>
        <div id="result"></div>
    </div>
    <script>
        const symbols = ['五筒', '二條', '胡', '紅中', '發財', '白板', '五萬', '九萬', '三筒'];
        const gridSize = 9;

        let playerScore = 0;

        function getRandomSymbol() {
            return symbols[Math.floor(Math.random() * symbols.length)];
        }

        function spin() {
            const betAmount = parseInt(document.getElementById('bet-amount').value);
            if (betAmount < 1 || betAmount > 10000) {
                alert('下注金額必須在 1 到 10000 之間');
                return;
            }

            const slotMachine = document.getElementById('slot-machine');
            slotMachine.innerHTML = '';
            const result = [];

            for (let i = 0; i < gridSize; i++) {
                result[i] = [];
                for (let j = 0; j < gridSize; j++) {
                    const symbol = getRandomSymbol();
                    result[i][j] = symbol;
                    const slot = document.createElement('div');
                    slot.className = 'slot';
                    slot.innerText = symbol;
                    slotMachine.appendChild(slot);
                }
            }

            const winnings = calculateWinnings(result);
            playerScore += winnings * betAmount;
            document.getElementById('result').innerText = `贏得 ${winnings * betAmount} 分數，當前分數: ${playerScore}`;
        }

        function calculateWinnings(result) {
            let winnings = 0;
            const huPositions = [];

            for (let i = 0; i < gridSize; i++) {
                for (let j = 0; j < gridSize; j++) {
                    if (result[i][j] === '胡') {
                        huPositions.push([i, j]);
                    }
                }
            }

            if (huPositions.length >= 3) {
                let freeGames = 0;
                if (huPositions.length === 3) freeGames = 12;
                if (huPositions.length === 4) freeGames = 14;
                if (huPositions.length === 5) freeGames = 22;
                winnings += freeGames;
                huPositions.forEach(pos => {
                    const slot = document.querySelector(`#slot-machine > div:nth-child(${pos[0] * gridSize + pos[1] + 1})`);
                    slot.classList.add('highlight');
                });
            }

            return winnings;
        }
    </script>
</body>
</html>