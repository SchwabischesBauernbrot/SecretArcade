```
## Clears the screen ready for a new game
ClearText()
DisplayGraphics()

## Sets up all the vars
number room = 1
number score
string name
number getfluff
string firstChoice
string inventory
string firstFight
number HEA = 10
number STR = 4
number DEF = 5
    
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

# Message writing function - prints the message, then
# pauses for a second
void Message(string text)
    Print(text)
    Sleep(0.8)
end

# Response engine. This is what is called when the player
# is asked for input.
void Response()
    loop
    string response
    string output
    response = Input("")
    if response == "yes"
    output = "true"
    return output
    else if response == "yep"
    output = "true"
    return output
    else if response == "duh"
    output = "true"
    return output
    else if response == "yeah"
    output = "true"
    return output
    else if response == "no"
    output = "false"
    return output
    else if response == "nope"
    output = "false"
    return output
    else if response == "nah"
    output = "false"
    return output
    else if response == "SHOUT"
    output = "shout"
    return output
    else if response == "WRIGGLE"
    output = "wriggle"
    return output
    else if response == "WAIT"
    output = "wait"
    return output
    else if response == "punch"
    output = "punch"
    return output
    else if response == "fight"
    output = "kick"
    return output
    else if response == "kick"
    output = "kick"
    return output
    else if response == "kill guard"
    output = "kill guard"
    return output
    else if response == "strong"
    output = "strong"
    return output
    else if response == "weak"
    output = "weak"
    return output
    else if response == "check"
    checkInventory(getfluff)
    else if response == "save"
    saveGame()
    else if response == "score"
    Print("Your score is " + score)
    else if response == "stats"
    Message("Your strength is "+STR+", your defence is "+DEF)
    Message("and your health is "+HEA+".")
    else if response == "help"
    Message("Most of the commands are context sensitive, but")
    Message("the commands CHECK, SAVE, SCORE and STATS are")
    Message("always avaialble.")
    else if response == "kill"
        string response2 = Input("Who will you kill? ")
        if response2 == "self"
        output = "dead" 
        return output
        else if response2 == "me"
        output = "dead" 
        return output
        else if response2 == "myself"
        output = "dead"
        return output
        else if response2 == "guard"
        output = "kill guard"
        return output
        else
        Message ("There's noone else here but you. Idiot.")
        end
    else if response == "kill self"
    output = "dead"
    return output
    else if response == "get ye flask"
    getFlask()
    else
    Message("What? Speak up kid, I can't hear ya.")
    end
    end
end


##OH GOD THIS IS THE FIGHTING FUNCTION
##IT'S KINDA MESSY
void fightEngine(number En_STR, number En_DEF, number En_HEA)
    number attack
    string action
    string victory
    number enemyAttack
    number healthBeforeAttack
    number EnHealthBeforeAttack       
    bool playerTurn = true
    loop
    Message("Your strength is "+STR+", your defence is "+DEF)
    Message("and your health is "+HEA+".")
    Message("Enemy's strength is "+En_STR+", defence is "+En_DEF)
    Message("and health is "+En_HEA)
    loop
    if playerTurn == true
    Message("Will you do a STRONG or WEAK attack?")
    action = Response()
    if action == "strong"
        attack = Int(STR + 10*Random())
        EnHealthBeforeAttack = En_HEA
        En_HEA = En_HEA - calcAttack(attack, En_DEF)
        if EnHealthBeforeAttack - En_HEA != 0
        Message("You do "+Int(EnHealthBeforeAttack - En_HEA)+ " points of damage")
        else
        Message("Your blow glances off the enemy. Poor effort")
        end
        playerTurn = false
        break
    else if action == "weak"
        attack = Int(10*Random())
        if attack > 6
            attack = 6
        else if attack < 3
            attack = 3
        end
        attack = STR + attack
        EnHealthBeforeAttack = En_HEA
        En_HEA -= calcAttack(attack, En_DEF)
        if EnHealthBeforeAttack - En_HEA > 0
        Message("You do "+Int(EnHealthBeforeAttack - En_HEA)+"points of damage")
        else
        Message("Your blow glances off the enemy. Poor effort")
        end
        playerTurn = false
        break
    else
    Message("I don't understand, have another crack.")
    end
    end
        if En_HEA < 0
        victory = "win"
        return victory
        end
    if playerTurn == false
    if Random() >= 0.5
        enemyAttack = Int(En_STR + 10*Random())
        healthBeforeAttack = HEA
        HEA -= calcAttack(enemyAttack, DEF)
        if healthBeforeAttack - HEA != 0
        Message("The enemy does "+Int(healthBeforeAttack-HEA)+" pointsof damage. Ouch.")
        else
        Message("You shrug off the enemy's blow and make some")
        Message("snide remark. You cheeky byugger")
        end
        playerTurn = true
    else 
        enemyAttack = Int(10*Random())
        if enemyAttack > 6
            enemyAttack = 6
        else if enemyAttack < 3
            enemyAttack = 3
        end
        enemyAttack = En_STR + enemyAttack
        healthBeforeAttack = HEA
        HEA -= calcAttack(enemyAttack, DEF)
        if healthBeforeAttack - HEA != 0
        Message("The enemy does "+Int(healthBeforeAttack-HEA)+" pointsof damage. Ouch.")
        else
        Message("You shrug off the enemy's blow and make some")
        Message("snide remark. You cheeky bugger")
        end
        playerTurn = true
    end
    end
    if HEA < 0
        victory = "lose"
        return victory
    end                
    end
    end
end


#function that calculates if the player or the enemy loses
#any health
void calcAttack(number attack, number defense)
    number newHealth
    number damage
    damage = attack - defense
    if damage > 0
        newHealth = damage 
        return newHealth
    else
        newHealth = 0
        return newHealth
    end
end

## Function that checks what's in the player's inventory
void checkInventory(number fluff)
    if fluff == 1
    Message("You have a piece of fluff.")
    else
    Message("You have nothing. A sad day indeed.")
    end
end

## If the player tries to die, we cater for it. Because
## the player /will/ try it.
void killSelf()
    Message("Well done, you managed to kill yourself.")
    Message("Everybody always tries this in text adventures.")
    Message("You're not special. You're an asshole.")
    Message(name +", your score was 0, because fuck you.")
    Input("Press enter to restart from the room where you died.")
    ClearText()
end

void getFlask()
    Message("You cannot get ye flask.")
    Message("Props for trying, though.")
end

## This function is the first bit of story for 
## the first room    
void writeRoom1Text1()
    number getfluff
    Message("Well, " + name + ", I'll level with you - you're in trouble.")
    Message("You're cuffed to a wall with little chance of escape.")
    loop
    Message("You're going to check your inventory, right?")
    string invCheck = Response()
    if invCheck == "false"
        getfluff = 2
    return getfluff
    else if invCheck == "true"
    Message("I'm not sure how you managed it, but you check." )
    Message("You find some pocket fluff. Fuck.")
    score ++
    getfluff = 1
    return getfluff
    else if invCheck == "dead"
    return invCheck
    else
    Print("Sorry, didn't quite get that.")
    end
    end
end

## The first possible death in the game.
void firstDeath()
    Message("Huh, I guess it's up to you.")
    Message("You wait patiently for demise.")
    Message("Your adventure ends here.")
    Message("Your score was " + score + ".")
    Input("Press enter to restart from the room where you died.")
    ClearText()
end

## The second bit of story in room 1.
void writeRoom1Text2()
    Message("Right then, bucko, you have a few choices.")
    Message("You can SHOUT for help.")
    Message("You could WRIGGLE in the hopes of freeing yourself")
    Message("Or you can sit and WAIT.")
    loop
    string action
    Print("What are you going to do? ")
    action = Response()
    if action == "shout"
    return action
    else if action == "wriggle"
    return action
    else if action == "wait"
    return action
    else if action == "dead"
    return action
    else
    Message("Sorry " + name + ", but I don't understand")
    end
    end
end

# The Shout story path in Room 1

void writeRoom1Shout()
    Message('You shout out, "Oh god, please help me!"')
    Message("Turns out that may not have been the best idea.")
    Message("Someone comes into view. He tells you,")
    Message('"I was going to help you until you burst my eardrums."')
    Message('"So, frankly, you can fuck off and die."') 
    Message("I'll be honest, " + name + ", that's kind of backfired.")
    Message("Not a disaster, though. I guess your best options")
    Message("now are to WRIGGLE or WAIT")
    loop
    string action
    action = Response()
    if action == "wriggle"
        return action
    else if action == "wait"
        return action
    else
    Message("Sorry " + name + ", but I don't understand")
    end
    end
end

void writeRoom1Wriggle()
    Message("Ah, now we're getting somewhere!")
    Message("You wriggle around a bit and your cuffs catch against")
    Message("an uneveness in the wall. The metal clinks and you")
    Message("feel the tension lessen. Are they weakening?")
    Message("You wriggle more and feel the chain snap.")
    Message("You're free! That'll teach  your captors for buying")
    Message("cheap cuffs. Your feet aren't bound, presumably")
    Message("because the budget was that tight.")
    Message("You creep forward to the door and spot a guard outside")
    Message("What will you do?")
    loop
    string action     
    action = Response()
    if action == "punch"
        Message("You clock the guard right in the back of the")
        Message("neck. He crumples to the floor. Triumph!")
        return "victory"
    else if action == "kick"
        Message("You deliver a solid boot into the guard's arse.")
        Message("He doubles over, but turns to face you.")
        Message("Get ready to fight!")
        string victory = fightEngine(2,3,5)
        if victory == "win"
        return "victory"
        else if victory == "lose"
        return "defeat"
        end
    else if action == "kill"
        Message("A ruthless choice. You approach the guard from")
        Message("behind and choke him out. He won't be getting back")
        Message("up.")
        return "victory"
    else
        Message("Not sure what you mean there, fella.")
    end
    end
end

#The second death sequence, that occurs if you lose the fight
void secondDeath()
    Message("Alas, you are stabbed through the heart by a guard")
    Message("with an axe to grind.")
    Message("Your adventure ends here.")
    Message("Your score was " + score + ".")
    Input("Press enter to restart from the room where you died.")
    ClearText()
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
        Message("You wake in a dark, enclosed space.")
        Message("You're not entirely sure how you got here.")
        Message("But before all that - what's your name?")
        name = Input("")
        score++
        Message(name + ", eh? Weird name for a kid like you.")

        getfluff = writeRoom1Text1()

        if getfluff == 1
            Append(inventory, "fluff ")
            firstChoice = writeRoom1Text2()
            score++
        else if getfluff == 2
            firstDeath()
            break
        else if getfluff == "dead"
            killSelf()
            break
        end
        
        loop
        if firstChoice == "SHOUT"
           score --
           firstChoice = writeRoom1Shout()
        else if firstChoice == "WRIGGLE"
            score++
            firstFight = writeRoom1Wriggle()
            break
        else if firstChoice == "WAIT"
            #wait(name)
        else if firstChoice == "dead"
            killSelf()
            break
        end
        end
        if firstFight == "victory"
            score++
            Message("Yay you won the fight")
            Input("")
        else if firstFight == "defeat"
            secondDeath()
            break
        end

    end
## end of main game loop    
end
## end of whole game loop
end
```