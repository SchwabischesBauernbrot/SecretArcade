```
#My simple RPG work-in-progress.
#Tile-map collision works, no fighting or interaction beyond that at the moment.
#Must be run on the arcade cab.
Print(" ")
ClearText()
DisplayGraphics()

var playerX = 128
var playerY = 130
var enemyX = 256
var enemyY = 48
var moveDir = 1

var maxHP = 10
var hpStat = 10
var strStat = 5
var defStat = 5

#COLLISION ARRAYS -----------------------

var w1r2 =  [" "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "]
var w1r3 =  [" "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "]
var w1r4 =  [" "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," ","#","#","#","#","#","#"]
var w1r5 =  [" "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "]
var w1r6 =  [" "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "]
var w1r7 =  [" "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "]
var w1r8 =  [" "," "," "," ","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#"]
var w1r9 =  [" "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "]
var w1r10 = [" "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "]
var w1r11 = [" "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "]
var w1r12 = ["#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#"," "," "," "," "]
var w1r13 = [" "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," ","#"," "," "," "," "]
var w1r14 = [" "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," ","#"," "," "," "," "]
var w1r15 = [" "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," ","#"," "," "," "," "]
var w1r16 = ["#","#","#","#","#"," ","#","#","#"," ","#","#","#","#","#","#","#","#","#","#","#","#","#","#"," ","#","#","#","#","#","#","#","#"," ","#","#"," "," "," "," "]
var w1r17 = [" "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," ","#"," "," "," "," "]
var w1r18 = [" "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," ","#"," "," "," "," "]
var w1r19 = [" "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," ","#"," "," "," "," "]
var w1r20 = ["#","#","#","#","#"," ","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#","#"," ","#","#","#"," "," "," "," "]
var w1r21 = [" "," "," "," ","#"," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," ","#"," "," "," "," "," "," "," "," "]
var w1r22 = [" "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "," "]


#COLLISION ARRAYS END -------------------

DrawStats()
DrawTiles()

#Update LOOP ---------------------------------------
loop

DrawEnemy(enemyX, enemyY)
DrawPlayer(playerX, playerY)
OnInput()


DisplayGraphics()
end
#Update END ----------------------------------------

void OnInput()
	if(IsKeyPressed("left"))
		DrawStats()
		Sleep(0.1)
		if(playerX != 32 && collTile((playerX - 40) / 8, (playerY - 10) / 8) == false)
			playerX -= 8
			DrawStats()
			#DoEnemy()
		end
	else if(IsKeyPressed("right"))
		DrawStats()
		Sleep(0.1)
		if(playerX != 344 && collTile((playerX - 24) / 8, (playerY - 10) / 8) == false)
			playerX += 8
			DrawStats()
			#DoEnemy()
		end
	else if(IsKeyPressed("up"))
		DrawStats()
		Sleep(0.1)
		if(playerY != 26 && collTile((playerX - 32) / 8, (playerY - 18) / 8) == false)
			playerY -= 8
			DrawStats()
			#DoEnemy()
		end
	else if(IsKeyPressed("down"))
		DrawStats()
		Sleep(0.1)
		if(playerY != 186 && collTile((playerX - 32) / 8, (playerY - 2) / 8) == false)
			playerY += 8
			DrawStats()
			#DoEnemy()
		end
	end
end


void DoEnemy()
	moveDir = Random()
	if(moveDir < 0.25)
		enemyY -= 16
	else if(moveDir < 0.5)
		enemyX += 16
	else if(moveDir < 0.75)
		enemyX -= 16
	else if(moveDir < 1)
		enemyY += 16
	end
end

#DRAW FUNCTIONS ------------------------
void DrawEnemy(number x, number y)
	Color(1, 0, 0)
	Rect(x, y, x + 8, y + 8)
#	Color(0, 0, 0)
#	Rect(x - 2, y - 4, x + 2, y + 2)

end


void DrawPlayer(number x, number y)
	Color(0.3, 0.3, 1)
	Rect(x, y, x + 8, y + 8)
#	Color(0, 0, 1)

#	Rect(x - 1, y - 2, x + 1, y + 2)

end


void DrawTiles()
	Color(0, 0.1, 0)
	Text(1, 1, "############################################")
	Text(1, 2, "##                                    EXIT##")
	Text(1, 3, "##                                    EXIT##")
	Text(1, 4, "##                                  ########")
	Text(1, 5, "##                                        ##")
	Text(1, 6, "##                                        ##")
	Text(1, 7, "##                                        ##")
	Text(1, 8, "##    ######################################")
	Text(1, 9, "##                                        ##")
	Text(1,10, "##                                        ##")
	Text(1,11, "##                                        ##")
	Text(1,12, "#######-########-########-########-###    ##")
	Text(1,13, "##        #        #        #        #    ##")
	Text(1,14, "##        #        #        #        #    ##")
	Text(1,15, "##        #        #                 #    ##")
	Text(1,16, "####### ### ############## ######## ##    ##")
	Text(1,17, "##        #        #        #        #    ##")
	Text(1,18, "##        #        #        #        #    ##")
	Text(1,19, "##        #                 #        #    ##")
	Text(1,20, "####### ########-########-######## ###    ##")
	Text(1,21, "##    l                                   ##")
	Text(1,22, "##                                        ##")
	Text(1,23, "############################################")

	Color(0.7, 0.7, 0)	#Exit
	Text(39, 2, "EXIT")
	Text(39, 3, "EXIT")
	Color(0.7, 0.7, 0.7)	#Doors
	Text(8, 12, "=")
	Text(17, 12, "=")
	Text(26, 12, "=")
	Text(35, 12, "=")
	Text(17, 20, "=")
	Text(26, 20, "=")
	Text(35, 20, "=")
	Text(7, 21, "l")
	Color(0.1, 0.1, 0.1)	#Rubble, passable
	Text(29, 15, "@")
	Text(8, 16, "@")
	Text(12, 16, "@")
	Text(27, 16, "@")
	Text(36, 16, "@")
	Text(20, 19, "@")
end


void DrawStats()
	Color(1, 1, 1)
	Text(48, 2, "X: " + playerX + "   ")
	Text(48, 3, "Y: " + playerY + "   ")
	Text(48, 4, "HP: " + hpStat + "/" + maxHP + "   ")
	Text(48, 5, "St: " + strStat + "   ")
	Text(48, 6, "Df: " + defStat + "   ")
end

#DRAW FUNCTIONS END ---------------------


#COLLISION FUNCTION ---------------------

bool collTile(number x, number y)
	if (x == -1 || x == 40)
		return true
	end

	if (y == 2)
		if (w1r2[x] == "#")
			return true
		else
			return false
		end
	else if (y == 3)
		if (w1r3[x] == "#")
			return true
		else
			return false
		end
	else if (y == 4)
		if (w1r4[x] == "#")
			return true
		else
			return false
		end
	else if (y == 5)
		if (w1r5[x] == "#")
			return true
		else
			return false
		end
	else if (y == 6)
		if (w1r6[x] == "#")
			return true
		else
			return false
		end
	else if (y == 7)
		if (w1r7[x] == "#")
			return true
		else
			return false
		end
	else if (y == 8)
		if (w1r8[x] == "#")
			return true
		else
			return false
		end
	else if (y == 9)
		if (w1r9[x] == "#")
			return true
		else
			return false
		end
	else if (y == 10)
		if (w1r10[x] == "#")
			return true
		else
			return false
		end
	else if (y == 11)
		if (w1r11[x] == "#")
			return true
		else
			return false
		end
	else if (y == 12)
		if (w1r12[x] == "#")
			return true
		else
			return false
		end
	else if (y == 13)
		if (w1r13[x] == "#")
			return true
		else
			return false
		end
	else if (y == 14)
		if (w1r14[x] == "#")
			return true
		else
			return false
		end
	else if (y == 15)
		if (w1r15[x] == "#")
			return true
		else
			return false
		end
	else if (y == 16)
		if (w1r16[x] == "#")
			return true
		else
			return false
		end
	else if (y == 17)
		if (w1r17[x] == "#")
			return true
		else
			return false
		end
	else if (y == 18)
		if (w1r18[x] == "#")
			return true
		else
			return false
		end
	else if (y == 19)
		if (w1r19[x] == "#")
			return true
		else
			return false
		end
	else if (y == 20)
		if (w1r20[x] == "#")
			return true
		else
			return false
		end
	else if (y == 21)
		if (w1r21[x] == "#")
			return true
		else
			return false
		end
	else if (y == 22)
		if (w1r22[x] == "#")
			return true
		else
			return false
		end
	else
		return false
	end
end

```