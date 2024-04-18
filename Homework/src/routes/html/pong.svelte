<html>
	<head>
		<meta charset="UTF-8">
		<title>Pong</title>
		<style type="text/css">
			body {
				background-image: url("C:/Users/elvir/OneDrive/Desktop/Pong.jpg");
				background-size: cover;
				background-position: center;
				background-repeat: no-repeat;
				position: relative; /* Make sure the body element is positioned relative */
				min-height: 100vh; /* set minimum height for the body element to ensure the button is at the bottom of the page */
			}
			
			.btn {
				background-color: #4CAF50;
				color: white;
				border: none;
				padding: 12px 24px;
				font-size: 16px;
				position: absolute;
				bottom: 20px;
				left: 50%;
				transform: translateX(-50%);
				cursor: pointer; 
			}
		</style>
	</head>
	<body>
		<img src="th-image.jpg" alt="The Image"> 
		<button class="btn">Gear Up</button>
	</body>
</html>