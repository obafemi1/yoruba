<script>
	import { onMount } from 'svelte';
	let ball;
	let rightPaddle;
	let leftPaddle;
	let container;
	let leftScore = 0;
	let rightScore = 0;
    const PADDLE_SPEED = 5; //change as needed
	const SCORE_TO_WIN = 5;

  
	onMount(() => {
		document.addEventListener('DOMContentLoaded', () => {
			ball = document.querySelector('.ball');
			leftPaddle = document.getElementById('leftPaddle');
			rightPaddle = document.getElementById('rightPaddle');
			container = document.querySelector('.container');
		});
		document.addEventListener(`keydown`, handleKeyDown);

		// Initial ball speed
		let ballSpeedX = 2;
		let ballSpeedY = 1;

		function update() {
			// Move ball
			ball.style.left = parseInt(ball.style.left) + ballSpeedX + 'px';
			ball.style.top = parseInt(ball.style.top) + ballSpeedY + 'px';

			// Check collision with container edges
			if (
				parseInt(ball.style.top) <= 0 ||
				parseInt(ball.style.top) + ball.offsetHeight >= container.offsetHeight
			) {
				ballSpeedY = -ballSpeedY;
			}

			// Check collision with paddles
			if (
				parseInt(ball.style.left) <= leftPaddle.offsetWidth &&
				parseInt(ball.style.top) >= leftPaddle.offsetTop &&
				parseInt(ball.style.top) <= leftPaddle.offsetTop + leftPaddle.offsetHeight
			) {
				ballSpeedX = -ballSpeedX;
			}

			if (
				parseInt(ball.style.left) + ball.offsetWidth >=
					container.offsetWidth - rightPaddle.offsetWidth &&
				parseInt(ball.style.top) >= rightPaddle.offsetTop &&
				parseInt(ball.style.top) <= rightPaddle.offsetTop + rightPaddle.offsetHeight
			) {
				ballSpeedX = -ballSpeedX;
			}
			if (parseInt(ball.style.left) <= 0) {
				rightScore++;
				document.getElementById('rightScore').textContent = rightScore;
				resetBall();
			}

			if (parseInt(ball.style.left) + ball.offsetWidth >= container.offsetWidth) {
				leftScore++;
				document.getElementById('leftScore').textContent = leftScore;
				resetBall();
			}
			if (leftScore >= SCORE_TO_WIN) {
				alert('Left player wins!');
				resetScores();
			} else if (rightScore >= SCORE_TO_WIN) {
				alert('Right player wins!');
				resetScores();
			}

			// Game loop
			requestAnimationFrame(update);
		}

		function handleKeyDown(event) {
			const keyPressed = event.key.toLowerCase();
			if (keyPressed === `arrowup` || keyPressed === `w`) {
				// Move left paddle up
				leftPaddle.style.top = Math.max(parseInt(leftPaddle.style.top) - PADDLE_SPEED, 0) + `px`;
			} else if (keyPressed === `arrowdown` || keyPressed === `s`) {
				// Move left paddle down
				leftPaddle.style.top =
					Math.min(
						parseInt(leftPaddle.style.top) + PADDLE_SPEED,
						container.offsetHeight - leftPaddle.offsetHeight
					) + `px`;
			} else if (keyPressed === `arrowup`) {
				// Move right paddle up
				rightPaddle.style.top = Math.max(parseInt(rightPaddle.style.top) - PADDLE_SPEED, 0) + `px`;
			} else if (keyPressed === `arrowdown`) {
				// Move right paddle down
				rightPaddle.style.top =
					Math.min(
						parseInt(rightPaddle.style.top) + PADDLE_SPEED,
						container.offsetHeight - rightPaddle.offsetHeight
					) + `px`;
			}
		}
		function resetScores() {
			leftScore = 0;
			rightScore = 0;
			document.getElementById('leftScore').textContent = leftScore;
			document.getElementById('rightScore').textContent = rightScore;
		}

		// Start game loop
		update();
	});
</script>

<div class="container">
	<div class="ball" bind:this={ball}></div>
	<div class="paddle" id="leftPaddle"></div>
	<div class="paddle" id="rightPaddle"></div>
	<div id="scoreboard">
		<span id="leftScore">0</span> - <span id="rightScore">0</span>
	</div>
</div>

<style>
	 
	body {
		font-family: "Roboto Mono", monospace;
		background-color: #0e0e10; 
	}

	.container {
		background-color: #2f2f31;
		border: 1px solid #2f2f2f;
		border-radius: 8px;
		padding: 24px;
		margin: 50px auto;
		box-shadow: 0 0 10px rgba(0, 0, 0, 0.5); 
	}

	.ball {
		width: 20px;
		height: 20px;
		background: linear-gradient(#ff00ff, #00ffff); 
		border-radius: 50%;
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		box-shadow: 0 0 10px rgba(255, 0, 255, 0.6), 0 0 20px rgba(0, 255, 255, 0.6),
			inset 0 0 20px rgba(0, 255, 255, 0.6), inset 0 0 10px rgba(255, 0, 255, 0.6); 
		transition: all 0.3s ease-out;
	}

	.paddle {
		width: 20px;
		height: 80px;
		background: linear-gradient(90deg, #ff00ff, #00ffff); 
		position: absolute;
		border-radius: 5px;
		border: 1px solid #4c4c4c;
		transition: all 0.3s ease-out;
		box-shadow: 0 0 5px rgba(255, 0, 255, 0.6), 0 0 10px rgba(0, 255, 255, 0.6),
			inset 0 0 10px rgba(0, 255, 255, 0.6), inset 0 0 5px rgba(255, 0, 255, 0.6); 
	}

	#leftPaddle {
		left: 0;
	}

	#rightPaddle {
		right: 0;
		top: 50%;
		transform: translate(0, -50%);
	}

	#scoreboard {
		display: flex;
		justify-content: space-between;
		margin-top: 24px;
		font-size: 2rem;
		color: #d6d6d6;
		font-weight: bold;
	}

	#leftScore {
		color: #00ffff;
	}

	#rightScore {
		color: #ff00ff;
	}

	.container::before {
		content: "";
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-image: linear-gradient(
				rgba(255, 255, 255, 0.03) 1px,
				transparent 1px
			),
			linear-gradient(90deg, rgba(255, 255, 255, 0.03) 1px, transparent 1px);
		background-size: 50px 50px;
		z-index: -1;
	}
</style>
