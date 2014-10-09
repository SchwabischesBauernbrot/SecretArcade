```
ClearText()
number speed = 10
number length = 16
number dir = 0
number newdir = 0
bool grow = false
number score = 0

number W = 512
number H = 256

number t = 4

number x = W/2
number y = 2*H/3

var snake = [[16,16],[16,17],[16,18],[16,19],[16,20]]
var food = []
SpawnFood()

var x1=0 
var x2=0
var y1=0 
var y2=0

var uT = Time()

Print(snake)
loop
 Color(0.2,0.3,0.15)
 Rect(0,0,512,256)
 
 GetInput()
 
 if Time()-uT > 1/speed
  if newdir != Mod(dir+2,4)
   dir = newdir
  end
  
  CheckCollision()
  UpdateSnake()

  uT = Time()
 end
 

 #Color(1,1,1)
 #DrawGrid()
 
 Color(0.1,0.1,0.1)
 
 DrawSnake()
 DrawFood()
 
 DisplayGraphics()
end

void GetInput()
 if IsKeyPressed("up")
  newdir = 0
 end
 if IsKeyPressed("right")
  newdir = 1
 end
 if IsKeyPressed("down")
  newdir = 2
 end
 if IsKeyPressed("left")
  newdir = 3
 end
end

void DrawBox(var i,var j)
    Rect(16*i,8*j,16*i+16,8*j+8)
end

void DrawGrid()
 loop i from 0 to 31
  Line(i*16,0,i*16,256)
  Line(0,i*8,512,i*8)
 end    
end

void DrawFood()
 DrawBox(food[0],food[1])
end

void DrawSnake()
 loop b in snake
  DrawBox(b[0],b[1])     
 end
end

void UpdateSnake()
 var tail = snake[Count(snake)- 1]
 number tx = tail[0]
 number ty = tail[1]
 loop i from 1 to Count(snake)- 1
  var a = snake[(Count(snake)-i)- 1]
  number ax=a[0]
  number ay=a[1]
  snake[Count(snake)-i] = [ax,ay]
 end    
 var b = snake[0]
 number bx=b[0]
 number by=b[1]
 if dir == 0
  by = by - 1
 else if dir == 1
  bx = bx + 1 
 else if dir == 2
  by = by + 1 
 else if dir == 3
  bx = bx - 1 
 end
 snake[0] = [bx,by]
 
 if grow
  snake[Count(snake)]=[tx,ty]
  grow = false
 end
end

void CheckCollision()
 var a = snake[0]
 if a[0] == food[0] && a[1] == food[1]
  grow = true
  SpawnFood()
  score = score + 1
 end
end

void SpawnFood()
 loop
  number i = Round(Random()*31)
  number j = Round(Random()*31)
  if Contains(snake,[i,j]) 
  else
   food = [i,j]
   break
  end
 end    
end

bool Contains(var L,var e)
 var r = false
 loop b in L    
  if e[0]==b[0] && e[1]==b[1]
   r = true
   break
  end
 end 
 return r   
end
```