<script>
	import { onMount } from 'svelte';
	let ball;
    let startbutton;
	let rightPaddle;
	let leftPaddle;
	let container;
	let leftScore = 0;
	let rightScore = 0;
	let w = 900;
	let h = 600;
	const PADDLE_SPEED = 5; //change as needed
	const SCORE_TO_WIN = 5;

	onMount(() => {
		ball = document.querySelector('.ball');
		leftPaddle = document.getElementById('leftPaddle');
		rightPaddle = document.getElementById('rightPaddle');
		container = document.querySelector('.container');

		w = window.innerWidth * 0.8;
		h = window.innerHeight * 0.8;
		document.addEventListener(`keydown`, handleKeyDown);

		
		
	});
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
			} else if (keyPressed === `arrowup` || keyPressed === `up`) {
				// Move right paddle up
				rightPaddle.style.top = Math.max(parseInt(rightPaddle.style.top) - PADDLE_SPEED, 0) + `px`;
			} else if (keyPressed === `arrowdown` || keyPressed === `down`) {
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
		function resetBall() {
			ball.style.left = `${w / 2}px`;
			ball.style.top = `${h / 2}px`;
		}
</script>

<div class="wrapper">
	<div class="scoreboard" style="text-align:center;">
		<span id="leftScore" style="color:#00ffff;margin-right:20px;">0</span> -
		<span id="rightScore" style="color:#ff00ff;margin-left:20px;">0</span>
	</div>
	<div class="container" style="width: {w}px; height: {h}px; margin-top:50px;">
		<div class="ball" bind:this={ball}></div>
		<div class="paddle" id="leftPaddle"></div>
		<div class="paddle" id="rightPaddle"></div>
		<button bind:this={startbutton} on:click={()=>{update()}} id="startBtn" class="start-btn">START</button>
	</div>
</div>

<style>
	.start-btn {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		padding: 12px 24px;
		background-color: #00ffff;
		color: #2f2f31;
		border: none;
		border-radius: 8px;
		font-size: 1.2rem;
		font-weight: bold;
		text-transform: uppercase;
		cursor: pointer;
		box-shadow: 0 0 5px rgba(0, 0, 0, 0.5);
		animation: pulse 1s infinite;
	}
	.start-btn:hover {
		background-color: #2f2f31;
		color: #00ffff;
	}
	@keyframes pulse {
		0% {
			transform: translate(-50%, -50%) scale(1);
		}

		50% {
			transform: translate(-50%, -50%) scale(1.1);
		}

		100% {
			transform: translate(-50%, -50%) scale(1);
		}
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
		box-shadow:
			0 0 10px rgba(255, 0, 255, 0.6),
			0 0 20px rgba(0, 255, 255, 0.6),
			inset 0 0 20px rgba(0, 255, 255, 0.6),
			inset 0 0 10px rgba(255, 0, 255, 0.6);
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
		box-shadow:
			0 0 5px rgba(255, 0, 255, 0.6),
			0 0 10px rgba(0, 255, 255, 0.6),
			inset 0 0 10px rgba(0, 255, 255, 0.6),
			inset 0 0 5px rgba(255, 0, 255, 0.6);
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
		font-size: 3rem;
		color: #00ffff;
		font-weight: bold;
		text-align: center;
	}

	#leftScore,
	#rightScore {
		animation: glow 1s infinite;
		font-family: 'DS-Digital', sans-serif;
		font-size: 2.5rem;
	}

	@keyframes glow {
		0% {
			box-shadow:
				0 0 5px rgba(255, 0, 255, 0.6),
				0 0 10px rgba(0, 255, 255, 0.6),
				inset 0 0 10px rgba(0, 255, 255, 0.6),
				inset 0 0 5px rgba(255, 0, 255, 0.6);
		}
		50% {
			box-shadow:
				0 0 10px rgba(255, 0, 255, 0.8),
				0 0 20px rgba(0, 255, 255, 0.8),
				inset 0 0 20px rgba(0, 255, 255, 0.8),
				inset 0 0 10px rgba(255, 0, 255, 0.8);
		}
		100% {
			box-shadow:
				0 0 5px rgba(255, 0, 255, 0.6),
				0 0 10px rgba(0, 255, 255, 0.6),
				inset 0 0 10px rgba(0, 255, 255, 0.6),
				inset 0 0 5px rgba(255, 0, 255, 0.6);
		}
	}

	.container {
		background-color: #2f2f31;
		border: 1px solid #2f2f2f;
		border-radius: 8px;
		padding: 24px;

		box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);

		overflow: hidden;
		position: absolute;
		right: 0;

		left: 0;
		margin-left: auto;
		margin-right: auto;

		background-image: linear-gradient(rgba(255, 255, 255, 0.03) 1px, transparent 1px),
			linear-gradient(90deg, rgba(255, 255, 255, 0.03) 1px, transparent 1px);
		background-size: 50px 50px;
		animation: glitch 1s linear infinite;
		z-index: -1;
	}

	@keyframes glitch {
		0% {
			background-position: 0 0;
		}
		25% {
			background-position: 5px 5px;
		}
		50% {
			background-position: -5px -5px;
		}
		75% {
			background-position: 5px -5px;
		}
		100% {
			background-position: 0 0;
		}
	}
</style>
