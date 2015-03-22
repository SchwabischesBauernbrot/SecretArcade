<p>The object of the game is to hack the bank's security system by guessing passwords of varying difficulty. You must hack the machine and read through the code to figure out the passwords. Sill a work in progress, and I want to add a few more levels for the final product.</p>

```
levelone()
leveltwo()
levelthree()

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
		array password = ["a","m","d","n","i"]
		input = Input("Please enter password:")
		if input == password[0] + password[2] + password[1] + password[4] + password[3]
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
```