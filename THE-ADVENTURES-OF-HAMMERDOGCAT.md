ClearText()

# variables
var sx = 512
var sy = 256
var padding = 10
var px = 256
var py = 128
var flipped = false
var drawBounds = false

var tickCounter = 0
var lastTick = 0
var timeBetweenTicks = 0.1

# cat variables
var boundsColor = [0.3,0.3,0.3]
number catScale
var catBounds
array catSprite
var catSprite0 = [0,20, 10,30, 60,30, 60,20, 70,10, 80,20, 80,40, 60,40, 60,30, 50,30, 50,50, 50,30, 20,30, 20,50]
var catSprite1 = [0,10, 10,30, 60,30, 60,20, 70,10, 80,20, 80,40, 60,40, 60,30, 50,30, 50,50, 50,30, 20,30, 20,50]
var catColor = [1,0,0]
var zombieColor = [0.1,0.9,0]
var moveSpeed = 30
bool isAlive = true
var hunger = 0
var lastFed = Time()
var dieOfHungerSeconds = 10
string lastPlayedSound

#player
number middleX = 18
number middleY = 14
string titlePrefix = "THE ADVENTURES OF "
string titleName = "HAMMERDOGCAT"
string titleMood = ""
string titlePronome = ""
array respawnMoods = ["GRUMPY ", "RAVAGING ", "SAD ", "DEPRESSED ", "GLUM ", "GLOOMY ", "WOEFUL ", "GRIEF-STRICKEN "]
array eatFoodMoods = ["HAPPY ", "CAREFREE ", "ADVENTUROUS ", "FORTUNATE "]
#array respawnMoods = ["GRUMPY ", "RAVAGING ", "SAD ", "DEPRESSED ", "GLUM ", "GLOOMY ", "WOEFUL ", "GRIEF-STRICKEN "]
#array eatFoodMoods = ["HAPPY ", "FULL ", "SATISFIED ", "PLEASED ", "CAREFREE ", "DELIGHTED ", "CONTENT ", "GLAD ", "ADVENTUROUS ", "FORTUNATE ", "WELL FED "]

#rooms
number infoX = 5
number infoY = 4
array roomFOSPs
array currentFOSP
number currentRoom = 0
array beenHere = ["I have already been here.", "I think I have seen this place before.", "The same ol' dusty spot...", "Let's go. Just leave this place forever."]
array newPlace = ["This seems to be a new and fresh place.", "I wonder what the next stop will bring.", "Let's never look back.", "I love going to new places, talking to new people."]

#food
number maxRandomFood = 10
number foodSpriteSize = 15
var foodColor = [0,0.7,0.7]
var foodBounds = [foodSpriteSize, foodSpriteSize]

#hunger bar
number hbx = 10
number hby = 25
var heartPosition = [hbx - 2, hby - 7]
var hbHeartOffset = 40
var hbh = 10
var hungerBarColor = [0,1,0]
var heartColor = [1,0,0]
var heartScale = 0.1
var heartSprite = [15, 10, 20, 5, 25, 10, 30, 5, 35, 10, 25, 20, 15, 10]

#text
array infoTextColor = [0.3,0.3,0.3]
array titleTextColor = [0.1,0.9,0]

loop
    if isAlive
    	AnimateCat()
    	Move()
    	HandleCollisions()
	else
		PrintText(middleX, middleY, titleTextColor, "PRESS SPACE FOR A SECOND CHANCE")
		if IsKeyPressed("space")
			Respawn()
		end
    end
  	PrintTitle()
    Draw()
    IncreaseHunger()
    UpdateTicks()
end

void PrintTitle()
	PrintText(0, 0, titleTextColor, titlePrefix + titleMood + titlePronome + titleName)
end

void PlayOnce(string sound)
	#if lastPlayedSound != sound
		PlaySound(sound)
		lastPlayedSound = sound
	#end
end

void Respawn()
	PlayOnce("Win")
	hunger = 0
	lastFed = Time()
	PrintClearText(middleX, middleY)
	titleMood = respawnMoods[RandomRange(0, Count(respawnMoods))]
	titlePronome = "ZOMBIE "
	catColor = zombieColor
	isAlive = true
end

