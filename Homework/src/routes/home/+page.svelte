<script>
    document.addEventListener('DOMContentLoaded', () => {
      const ball = document.querySelector('.ball');
      const leftPaddle = document.getElementById('leftPaddle');
      const rightPaddle = document.getElementById('rightPaddle');
      const container = document.querySelector('.container');

      // Initial ball speed
      let ballSpeedX = 2;
      let ballSpeedY = 1;

      function update() {
        // Move ball
        ball.style.left = parseInt(ball.style.left) + ballSpeedX + 'px';
        ball.style.top = parseInt(ball.style.top) + ballSpeedY + 'px';

        // Check collision with container edges
        if (parseInt(ball.style.top) <= 0 || parseInt(ball.style.top) + ball.offsetHeight >= container.offsetHeight) {
          ballSpeedY = -ballSpeedY;
        }

        // Check collision with paddles
        if (parseInt(ball.style.left) <= leftPaddle.offsetWidth &&
            parseInt(ball.style.top) >= leftPaddle.offsetTop &&
            parseInt(ball.style.top) <= leftPaddle.offsetTop + leftPaddle.offsetHeight) {
          ballSpeedX = -ballSpeedX;
        }

        if (parseInt(ball.style.left) + ball.offsetWidth >= container.offsetWidth - rightPaddle.offsetWidth &&
            parseInt(ball.style.top) >= rightPaddle.offsetTop &&
            parseInt(ball.style.top) <= rightPaddle.offsetTop + rightPaddle.offsetHeight) {
          ballSpeedX = -ballSpeedX;
        }

        // Game loop
        requestAnimationFrame(update);
      }

      // Start game loop
      update();
    });
  </script>


