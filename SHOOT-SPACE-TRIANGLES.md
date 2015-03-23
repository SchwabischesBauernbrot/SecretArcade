```
ClearText()
DisplayGraphics()

Print("")
Print("")
Print("")
Print("")
Print("")
Print("")
Print("                              SHOOT")
PlaySound("Powerup 1")
Sleep(0.5)
Print("")
Print("")
Print("                              SPACE")
PlaySound("Powerup 2")
Sleep(0.5)
Print("")
Print("")
Print("                            TRIANGLES")
PlaySound("Powerup 3")
Sleep(1)

ClearText()

array stars = []
array enemy = []

number maxStarCount = 8
number starCount = 0
number playerSpeed = 100.0
number spaceWidth = 1000.0
number scale = 100.0
number aspect = 0.5
number laserStartTime = 0
number laserDuration = 0.25
bool laserOn = false
bool spaceKeyDownLast = false
number score = 0
number hitArea = 20.0
bool playExplosion = false
bool playLaser = false
string currentMessage = ""

number totalFrames = 0
number gameStartTime = Time()

CreateStars()
CreateEnemy()
PlayGame()

void PlayGame()
    number T = Time()
    number dt

    ClearText()

    Color(0,0,0)
    Rect(0,0,512,256)


    loop
        dt = Time() - T
        T = Time()

        totalFrames ++

        playExplosion = false
        playLaser = false

        Steer(dt)
        Fire(dt)
        MoveForward(dt)
        EnemyAi(dt)

        CheckHit()

        DrawStars()
        DrawEnemy()
        DrawCrossHair()

        DisplayGraphics()
        if playExplosion
            PlaySound("Explosion 1")
        else if playLaser
            PlaySound("Laser 1")
        end

        # dynamic frame rate star count adjustment
        if dt < 0.04 and maxStarCount < 25
            maxStarCount ++
        else if dt > 0.1 and maxStarCount > 8
            maxStarCount --
        end
    end
end


void CreateStars()
    starCount = 0
    stars = []
    loop x from 1 to 30
        array star = []
        star["x"] = Random() * spaceWidth - spaceWidth * 0.5
        star["y"] = Random() * spaceWidth - spaceWidth * 0.5
        star["z"] = Random() * 500.0 + 100.0
        star["r"] = Random()
        star["g"] = Random()
        star["b"] = Random()

        stars[starCount] = star
        starCount = starCount + 1

    end
end



void CreateEnemy()
    enemy["x"] = Random() * spaceWidth - spaceWidth * 0.5
    enemy["y"] = Random() * spaceWidth - spaceWidth * 0.5
    enemy["z"] = Random() * 500.0 + 100.0
    enemy["lxx"] = 1.0
    enemy["lxy"] = 0.0
    enemy["lyx"] = 0.0
    enemy["lyy"] = 1.0
    enemy["velx"] = 0
    enemy["vely"] = 0
    enemy["velz"] = Random() * 40.0 + 60.0
    enemy["r"] = Random()*0.9 + 0.1
    enemy["g"] = Random()*0.9 + 0.1
    enemy["b"] = Random()*0.9 + 0.1
    enemy["deathTime"] = 0
    enemy["rotSpeed"] = Random()
    enemy["gotAway"] = false
end


void RotateThing(array obj, number cosTurn, number sinTurn, string xAxis, string yAxis)
    number x = obj[xAxis] * cosTurn - obj[yAxis] * sinTurn
    number y = obj[xAxis] * sinTurn + obj[yAxis] * cosTurn
    obj[xAxis] = x
    obj[yAxis] = y
end


void Steer(number dt)
    number turn = 3.14159 * dt / 2
    number cosTurn = Cos(turn)
    number sinTurn = Sin(turn)
    number nSinTurn = 0.0 - sinTurn

    # rotate all
    if IsKeyPressed("left")
        loop i from 0 to maxStarCount-1
            var aStar = stars[i]
            number x = aStar["x"] * cosTurn - aStar["y"] * sinTurn
            number y = aStar["x"] * sinTurn + aStar["y"] * cosTurn
            aStar["x"] = x
            aStar["y"] = y
        end
        RotateThing(enemy, cosTurn, sinTurn, "x", "y")
        RotateThing(enemy, cosTurn, sinTurn, "velx", "vely")
        RotateThing(enemy, cosTurn, sinTurn, "lxx", "lxy")
        RotateThing(enemy, cosTurn, sinTurn, "lyx", "lyy")
    end
    if IsKeyPressed("right")
        loop i from 0 to maxStarCount-1
            var aStar = stars[i]
            number x = aStar["x"] * cosTurn - aStar["y"] * nSinTurn
            number y = aStar["x"] * nSinTurn + aStar["y"] * cosTurn
            aStar["x"] = x
            aStar["y"] = y
        end
        RotateThing(enemy, cosTurn, nSinTurn, "x", "y")
        RotateThing(enemy, cosTurn, nSinTurn, "velx", "vely")
        RotateThing(enemy, cosTurn, nSinTurn, "lxx", "lxy")
        RotateThing(enemy, cosTurn, nSinTurn, "lyx", "lyy")
    end
    if IsKeyPressed("down")
        loop i from 0 to maxStarCount-1
            var aStar = stars[i]
            number z = aStar["z"] * cosTurn - aStar["y"] * sinTurn
            number y = aStar["z"] * sinTurn + aStar["y"] * cosTurn
            aStar["z"] = z
            aStar["y"] = y
        end
        RotateThing(enemy, cosTurn, sinTurn, "z", "y")
        RotateThing(enemy, cosTurn, sinTurn, "velz", "vely")
    end
    if IsKeyPressed("up")
        loop i from 0 to maxStarCount-1
            var aStar = stars[i]
            number z = aStar["z"] * cosTurn - aStar["y"] * nSinTurn
            number y = aStar["z"] * nSinTurn + aStar["y"] * cosTurn
            aStar["z"] = z
            aStar["y"] = y
        end
        RotateThing(enemy, cosTurn, nSinTurn, "z", "y")
        RotateThing(enemy, cosTurn, nSinTurn, "velz", "vely")
    end
end


void Fire(number dt)
    bool spaceKeyDown = IsKeyPressed("space")

    # turn off laser if on
    if laserOn
        if Time() > laserStartTime + laserDuration
            laserOn = false
        end
    else
        if spaceKeyDownLast == false && spaceKeyDown
            laserOn = true
            playLaser = true
        end
    end

    spaceKeyDownLast = spaceKeyDown
end


void MoveForward(number dt)
    number dz = dt * playerSpeed
    loop i from 0 to maxStarCount-1
        var s = stars[i]
        s["z"] = s["z"] - dz
        if s["z"] <= 5.0
            # recycle star
            s["x"] = Random() * spaceWidth - spaceWidth * 0.5
            s["y"] = Random() * spaceWidth - spaceWidth * 0.5
            s["z"] = Random() * 500.0 + 100.0
            s["r"] = Random()
            s["g"] = Random()
            s["b"] = Random()
        end
    end
    enemy["z"] = enemy["z"] - dz
end


void EnemyAi(number dt)
    # fly in circles?
    number cosTurn = Cos(enemy["rotSpeed"] * dt)
    number sinTurn = Sin(enemy["rotSpeed"] * dt)

    RotateThing(enemy, cosTurn, sinTurn, "velx", "velz")

    enemy["x"] = enemy["x"] + enemy["velx"] * dt
    enemy["y"] = enemy["y"] + enemy["vely"] * dt
    enemy["z"] = enemy["z"] + enemy["velz"] * dt

    if enemy["deathTime"] != 0 && Time() > enemy["deathTime"] + 1.0
        # spawn a new one.
        CreateEnemy()
        currentMessage = "                         "
    end
end


void CheckHit()
    if enemy["deathTime"] == 0
        if laserOn
            if Abs(enemy["x"]) <= hitArea && Abs(enemy["y"]) <= hitArea
                enemy["deathTime"] = Time()
                score ++
                playExplosion = true
            end
        else
            if Abs(enemy["x"]) > spaceWidth or Abs(enemy["y"]) > spaceWidth or Abs(enemy["z"]) > spaceWidth
                enemy["deathTime"] = Time()
                enemy["gotAway"] = true
                currentMessage = "ENEMY GOT AWAY"
            end
        end

    end
end

void DrawStars()

    number x = 0.0
    number y = 0.0
    number z
    loop i from 0 to maxStarCount-1
        var s = stars[i]
        if s["z"] > 5.0
            z = 1.0/s["z"]
            x = scale * s["x"] * z  + 256.0
            y = scale * s["y"] * z * aspect + 128.0
            Color(s["r"], s["g"], s["b"])
            Rect(x, y, x+2, y+2)
        end
    end
end

void DrawEnemy()
    if enemy["z"] < 5.0 or enemy["gotAway"]
        return
    end

    number z = 1.0/enemy["z"]
    number x = scale * enemy["x"] * z  + 256.0
    number y = scale * enemy["y"] * z * aspect + 128.0
    number size = 20.0 * z * scale
    number lxx = enemy["lxx"] * size
    number lxy = enemy["lxy"] * size * aspect
    number lyx = enemy["lyx"] * size
    number lyy = enemy["lyy"] * size * aspect

    number x0 = x - lyx
    number y0 = y - lyy
    number x1 = x + lxx + lyx
    number y1 = y + lxy + lyy
    number x2 = x - lxx - lyx
    number y2 = y - lxy - lyy

    if enemy["deathTime"] == 0
        Color(Random(),Random(),Random())
        Line(x0, y0, x1, y1)
        Line(x1, y1, x2, y2)
        Line(x2, y2, x0, y0)
    else
        number t = Time() - enemy["deathTime"]
        Color(1.0 - t, 0, 0)
        size = size * (1.0 + t)
        Rect(x-size, y-size*aspect, x+size, y+size*aspect)
    end
end



void DrawCrossHair()
    Color(0,0,1)

    Line(235, 128, 246, 128)
    Line(266, 128, 277, 128)

    Line(256, 118, 256, 123)
    Line(256, 133, 256, 138)

    Color(1, 1, 0)
    Text(1, 1, "Score " + score)

    Color(0, 0.1, 0)
    Text(16, 23, "" + enemy["x"] + ":" + enemy["y"] + ":" + enemy["z"])

    Color(Random(),Random(),Random())
    Text(24, 12, currentMessage)

    if laserOn
        Color(Random(),Random(),Random())
        Line(0, 256, 256, 128)
        Line(512, 256, 256, 128)
    end

end

number Abs(number x)
    if x < 0
        return -x
    else
        return x
    end
end
```