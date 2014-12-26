```
#Traffic v.0.2
#KNOWN BUGS
# - Cars spawn in collision route and out of the road sometimes

#screen
number screenWidth = 512
number screenHeight = 256

#game
number gameScreenW = 260
number gameScreenH = screenHeight
number gameScreenX = (screenWidth / 2) - (gameScreenW / 2)
number gameScreenY = 25
number gameSpeed = 0.03
number gameLevel = 10

#Player
number playerWidth = 30
number playerHeight = 30
number playerSpeed = 5
array playerInit = [gameScreenX + (gameScreenW / 2) - (playerWidth / 2), 160]
array playerPos = [playerInit[0], playerInit[1]]
bool playerCollided = false

#Player Collisions
number colScrnR = gameScreenX + gameScreenW - (playerWidth / 2)
number colScrnL = gameScreenX - (playerWidth / 2)

#Obstacles
bool obstacleOnScreen = false
array obstacles = []
array obstaclesIxs = GetIndexes(obstacles)
number obstacleW = 30
number obstacleH = 30
number shouldCreateObstacle
number laneLeft = gameScreenX + (gameScreenW / 4) - (obstacleW / 2)
number laneRight = gameScreenX + (gameScreenW / 2)  + obstacleW
number oVariation = (gameScreenW / 8)

#bg
number scenarioH = gameScreenH / 6
number scenarioW = gameScreenW / 5
number scenarioX = 0
number scenarioY = gameScreenY - scenarioH

#init
ClearText()
DisplayGraphics()
number score = 0
number lives = 3
number highscore = 1000

#functions
var debug = ""
string debugMsg = ""

string Debug(array a)
	debug = a[0]
	debugMsg = a[1]
end

void updateScore(number add)
    score += add
    ClearText()
    Print("Score: " + score + "        Hi: " + highscore + "      Lives: " + lives)
    #Print("pX: " + playerPos[0] + "   pY: " + playerPos[1])
    #Print(shouldCreateObstacle)
    Print(debugMsg + debug)
end

void createBG()
    number grey = 0.2
    Color(0, 0.5, 0.1)
    Rect(0, 0, screenWidth, screenHeight) #grass
    Color(grey, grey, grey)
    Rect(gameScreenX, gameScreenY, gameScreenX + gameScreenW, gameScreenY + gameScreenH) #street
    Color(0.8, 0.8, 0.8)
    number middleGS = gameScreenX + (gameScreenW / 2)
    Rect( (middleGS - 6), gameScreenY, (middleGS - 2), screenHeight) #lane
    Rect( (middleGS + 2), gameScreenY, (middleGS + 6), screenHeight) #lane
    
    if (scenarioY > gameScreenH)
    	scenarioY = -scenarioH
    end
    scenarioY += 10
    Color(0, 0.3, 0.1)
    Rect(scenarioX, scenarioY, scenarioX + scenarioW, scenarioY + scenarioH)
    Rect(screenWidth - scenarioW, scenarioY + 40, screenWidth, scenarioY + 60 + scenarioH)
end

void drawPlayer(number x,number y)
    Color(0.8, 0, 0)
    Rect(x, y, x + playerWidth, y + playerHeight)
    Color(0.2,0.2,0.8)
    Rect(x + 2, y + (playerHeight / 4), x + playerWidth - 2, y + 10)
    Color(0.1, 0.1, 0.1)
    Rect(x + 2, y + (playerHeight / 4) + 2, x + playerWidth - 2, y + (playerHeight / 4) + 15)
    Color(0.5, 0.5, 0)
    Rect(x + 5, y + (playerHeight / 4) + 3, x + 10, y + (playerHeight / 4) + 7)
end

void drawObstacle(number x, number y, array color, number orientation)
    Color(color[0], color[1], color[2])
    Rect(x, y, x + obstacleW, y + obstacleH)
    Color(1, 1, 1)
    if (orientation == 1)
    	Rect(x + 2, y + (obstacleH / 4), x + obstacleW - 2, y + 10)
    else
    	Rect(x + 2, y + obstacleH - (obstacleH / 4), x + obstacleW - 2, y + obstacleH - 10)
    end

    #Collision with player
    #Debug([playerPos, "playerPos"])
	if (x <= playerPos[0] + playerWidth && x + obstacleW >= playerPos[0])
		#Debug([playerPos, "Collison Possible!"])
		if (y <= playerPos[1] && y + obstacleH >= playerPos[1])
			#Debug([playerPos, "Collison Detected!"])
			PlaySound("Hit 1")
			playerCollided = true
		else if (y <= playerPos[1] + playerHeight && y + obstacleH >= playerPos[1] + playerHeight)
			PlaySound("Hit 3")
			#Debug([playerPos, "Collison Detected!"])
			playerCollided = true
		end
	end
end

void drawObstacles()
	
	#o[0] = x
	#o[1] = y
	#o[2] = velocity
	#o[3] = [color1, color2, color3]
	#o[4] = orientation

    obstaclesIxs = GetIndexes(obstacles)
    
    loop i in obstaclesIxs
	    array o = obstacles[i]
        if (o[1] >= screenHeight || o[1] < (gameScreenY - obstacleH))
            Remove(obstacles, i)
            #Sleep(0.1)
            PlaySound("Jump 3")
            updateScore(gameLevel)
            break
        end
        o[1] = o[1] + o[2]
        drawObstacle(o[0], o[1], o[3], o[4])
	end
end

void removeObstacles()
    obstaclesIxs = GetIndexes(obstacles)
    loop i in obstaclesIxs
	    Remove(obstacles, i)
	end
end

void createObstacle()
    obstacleOnScreen = true
    number lane = Round(Random() * 6)
    number xAppear = 0
    number yAppear = gameScreenY - (obstacleH / 2)
    number obsVel = 1
    number obsOr = 0 #down

    if (lane == 0)
    	xAppear = laneRight
    	obsVel = 2
    	obsOr = 1
    else if (lane == 1)
    	xAppear = laneLeft
    	obsVel = 6
    	obsOr = 0
    else if (lane == 2)
    	xAppear = laneRight
    	obsVel = 2
    	obsOr = 1
    else if (lane == 3)
    	PlaySound("Coin 4")
    	xAppear = laneRight + (gameScreenW / 8)
    	yAppear = gameScreenH - obstacleH
    	obsVel = -2
    	obsOr = 1
    else if (lane == 4)
    	xAppear = laneLeft
    	obsVel = 4
    	obsOr = 0
    else if (lane == 5)
    	xAppear = laneLeft
    	obsVel = 8
    	obsOr = 0
    else if (lane == 6)
    	xAppear = gameScreenX + (gameScreenW / 2) - (obstacleW / 2)
    	obsVel = 10
    	obsOr = 0
    end

    xAppear += Round(Random() * oVariation)
    xAppear -= Round(Random() * oVariation)


	obstaclesIxs = GetIndexes(obstacles)
	loop i in obstaclesIxs
		array o = obstacles[i]
		loop
			Debug([xAppear + " - " + xAppear + obstacleW, "xAppear: "])
			if (xAppear + obstacleW >= o[0] && xAppear <= (o[0] + obstacleW) )
			  xAppear += Round(Random() * oVariation)
			  xAppear -= Round(Random() * oVariation)
			else
				break
			end
		end
	end

    Append(obstacles, [xAppear, yAppear, obsVel, [Random(), 0.1, Random()], obsOr])
end

void Msg(string msg)
    loop i from 0 to 25 - (Count(msg) / 2)
        PrintS(" ")
    end
    Print(msg)
end

void loseLife()
	lives -= 1
	if (lives < 0)
		youLose()
		break
	end
	removeObstacles()
	playerPos[0] = playerInit[0]
	Sleep(0.5)
	PlaySound("Blip 1")
	Sleep(3)
	playerCollided = false
end

void youLose()
    Sleep(1)
    DisplayGraphics()
    loop i from 0 to 50
        PrintS("=")
    end
    Print("")
    Print("")
    Print("")
    Print("")
    Print("")
    Print("")
    Print("")
    Msg("GAME OVER")
    Msg("Your Score: " + score)
    Sleep(1)
    if (score > highscore)
        PlaySound("Win")
        Msg("CONGRATULATIONS! NEW HIGHSCORE!")
    else
        PlaySound("Lose")
        Msg("Highscore: " + highscore)
    end
end

#init
ClearText()
DisplayGraphics()
Print("")
Print("")
Print("")
Print("")
Print("")
Print("")
Print("")
Msg("Traffic! v.0.2 by jackmcmorrow")
Sleep(2)
updateScore(0)
createBG()
Color(0, 0, 0)
Rect(0, 0, screenWidth, gameScreenY)
DisplayGraphics()
createObstacle()
Print("")
Print("")
Print("")
Print("")
Print("")
Print("")
Print("")
Msg("Ready!")
Sleep(0.5)
Msg(" Set!")
Sleep(0.5)
Msg(" GO!")
Sleep(0.5)

#main loop
loop
    if (IsKeyPressed("left"))
        playerPos[0] = playerPos[0] - playerSpeed
    end
    
    if (IsKeyPressed("right"))
        playerPos[0] = playerPos[0] + playerSpeed
    end

    if (IsKeyPressed("up") && playerPos[1] > gameScreenY)
        playerPos[1] = playerPos[1] - playerSpeed
    else if (playerPos[1] < playerInit[1])
    	playerPos[1] = playerPos[1] + (playerSpeed / 2)
    end

    if (IsKeyPressed("down") && playerPos[1] < (gameScreenH - playerHeight))
        playerPos[1] = playerPos[1] + (playerSpeed / 2)
    else if (playerPos[1] > playerInit[1])
    	playerPos[1] = playerPos[1] - (playerSpeed / 2)
    end
    
    if (playerPos[0] < colScrnL || playerPos[0] > colScrnR)
    	playerPos[0] = playerInit[0]
        PlaySound("Hit 2")
        loseLife()
    end
    
    createBG()
    drawPlayer(playerPos[0], playerPos[1])
    drawObstacles()
    Color(0,0,0)
    Rect(0, 0, screenWidth, gameScreenY)
    
    shouldCreateObstacle = Random()
    if (shouldCreateObstacle >= 0.95 && Count(obstacles) < gameLevel)
        createObstacle()
    end
    
    updateScore(0)
    DisplayGraphics()
    Sleep(gameSpeed)

    if (playerCollided)
    	loseLife()
    end
end
```