```
## Clears the screen ready for a new game
ClearText()
DisplayGraphics()

## Sets up all the vars
number room
room = 1
number score
string name
number getfluff
string firstChoice
string inventory
    
## Checks if there is a savegame and loads it
if HasFloppy()
    LoadData()
    end
    
## Whole game loop - title shows on every restart    
loop

# Sets score to 0 on death/restart
score = 0

## Function that creates save data
void saveGame()
    if HasFloppy()
    SaveData(inventory)
    SaveData(room)
    SaveData(score)
    SaveData(getfluff)
    SaveData(name)
    Print("Your game has been saved!")
    else
    Print("No floppy found - you can't save. Sorry!")
    end
end

## Function that checks what's in the player's inventory
void checkInventory(number fluff)
    if fluff == 1
    Print ("You have a piece of fluff.")
    else
    Print ("You have nothing. A sad day indeed.")
    end
end

## If the player tries to die, we cater for it. Because
## the player /will/ try it.
void killSelf(string name)
    Print("Well done, you managed to kill yourself.")
    Print("Everybody always tries this in text adventures.")
    Print("You're not special. You're an asshole.")
    Print(name +", your score was 0, because fuck you.")
    Input("Press enter to restart from the room where you died.")
    ClearText()
end

void getFlask()
    Print("You cannot get ye flask.")
    Print("Props for trying, though.")
end

## This function is the first bit of story for 
## the first room    
void writeRoom1Text1(string name)
    bool invCheck
    number getfluff
    Print("Well, " + name + ", I'll level you - you're in trouble")
    Print("You're cuffed to a wall with little chance of escape.")
    loop
    Print("You're going to check your inventory, right?")
    string response = Input("")
    if response == "yes"
    invCheck = true
    break
    else if response == "yep"
    invCheck = true
    break
    else if response == "duh"
    invCheck = true
    break
    else if response == "yeah"
    invCheck = true
    break
    else if response == "no"
    invCheck = false
    break
    else if response == "nope"
    invCheck = false
    break
    else if response == "nah"
    invCheck = false
    break
    else if response == "check"
    checkInventory(getfluff)
    else if response == "save"
    saveGame()
    else if response == "kill"
        string response2 = Input("Who will you kill? ")
        if response2 == "self"
        killSelf(name)
        getfluff = 0 
        return getfluff
        else if response2 == "me"
        killSelf(name)
        getfluff = 0 
        return getfluff
        else if response2 == "myself"
        killSelf(name)
        getfluff = 0 
        return getfluff
        else
        Print ("There's noone else here but you. Idiot.")
        end
    else if response == "kill self"
    killSelf(name)
    getfluff = 0 
    return getfluff
    else if response == "get ye flask"
    getFlask()
    else
    Print("What? Speak up kid, I can't hear ya.")
    end
    end
    if invCheck == false
        getfluff = 2
    return getfluff
    else if invCheck == true
    Print("I'm not sure how you managed it, but you check." )
    Print("You find some pocket fluff. Fuck.")
    score ++
    getfluff = 1
    return getfluff
    else
    return
    end
end

## The first possible death in the game.
void firstDeath()
    Print("Huh, I guess it's up to you.")
    Print("You wait patiently for demise.")
    Print("Your adventure ends here.")
    Print("Your score was " + score + ".")
    Input("Press enter to restart from the room where you died.")
    ClearText()
end

## The second bit of story in room 1.
void writeRoom1Text2(string name)
    Print("Right then, bucko, you have a few choices.")
    Print("You can SHOUT for help.")
    Print("You could WRIGGLE in the hopes of freeing yourself")
    Print("Or you can sit and WAIT.")
    loop
    string response
    response = Input("What are you going to do? ")
    if response == "SHOUT"
    return response
    else if response == "WRIGGLE"
    return response
    else if response == "WAIT"
    return response
    else if response == "check"
    checkInventory(getfluff)
    else if response == "save"
    saveGame()
    else if response == "kill"
        string response2 = Input("Who will you kill? ")
        if response2 == "self"
        string die = "die"
        return die
        else if response2 == "me"
        string die = "die"
        return die
        else if response2 == "myself"
        string die = "die"
        return die
        else
        Print ("There's noone else here but you. Idiot.")
        end
    else if response == "kill self"
    string die = "die"
    return die 
    else if response == "get ye flask"
    getFlask()
    else
    Print("Sorry " + name + ", but I don't understand")
    end
    end
end

# A title screen!
Print("")
Print("")
Print("")
Print("")
Print("")
Print("")
Print("                         Text Adventure")
Print("                          Athene Allen")
Input("                      Press enter to start")
ClearText()



## Main game loop.
loop
    if room == 1
        Print("You wake in a dark, enclosed space.")
        Print("You're not entirely sure how you got here.")
        Print("But before all that - what's your name?")
        name = Input("")
        score++
        Print(name + ", eh? Weird name for a kid like you.")

        getfluff = writeRoom1Text1(name)

        if getfluff == 1
            Append(inventory, "fluff ")
            firstChoice = writeRoom1Text2(name)
        else if getfluff == 2
            firstDeath()
            break
        else if getfluff == 0
            break
        end

        if firstChoice == "SHOUT"
            #shout(name)
        else if firstChoice == "WRIGGLE"
            #wriggle(name)
        else if firstChoice == "WAIT"
            #wait(name)
        else if firstChoice == "die"
            killSelf(name)
            break
        end

    end
## end of main game loop    
end
## end of whole game loop
end
```