void HandleCollisions()
	#check cat collisions against food
	number i = Count(currentFOSP) - 1
	loop
		if i < 0
			break
		end

		#collision happened
		if CollidedWithFood(currentFOSP[i], foodBounds)
			currentFOSP[i] = []
			PlayPickupSound()
			hunger = 0
			lastFed = Time()
			titleMood = eatFoodMoods[RandomRange(0, Count(eatFoodMoods))]
		end

		i--
	end
end

# AABB collision testing
bool CollidedWithFood(array pos, array bounds)
	if Count(pos) != 2
		return false
	end

	number scale = GetScale(pos[1])
	number fw = (bounds[0] * scale)
	number fh = (bounds[1] * scale)
	number cw = (catBounds[0] * catScale)
	number ch = (catBounds[1] * catScale)

	return px < pos[0] + fw && px + cw > pos[0] && py < pos[1] + fh && ch + py > pos[1]
end

void Abs(number a)
	if a < 0
		return -a
	else
		return a
	end
end

void PlayPickupSound()
	var rand = Int(Random() * 5)
	if(rand < 1)
		PlayOnce("Coin 1")
	else if(rand < 2)
		PlayOnce("Coin 2")
	else if(rand < 3)
		PlayOnce("Coin 3")
	else
		PlayOnce("Coin 4")
	end
end

void IncreaseHunger()
	hunger = (Time() - lastFed) / dieOfHungerSeconds
	if hunger > 1
		if titleMood != "DEAD "
			PlayOnce("Lose")
		end
		titleMood = "DEAD "
		isAlive = false
	else if hunger > 0.8
		if titleMood != "REALLY HUNGRY "
			PlayOnce("Warning")
		end
		titleMood = "REALLY HUNGRY "
	else if hunger > 0.5
		if titleMood != "HUNGRY "
			PlayOnce("Warning")
		end
		titleMood = "HUNGRY "
	else if hunger > 0.3
		if titleMood != "SLIGHTLY HUNGRY "
			PlayOnce("Warning")
		end
		titleMood = "SLIGHTLY HUNGRY "
	end
end

void UpdateTicks()
	if Time() > lastTick + timeBetweenTicks
		lastTick = Time()
    	tickCounter++
    	#PrintText(0, 7, [1,1,1], "RandomRange: " + RandomRange(0,2))
	end
end

void AnimateCat()
	if Mod(tickCounter,2) == 0
		catSprite = catSprite0
	else
		catSprite = catSprite1
	end
end

number GetScale(number yPosition)
	# scale is relative to y-position
	return yPosition / sy
end

void Move()
	number scale = GetScale(py)
	if(IsKeyPressed("left"))
		px = px - moveSpeed * scale
		flipped = true
	end
	if(IsKeyPressed("right"))
		px = px + moveSpeed * scale
		flipped = false
	end
	if(IsKeyPressed("up"))
		py = py - moveSpeed * scale
	end
	if(IsKeyPressed("down"))
		py = py + moveSpeed * scale
	end

	# update cat bounds
	catScale = GetScale(py)
	catBounds = GetBounds(catSprite)

	WrapMove(catBounds)
end

#position is in text grid, one change in position is 8 pixels (?)
void PrintText(number posX, number posY, array color, string text)
	Color(color[0], color[1], color[2])
	Text(posX, posY, text + "                                                                                                               ")
end

void PrintClearText(number posX, number posY)
	Text(posX, posY, "                                                                                                                                                        ")
end

#exclusive
number RandomRange(number min, number max)
	return Int(min + (Random() * (max - min)))
end

void ChangeRoom()
	if HasIndex(roomFOSPs, currentRoom)
		PrintText(infoX, infoY, infoTextColor, beenHere[RandomRange(0, Count(beenHere))])
	else
		PrintText(infoX, infoY, infoTextColor, newPlace[RandomRange(0, Count(newPlace))])
		roomFOSPs[currentRoom] = RandomizeFood()
	end

	currentFOSP = roomFOSPs[currentRoom]
end

void WrapMove(array catBounds)
	if(px < padding)
		currentRoom--
		ChangeRoom()
		px = sx - padding
	end
	if(px > sx - padding)
		currentRoom++
		ChangeRoom()
		px = padding
	end
	if(py < padding)
		py = padding
	end
	var lowerLimit = sy - padding + catBounds[1]
	if(py > lowerLimit)
		py = lowerLimit
	end
