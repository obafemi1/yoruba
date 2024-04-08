<html>
<head>
    <title>Hit the Square Game</title>
    <meta charset="UTF-8">
    <style>
        body { background-color: #222; color: #eee; }
        #square { width: 200px; height: 200px; background-color: yellow; }
    </style>
</head>
<body>
    <div id="square"></div>
    <p>Score: <span id="score">0</span></p>
    <p>Click the yellow square as many times as you can before it blows up!</p>
    <script>
        var square = document.getElementById("square");
        var score = document.getElementById("score");
        var clicksLeft = 0;
        var timerId = null;
        var maxScore = 0;

        function getRandInt(min, max) {
            return Math.floor(Math.random() * (max - min + 1) ) + min;
        }

        function explodeSquare() {
            clicksLeft = 0;
            square.style.backgroundColor = "red";
            clearTimeout(timerId);
            timerId = setTimeout(resetSquare, getRandInt(1000, 5000));
            if (maxScore < parseInt(score.innerHTML)) {
                maxScore = parseInt(score.innerHTML);
            }
        }

        function resetSquare() {
            clicksLeft = getRandInt(1, 20);
            square.style.backgroundColor = "yellow";
        }

        function handleClick() {
            if (clicksLeft > 0) {
                clicksLeft--;
                score.innerHTML = parseInt(score.innerHTML) + 1;
            } else {
                explodeSquare();
            }
        }

        resetSquare();
        square.addEventListener("click", handleClick);

        window.addEventListener("beforeunload", function(event) {
            localStorage.setItem("maxScore", maxScore);
        });

        var storedMaxScore = localStorage.getItem("maxScore");
        if (storedMaxScore != null) {
            maxScore = parseInt(storedMaxScore);
        }
    </script>
</body>
</html>