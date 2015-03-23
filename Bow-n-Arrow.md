	number screenWidth = 500
	number screenHeight = 240

	string keyUp = "up"
	string keyDown = "down"
	string keyLeft = "left"
	string keyRight = "right"
	string keySpace = "space"

	number score = 0
	array clrWhite = [255/255,255/255,255/255]
	array clrGreen = [0/255,255/255,0/255]
	array clrBlack = [0/255,0/255,0/255]
	array clrBrown = [255/255, 125/255, 0/255]
	array clrGrey = [160/255, 160/255, 160/255]

	number gt = Time()

	number playerStarPosX = screenWidth / 2
	number playerStarPosY = screenHeight - 20
	number playerX = playerStarPosX
	number playerY = playerStarPosY
	number playerSpeed = 250
	number playerSize = 50

	number targetStarPosX = 50
	number targetStarPosY = 50
	number targetSpeedX = 80
	number targetSpeedY = 80;
	number targetWidth = 20
	number targetHeight = 20
	number targetMoveRange = 50
	number targetWaveSize = 50
	number TargetX = targetStarPosX;
	number TargetY = targetStarPosY;

	number printTextCooldown = 2
	number currTextCooldown = 0

	number yScale = 0.5
	number xScale = 1

	bool isKeyUpPressed = false
	bool isKeyDownPressed = false
	bool isKeyLeftPressed = false
	bool isKeyRightPressed = false
	bool isKeySpacePressed = false

	number ArrowStartPosY = playerY - 15
	number ArrowX = playerX
	number ArrowY = ArrowStartPosY
	number ArrowSpeed = 150
	number ArrowWidth = 5
	number arrowHeadSize = 10
	number ArrowHeight = 60
	number ArrowCooldown = 1
	number currArrowCooldown = 0

	array deadArrowsPos = []

	number beginingGameOverTime = 10
	number gameOverTime = beginingGameOverTime
	bool shotIsFired = false

	string printMe = ""

	ClearText()
		
	var startTime = Time()
	number TimeSinceStart()
			return Time() - startTime 
	end

	# Game loop
	loop
		number dt = Time() - gt
		gt = Time()

		if(gameOverTime - TimeSinceStart() > 0)
			ReadInput()
			HandlePlayerInput(dt)
			UpdateTarget(dt)
			if(shotIsFired)
				UpdatePlayerArrow(dt)
			end
			CheckArrowTargetCollision()
			DrawMain()
		else # Game over
			ClearText()
			Print("Your score: " + score)
			Print("Press SPACE to play again")
			ReadInput()
			if(isKeySpacePressed)
				ResetGame()
			end
		end

		printMe = ""
	end

	void ResetGame()
		startTime = Time()
		score = 0
		deadArrowsPos = []
		TargetX = targetStarPosX
		TargetY = targetStarPosY
		targetSpeedX = Abs(targetSpeedX)
		targetSpeedY = Abs(targetSpeedY)
		currArrowCooldown = 0
		currTextCooldown = 0
		gameOverTime = beginingGameOverTime
		shotIsFired = false
		playerX = playerStarPosX
		playerY	= playerStarPosY
	end

	void DrawMain()
		# Target
		DrawObject(TargetX, TargetY, targetWidth, targetHeight, clrGreen)

		# Player Arrow
		DrawObject(ArrowX, ArrowY, ArrowWidth, ArrowHeight - arrowHeadSize, clrBrown) 
		DrawObject(ArrowX - arrowHeadSize/4, ArrowY, arrowHeadSize, arrowHeadSize, clrGrey)
		
		# Dead arrows
		loop pos in deadArrowsPos
			DrawObject(pos[0], pos[1], ArrowWidth, ArrowHeight - arrowHeadSize, clrBrown) 
		end
		
		DrawBow()
		ClearText()
		Print("Score: " + score)
		Print("Time: " + gameOverTime - TimeSinceStart())
		PrintForAWhile("+2 sec")
		Print(printMe)
		DisplayGraphics()
	end

	void DrawBow()
		Color(clrBrown[0], clrBrown[1], clrBrown[2])
		# Angles to the right
		Line(playerX + 30, playerY, playerX  + 60, playerY + 15)

		# Angles to the left
		Line(playerX - 30, playerY, playerX - 60, playerY + 15)

		# Straight line
		Line(playerX - 30, playerY, playerX + 30, playerY)

		Color(clrWhite[0], clrWhite[1], clrWhite[2])
		Line(playerX - 60, playerY + 15 playerX + 60, playerY + 15)
	end

	void PrintForAWhile(string text)
		if(currTextCooldown > TimeSinceStart())
			Print(text)
		end
	end

	void CheckArrowTargetCollision()
		if(Collided(ArrowX, ArrowY, ArrowWidth, ArrowHeight, TargetX, TargetY, targetWidth, targetHeight))
			shotIsFired = false
			ResetArrowPos()
			gameOverTime += 2
			score += 10
			currTextCooldown = TimeSinceStart() + printTextCooldown	
			PlaySound("Powerup 5")
		end
	end

	bool Collided(number rect1X, number rect1Y, number rect1Width, number rect1Height, number rect2X, number rect2Y, number rect2Width, number rect2Height)
		if (rect1X < rect2X + rect2Width * xScale && rect1X + rect1Width * xScale > rect2X && rect1Y < rect2Y + rect2Height * yScale && rect1Height * yScale + rect1Y > rect2Y)
			# Collision detected!
			return true
		end
		return false
	end

	void UpdatePlayerArrow(number dt)
		ArrowY -= ArrowSpeed * dt

		# Checks if the arrow has left the screen
		if(ArrowY <= 0)
			shotIsFired = false
			deadArrowsPos[Count(deadArrowsPos)] = [ArrowX, ArrowY]
			PlaySound("Hit 1")
			ResetArrowPos()
		end
	end

	void ResetArrowPos()
		ArrowX = playerX
		ArrowY = ArrowStartPosY
	end

	void UpdateTarget(number dt)
		TargetX +=  targetSpeedX * dt
		TargetY += ((targetWaveSize * Sin(Time())) * yScale) * dt

		# Checks if the Target collides with any walls
		if(TargetX < 0 + targetWidth)
			targetSpeedX = -targetSpeedX
		end
		if(TargetX > screenWidth - targetWidth)
			targetSpeedX = -targetSpeedX		
		end

	end

	void ReadInput()

		if(IsKeyPressed(keyUp))
			isKeyUpPressed = true
		else
			isKeyUpPressed = false	
		end
	    if(IsKeyPressed(keyDown))
			isKeyDownPressed = true
		else
			isKeyDownPressed = false
	    end
	    if(IsKeyPressed(keyLeft))
			isKeyLeftPressed = true
			
		else
			isKeyLeftPressed = false
		end
		if(IsKeyPressed(keyRight))
			isKeyRightPressed = true
		else
			isKeyRightPressed = false	
		end
		if(IsKeyPressed(keySpace))
			isKeySpacePressed = true
		else
			isKeySpacePressed = false
		end
		 # No key pressed...
		if(isKeyUpPressed == false && isKeyDownPressed == false && isKeyLeftPressed == false && isKeyRightPressed == false)
			isKeyUpPressed = false
	 		isKeyDownPressed = false
	 		isKeyLeftPressed = false
			isKeyRightPressed = false
		end
	end

	void DrawObject(number x, number y, number width, number height, array colorRgb)
	   Color(colorRgb[0], colorRgb[1], colorRgb[2])
	   RectWithScale(x, y, width, height)
	end

	void HandlePlayerInput(number dt)   
	    if(isKeyLeftPressed)
	        playerX -= playerSpeed * dt
	          if(shotIsFired == false)
		        ArrowX = playerX
		        ArrowY = ArrowStartPosY
	        end
	    end
	    if(isKeyRightPressed)
	        playerX += playerSpeed * dt
	        if(shotIsFired == false)
		        ArrowX = playerX
		        ArrowY = ArrowStartPosY
	        end
	    end
	    if(isKeySpacePressed)

			if(shotIsFired == false && currArrowCooldown < TimeSinceStart())
				InstantiateArrow()
				currArrowCooldown = TimeSinceStart() + ArrowCooldown
				PlaySound("Powerup 4")
			end
	    end
	end

	void InstantiateArrow()
		ArrowX = playerX
		ArrowY = playerY
		shotIsFired = true
	end

	void RectWithScale(number x, number y, number width, number height)
		# Includes screen size
		Rect(x, y, x + (width * xScale), y + (height * yScale))
	end

	void Abs(number n)
		if(n < 0)
			return -n
		end
		return n
	end