end

array RandomizeFood()
	number foodToSpawn = Int(Random() * maxRandomFood)

	number i = 0
	array FOSP

	loop
		if i > foodToSpawn
			break
		end

		var x = Clamp(Int(Random() * sx), padding, sx - padding)
		var y = Clamp(Int(Random() * sy), padding, sx - padding)
		FOSP[i] = [x, y]

		i++
	end

	return FOSP
end

number Clamp(number n, number min, number max)
	if n < min
		return min
	else if n > max
		return max
	else
		return n
	end
end

void Draw()
    DisplayDataAsImage(catSprite, catScale, [px,py], catColor, flipped, catBounds, isAlive == false)

    #draw food
	Color(foodColor[0], foodColor[1], foodColor[2])
    loop i in GetIndexes(currentFOSP)
    	array pos = currentFOSP[i]
		if Count(pos) == 2
			number size = foodSpriteSize * GetScale(pos[1])
			Rect(pos[0],pos[1], pos[0]+size,pos[1]+size)
		end
    end

	#draw hunger bar
	DrawHungerBar()

	#draw bounds
    if drawBounds
   		DisplayDataAsImage(GetBoundsAsRect(catBounds), GetScale(py), [px,py], boundsColor, false, catBounds, false)

   		#draw bounds for the food
   		#loop i in GetIndexes(currentFOSP)
   		#	array fops = currentFOSP[i]
   		#	if Count(fops) == 2
   		#		DisplayDataAsImage(GetBoundsAsRect(foodBounds), GetScale(fops[1]), fops, boundsColor, false, foodBounds, false)
   		#	end
   		#end
    end

	DisplayGraphics()
end

void DrawHungerBar()
	#draw heart
	DisplayDataAsImage(heartSprite, 1, heartPosition, heartColor, false, [0,0], false)

	#draw filled rect
	number filled = 1 - hunger
	var startX = hbx+hbHeartOffset
	var endBarX = (sx-hbx)*filled
	if endBarX < startX
		endBarX = startX
	end
	Color(hungerBarColor[0], hungerBarColor[1], hungerBarColor[2])
	Rect(startX,hby, endBarX,hby+hbh)

	#draw outline
	var barSprite = [startX,hby, sx-hbx,hby, sx-hbx,hby+hbh, startX,hby+hbh, startX,hby]
	DisplayDataAsImage(barSprite, 1, [0,0], hungerBarColor, false, [0,0], false)
end

array GetBoundsAsRect(array bounds)
	return [0,0, bounds[0],0, bounds[0],bounds[1], 0,bounds[1], 0,0]
end

array GetBounds(array data)
	var x = 0
	var y = 0
	number i = 0
    loop
        if i >= Count(data)
            break
        end

        if data[i] > x
        	x = data[i]
        end

        if data[i + 1] > y
        	y = data[i + 1]
        end

        i += 2
    end

    return [x,y]
end

void DisplayDataAsImage(array data, number scale, array position, array color, bool flipped, array bounds, bool upsideDown)
	if Count(position) != 2
		PrintText(middleX, middleY, infoTextColor, "position needs to be exactly 2 elements long")
	end

    if Count(data) < 2
		PrintText(middleX, middleY, infoTextColor, "Not enough data")
    end

    Color(color[0], color[1], color[2])

    var flipMultiplierX = 1
    var flipMultiplierY = 1
    var xOffset = 0
    var yOffset = 0
    if flipped
    	flipMultiplierX = -1
	    xOffset = bounds[0] * scale
    end
    if upsideDown
    	flipMultiplierY = -1
	    yOffset = bounds[1] * scale
    end

    var x = position[0] + (data[0] * scale * flipMultiplierX) + xOffset
	    var y = position[1] + (data[1] * scale * flipMultiplierY) + yOffset
	    
	    number i = 2
	    loop
	        if i >= Count(data)
	            break
	        end

	        var x2 = position[0] + (data[i] * scale * flipMultiplierX) + xOffset
	        var y2 = position[1] + (data[i + 1] * scale * flipMultiplierY) + yOffset

	        Line(x, y, x2, y2)
	        
	        x = x2
	        y = y2
	        
	        i += 2
	    end
end