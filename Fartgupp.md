```
# Prints title screen
void printTitleScreen()
	ClearText()
	Print("")
	Print("                                                      ")
	Print("           Fartgupp                                   ")
	Print("           ______         _                           ")
	Print("           |  ___|       | |                          ")
	Print("           | |_ __ _ _ __| |_ __ _ _   _ _ __  _ __   ")
	Print("           |  _/ _` | '__| __/ _` | | | | '_ || '_ |  ")
	Print("           | | |(_| | |  | | |(_| | |_| | |_) | |_) | ")
	Print("           |_| |__,_|_|   |__|__, ||__,_| .__/| .__/  ")
	Print("                              __/ |     | |   | |     ")
	Print("                             |___/      |_|   |_|     ")
	Print("                                                      ")
	Print("                      Do you speak IKEA?              ")
	Print("             Answer the 20 questions to find out!     ")
	Print("                                                      ")
	Print("                                                      ")
	Print("                                                      ")
	Print("                                                      ")
	Print("                                                      ")
	Print("                       A quiz game by                 ")
	Print("                  @IamHerrEmil & @IamVikiPapp         ")
	Print("                                                      ")
	Print("                                                      ")
	Input("                     Press enter to start             ")
	ClearText()
end

# Clear screen when first launching the game
ClearText()
DisplayGraphics()

# List of Swedish words, and what they are
array ord = []
ord[0] =  ["Omsorg",		"an IKEA product"]
ord[1] =  ["Lighvan",		"a cheese"] 
ord[2] =  ["Vintersorg",	"a Swedish metal band"]
ord[3] =  ["Freda",			"a Lord of The Rings character"]
ord[4] =  ["Bagis",			"an IKEA product"]
ord[5] =  ["Bitto",			"a cheese"] 
ord[6] =  ["Narnia",		"a Swedish metal band"]
ord[7] =  ["Grimbold",		"a Lord of The Rings character"]
ord[8] =  ["Tysnes",		"an IKEA product"]
ord[9] =  ["Caravane",		"a cheese"] 
ord[10] = ["Marduk",		"a Swedish metal band"]
ord[11] = ["Deagol",		"a Lord of The Rings character"]
ord[12] = ["Sigurd",		"an IKEA product"]
ord[13] = ["Saga",			"a cheese"] 
ord[14] = ["Bathory",		"a Swedish metal band"]
ord[15] = ["Galdor",		"a Lord of The Rings character"]
ord[16] = ["Klappar haj",	"an IKEA product"]
ord[17] = ["Svecia",		"a cheese"] 
ord[18] = ["Watain",		"a Swedish metal band"]
ord[19] = ["Grima",			"a Lord of The Rings character"]

bool IsUppercase(var c)
 return CharToInt(c) >= -32 && CharToInt(c) <= -7
end

var ToLowercase(var text)
 var res = ""
 loop c in text
  if IsUppercase(c)
   res += IntToChar(CharToInt(c) + 32)
  else
   res += c
  end
 end
 return res
end

# Check if player input is yes or no
bool getPlayerResponse()
	string input
	# Loop in case fail input and try again
	loop
		input = Input("")
		input = ToLowercase(input)
		if input == "yes"
			return true
		else if input == "y"
			return true
		else if input == "yep"
			return true
		else if input == "yup"
			return true
		else if input == "yeah"
			return true
		else if input == "it is"
			return true
		else if input == "yepp"
			return true
		else if input == "aha"
			return true
		else if input == "ja"
			return true
		else if input == "sure"
			return true
		else if input == "certainly"
			return true
		else if input == "absolutely"
			return true
		else if input == "indeed"
			return true
		else if input == "affirmative"
			return true
		else if input == "aye"
			return true
		else if input == "yah"
			return true
		else if input == "uh-huh"
			return true
		else if input == "OK"
			return true
		else if input == "righto"
			return true
		else if input == "yea"
			return true
		else if input == "correct"
			return true
		else if input == "right"
			return true
		else if input == "true"
			return true
		else if input == "no"
			return false
		else if input == "false"
			return false
		else if input == "incorrect"
			return false
		else if input == "untrue"
			return false
		else if input == "inaccurate"
			return false
		else if input == "not"
			return false
		else if input == "negative"
			return false
		else if input == "never"
			return false
		else if input == "not really"
			return false
		else if input == "nae"
			return false
		else if input == "nope"
			return false
		else if input == "nah"
			return false
		else if input == "no way"
			return false
		else if input == "no siree"
			return false
		else if input == "nay"
			return false
		else if input == "naw"
			return false
		else if input == "n"
			return false
		else
			Print("Cannot compute answer, is it or is it not?")
		end
	end
end

string getRandomType()
	number randomNumber
	randomNumber = Random()
	# Print("randomNumber: " + randomNumber)
	if randomNumber < 0.25
		return "a Swedish metal band"
	else if randomNumber > 0.25 and randomNumber < 0.5
		return "an IKEA product"
	else if randomNumber > 0.5 and randomNumber < 0.75
		return "a cheese"
	else if randomNumber > 0.75
		return "a Lord of The Rings character"
	else
		# If by some small chance, the random number is exactly, 0.25, 0.5 or 0.75, just pick one of them
		return "a metal band"
	end
