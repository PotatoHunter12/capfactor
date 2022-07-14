<!DOCTYPE html>
<html lang="en">
    <head>
        <title>Snake game</title>
        <meta charset="UTF-8">
		<meta name="opis" content="snake game">
		<meta name="ključnebesede" content=" HTML, javascript, snake ">
		<meta name="avtor" content="Ambrož Perovšek" />
        <style>
            html, body {
                height: 96%;
                margin: 0;
            }

            body {
                background: black;
                display: flex;
                align-items: center;
                justify-content: center;
            }
            canvas {
                margin-top: 10pt;
                position: fixed;

            }
        </style>
    </head>
    <body>
        <canvas  id="game" width="900" height="900"></canvas>
        <script>
            var canvas = document.getElementById('game');
            var context = canvas.getContext('2d');

            var grid = 16;
            var count = 0;
            var pts = 0;
            var hi = 69;
            
            var snake = {
                x: 160, y: 160, 
                dx: grid, dy: 0, 
                cells: [], 
                maxCells: 4
            };
            var apple = {
                x: 320,
                y: 320
            };

            

            function getRandomInt(min, max) {
                return Math.floor(Math.random() * (max - min)) + min;
            }

            function kvadrat(){

                context.beginPath();
                context.lineWidth="4";
                context.strokeStyle="white";
                context.rect(93,93,710,710);
                context.stroke();
            }

            function loop() {
                requestAnimationFrame(loop);

                
                if (++count < 4) {
                    return;
                }

                count = 0;
                context.clearRect(0,0,900,900);

                context.font = "30px Arial";
                context.fillStyle="white";
                context.fillText("Score: "+pts, 90, 50);

                context.font = "30px Arial";
                context.fillStyle="white";
                context.fillText("Highscore: "+hi, 640, 50);
                
                snake.x += snake.dx;
                snake.y += snake.dy;

                if (snake.x < 96) {
                    snake.x = 800 - grid;
                }
                else if (snake.x >= 800) {
                    snake.x = 96;
                }
                
                if (snake.y < 96) {
                    snake.y = 800 - grid;
                }
                else if (snake.y >= 800) {
                    snake.y = 96;
                }
                kvadrat();
                snake.cells.unshift({x: snake.x, y: snake.y});

                if (snake.cells.length > snake.maxCells) {
                    snake.cells.pop();
                }
                
                context.fillStyle = 'maroon';
                context.fillRect(apple.x+2, apple.y+2, grid-4, grid-4);

                context.fillStyle = 'white';
                snake.cells.forEach(function(cell, index) {

                    context.fillRect(cell.x, cell.y, grid, grid);  

                    if (cell.x === apple.x && cell.y === apple.y) {
                        snake.maxCells++;
                        pts++;
                        if (hi < pts){
                            hi = pts;
                            }
                        
                        apple.x = getRandomInt(0, 44) * grid + 96;
                        apple.y = getRandomInt(0, 44) * grid + 96;
                    }

                    for (var i = index + 1; i < snake.cells.length; i++) 
                    {      
                        if (cell.x === snake.cells[i].x && cell.y === snake.cells[i].y) 
                        {
                            snake.x = 160;
                            snake.y = 160;
                            snake.cells = [];
                            snake.maxCells = 4;
                            snake.dx = grid;
                            snake.dy = 0;

                            pts = 0;


                            apple.x = getRandomInt(0, 44) * grid + 96;
                            apple.y = getRandomInt(0, 44) * grid + 96;
                        }
                    }
                });
            }
            document.addEventListener('keydown', function(e) 
            {
                if (e.which === 37 && snake.dx === 0) {
                    setTimeout(() => {  
                        snake.dx = -grid;
                        snake.dy = 0;
                    }, 65);
                    
                }
                
                else if (e.which === 38 && snake.dy === 0) {
                    setTimeout(() => {  
                        snake.dy = -grid;
                        snake.dx = 0;
                    }, 65);
                    
                }
                
                else if (e.which === 39 && snake.dx === 0) {
                    setTimeout(() => {  
                        snake.dx = grid;
                        snake.dy = 0;
                    }, 65);
                }
                
                else if (e.which === 40 && snake.dy === 0) {
                    setTimeout(() => {  
                        snake.dy = grid;
                        snake.dx = 0;
                    }, 65);
                }
            });
            
            requestAnimationFrame(loop);
        </script>
    </body>
</html>