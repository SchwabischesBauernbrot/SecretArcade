<p>The object of the game is to hack the bank's security system by guessing passwords of varying difficulty. You must hack the machine and read through the code to figure out the passwords. Sill a work in progress, and I want to add a few more levels for the final product.</p>

```
ClearText()
Print("   _____          _         _____")
Print("  / ____|        | |       / ____| ")
Print(" | |     ___   __| | ___  | |     __ _ _ __   ___ _ __ ")
Print(" | |    / _ \ / _` |/ _ \ | |    / _` | '_ \ / _ \ '__|")
Print(" | |___| (_) | (_| |  __/ | |___| (_| | |_) |  __/ |  ")
Print("  \_____\___/ \__,_|\___|  \_____\__,_| .__/ \___|_|")
Print("                                      | |            ")
Print("                                      |_|        ")
Print("")
Print("                     By ZDog Technology")
Sleep(4)
ClearText()

levelone()
leveltwo()
levelthree()
levelfour()
levelfive()

void levelone()
	loop
		ClearText()
		Print("Welcome to BankMaster Security Systems!")
		string input
		input = Input("Please enter password:")
		if input == "Guest"
			Print("Access Granted!")
			Sleep(1)
			break
		else
			Print("Access Denied!")
			Sleep(1)
		end
	end
end

void leveltwo()
	loop
		ClearText()
		Print("Level 2 of Security")
		string input
		array password = ["a","d","m","i","n"]
		input = Input("Please enter password:")
		if input == password[0] + password[1] + password[2] + password[3] + password[4]
			Print("Access Granted!")
			Sleep(1)
			break
		else
			Print("Access Denied!")
			Sleep(1)
		end
	end
end

void convert(array a)
 number count = Count(a) - 1
	loop x from 0 to count
		a[x] = IntToChar(CharToInt(a[x])+1)
		x += 1
	end
 return a
end

void levelthree()
	loop
		ClearText()
		Print("Level 3 of Security")
		array answer = "qbttxpse"
		array input = Input("Please enter password:")
  		convert(input)
  		number count = Count(input) - 1
  		bool equal
  		loop x from 0 to count
			if input[x] == answer[x]
				x+=1
				equal = true
			else
				equal = false
			end
		end
		if equal == true
			Print("Access Granted!")
			Sleep(1)
			break
		else
			Print("Access Denied!")
			Sleep(1)
		end
	end
end

void levelfour()
	loop
		ClearText()
		Print("Level 4 of Security")
		string input
		input = Input("Please enter password:")
		if input == "42"
			Print("Access Granted!")
			Sleep(1)
			break
		else
			Print("Access Denied!")
			Sleep(1)
		end
	end
end

void convert2(array a)
	number count = Count(a) - 1
	array new
	loop x from 0 to count
		new[x] = a[x]
	end
	loop x from 0 to count
		number cell = count - x
		a[x] = new[cell]
		x += 1
	end
  return a
end

void levelfive()
	loop
		ClearText()
		Print("Level 5 of Security")
		array answer = "sdrawkcab"
		array input = Input("Please enter password:")
  		convert2(input)
  		number count = Count(input) - 1
  		bool equal
  		loop x from 0 to count
			if input[x] == answer[x]
				x+=1
				equal = true
			else
				equal = false
			end
		end
		if equal == true
			Print("Access Granted!")
			Sleep(1)
			break
		else
			Print("Access Denied!")
			Sleep(1)
		end
	end
end
```