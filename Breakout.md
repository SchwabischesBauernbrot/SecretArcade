# Breakout variant of Eriks original Pong code

```
ClearText()
DisplayGraphics()
var score = 0

var ballX = 256
var ballY = 300

var ballMaxSpeed = 5
var ballSpeedX = 5
var ballSpeedY = -5

var paddleX = 250
var paddleY = 200
var paddleSize = 50
var paddleSpeed = 15

number boxWidth = 64
number boxHeight = 16
var boxes = []

number nMaxCols=8
number nBoxRows=2
number nBoxCols=8

number T = Time()
number dT = 0

loop 
 ClearText()
 Print("Get ready!")
 DisplayGraphics()        
 Sleep(1)
 InitBoxes()
 loop
     OnInput()
     ClearText()
     DrawScore()
     UpdateBall()
     DrawBall(ballX, ballY)  
     DrawBoxes()    
     DrawPaddle(paddleX, paddleY)
     dT = (Time() - T)*20
     T = Time()    
     DisplayGraphics()
     if score == nBoxCols*nBoxRows
      WinScreen()
      break
     end
 end
end

void DrawWin()
    number x = 30
    Line(x+80,100,x+100,150)
    Line(x+80,100,x+100,100)
    Line(x+100,100,x+120,130)
    Line(x+100,150,x+130,150)
    
    Line(x+120,130,x+135,100)
    Line(x+135,100,x+155,100)
    Line(x+155,100,x+170,130)

    Line(x+130,150,x+145,130)
    Line(x+145,130,x+160,150)
    
    Line(x+170,130,x+190,100)
    Line(x+210,100,x+190,150)
    Line(x+190,100,x+210,100)
    Line(x+160,150,x+190,150)
    
    Line(x+230,100,x+230,150)
    Line(x+230,100,x+250,100)
    Line(x+230,150,x+250,150)
    Line(x+250,100,x+250,150)
    
    Line(x+270,100,x+270,150)
    Line(x+270,100,x+290,100)
    Line(x+270,150,x+290,150)
    
    Line(x+290,150,x+290,120)
    Line(x+290,100,x+330,130)
    Line(x+330,100,x+330,130)
    Line(x+290,120,x+330,150)

    Line(x+330,100,x+350,100)
    Line(x+330,150,x+350,150)
    Line(x+350,100,x+350,150)
end

void WinScreen()
    DisplayGraphics()
    T = Time()
    loop
        dT = Time()-T
        ClearText()
        Print("")
        DrawWin()
        DisplayGraphics()

        if dT>3
            score = 0
            break
        end
    end
end


void OnInput()
    if(IsKeyPressed("left"))
        paddleX = paddleX - paddleSpeed*dT
    else if(IsKeyPressed("right"))
        paddleX = paddleX + paddleSpeed*dT
    end
end

bool CheckBoxCollision()
   number boxId = 0
   number i = Int(ballX/boxWidth)
   number j = Int(ballY/boxHeight)
   var k = j*nMaxCols+i 
   if k>=0
    if i<nBoxCols
     if j<nBoxRows
      if Count(boxes[k])>1
       boxes[k] = []
       score++
       return true
      end
     end
    end
   end

   return false
end

void DrawScore()
    Print("")
    Print("")
    Print("")
    Print("")
    Print("Score: " + score)
end

void InitBoxes()
 loop j from 0 to nBoxRows - 1
  loop i from 0 to nBoxCols - 1
   var c = [Random(),Random(),Random()] 
   number x = i*boxWidth 
   number y = j*boxHeight
   boxes[i+j*nMaxCols] = [x+1,y+1,x+boxWidth - 1,y+boxHeight - 1,c]
  end
 end
end

void DrawBoxes()
 loop box in boxes
  if Count(box)>1
   var c = box[4]
   Color(c[0],c[1],c[2])
   Rect(box[0],box[1],box[2],box[3])
  end 
 end
 Color(1,1,1)
end

void DrawBall(number x, number y)
    var size = 3
    Rect(x - size, y - size, x + size, y + size)
end

void DrawPaddle(number x, number y)
    Line(x, y, x + paddleSize, y)
end

void UpdateBall()
    ballX = ballX + ballSpeedX*dT
    ballY = ballY + ballSpeedY*dT

    if ballX > 512 and ballSpeedX > 0
        ballX = 512
        ballSpeedX = -ballSpeedX
    end
    
    if ballX < 0 and ballSpeedX < 0
        ballX = 0
        ballSpeedX = -ballSpeedX
    end

    if WithinPaddleBounds() and ballSpeedY > 0
        # Collide with paddle?
        if ballX > paddleX and ballX < paddleX + paddleSize
            if ballX < paddleX + 15 and ballSpeedX > 0
                # spin
                ballSpeedX *= -1.1
            else if ballX > paddleX + 35 and ballSpeedX < 0
                # spin
                ballSpeedX *= -1.1
            else
                # calm down
                ballSpeedX *= 0.95
            end
            ballSpeedY = -ballSpeedY
            ballSpeedY *= 1.05
            return
        end
    end

    # IF BALL GOES BELOW BOTTOM
    if ballY > 256 and ballSpeedY > 0
        ballSpeedY = -ballSpeedY
#        score = 0 # lose!
        Print("Ouch!")
        Sleep(1)
        ballX = 256
        ballY = 256
        ballSpeedY = -ballMaxSpeed
        if Random() > 0.5
            ballSpeedX = ballMaxSpeed
        else
            ballSpeedX = -ballMaxSpeed
        end
    end

    bool collided = CheckBoxCollision()
    if collided
        ballSpeedY = -ballSpeedY
    end 
    
    if ballY < 0 and ballSpeedY < 0
        ballSpeedY = -ballSpeedY
    end
end

bool WithinPaddleBounds()
    return ballY > paddleY - 10 and ballY < paddleY + 5
end
```