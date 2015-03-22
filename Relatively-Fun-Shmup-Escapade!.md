```
var shipx = 50
var shipy = 100
var shippointx = shipx+60
var shippointy = shipy + 10
var lastT = Time()
var shipspeed = 200
var shot_array = []
var shotspeed = 400
var t_since_last_shot = 0
var shot_lifetime_timer = 100
var enemy_array = []
var enemyspeed = 10
var time_since_last_enemy_gen = 0
var time_since_last_enemy_shot = 0
var enemy_lifetime = 2
var score = 0

void On_Input(number dt)
	if(IsKeyPressed("up"))
		if(IsKeyPressed("left"))
			shipx = shipx - shipspeed*dt
			shipy = shipy - shipspeed*dt
		else if (IsKeyPressed("right"))
			shipx = shipx + shipspeed*dt
			shipy = shipy - shipspeed*dt
		else
			shipy = shipy - shipspeed*dt
		end
	else if(IsKeyPressed("down"))
		if(IsKeyPressed("left"))
			shipx = shipx - shipspeed*dt
			shipy = shipy + shipspeed*dt
		else if (IsKeyPressed("right"))
			shipx = shipx + shipspeed*dt
			shipy = shipy + shipspeed*dt
		else
			shipy = shipy + shipspeed*dt
		end
	else if (IsKeyPressed("left"))
		if(IsKeyPressed("down"))
			shipx = shipx - shipspeed*dt
			shipy = shipy + shipspeed*dt
		else if(IsKeyPressed("up"))
			shipx = shipx - shipspeed*dt
			shipy = shipy - shipspeed*dt
		else
			shipx = shipx - shipspeed*dt
		end
	else if (IsKeyPressed("right"))
		if(IsKeyPressed("down"))
			shipx = shipx + shipspeed*dt
			shipy = shipy + shipspeed*dt
		else if(IsKeyPressed("up"))
			shipx = shipx + shipspeed*dt
			shipy = shipy - shipspeed*dt
		else
			shipx = shipx + shipspeed*dt
		end
	end
	if (IsKeyPressed("space"))
		if t_since_last_shot == 0
			Append(shot_array, [shipx+15, shipy+5, shot_lifetime_timer])
			PlaySound("Laser 1")
			t_since_last_shot = 50
		end
	end
end

void draw_ship(number x, number y)
	Line(x, y, x+10, y)
	Line(x, y, x, y+10)
	Line(x, y+10, x+10, y+10)
	Line(x+10, y, x+15, y+5)
	Line(x+10, y+10, x+15, y+5)
end

void update_shot_time()
	if t_since_last_shot < 0
		t_since_last_shot = 0
	else if t_since_last_shot > 0
		t_since_last_shot -= 1
	end
end

void update_shots(number dt)
	if Count(shot_array) == 0
		return
	end
	loop shot_index in GetIndexes(shot_array)
		var shot = shot_array[shot_index]
		if shot[2] > 0
			shot[0] = shot[0] + shotspeed*dt
			Line(shot[0], shot[1], shot[0]+2, shot[1])
			shot[2] = shot[2] - 1
		else
			Remove(shot_array, shot_index)
		end
		loop enemy_index in GetIndexes(enemy_array)
			var enemy = enemy_array[enemy_index]
			if shot[0] >= enemy[0] - 5 && shot[0] <= enemy[0] +10
				if shot[1] >= enemy[1] && shot[1] <= enemy[1] +10
					PlaySound("Explosion 2")
					Remove(enemy_array, enemy_index)
					Remove(shot_array, shot_index)
					score += 1
				end
			end
		end
	end
end

void draw_enemy(number x, number y)
	Line(x, y, x+10, y)
	Line(x+10, y, x+10, y+10)
	Line(x, y+10, x+10, y+10)
	Line(x, y, x- 5, y+5)
	Line(x, y+10, x- 5, y+5)
end

void generate_enemies()
	if time_since_last_enemy_gen == 0
		var random = Random()
		if random < 0.33
		create_enemy_pattern_one()
		time_since_last_enemy_gen = 500
		else if random > 0.33 && random < 0.66
		create_enemy_pattern_two()
		time_since_last_enemy_gen = 500
		else if random > 0.66
		create_enemy_pattern_three()
		time_since_last_enemy_gen = 500
		end
	else
		time_since_last_enemy_gen -= 1
	end
end

void create_enemy_pattern_one()
	Append(enemy_array, [400, 75, [], enemy_lifetime])
	Append(enemy_array, [350, 100, [], enemy_lifetime])
	Append(enemy_array, [350, 125, [], enemy_lifetime])
	Append(enemy_array, [400, 150, [], enemy_lifetime])
end

void create_enemy_pattern_two()
	Append(enemy_array, [350, 25, [], enemy_lifetime])
	Append(enemy_array, [350, 75, [], enemy_lifetime])
	Append(enemy_array, [350, 125, [], enemy_lifetime])
	Append(enemy_array, [350, 175, [], enemy_lifetime])
	Append(enemy_array, [350, 225, [], enemy_lifetime])
end

void create_enemy_pattern_three()
	Append(enemy_array, [400, 125, [], enemy_lifetime])
	Append(enemy_array, [415, 125, [], enemy_lifetime])
	Append(enemy_array, [430, 125, [], enemy_lifetime])
	Append(enemy_array, [445, 125, [], enemy_lifetime])
end

void draw_enemies()
	loop enemy in enemy_array
		draw_enemy(enemy[0], enemy[1])
	end
end

void update_enemies(number dt)
	loop enemy_index in GetIndexes(enemy_array)
		var enemy = enemy_array[enemy_index]
		enemy[0] = enemy[0] - enemyspeed*dt
		if time_since_last_enemy_shot == 0
			Append(enemy[2], [enemy[0] - 5, enemy[1] + 5, shot_lifetime_timer])
			if enemy_index == Count(enemy_array) - 1
				time_since_last_enemy_shot = 100
				PlaySound("Laser 2")
			end
		else if enemy_index == Count(enemy_array) - 1
			time_since_last_enemy_shot -= 1
		end
		if enemy[3] > 0
			enemy[3] = enemy[3] -1
		else
			Remove(enemy_array, enemy_index)
		end
	end
end

void update_enemy_shots(number dt)
	loop enemy in enemy_array
		if Count(enemy[2]) == 0
			return
		end
		var local_arr = enemy[2]
		loop shot_index in GetIndexes(local_arr)
			var shot = local_arr[shot_index]
			if shot[2] > 0
				shot[0] = shot[0] - shotspeed*dt
				Line(shot[0], shot[1], shot[0]+2, shot[1])
				shot[2] = shot[2] - 1
			else
				Remove(enemy[2], shot_index)
			end
			if shot[0] >= shipx && shot[0] <= shipx + 15
				if shot[1] >= shipy && shot[1] <= shipy + 10
					PlaySound("Explosion 1")
					GameOver()
				end
			end
		end
	end
end

void draw_score()
  Color(1, 1, 0)
	Text(1, 1, "Score: " + score)
end

void GameOver()
	ClearText()
	DisplayGraphics()
	DisplayGraphics()
	Print("")
	Print("")
	Print("")
	Print("")
	Print("")
	Print("")
	Print("")
	Print("                         Game over.")
	Print("                    Your score was " +score)
	Input("                 Press enter to try again.")
	ClearText()
	shipx = 50
	shipy = 100
	shippointx = shipx+60
	shippointy = shipy + 10
	lastT = Time()
	shot_array = []
	t_since_last_shot = 0
	enemy_array = []
	time_since_last_enemy_gen = 0
	time_since_last_enemy_shot = 0
	score = 0
	break
end

void SetRainbowColor()
	number t = Repeat(Time(), 1)
	var col = HSVtoRGB(t, 1, 1)
	Color(col[0], col[1], col[2])
end

ClearText()
DisplayGraphics()
Print("")
Print("")
Print("")
Print("")
Print("")
Print("")
Print("")
Print("              Relatively Fun Shmup Escapade!")
Print("         A Secret Arcade Jam Game by @AtheneAllen")
Print("                   Press enter to begin!")
Input("")
ClearText()

loop
	number dt = Time() - lastT
	lastT = Time()
	SetRainbowColor()
	draw_ship(shipx,shipy)
	On_Input(dt)
	update_shot_time()
	update_shots(dt)
	generate_enemies()
	draw_enemies()
	update_enemies(dt)
	update_enemy_shots(dt)
	draw_score()
	DisplayGraphics()
end
```