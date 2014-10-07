```
Print("Get ready!")

PlaySound("Powerup 5")
Sleep(0.3)
PlaySound("Powerup 5")
Sleep(0.3)
PlaySound("Powerup 5")
Sleep(0.3)

var score = 0

var ballX = 20
var ballY = 50

var startSpeed = -200

var ballSpeedX = startSpeed
var ballSpeedY = startSpeed

var paddleSpeed = 300

var paddleX = 250
var paddleY = 220

var paddleSize = 50

var lastT = Time()

loop
	number dt = Time() - lastT
	lastT = Time()

	OnInput(dt)
	UpdateBall(dt)
	ClearText()
	DrawScore()
	SetRainbowColor()
	DrawBall(ballX, ballY)
	DrawPaddle(paddleX, paddleY)
	DisplayGraphics()
end

void OnInput(number dt)
	if(IsKeyPressed("left"))
		paddleX = paddleX - paddleSpeed * dt
	else if(IsKeyPressed("right"))
		paddleX = paddleX + paddleSpeed * dt
	end
end

void DrawScore()
	Print("Score: " + score)
end

void SetRainbowColor()
	number t = Repeat(Time(), 1)
	var col = HSVtoRGB(t, 1, 1)
	Color(col[0], col[1], col[2])
end

void DrawBall(number x, number y)
	var size = 3
	Rect(x - size, y - size, x + size, y + size)
end

void DrawPaddle(number x, number y)
	Line(x, y, x + paddleSize, y)
end

void UpdateBall(number dt)
	ballX = ballX + ballSpeedX * dt
	ballY = ballY + ballSpeedY * dt

	if ballX > 512 and ballSpeedX > 0
		ballX = 512
		ballSpeedX = -ballSpeedX
		PlaySound("Hit 2")
	end
	
	if ballX < 0 and ballSpeedX < 0
		ballX = 0
		ballSpeedX = -ballSpeedX
		PlaySound("Hit 2")
	end

	if WithinPaddleBounds() and ballSpeedY > 0
		# Collide with paddle?
		if ballX > paddleX and ballX < paddleX + paddleSize
			if ballX < paddleX + 15 and ballSpeedX > 0
				# spin
				ballSpeedX *= -1.2
			else if ballX > paddleX + 35 and ballSpeedX < 0
				# spin
				ballSpeedX *= -1.2
			else
				# calm down
				ballSpeedX *= 0.95
			end
			ballY = paddleY
			ballSpeedY = -ballSpeedY
			ballSpeedY *= 1.05
			score++
			PlaySound("Hit 1")
			return
		end
	end

	if ballY > 256 and ballSpeedY > 0
		ballSpeedY = -ballSpeedY
		score = 0 # lose!
		Print("Ouch!")
		PlaySound("Lose")
		Sleep(2)
		ballX = 256
		ballY = 128
		ballSpeedY = startSpeed
		if Random() > 0.5
			ballSpeedX = startSpeed
		else
			ballSpeedX = -startSpeed
		end
	end
	
	if ballY < 0 and ballSpeedY < 0
		ballSpeedY = -ballSpeedY
		PlaySound("Hit 2")
	end
end

bool WithinPaddleBounds()
	return ballY > paddleY - 10 and ballY < paddleY + 5
end
```