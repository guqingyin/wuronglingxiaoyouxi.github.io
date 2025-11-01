[Uploading index.html…]()
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>九宫格三子连线游戏</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Microsoft YaHei', sans-serif;
        }
        
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            padding: 20px;
        }
        
        .container {
            background-color: rgba(255, 255, 255, 0.9);
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            padding: 30px;
            max-width: 800px;
            width: 100%;
        }
        
        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 20px;
            font-size: 2.2rem;
        }
        
        .game-mode {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-bottom: 20px;
        }
        
        .mode-btn {
            padding: 12px 25px;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            background-color: #95a5a6;
            color: white;
        }
        
        .mode-btn.active {
            background-color: #3498db;
        }
        
        .mode-btn:hover {
            transform: translateY(-2px);
        }
        
        .game-info {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            background-color: #f5f5f5;
            padding: 15px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        .player-info {
            text-align: center;
            padding: 10px;
            border-radius: 8px;
            flex: 1;
            margin: 0 10px;
            transition: all 0.3s ease;
        }
        
        .player-info.active {
            background-color: #e3f2fd;
            box-shadow: 0 0 10px rgba(33, 150, 243, 0.5);
        }
        
        .player-name {
            font-size: 1.2rem;
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .player-marker {
            font-size: 2rem;
            margin-bottom: 5px;
        }
        
        .game-board {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            grid-gap: 10px;
            margin: 20px auto;
            max-width: 400px;
        }
        
        .cell {
            aspect-ratio: 1;
            background-color: #fff;
            border: 2px solid #333;
            border-radius: 8px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 3rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            position: relative;
        }
        
        .cell:hover {
            background-color: #f0f0f0;
            transform: translateY(-2px);
        }
        
        .cell.disabled {
            cursor: not-allowed;
            opacity: 0.7;
        }
        
        .cell.disabled:hover {
            background-color: #fff;
            transform: none;
        }
        
        .cell.player-a {
            color: #e74c3c;
        }
        
        .cell.player-b {
            color: #3498db;
        }
        
        .piece-order {
            position: absolute;
            top: 5px;
            left: 5px;
            font-size: 0.8rem;
            font-weight: bold;
            background: rgba(0, 0, 0, 0.7);
            color: white;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .player-a-piece {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background-color: #e74c3c;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        .player-b-piece {
            width: 60px;
            height: 60px;
            border-radius: 8px;
            background-color: #3498db;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        .controls {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 20px;
        }
        
        button {
            padding: 12px 25px;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        #restart-btn {
            background-color: #2ecc71;
            color: white;
        }
        
        #restart-btn:hover {
            background-color: #27ae60;
            transform: translateY(-2px);
        }
        
        #rules-btn {
            background-color: #f39c12;
            color: white;
        }
        
        #rules-btn:hover {
            background-color: #d35400;
            transform: translateY(-2px);
        }
        
        .message {
            text-align: center;
            margin-top: 20px;
            padding: 15px;
            border-radius: 8px;
            font-size: 1.2rem;
            font-weight: bold;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .move-count {
            text-align: center;
            margin-top: 10px;
            color: #7f8c8d;
            font-size: 0.9rem;
        }
        
        .rules-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        
        .rules-content {
            background-color: white;
            padding: 30px;
            border-radius: 15px;
            max-width: 500px;
            width: 90%;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
        }
        
        .rules-content h2 {
            margin-bottom: 15px;
            color: #333;
            text-align: center;
        }
        
        .rules-content ul {
            margin-left: 20px;
            margin-bottom: 20px;
        }
        
        .rules-content li {
            margin-bottom: 10px;
            line-height: 1.5;
        }
        
        .close-btn {
            display: block;
            margin: 0 auto;
            padding: 10px 20px;
            background-color: #e74c3c;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
        }
        
        .close-btn:hover {
            background-color: #c0392b;
        }
        
        .phase-indicator {
            text-align: center;
            margin-bottom: 15px;
            font-weight: bold;
            font-size: 1.1rem;
            padding: 10px;
            border-radius: 8px;
            background-color: #e3f2fd;
        }
        
        .movement-hint {
            text-align: center;
            margin-top: 10px;
            color: #e74c3c;
            font-weight: bold;
        }
        
        .winning-cell {
            animation: pulse 1s infinite;
            box-shadow: 0 0 15px gold !important;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        
        .thinking {
            opacity: 0.7;
            pointer-events: none;
        }
        
        @media (max-width: 600px) {
            .game-info {
                flex-direction: column;
            }
            
            .player-info {
                margin: 5px 0;
            }
            
            .cell {
                font-size: 2.5rem;
            }
            
            .player-a-piece, .player-b-piece {
                width: 50px;
                height: 50px;
            }
            
            .game-mode {
                flex-direction: column;
                gap: 10px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>九宫格三子连线游戏</h1>
        
        <div class="game-mode">
            <button class="mode-btn active" id="pvp-mode">玩家对战</button>
            <button class="mode-btn" id="pvc-mode">人机对战</button>
        </div>
        
        <div class="phase-indicator" id="phase-indicator">放置阶段 - 放置你的棋子</div>
        
        <div class="game-info">
            <div class="player-info active" id="player-a-info">
                <div class="player-name">玩家 A (先手)</div>
                <div class="player-marker">
                    <div class="player-a-piece">●</div>
                </div>
                <div class="player-pieces">棋子: <span id="player-a-pieces">3</span></div>
                <div class="movement-hint" id="player-a-move-hint"></div>
            </div>
            <div class="player-info" id="player-b-info">
                <div class="player-name">玩家 B (后手)</div>
                <div class="player-marker">
                    <div class="player-b-piece">■</div>
                </div>
                <div class="player-pieces">棋子: <span id="player-b-pieces">3</span></div>
                <div class="movement-hint" id="player-b-move-hint"></div>
            </div>
        </div>
        
        <div class="game-board" id="game-board">
            <!-- 九宫格单元格将由JavaScript生成 -->
        </div>
        
        <div class="message" id="message">玩家 A 的回合</div>
        <div class="move-count" id="move-count">手数: 0</div>
        
        <div class="controls">
            <button id="restart-btn">重新开始</button>
            <button id="rules-btn">游戏规则</button>
        </div>
    </div>
    
    <div class="rules-modal" id="rules-modal">
        <div class="rules-content">
            <h2>游戏规则</h2>
            <ul>
                <li>两位玩家轮流在九宫格中放置棋子，先手为玩家A(●)，后手为玩家B(■)</li>
                <li>每位玩家各有三枚棋子，放置阶段双方轮流放置棋子</li>
                <li>棋子上的数字表示放置顺序：1=最早，2=中间，3=最新</li>
                <li>当所有棋子放置完毕后（第6步结束），进入移动阶段</li>
                <li>移动阶段只能移动本方最早放置的棋子（即标记为1的棋子）</li>
                <li>棋子可以移动到任意空位</li>
                <li>任何阶段中，如果三子连线（横、竖、斜）则立即获胜</li>
                <li>如果150手内无人获胜，则为平局</li>
            </ul>
            <button class="close-btn" id="close-rules">关闭</button>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // 游戏状态
            const gameState = {
                board: Array(9).fill(null),
                currentPlayer: 'A',
                phase: 'placement', // 'placement' 或 'movement'
                playerAPieces: 3,
                playerBPieces: 3,
                placedPieces: {
                    A: [], // 按放置顺序存储棋子位置 [最早, 中间, 最新]
                    B: []
                },
                gameOver: false,
                winner: null,
                moveCount: 0,
                gameMode: 'pvp', // 'pvp' 或 'pvc'
                isAITurn: false,
                maxMoves: 150 // 最大手数限制
            };

            // DOM 元素
            const gameBoard = document.getElementById('game-board');
            const playerAInfo = document.getElementById('player-a-info');
            const playerBInfo = document.getElementById('player-b-info');
            const playerAPieces = document.getElementById('player-a-pieces');
            const playerBPieces = document.getElementById('player-b-pieces');
            const message = document.getElementById('message');
            const moveCount = document.getElementById('move-count');
            const restartBtn = document.getElementById('restart-btn');
            const rulesBtn = document.getElementById('rules-btn');
            const rulesModal = document.getElementById('rules-modal');
            const closeRules = document.getElementById('close-rules');
            const phaseIndicator = document.getElementById('phase-indicator');
            const playerAMoveHint = document.getElementById('player-a-move-hint');
            const playerBMoveHint = document.getElementById('player-b-move-hint');
            const pvpModeBtn = document.getElementById('pvp-mode');
            const pvcModeBtn = document.getElementById('pvc-mode');

            // 初始化游戏板
            function initializeBoard() {
                gameBoard.innerHTML = '';
                for (let i = 0; i < 9; i++) {
                    const cell = document.createElement('div');
                    cell.className = 'cell';
                    cell.dataset.index = i;
                    cell.addEventListener('click', () => handleCellClick(i));
                    gameBoard.appendChild(cell);
                }
            }

            // 清除获胜高亮
            function clearWinningHighlight() {
                const cells = document.querySelectorAll('.cell');
                cells.forEach(cell => {
                    cell.classList.remove('winning-cell');
                    cell.style.boxShadow = ''; // 清除内联样式
                });
            }

            // 处理单元格点击
            function handleCellClick(index) {
                if (gameState.gameOver || gameState.isAITurn) return;
                
                if (gameState.phase === 'placement') {
                    handlePlacementPhase(index);
                } else {
                    handleMovementPhase(index);
                }
                
                // 如果是人机模式且游戏未结束，让AI行动
                if (gameState.gameMode === 'pvc' && !gameState.gameOver && gameState.currentPlayer === 'B') {
                    gameState.isAITurn = true;
                    setTimeout(makeAIMove, 800); // 延迟800ms让玩家看到AI思考
                }
            }

            // 处理放置阶段
            function handlePlacementPhase(index) {
                // 如果单元格已被占用，则忽略点击
                if (gameState.board[index] !== null) return;
                
                // 放置棋子
                gameState.board[index] = gameState.currentPlayer;
                gameState.placedPieces[gameState.currentPlayer].push(index);
                gameState.moveCount++;
                
                // 更新棋子计数
                if (gameState.currentPlayer === 'A') {
                    gameState.playerAPieces--;
                } else {
                    gameState.playerBPieces--;
                }
                
                // 更新UI
                updateBoard();
                
                // 检查胜利条件
                if (checkWin(gameState.currentPlayer)) {
                    gameState.gameOver = true;
                    gameState.winner = gameState.currentPlayer;
                    const winnerName = gameState.currentPlayer === 'A' ? '玩家 A' : 
                                     (gameState.gameMode === 'pvc' ? 'AI' : '玩家 B');
                    message.textContent = `${winnerName} 获胜！`;
                    highlightWinningLine(gameState.currentPlayer);
                    return;
                }
                
                // 检查是否进入移动阶段
                if (gameState.playerAPieces === 0 && gameState.playerBPieces === 0) {
                    gameState.phase = 'movement';
                    phaseIndicator.textContent = '移动阶段 - 移动你的第一子（标记为1的棋子）';
                    updateMovementHints();
                }
                
                // 切换玩家
                switchPlayer();
            }

            // 处理移动阶段
            function handleMovementPhase(index) {
                const currentPlayer = gameState.currentPlayer;
                const firstPieceIndex = gameState.placedPieces[currentPlayer][0];
                
                // 如果点击的是空单元格，则移动第一子
                if (gameState.board[index] === null) {
                    // 移动棋子
                    gameState.board[firstPieceIndex] = null;
                    gameState.board[index] = currentPlayer;
                    
                    // 更新放置的棋子记录
                    gameState.placedPieces[currentPlayer].shift();
                    gameState.placedPieces[currentPlayer].push(index);
                    gameState.moveCount++;
                    
                    // 更新UI
                    updateBoard();
                    
                    // 检查胜利条件
                    if (checkWin(currentPlayer)) {
                        gameState.gameOver = true;
                        gameState.winner = currentPlayer;
                        const winnerName = currentPlayer === 'A' ? '玩家 A' : 
                                         (gameState.gameMode === 'pvc' ? 'AI' : '玩家 B');
                        message.textContent = `${winnerName} 获胜！`;
                        highlightWinningLine(currentPlayer);
                        return;
                    }
                    
                    // 检查平局（150手）
                    if (gameState.moveCount >= gameState.maxMoves) {
                        gameState.gameOver = true;
                        message.textContent = `平局！${gameState.maxMoves}手内无人获胜`;
                        return;
                    }
                    
                    // 切换玩家
                    switchPlayer();
                    updateMovementHints();
                }
            }

            // AI移动逻辑
            function makeAIMove() {
                if (gameState.gameOver) return;
                
                let moveIndex;
                
                if (gameState.phase === 'placement') {
                    // 放置阶段的AI逻辑
                    moveIndex = getAIPlacementMove();
                } else {
                    // 移动阶段的AI逻辑
                    moveIndex = getAIMovementMove();
                }
                
                // 执行AI移动
                if (moveIndex !== -1) {
                    if (gameState.phase === 'placement') {
                        handlePlacementPhase(moveIndex);
                    } else {
                        handleMovementPhase(moveIndex);
                    }
                }
                
                gameState.isAITurn = false;
            }

            // 获取AI放置移动
            function getAIPlacementMove() {
                // 1. 检查是否能立即获胜
                for (let i = 0; i < 9; i++) {
                    if (gameState.board[i] === null) {
                        gameState.board[i] = 'B';
                        if (checkWin('B')) {
                            gameState.board[i] = null;
                            return i;
                        }
                        gameState.board[i] = null;
                    }
                }
                
                // 2. 阻止玩家获胜
                for (let i = 0; i < 9; i++) {
                    if (gameState.board[i] === null) {
                        gameState.board[i] = 'A';
                        if (checkWin('A')) {
                            gameState.board[i] = null;
                            return i;
                        }
                        gameState.board[i] = null;
                    }
                }
                
                // 3. 优先选择中心位置
                if (gameState.board[4] === null) return 4;
                
                // 4. 优先选择角落位置
                const corners = [0, 2, 6, 8];
                const availableCorners = corners.filter(i => gameState.board[i] === null);
                if (availableCorners.length > 0) {
                    return availableCorners[Math.floor(Math.random() * availableCorners.length)];
                }
                
                // 5. 选择边缘位置
                const edges = [1, 3, 5, 7];
                const availableEdges = edges.filter(i => gameState.board[i] === null);
                if (availableEdges.length > 0) {
                    return availableEdges[Math.floor(Math.random() * availableEdges.length)];
                }
                
                return -1;
            }

            // 获取AI移动移动
            function getAIMovementMove() {
                const firstPieceIndex = gameState.placedPieces.B[0];
                
                // 1. 检查是否能立即获胜
                for (let i = 0; i < 9; i++) {
                    if (gameState.board[i] === null) {
                        // 模拟移动
                        gameState.board[firstPieceIndex] = null;
                        gameState.board[i] = 'B';
                        if (checkWin('B')) {
                            // 恢复棋盘
                            gameState.board[firstPieceIndex] = 'B';
                            gameState.board[i] = null;
                            return i;
                        }
                        // 恢复棋盘
                        gameState.board[firstPieceIndex] = 'B';
                        gameState.board[i] = null;
                    }
                }
                
                // 2. 阻止玩家获胜
                for (let i = 0; i < 9; i++) {
                    if (gameState.board[i] === null) {
                        // 模拟玩家移动
                        const playerFirstPiece = gameState.placedPieces.A[0];
                        gameState.board[playerFirstPiece] = null;
                        gameState.board[i] = 'A';
                        if (checkWin('A')) {
                            // 恢复棋盘
                            gameState.board[playerFirstPiece] = 'A';
                            gameState.board[i] = null;
                            return i;
                        }
                        // 恢复棋盘
                        gameState.board[playerFirstPiece] = 'A';
                        gameState.board[i] = null;
                    }
                }
                
                // 3. 随机选择空位移动
                const emptyCells = [];
                for (let i = 0; i < 9; i++) {
                    if (gameState.board[i] === null) {
                        emptyCells.push(i);
                    }
                }
                
                if (emptyCells.length > 0) {
                    return emptyCells[Math.floor(Math.random() * emptyCells.length)];
                }
                
                return -1;
            }

            // 切换玩家
            function switchPlayer() {
                gameState.currentPlayer = gameState.currentPlayer === 'A' ? 'B' : 'A';
                updatePlayerInfo();
                
                const currentPlayerName = gameState.currentPlayer === 'A' ? '玩家 A' : 
                                       (gameState.gameMode === 'pvc' ? 'AI' : '玩家 B');
                message.textContent = `${currentPlayerName} 的回合`;
            }

            // 更新游戏板
            function updateBoard() {
                const cells = document.querySelectorAll('.cell');
                cells.forEach((cell, index) => {
                    // 清除之前的内容
                    cell.textContent = '';
                    cell.innerHTML = '';
                    cell.classList.remove('player-a', 'player-b', 'winning-cell', 'disabled');
                    
                    if (gameState.board[index] === 'A') {
                        const piece = document.createElement('div');
                        piece.className = 'player-a-piece';
                        piece.textContent = '●';
                        cell.appendChild(piece);
                        cell.classList.add('player-a');
                        
                        // 添加顺序标记
                        const order = getPieceOrder('A', index);
                        if (order > 0) {
                            const orderMarker = document.createElement('div');
                            orderMarker.className = 'piece-order';
                            orderMarker.textContent = order;
                            cell.appendChild(orderMarker);
                        }
                    } else if (gameState.board[index] === 'B') {
                        const piece = document.createElement('div');
                        piece.className = 'player-b-piece';
                        piece.textContent = '■';
                        cell.appendChild(piece);
                        cell.classList.add('player-b');
                        
                        // 添加顺序标记
                        const order = getPieceOrder('B', index);
                        if (order > 0) {
                            const orderMarker = document.createElement('div');
                            orderMarker.className = 'piece-order';
                            orderMarker.textContent = order;
                            cell.appendChild(orderMarker);
                        }
                    }
                    
                    // 在人机模式下，如果是AI的回合，禁用所有单元格
                    if (gameState.gameMode === 'pvc' && gameState.isAITurn) {
                        cell.classList.add('disabled');
                    }
                });
                
                // 更新棋子计数
                playerAPieces.textContent = gameState.playerAPieces;
                playerBPieces.textContent = gameState.playerBPieces;
                
                // 更新玩家B名称
                const playerBName = document.querySelector('#player-b-info .player-name');
                playerBName.textContent = gameState.gameMode === 'pvc' ? 'AI (后手)' : '玩家 B (后手)';
                
                // 更新手数显示
                moveCount.textContent = `手数: ${gameState.moveCount}`;
            }

            // 获取棋子顺序（1=最早，2=中间，3=最新）
            function getPieceOrder(player, index) {
                const pieces = gameState.placedPieces[player];
                const position = pieces.indexOf(index);
                return position + 1; // 返回1,2,3
            }

            // 更新移动提示
            function updateMovementHints() {
                if (gameState.phase === 'movement') {
                    const aFirstPiece = gameState.placedPieces.A[0];
                    const bFirstPiece = gameState.placedPieces.B[0];
                    
                    playerAMoveHint.textContent = `移动棋子: 位置${aFirstPiece + 1}`;
                    playerBMoveHint.textContent = `移动棋子: 位置${bFirstPiece + 1}`;
                } else {
                    playerAMoveHint.textContent = '';
                    playerBMoveHint.textContent = '';
                }
            }

            // 更新玩家信息
            function updatePlayerInfo() {
                if (gameState.currentPlayer === 'A') {
                    playerAInfo.classList.add('active');
                    playerBInfo.classList.remove('active');
                } else {
                    playerBInfo.classList.add('active');
                    playerAInfo.classList.remove('active');
                }
            }

            // 检查胜利条件
            function checkWin(player) {
                const winPatterns = [
                    [0, 1, 2], [3, 4, 5], [6, 7, 8], // 横
                    [0, 3, 6], [1, 4, 7], [2, 5, 8], // 竖
                    [0, 4, 8], [2, 4, 6]             // 斜
                ];
                
                return winPatterns.some(pattern => {
                    return pattern.every(index => gameState.board[index] === player);
                });
            }

            // 高亮显示获胜连线
            function highlightWinningLine(player) {
                const winPatterns = [
                    [0, 1, 2], [3, 4, 5], [6, 7, 8], // 横
                    [0, 3, 6], [1, 4, 7], [2, 5, 8], // 竖
                    [0, 4, 8], [2, 4, 6]             // 斜
                ];
                
                const winningPattern = winPatterns.find(pattern => {
                    return pattern.every(index => gameState.board[index] === player);
                });
                
                if (winningPattern) {
                    winningPattern.forEach(index => {
                        const cell = document.querySelector(`.cell[data-index="${index}"]`);
                        cell.classList.add('winning-cell');
                    });
                }
            }

            // 切换游戏模式
            function switchGameMode(mode) {
                gameState.gameMode = mode;
                pvpModeBtn.classList.toggle('active', mode === 'pvp');
                pvcModeBtn.classList.toggle('active', mode === 'pvc');
                
                // 更新玩家B名称
                const playerBName = document.querySelector('#player-b-info .player-name');
                playerBName.textContent = mode === 'pvc' ? 'AI (后手)' : '玩家 B (后手)';
                
                restartGame();
            }

            // 重新开始游戏
            function restartGame() {
                // 清除获胜高亮
                clearWinningHighlight();
                
                gameState.board = Array(9).fill(null);
                gameState.currentPlayer = 'A';
                gameState.phase = 'placement';
                gameState.playerAPieces = 3;
                gameState.playerBPieces = 3;
                gameState.placedPieces = { A: [], B: [] };
                gameState.gameOver = false;
                gameState.winner = null;
                gameState.moveCount = 0;
                gameState.isAITurn = false;
                
                updateBoard();
                updatePlayerInfo();
                message.textContent = '玩家 A 的回合';
                phaseIndicator.textContent = '放置阶段 - 放置你的棋子';
                updateMovementHints();
            }

            // 事件监听器
            restartBtn.addEventListener('click', restartGame);
            
            rulesBtn.addEventListener('click', () => {
                rulesModal.style.display = 'flex';
            });
            
            closeRules.addEventListener('click', () => {
                rulesModal.style.display = 'none';
            });
            
            pvpModeBtn.addEventListener('click', () => switchGameMode('pvp'));
            pvcModeBtn.addEventListener('click', () => switchGameMode('pvc'));
            
            // 点击模态框外部关闭
            window.addEventListener('click', (e) => {
                if (e.target === rulesModal) {
                    rulesModal.style.display = 'none';
                }
            });

            // 初始化游戏
            initializeBoard();
            updatePlayerInfo();
        });
    </script>
</body>
</html>
