# Tizen-App-Gomoko
###概述
Gomoko基于web project开发，本篇主要介绍Gomoko的核心算法。
###核心算法
游戏参数初始化
<pre>
        var canvas;
        var context;
        var isWhite = true;
        var isWell = false;
        var img_b = new Image();
        img_b.src = "images/b.png";
        var img_w = new Image();
        img_w.src = "images/w.png";
        var chessData = new Array(15);
        for (var x = 0; x < 15; x++) {
            chessData[x] = new Array(15);
            for (var y = 0; y < 15; y++) {
                chessData[x][y] = 0;
            }
        }
</pre>
绘制矩形
<pre>
        function drawRect() {
            canvas = document.getElementById("canvas");
            context = canvas.getContext("2d");
            for (var i = 0; i <= 320; i += 20) {
                context.beginPath();
                context.moveTo(0, i);
                context.lineTo(320, i);
                context.closePath();
                context.stroke();
                context.beginPath();
                context.moveTo(i, 0);
                context.lineTo(i, 320);
                context.closePath();
                context.stroke();
            }
        }
</pre>
下子
<pre>
        function play(e) {
            var x = parseInt((e.clientX - 10) / 20);
            var y = parseInt((e.clientY - 10) / 20);
            if (chessData[x][y] != 0) {
                alert("You cannot put your chess here!");
                return;
            }
            if (isWhite) {
                isWhite = false;
                drawChess(1, x, y);
            }
            else {
                isWhite = true;
                drawChess(2, x, y);
            }
        }
</pre>
绘制棋子
<pre>
        function drawChess(chess, x, y) {
            if (isWell == true) {
                alert("Game is over, please refresh to start.");
                return;
            }
            if (x >= 0 && x < 15 && y >= 0 && y < 15) {
                if (chess == 1) {
                    context.drawImage(img_w, x * 20 + 10, y * 20 + 10);
                    chessData[x][y] = 1;
                }
                else {
                    context.drawImage(img_b, x * 20 + 10, y * 20 + 10);
                    chessData[x][y] = 2;
                }
                judge(x, y, chess);
            }
        }
</pre>
判断是否胜利
<pre>
        function judge(x, y, chess) {
            var count1 = 0;
            var count2 = 0;
            var count3 = 0;
            var count4 = 0;
            for (var i = x; i >= 0; i--) {
                if (chessData[i][y] != chess) {
                    break;
                }
                count1++;
            }
            for (var i = x + 1; i < 15; i++) {
                if (chessData[i][y] != chess) {
                    break;
                }
                count1++;
            }
            for (var i = y; i >= 0; i--) {
                if (chessData[x][i] != chess) {
                    break;
                }
                count2++;
            }
            for (var i = y + 1; i < 15; i++) {
                if (chessData[x][i] != chess) {
                    break;
                }
                count2++;
            }
            for (var i = x, j = y; i >= 0, j >= 0; i--, j--) {
                if (chessData[i][j] != chess) {
                    break;
                }
                count3++;
            }
            for (var i = x + 1, j = y + 1; i < 15, j < 15; i++, j++) {
                if (chessData[i][j] != chess) {
                    break;
                }
                count3++;
            }
            for (var i = x, j = y; i >= 0, j < 15; i--, j++) {
                if (chessData[i][j] != chess) {
                    break;
                }
                count4++;
            }
            for (var i = x + 1, j = y - 1; i < 15, j >= 0; i++, j--) {
                if (chessData[i][j] != chess) {
                    break;
                }
                count4++;
            }
            if (count1 >= 5 || count2 >= 5 || count3 >= 5 || count4 >= 5) {
                if (chess == 1) {
                    alert("White Win");
                    window.location.reload();
                }
                else {
                    alert("Black Win");
                    window.location.reload();
                }
                isWell = true;
            }
        }
</pre>
