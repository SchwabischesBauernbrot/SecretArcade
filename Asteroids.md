```
ClearText()
DisplayGraphics()
Print("")
Print("")
Print("")
Print("")
Print("")
Print("")
Print("")
Print("")
Print("")
Print("")
Print("                      Asteroids by drZool")
Print("")
Print("                          Version 0.4")

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
            @["x"] = @["x"] + @["velx"] * dt * 30
            @["y"] = @["y"] + @["vely"] * dt * 30

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
            bullet = []
        else
            CollisionDetectBullet()
        end
    end

    CollisionDetectShip()

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
        if( HasIndex(@, "type") )
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

void CollisionDetectShip()
    #test collide bullet with asteroids
    loop objects
        if( HasIndex(@, "type") )
            if(@["type"] == "asteroid")
                number radiusSquared = @["radius"] * @["radius"]
                radiusSquared += ship["radius"] * ship["radius"]
                number distanceSquared = GetSquaredDistance(ship, @)
                if( distanceSquared <= radiusSquared)
                    ExplodeShip()
                    return
                end
            end
        end
    end
end

void ExplodeShip()
  PlaySound("Explosion 2")
  lives --

  if( lives <= 0)
    ClearText()
    Print("")
    Print("")
    Print("")
    Print("")
    Print("")
    Print("")
    Print("")
    Print("")
    Print("")
    Print("")
    Print("                         Game Over")
    Print("")
    Print("                          Score "+ score)
    Sleep(10)

    lives = 3
    level = 1
    currentlevel = 0
  else
    currentlevel -- # restart level
  end
end

void ExplodeAsteroid(array asteroid)
    PlaySound("Explosion 1")
    score ++

    if( asteroid["size"] > 1 )
        loop from 1 to 2
            var theta = asteroid["velangle"] + RandomRange(-2,2)
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

void RemoveBullet()
    bullet = []
    if( HasIndex(objects, "bullet"))
      Remove( objects, "bullet")
    end
end

void RemoveObject(array object)
    Remove( objects, object["name"])
end

number GetSquaredDistance(array a, array b)
    number dx = a["x"] - b["x"]
    number dy = a["y"] - b["y"]
    return dx * dx + dy * dy
end

void Render()

    ClearText()
    Print("Level: "+ currentlevel + " Lives: " + lives + " Score: " + score)

    loop objects
        if Count(@) != 0
            array lines = @["lines"]
            if( @["rot"] != 0)
                number rot = Round (@["rot"]*100)
                if( @["cachedrot"] == rot)
                  lines = @["cachedlines"]
                else
                  lines = RotatePoints(lines, @["rot"])
                  @["cachedrot"] = rot
                  @["cachedlines"] = lines
                end
            end
            DrawLines(lines, @["x"], @["y"], true)
        end
    end

    DisplayGraphics()

end

void DrawLines(array lines, number x, number y, bool close)
  array newLines = []
  number linesCount = Round(Count(lines)/2)
  if( linesCount <= 0)
    return
  end

  loop i from 0 to (linesCount - 1)
    Append(newLines, (lines[i*2]+x))
    Append(newLines, (lines[i*2+1]+y))
  end
  if(close)
    Append(newLines,newLines[0])
    Append(newLines,newLines[1])
  end
  Lines(newLines)
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
    obj["cachedrot"] = 0
    obj["cachedlines"] = []
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

    ship = CreateObject("ship", (warp_left + warp_right) / 2, (warp_top + warp_bottom) / 2, [-10, -6, 10, 0, -10, 6, -7, 0], "ship")
    ship["radius"] = 8

    loop from 1 to level
        CreateAsteroid(3, RandomRange(warp_left,warp_right), RandomRange(warp_top,warp_bottom), RandomRange(0, 3.14*2) )
    end

    DisplayGraphics()
end

void CreateAsteroid(number size, number x, number y, number theta)
    
    asteroidId++
    asteroidCount++

    var speed = AngleToVector(theta, RandomRange(1.2,1.4) - (3 / size))

    array lines = CreateCircleOfLines(4 + size*2, 10 + size)
    MutateLines(lines, size + 2)

    var asteroid = CreateObject("asteroid" + asteroidId, x, y, lines , "asteroid")
    asteroid["radius"] = 10

    asteroid["velangle"] = theta
    asteroid["velx"] = speed[0]
    asteroid["vely"] = speed[1]
    asteroid["size"] = size

    return asteroid
end

void MutateLines(array lines, number range)
  loop i from 0 to Count(lines) - 1
    lines[i] = lines[i] + RandomRange(-range, range)
  end
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

array RotatePoints(array lines, number theta)

    number linesCount = Count(lines)
    number halfLinesCount = Round(linesCount / 2) - 1

    array newLines = []

    loop istep from 0 to halfLinesCount
        number i = istep*2
        number px = lines[i]
        number py = lines[i + 1]

        Append(newLines, Cos(theta) * px - Sin(theta) * py )
        Append(newLines, Sin(theta) * px + Cos(theta) * py )
    end

    return newLines
end

number RandomRange(number low, number high)
    return low + (Random() * (high - low))
end

```