end

void printBadEnding()
	ClearText()
	Print("")
	Print("                                                      ")
	Print("    Herregud...                                       ")
	Print("     _   _                                    _       ")
	Print("    | | | |                                  | |      ")
	Print("    | |_| | ___ _ __ _ __ ___  __ _ _   _  __| |      ")
	Print("    |  _  |/ _ | '__| '__/ _ |/ _` | | | |/ _` |      ")
	Print("    | | | |  __/ |  | | |  __/ (_| | |_| | (_| |_ _ _ ")
	Print("    |_| |_/|___|_|  |_|  |___||__, ||__,_||__,_(_|_|_)")
	Print("                               __/ |                  ")
	Print("                              |___/                   ")
	Print("                                                      ")
	Print("                                                      ")
	Print("        You did not get so many correct answers this  ")
	Print("             time, maybe you want to try again?       ")
	Print("                                                      ")
	Print("                                                      ")
	Print("                                                      ")
	Print("                                                      ")
	Print("                                                      ")
	Print("                                                      ")
	Print("                                                      ")
	Print("                                                      ")
	Input("                Press enter to play again.            ")
	ClearText()
end

void printOKEnding()
	ClearText()
	Print("")
	Print("                                                      ")
	Print("             Lagom.                                   ")
	Print("              _                                       ")
	Print("             | |                                      ")
	Print("             | |     __ _  __ _  ___  _ __ ___        ")
	Print("             | |    / _` |/ _` |/ _ || '_ ` _ |       ")
	Print("             | |___| (_| | (_| | (_) | | | | | |_     ")
	Print("             |_____/|__,_||__, ||___/|_| |_| |_(_)    ")
	Print("                           __/ |                      ")
	Print("                          |___/                       ")
	Print("                                                      ")
	Print("                                                      ")
	Print("                    You have an approximate           ")
	Print("                   knowledge of many things!          ")
	Print("                                                      ")
	Print("          You know the difference between black death ")
	Print("         metal and flatpack furniture, so you got that")
	Print("                 going for you, which is nice.        ")
	Print("                                                      ")
	Print("                                                      ")
	Print("                                                      ")
	Print("                                                      ")
	Input("                  Press enter to play again.          ")
	ClearText()
end

void printGoodEnding()
	ClearText()
	Print("")
	Print("                                                      ")
	Print("               Duktig!                                ")
	Print("               ______       _    _   _       _        ")
	Print("               |  _  |     | |  | | (_)     | |       ")
	Print("               | | | |_   _| | _| |_ _  __ _| |       ")
	Print("               | | | | | | | |/ / __| |/ _` | |       ")
	Print("               | |/ /| |_| |   <| |_| | (_| |_|       ")
	Print("               |___/  |__,_|_||_||__|_||__, (_)       ")
	Print("                                        __/ |         ")
	Print("                                       |___/          ")
	Print("                                                      ")
	Print("                     You do speak IKEA.               ")
	Print("                                                      ")
	Print("        You have been living inside an IKEA store for ")
	Print("        years now. You spend your days in an eternal  ")
	Print("         Fredagsmys, re-reading familiar tales about  ")
	Print("       Middle Earth. The personell have gotten used to")
	Print("          the constant metal music and the smell of   ")
	Print("             cheese in the living room section.       ")
	Print("                                                      ")
	Print("                                                      ")
	Print("                                                      ")
	Input("                  Press enter to play again.          ")
	ClearText()
end



# Main loop, restarts the game if you reach the end
loop
	printTitleScreen()
	number correctAnswers = 0

	# Game loop, loops through list of words
	number numberOfWords = Count(ord)
	loop x from 0 to (numberOfWords - 1)
		ClearText()

		# For every item, grab name and type
		array currentItem = ord[x]
		string currentWord = currentItem[0]
		string currentType = currentItem[1]
		Print("")
		Print("__________========== Question " + (x + 1) +  " ==========__________")
		Print("")

		# Grab a random Type
		string randomType
		randomType = getRandomType()

		# Ask player the question
		Print("Is " + currentWord + " " + randomType + "?")

		# Get player input
		bool playerThinksYes
		playerThinksYes = getPlayerResponse()

		# Check if player guessed correctly
		Print("")
		if currentType == randomType
			if playerThinksYes
				Print("CORRECT, " + currentWord + " is " + currentType + "!")
				correctAnswers += 1
			else
				Print("WRONG, " + currentWord + " is " + currentType + "!")
			end
		else
			if playerThinksYes
				Print("WRONG, " + currentWord + " is " + currentType + "!")
			else
				Print("CORRECT, " + currentWord + " is " + currentType + "!")
				correctAnswers += 1
			end
		end
		# Pause before displaying next question
		Input("")
	end

	# Game Over screens
	if correctAnswers < 10
		printBadEnding()
	else if correctAnswers < 18
		printOKEnding()
	else
		printGoodEnding()
	end
end
```