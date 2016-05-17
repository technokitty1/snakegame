# snakegame

<!DOCTYPE html>
<html>
<head>
	<title>Snake Plus Plus</title>
</head>
<body>
	
	<canvas id="gameView" width="600" height="300"></canvas>
	<button id="pause" onclick="pauseUnpause()">Pause</button>
	<script>
		//Constants
		const SNAKESIZE = 15;
		const APPLESIZE = 10;
		const GAMEWIDTH = 600;
		const GAMEHEIGHT = 300;
		
		//Initial Conditions and Variables
		var bgImage = new Image();
		bgImage.src = "images/background.jpg";
		
		var snake = {speed:120,velX:0,velY:0,posX:50,posY:50};
		snake.velX = snake.speed;
		var apple = {posX:400,posY:200};
		
		var pause = false;
		var debugCount = 0;
		var bodyParts = 1;
		
		
		//Canvas-specific stuff
		var game = document.getElementById("gameView");
		var gameContext = gameView.getContext("2d");
		
		// Cross-browser support for requestAnimationFrame
		requestAnimationFrame = window.requestAnimationFrame || window.webkitRequestAnimationFrame || window.msRequestAnimationFrame || window.mozRequestAnimationFrame;
		
		// Handle keyboard controls
		var keysDown = {};
		addEventListener("keydown", function (e) {keysDown[e.keyCode] = true;}, false);
		addEventListener("keyup", function (e) {delete keysDown[e.keyCode];}, false);
		
		//Start Game Loop
		var then = Date.now();
		main();
		
		//Update loop
		function update(modifier) {
			//Inputs
			if(38 in keysDown && snake.velY <=0) { //go up
				snake.velX = 0;
				snake.velY = -snake.speed;
				snake.posY += snake.velY*modifier;
			}
			else if(40 in keysDown && snake.velY >=0) { //go down
				snake.velX = 0;
				snake.velY = snake.speed;
				snake.posY += snake.velY*modifier;
			}
			else if(37 in keysDown && snake.velX <=0) { //go left
				snake.velY = 0;
				snake.velX = -snake.speed;
				snake.posX += snake.velX*modifier;
			}
			else if(39 in keysDown && snake.velX >=0) { //go right
				snake.velY = 0;
				snake.velX = snake.speed;
				snake.posX += snake.velX*modifier;
			}
			else {
				snake.posX += snake.velX*modifier;
				snake.posY += snake.velY*modifier;
			}
			//Screen Wrapping
			if (snake.posX > GAMEWIDTH){
				snake.posX = 0;
			}
			else if (snake.posX < 0) {
				snake.posX = GAMEWIDTH;
			}
			if (snake.posY > GAMEHEIGHT){
				snake.posY = 0;
			}
			else if (snake.posY < 0) {
				snake.posY = GAMEHEIGHT;
			}			
			//Collision
			if (collisionCheck()){		
				//Spawn a new apple to eat
				bodyParts++;
				spawnApple();
			}
		}
		
		//Render everything
		function render() {
			gameContext.drawImage(bgImage,0,0);
			gameContext.fillRect(snake.posX-SNAKESIZE/2,snake.posY-SNAKESIZE/2,SNAKESIZE,SNAKESIZE);
			gameContext.fillRect(apple.posX-APPLESIZE/2,apple.posY-APPLESIZE/2,APPLESIZE,APPLESIZE);			
			gameContext.fillStyle = "rgb(250, 250, 250)";
			gameContext.font = "24px Helvetica";
			gameContext.textAlign = "left";
			gameContext.textBaseline = "top";
			gameContext.fillText("Apples Eaten: " + bodyParts, 32, 32);
		}		
		

		//Below are the methods that are called above
		//main game loop
		function main(){
			var now = Date.now();
			var delta = now - then;
			
			update(delta/1000);
			render();
			
			then = now;
			if (!pause){
				requestAnimationFrame(main);
			}
		}	

		//spawns an Apple
		function spawnApple(){
			apple.posX = getRandomArbitrary(30,570);
			apple.posY = getRandomArbitrary(30,270);
			//This is also a recursive function (in a way). Can be written as a "do while" loop also
			if (collisionCheck()){
				spawnApple();
			}
		}
		
		//spawns random number from min to max
		function getRandomArbitrary(min, max){
			//Math object is very useful for doing mathematics in Javascript (below gets a random number)
			var randomNo = Math.random() * (max - min) + min;
			return randomNo;
		}
		
		//check for Collision
		function collisionCheck(){
			if (Math.abs(snake.posX-apple.posX) <= (SNAKESIZE+APPLESIZE)/2 && Math.abs(snake.posY-apple.posY) <=(SNAKESIZE+APPLESIZE)/2){
				return true;
			}
			else{
				return false;
			}
		}
		//check for distance
		function distanceCheck(object1,object2){
			var distanceX = Math.abs(object1.posX-object2.posX);
			var distanceY = Math.abs(object1.posY-object2.posY);
			if (distanceX <= SNAKESIZE && distanceY <=SNAKESIZE){
				return false;
			}
			else{
				return true;
			}
		}
		
		//pause the game
		function pauseUnpause(){
			if (pause == false){
				pause = true;
				document.getElementById("pause").innerHTML = "Play";
			}
			else{
				pause = false;
				then = Date.now();
				document.getElementById("pause").innerHTML = "Pause";
				main();
			}
		}		
	</script>
</body>
</html>
