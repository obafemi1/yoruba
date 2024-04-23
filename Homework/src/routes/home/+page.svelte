<script>
	import { onMount } from 'svelte';
	let ball;
	let rightPaddle;
	let leftPaddle;
	let container;

	onMount(() => {
		document.addEventListener('DOMContentLoaded', () => {
			ball = document.querySelector('.ball');
			leftPaddle = document.getElementById('leftPaddle');
			rightPaddle = document.getElementById('rightPaddle');
			container = document.querySelector('.container');
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

			// Game loop
			requestAnimationFrame(update);
		}

		// Start game loop
		update();
	});
</script>

<div class="container">
	<div class="ball" bind:this={ball}></div>
	<div class="paddle" id="leftPaddle"></div>
	<div class="paddle" id="rightPaddle"></div>
</div>

<style>
	ball {
		width: 20px;
		height: 20px;
		background: red;
		border-radius: 50%;
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
	}

	.paddle {
		width: 10px;
		height: 80px;
		background: blue;
		position: absolute;
	}

	#leftPaddle {
		left: 0;
	}

	#rightPaddle {
		right: 0;
	}
</style>
