```
ClearText()
DisplayGraphics()
Print("")
Print("")
Print("")
Print("")
Print("         Asteroids 0.1 by drZool")

Sleep(2)

var lastT = Time()
number score = 0
number lives = 3
number level = 1
number currentlevel = 0
number asteroidId = 0
number asteroidCount = 0

number ship_thrust = 0
bool ship_fire = false
array ship = []
array objects = []
array bullet = []

number warp_left = -5
number warp_right = 510
number warp_top = -5
number warp_bottom = 260
number warp_width = warp_right - warp_left
number warp_height = warp_bottom - warp_top

array frame = [warp_left,warp_top,warp_right,warp_top,warp_right,warp_bottom,warp_left,warp_bottom]
array asteroids = []

loop
	number dt = Time() - lastT
	lastT = Time()
	UpdateGame(dt)
	Render()
	DisplayGraphics()
end

void UpdateGame(number dt)

	if( currentlevel != level)
		InitLevel(level)
	end

	OnInput(dt)

	var xrot = Cos(ship["rot"])
	var yrot = Sin(ship["rot"])

	ship["velx"] = ship["velx"] + (xrot * ship_thrust * dt)
	ship["vely"] = ship["vely"] + (yrot * ship_thrust * dt)

	loop objects
		if( Count(@) > 0)
			@["x"] = @["x"] + @["velx"]
			@["y"] = @["y"] + @["vely"]

			if( @["x"] < warp_left)
				@["x"] = @["x"] + warp_width
			else if ( @["x"] > warp_right)
				@["x"] = @["x"] - warp_width
			end

			if( @["y"] < warp_top)
				@["y"] = @["y"] + warp_height
			else if ( @["y"] > warp_bottom )
				@["y"] = @["y"] - warp_height
			end
		end
	end

	if( Count(bullet) == 0 )
		if( ship_fire )
			Fire()
		end
	else
		if( bullet["dietime"] < Time())
			RemoveBullet()
		else
			CollisionDetectBullet()
		end
	end

	Render()

end

void Fire()
	bullet = CreateObject("bullet", ship["x"] , ship["y"], [-5,0,5,0], "bullet" )
	bullet["rot"] = ship["rot"]
	bullet["velx"] = Cos(ship["rot"]) * 3
	bullet["vely"] = Sin(ship["rot"]) * 3
	bullet["dietime"] = Time() + 3
	PlaySound("Laser 1")
end

void CollisionDetectBullet()
	#test collide bullet with asteroids
	loop objects
		if( Count(@) > 0 )
			if(@["type"] == "asteroid")
				number radiusSquared = @["radius"] * @["radius"]
				number distanceSquared = GetSquaredDistance(bullet, @)
				if( distanceSquared <= radiusSquared)
					ExplodeAsteroid(@)
					RemoveBullet()
					return
				end
			end
		end
	end
end

void ExplodeAsteroid(array asteroid)
	PlaySound("Explosion 1")
	score ++

	if( asteroid["size"] > 1 )
		loop from 1 to 2
			var theta = asteroid["velangle"] + RandomRange(-1,1)
			CreateAsteroid( asteroid["size"] - 1, asteroid["x"] , asteroid["y"] , theta)
		end
	end

	asteroidCount--
	RemoveObject(asteroid)

	if( asteroidCount <= 0)
		level ++
	end
end

array AngleToVector(number theta, number length)
	return [Cos(theta) * length, Sin(theta) * length]
end

void RemoveObject(array object)
	string name = object["name"]
	object = []
	objects[name] = object
	# Remove( objects, name) #todo 
end

void RemoveBullet()
	bullet = []
	objects["bullet"] = bullet
end

number GetSquaredDistance(array a, array b)
	number dx = a["x"] - b["x"]
	number dy = a["y"] - b["y"]
	return dx * dx + dy * dy
end

void Render()

	ClearText()	
	Print("Level: "+ currentlevel + " Score: " + score)

	loop objects
		if Count(@) != 0
			array lines = @["lines"]
			if( @["rot"] != 0)
				lines = RotatePoints(lines, @["rot"])	# todo lookup rotations?	
			end
			Lines(lines, @["x"], @["y"], true)
		end
	end

	DisplayGraphics()
    
end

void OnInput(number dt)
	number turnspeed = 1.5

	if(IsKeyPressed("left"))
		ship["rot"] = ship["rot"] - dt * turnspeed
	else if(IsKeyPressed("right"))
		ship["rot"] = ship["rot"] + dt * turnspeed
	end

	if(IsKeyPressed("up"))
		ship_thrust = 1
	else if(IsKeyPressed("down"))
		ship_thrust = -1
	else
		ship_thrust = 0
	end

	ship_fire = (IsKeyPressed("space"))
end

array CreateObject(string name, number x, number y, array lines, string type )

	array obj = []
	obj["name"] = name
	obj["x"] = x
	obj["y"] = y
	obj["velx"] = 0
	obj["vely"] = 0
	obj["lines"] = lines
	obj["rot"] = 0
	obj["type"] = type

	objects[name] = obj

	return obj
end

void InitLevel(number level)
	currentlevel = level

	if( level > 1)
		PlaySound("Powerup 1")
	end

	objects = []
	asteroidCount = 0
	# CreateObject("warpFrame", 0, 0, frame, "debug")

	ship = CreateObject("ship", (warp_left + warp_right) / 2, (warp_top + warp_bottom) / 2, [-10, -6, 10, 0, -10, 6], "ship")
	ship["radius"] = 8

	loop from 1 to level
		CreateAsteroid(3, RandomRange(warp_left,warp_right), RandomRange(warp_top,warp_bottom), RandomRange(0, 3.14*2) )
	end

	DisplayGraphics()
end

void CreateAsteroid(number size, number x, number y, number theta)

	asteroidId++
	asteroidCount++

	var speed = AngleToVector(theta, RandomRange(0.25,0.5))

	var asteroid = CreateObject("asteroid" + asteroidId, x, y, CreateCircleOfLines(6 + size, 10 + size) , "asteroid")
	asteroid["radius"] = 10

	asteroid["velangle"] = theta
	asteroid["velx"] = speed[0]
	asteroid["vely"] = speed[1]
	asteroid["size"] = size

	return asteroid
end

array CreateCircleOfLines( number radius, number points)
	array newLines = []
	number halfPoints = points / 2
	number anglePerStep = (3.14*4)/points
	loop i from 0 to halfPoints - 1
		number theta = anglePerStep * i
		newLines[i*2] = Cos(theta) * radius
		newLines[i*2 + 1] = Sin(theta) * radius
	end
	return newLines
end

void Lines(array lines, number x, number y, bool close)
	number linesCount = Count(lines)
	number halfLinesCount = Round(linesCount/2) - 2

	loop istep from 0 to halfLinesCount
		number i = istep*2
		Line( x + lines[i], y + lines[i + 1], x + lines[i + 2], y + lines[i + 3])
	end
	
	if( close )
		Line( x + lines[0] , y + lines[1],  x + lines[linesCount - 2], y + lines[linesCount - 1] )
	end
end

array RotatePoints(array lines, number theta)

	number linesCount = Count(lines)
	number halfLinesCount = Round(linesCount / 2) - 1

	array newLines = []

	loop istep from 0 to halfLinesCount
		number i = istep*2
		number px = lines[i]
		number py = lines[i + 1]

		newLines[i] = Cos(theta) * px - Sin(theta) * py
		newLines[i + 1] = Sin(theta) * px + Cos(theta) * py 
	end
	
	return newLines
end

void DrawSprite(array sprite, number w, number x, number y)
    var i = 0
    var count = Count(sprite)
    loop

	    if( sprite[i] > 0 )    
	    	var lx = x + Mod(i, w)
	    	var ly = y + (i / w)
	    	Rect(lx, ly, lx+1, ly+1)
	    end
	    
	    i = i + 1
	    if( i >= count)
	        return
	    end
    end
end


number RandomRange(number low, number high)
	return low + (Random() * (high - low))
end

void SetRainbowColor()
	number t = Repeat(Time(), 1)
	var col = HSVtoRGB(t, 1, 1)
	Color(col[0], col[1], col[2])
end
```