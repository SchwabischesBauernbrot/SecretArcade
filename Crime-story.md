The crime story requires the player to go to the cabinet during different times of the day.
I track 3 times: AM (06-14), PM (14-20), Night (20-24,00-06).
Note that in the Secret Arcade demo, time doesn't pass but you can set the time using the debug console. Press F12, type SetHour(x), press Enter. You'll have to restart the game afterwards, but it doesn't save anything; you just need to manually gather clues from the different times of the day.

PS to Erik: Found interesting side-case. Since this game didn't (I added it now) use DisplayGraphics, the graphics were never cleared in-between games. This made itself apparent going from a different game to this, because the old game's graphics remained indefinitely.


```
ClearText()
DisplayGraphics()
Print("")
Print("")
Print("")
Print("")
Print("Entering office... Hold on...")

#Takes a list and the current selection index.
#Handles up/down input, wrapping around.
#Returns the new selection index.
var HandleListInput_data = [false, false]
var HandleListInput(var options, var selection)
 if(IsKeyPressed("up"))
  if(HandleListInput_data[0] == false)
   selection = selection - 1
   if (selection < 0)
    selection = Count(options) - 1
   end
  end
  HandleListInput_data[0] = true
 else
  HandleListInput_data[0] = false
 end
 if(IsKeyPressed("down"))
  if(HandleListInput_data[1] == false)
   selection = selection + 1
   if(selection >= Count(options))
    selection = 0
   end
   HandleListInput_data[1] = true
  end
 else
  HandleListInput_data[1] = false
 end

 return selection
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

var ToUppercase(var text)
 var res = ""
 loop c in text
  if IsLowercase(c)
   res += IntToChar(CharToInt(c) - 32)
  else
   res += c
  end
 end
 return res
end

bool IsLowercase(var c)
 return CharToInt(c) >= 0 && CharToInt(c) <= 25
end

bool IsUppercase(var c)
 return CharToInt(c) >= -32 && CharToInt(c) <= -7
end

array CopyArray(array a)
	array res = []
	loop i in a
		res[Count(res)] = i
	end
	return res
end


#Time
var time_night_index = 0;
var time_am_index = 1;
var time_pm_index = 2;

array time_descs
time_descs[time_night_index] = "Time: Night"
time_descs[time_am_index] = "Time: AM"
time_descs[time_pm_index] = "Time: PM"

#Only call this when the game starts, to avoid strange side-cases where it would switch state mid-session
var GetTimeOfDay()
	var hour = GetHour()
	if (hour < 6 || hour > 20)
		return time_night_index
	else if (hour < 14)
		return time_am_index
	end
	return time_pm_index
end

var GetTimeDesc()
	if (time_index < 0 || time_index >= Count(time_descs))
		return ""
	end
	return time_descs[time_index]
end

#Scene indices
array scenes
number scene_main_index = 0
number scene_court_n_index = 1
number scene_court_am_index = 2
number scene_court_pm_index = 3
number scene_court_pm_killer_index = 4
number scene_court_pm_cause_index = 5
number scene_court_pm_motive_index = 6
number scene_court_pm_proofs_index = 7
number scene_court_pm_verdict_index = 8
number scene_evidence_index = 14
number scene_briefing_index = 15
number scene_briefing_2_index = 16
number scene_evidence_cards_index = 17
number scene_evidence_score_index = 18
number scene_evidence_statement_anna_index = 19
number scene_evidence_statement_kim_index = 20
number scene_evidence_statement_kim_2_index = 21
number scene_evidence_statement_gregor_1_index = 22
number scene_evidence_statement_gregor_2_index = 23
number scene_crime_n_index = 24
number scene_crime_n_picked_lock_index = 25
number scene_crime_n_break_index = 26
number scene_crime_n_look_more_index = 27
number scene_crime_am_index = 28
number scene_crime_am_play_index = 29
number scene_crime_pm_index = 30
number scene_crime_pm_talk_index = 31
number scene_restaurant_n_index = 35
number scene_restaurant_n_lock_index = 36
number scene_restaurant_am_index = 37
number scene_restaurant_am_talk_index = 38
number scene_restaurant_pm_index = 39
number scene_restaurant_pm_fugu_index = 40
number scene_restaurant_pm_company_index = 41

#Primary indices to keep track of state
number option_index = 0
number scene_index = scene_main_index
number time_index = GetTimeOfDay()
array proof = []

bool was_pressed_space = false
bool was_pressed_left = false
bool was_pressed_right = false
var lastT = Time()

#Options
void AddOption(var option_list, string option_text, number scene_destination_index, bool reset_selection)
	var option = [ToLowercase(option_text), ToUppercase(option_text), scene_destination_index, reset_selection]
	option_list[Count(option_list)] = option
end

array options_main
AddOption(options_main, "Briefing (aka what am I doing?)", scene_briefing_index, true)
AddOption(options_main, "Check evidence", scene_evidence_index, true)
if(time_index == time_night_index)
	AddOption(options_main, "Investigate crime scene (night)", scene_crime_n_index, true)
else if(time_index == time_am_index)
	AddOption(options_main, "Investigate crime scene (AM)", scene_crime_am_index, true)
else
	AddOption(options_main, "Investigate crime scene (PM)", scene_crime_pm_index, true)
end
if(time_index == time_night_index)
	AddOption(options_main, "Investigate restaurant (night)", scene_restaurant_n_index, true)
else if(time_index == time_am_index)
	AddOption(options_main, "Investigate restaurant (AM)", scene_restaurant_am_index, true)
else
	AddOption(options_main, "Investigate restaurant (PM)", scene_restaurant_pm_index, true)
end
if(time_index == time_night_index)
	AddOption(options_main, "Go to court!", scene_court_n_index, true)
else if(time_index == time_am_index)
	AddOption(options_main, "Go to court!", scene_court_am_index, true)
else
	AddOption(options_main, "Go to court!", scene_court_pm_index, true)
end

array options_evidence
AddOption(options_evidence, "Playing cards", scene_evidence_cards_index, false)
AddOption(options_evidence, "Score card", scene_evidence_score_index, false)
AddOption(options_evidence, "Testimony - Ms. Anna", scene_evidence_statement_anna_index, false)
AddOption(options_evidence, "Testimony - Mrs. Kim, pt1", scene_evidence_statement_kim_index, false)
AddOption(options_evidence, "Testimony - Mrs. Kim, pt2", scene_evidence_statement_kim_2_index, false)
AddOption(options_evidence, "Testimony - Mr. Gregor, pt1", scene_evidence_statement_gregor_1_index, false)
AddOption(options_evidence, "Testimony - Mr. Gregor, pt2", scene_evidence_statement_gregor_2_index, false)
AddOption(options_evidence, "Back to office", scene_main_index, true)

array options_court_n
AddOption(options_court_n, "Back to office", scene_main_index, true)
array options_court_am
AddOption(options_court_am, "Back to office", scene_main_index, true)
array options_court_pm
AddOption(options_court_pm, "I've got this one in the bag!", scene_court_pm_killer_index, true)
AddOption(options_court_pm, "Back to office", scene_main_index, true)
array options_court_pm_killer
AddOption(options_court_pm_killer, "Mr. Gregor", scene_court_pm_cause_index, true)
AddOption(options_court_pm_killer, "Mrs. Kim", scene_court_pm_cause_index, true)
AddOption(options_court_pm_killer, "Ms. Anna", scene_court_pm_cause_index, true)
array options_court_pm_cause
AddOption(options_court_pm_cause, "Heart attack", scene_court_pm_motive_index, true)
AddOption(options_court_pm_cause, "Poisoned playing cards", scene_court_pm_motive_index, true)
AddOption(options_court_pm_cause, "Fish soup", scene_court_pm_motive_index, true)
AddOption(options_court_pm_cause, "Gunshot", scene_court_pm_motive_index, true)
AddOption(options_court_pm_cause, "Fugu fish", scene_court_pm_motive_index, true)
array options_court_pm_motive
AddOption(options_court_pm_motive, "Revenge", scene_court_pm_proofs_index, true)
AddOption(options_court_pm_motive, "A wager", scene_court_pm_proofs_index, true)
AddOption(options_court_pm_motive, "Accident", scene_court_pm_proofs_index, true)
AddOption(options_court_pm_motive, "Winning the game", scene_court_pm_proofs_index, true)
array options_court_pm_proofs
AddOption(options_court_pm_proofs, "Hang on, let me look it over again...", scene_court_pm_killer_index, true)
AddOption(options_court_pm_proofs, "Yes, I'm certain!", scene_court_pm_verdict_index, true)
array options_court_pm_verdict

array options_briefing
AddOption(options_briefing, "More", scene_briefing_2_index, true)
array options_briefing_2
AddOption(options_briefing_2, "Back to office", scene_main_index, true)

array options_crime_n
AddOption(options_crime_n, "Pick the lock", scene_crime_n_picked_lock_index, true)
AddOption(options_crime_n, "Back to office", scene_main_index, true)
array options_crime_n_picked_lock
AddOption(options_crime_n_picked_lock, "Take a break", scene_crime_n_break_index, true)
AddOption(options_crime_n_picked_lock, "Look some more", scene_crime_n_look_more_index, true)
AddOption(options_crime_n_picked_lock, "Back to office", scene_main_index, true)
array options_crime_n_break
AddOption(options_crime_n_break, "Look some more", scene_crime_n_look_more_index, true)
AddOption(options_crime_n_break, "Back to office", scene_main_index, true)
array options_crime_n_look_more
AddOption(options_crime_n_look_more, "Take a break", scene_crime_n_break_index, true)
AddOption(options_crime_n_look_more, "Back to office", scene_main_index, true)
array options_crime_am
AddOption(options_crime_am, "Accept the offer", scene_crime_am_play_index, true)
AddOption(options_crime_am, "Decline and head back to the office", scene_main_index, true)
array options_crime_am_play
AddOption(options_crime_am_play, "Back to office", scene_main_index, true)
array options_crime_pm
AddOption(options_crime_pm, "Talk to the players", scene_crime_pm_talk_index, true)
AddOption(options_crime_pm, "Back to office", scene_main_index, true)
array options_crime_pm_talk
AddOption(options_crime_pm_talk, "Back to office", scene_main_index, true)

array options_restaurant_n
AddOption(options_restaurant_n, "Pick the lock", scene_restaurant_n_lock_index, true)
AddOption(options_restaurant_n, "Back to office", scene_main_index, true)
array options_restaurant_n_lock
AddOption(options_restaurant_n_lock, "Back to office", scene_main_index, true)
array options_restaurant_am
AddOption(options_restaurant_am, "Head into the kitchen", scene_restaurant_am_talk_index, true)
AddOption(options_restaurant_am, "Back to office", scene_main_index, true)
array options_restaurant_am_talk
AddOption(options_restaurant_am_talk, "Back to office", scene_main_index, true)
array options_restaurant_pm
AddOption(options_restaurant_pm, "Ask about the specialty dish", scene_restaurant_pm_fugu_index, true)
AddOption(options_restaurant_pm, "Ask about the suspects", scene_restaurant_pm_company_index, true)
AddOption(options_restaurant_pm, "Back to office", scene_main_index, true)
array options_restaurant_pm_fugu
AddOption(options_restaurant_pm_fugu, "Ask about the suspects", scene_restaurant_pm_company_index, true)
AddOption(options_restaurant_pm_fugu, "Back to office", scene_main_index, true)
array options_restaurant_pm_company
AddOption(options_restaurant_pm_company, "Ask about the specialty dish", scene_restaurant_pm_fugu_index, true)
AddOption(options_restaurant_pm_company, "Back to office", scene_main_index, true)

#Descriptions - Main
array desc_main = ["Back at the office. Now what to do...?"]

#Descriptions - Briefing
array desc_briefing = ["You're currently investigating the death of a certain Mr. Klaus.","A few days ago, he and three others were playing a game of competitive cards.","In the middle of the game he is reported to have suddenly fallen over, coughing and choking. Within seconds, he was dead.","You currently have two locations of interest:","1. The crime scene. A notorious card game black club in the docks.","2. The restaurant where the victim, and the suspects, are reported to have met just before leaving for the crime scene."]
array desc_briefing_2 = ["You have three suspects; the fellow card players during which Mr. Klaus died:","Mr. Gregor, Mrs. Kim, and Ms. Anna.""These three are also the sole witnesses. Their testimonials can be found in among the evidence.","","You'll need to check out the crime scene and restaurant at different times of the day, as well as inspect the evidence thoroughly, if you hope to ever have a chance of breaking this one!"]

#Descriptions - Court
array desc_court_n = ["The courthouse is closed.","You'll need to come back during the afternoon."]
array desc_court_am = ["While the courthouse is open, all court proceedings are handled during the afternoon.","You'll have to come back later in the day."]
array desc_court_pm = ["You enter the town's paltry courthouse, hoping you've gathered enough intelligence.","First of, you'll need to point out which one of the three suspects you believe is the killer.","After that, you'll need to present the evidence you believe proves this.","It is then up to the judge and jury to convene and decide whether you're case is strong enough.","","Once you go to court, there's no backing!","Are you sure you want to do this now?!"]
array desc_court_pm_killer = ["First of all, which of the three key suspects is the killer?"]
array desc_court_pm_cause = ["What was the cause of death?"]
array desc_court_pm_motive = ["What was the killer's motive?"]
array desc_court_pm_proofs = ["So, in summary...",""]
array desc_court_pm_verdict = ["After an agonizing break during which the jury decides what to make of this, the verdict is called out:",""]

#Descriptions - Evidence
array desc_evidence = ["You look over the desk with the evidence you've got."]

array desc_evidence_cards = ["It is a normal deck of Hanafuda playing cards.","Many of the cards bear traces of arsenic and smell slightly of garlic."]

array desc_evidence_score = ["It is a score card with the names of all three suspects and the victim at the top.","It appears they were in the last rounds of their game, with the tally as follows:","Klaus - 12","Kim - 10","Gregor - 7","Anna - 6"]

array desc_evidence_statement_anna = ["Ms. Anna is one of the three suspects. Her testimony is as follows:","","I don't want to be someone who throws blame for no reason, but I always suspected that Mrs. Kim was up to no good.","My evidence..? Hmm, well...","The easiest is of course that she was in second place when the match was coming to its close.","Now I'm no detective, but I know there was a lot at stake and that she ain't the type to accept a loss!"]

array desc_evidence_statement_kim = ["Mrs. Kim is one of the three suspects. Her testimony is as follows:","","It's really deplorable what's happened! I can hardly believe something like that went down in our nice li'l club!","Oh, that poor man...","It makes the dinner we had almost seem like the Last Supper, with someone of US as Judas.","Whush! Scary!"]
array desc_evidence_statement_kim_2 = ["The dinner? Yes, we went to an Asian restaurant just before.","It was, hmm... Was it Gregor's idea, maybe? I'm not sure, I'm sorry.","They had the most delicious fish soup. I think we had the same all of us.","Or at least we all ate some kind of fish soup, I'm afraid I was too occupied with my own plate.","And the upcoming match, of course. It WAS rather serious.","I just wish it hadn't ended even MORE serious..."]

array desc_evidence_statement_gregor_1 = ["Mr. Gregor is one of the three suspects. His testimony is as follows:","","A terrible crime, this. And in the middle of a game!","Outrageous, I say! Blasphemous, at least!"]
array desc_evidence_statement_gregor_2 = ["Now, I don't know what that woman has told you, but it MIGHT be worth noting that it was Anna who dealt the cards.","Just after she'd done that, the lad keeled over. Klaus, that is. Not Anna. She's not a lad, nor did she keel over.","There was something fishy going with the way she dealt those cards, I'm sure of it...","Something quite peculiar, I mean.","No, I don't have any proof. Sorry I can't be of more help."]

array desc_crime_n = ["The card game club has closed for the night.","That said, you COULD break in...","Those years lockpicking in San Jose could sure be of use."]
array desc_crime_n_picked_lock = ["You pick the lock deftly. Surely no-one will notice it come morning!","You don't want to flick the light on, for fear of being noticed, so it's difficult and time-consuming to find your way around the dark halls.","Even though the club's been abandoned for several hours, the strong cigarette smell still lingers.","Maybe it wouldn't be a bad idea to take a break and a smoke..."]
array desc_crime_n_break = ["It doesn't take much rummaging to find an already open pack of cigs.","You've still got a habit of carrying around a lighter in your pocket, even after all these years.","At first inhale, your lungs remind you with a coarse cough of why you quit, but what damage could one more do?","","Ahh... Inhale. Exhale."]
array desc_crime_n_look_more = ["Some more scouring turns out a tossed and wrinkled piece of paper.","You don't dare to take it with you, but in a rough handwriting it reads as follows:","","G: 50M on K","A: against","","At the bottom left corner there's something else wriggled, in a different and prettier style:","this ones mine Gregs :)"]
array desc_crime_am = ["The card game club is open, but the only one who's around is Ms. Anna; one of the key suspects.","It seems the incident hasn't made her abandon her hobby.","As you approach, she turns to you and wriggles her hands.","''Have you caught the culprit yet? Or is he still afoot..?''","You reassure her that the case is still being worked on and that you're counting on her cooperation, come trial.","Supposedly calmed, she invites you to a game of cards."]
array desc_crime_am_play = ["You accept the offer, half-expectant to be crushed by this professional in a game you've only played on the off nights out.","Surprisingly, the game ends quite greatly in your favour.","She laughs it off as going easy on an amateur, and you come to wonder just how much luck this game really has."]
array desc_crime_pm = ["You enter the card club at its peak hours.","The whole place is smack full of all kinds of people; but none of the three key suspects are anywhere to be seen.","As you scout the room for telltale signs, one party calls out to you."]
array desc_crime_pm_talk = ["''Oy mate, ye look like the kinda ''","''Ye're a cop, aintcha? Lookin' into that killing, aintcha?''","''Jes so ye's know, them Kim and Gregs are common round here, but the other two not so much.''","''Who's the better of 'em? Why, Kim fo' chrissake! Ain't none can ace that lass, and Gregs knows it.''"]

array desc_restaurant_n = ["You reach a homely-looking Japanese restaurant in fancier parts of the docks district.","This time of night it's long since closed down, but the lock looks susceptible to tinkering..."]
array desc_restaurant_n_lock = ["You start to tinker with the lock when a patroling police officer approaches.","You immediately step away and look innocent, but the officer keeps her eyes on you."]
array desc_restaurant_am = ["You enter a quaint Japanese restaurant.","It's still too early in the day for dinners, and they don't appear to serve lunches here, but it's open for bookings, supposedly.","That said, you can't seem to see any waiters, or any staff for that matter, but there's a barrage of outlandish curses coming from the kitchen."]
array desc_restaurant_am_talk = ["You slowly step into the kitchen, peering around the doorframe.","Inside, a woman in a speckled apron is throwing bowls and a tantrum.","''I swear, that darn fish has been STOLEN! Grr!''","''My precious fish, stolen! Grr, grr!!''","''I'll never take another toilet break, by Amaterasu!''","She keeps cursing for a while, before sitting down a sighing.","''And to top it off, one of those blasted card players tossed a whole bowl of my soup.''","''MY SOUP! Like it was nothing but dishwater! Grr, grr, grr!!!''"]
array desc_restaurant_pm = ["You enter a comfy Japanese restaurant.","The waiter, a short, stout man in what looks like his late forties, approach you with a smile as broad as his belly.","''Would you like a table for one, sir, or are you expecting company?''"]
array desc_restaurant_pm_fugu = ["''But I can see you're here for our specialty!'' he says with a glint in his eyes.","''But I'm afraid you're out of luck... We're currently out of our delicate fugu fish.''","''Oh, please don't make that face, sir! It's perfectly healthy and safe to eat. We're experts in its preparation and I assure you that you won't find a more pleasing serving of it anywhere!''","''The Michelin experts themselves have given us two stars on behalf of that dish, if I may be so bold to say.''"]
array desc_restaurant_pm_company = ["''Oh, I believe I know who you mean. They were here just the other day, for our fish soup.''","''I can heartily recommend, sir, that you give it a try. I promise you won't regret it, oh no!''","''If they seemed suspicious...? Why, that's a suspicious thing to ask, if you don't mind me saying...''","''But I can see you're the determined sort, so okay!''","''There WAS one thing that struck me about them... One of the men seemed very anxious and kept glancing around.''","''I was trying to keep an eye on him, because I feared he might be filcher.''","''Someone who has a little too quick fingers, if you know what I mean.''","''I don't think he ever did anything, though, but I can't say for sure. I had other duties, after all.''"]

#Scenes
scenes[scene_main_index] = [options_main, desc_main]
scenes[scene_court_n_index] = [options_court_n, desc_court_n]
scenes[scene_court_am_index] = [options_court_am, desc_court_am]
scenes[scene_court_pm_index] = [options_court_pm, desc_court_pm]
scenes[scene_court_pm_killer_index] = [options_court_pm_killer, desc_court_pm_killer]
scenes[scene_court_pm_cause_index] = [options_court_pm_cause, desc_court_pm_cause]
scenes[scene_court_pm_motive_index] = [options_court_pm_motive, desc_court_pm_motive]
scenes[scene_court_pm_proofs_index] = [options_court_pm_proofs, desc_court_pm_proofs]
scenes[scene_court_pm_verdict_index] = [options_court_pm_verdict, desc_court_pm_verdict]
scenes[scene_evidence_index] = [options_evidence, desc_evidence]
scenes[scene_evidence_cards_index] = [options_evidence, desc_evidence_cards]
scenes[scene_evidence_score_index] = [options_evidence, desc_evidence_score]
scenes[scene_evidence_statement_anna_index] = [options_evidence, desc_evidence_statement_anna]
scenes[scene_evidence_statement_kim_index] = [options_evidence, desc_evidence_statement_kim]
scenes[scene_evidence_statement_kim_2_index] = [options_evidence, desc_evidence_statement_kim_2]
scenes[scene_evidence_statement_gregor_1_index] = [options_evidence, desc_evidence_statement_gregor_1]
scenes[scene_evidence_statement_gregor_2_index] = [options_evidence, desc_evidence_statement_gregor_2]
scenes[scene_briefing_index] = [options_briefing, desc_briefing]
scenes[scene_briefing_2_index] = [options_briefing_2, desc_briefing_2]
scenes[scene_crime_n_index] = [options_crime_n, desc_crime_n]
scenes[scene_crime_n_picked_lock_index] = [options_crime_n_picked_lock, desc_crime_n_picked_lock]
scenes[scene_crime_n_break_index] = [options_crime_n_break, desc_crime_n_break]
scenes[scene_crime_n_look_more_index] = [options_crime_n_look_more, desc_crime_n_look_more]
scenes[scene_crime_am_index] = [options_crime_am, desc_crime_am]
scenes[scene_crime_am_play_index] = [options_crime_am_play, desc_crime_am_play]
scenes[scene_crime_pm_index] = [options_crime_pm, desc_crime_pm]
scenes[scene_crime_pm_talk_index] = [options_crime_pm_talk, desc_crime_pm_talk]
scenes[scene_restaurant_n_index] = [options_restaurant_n, desc_restaurant_n]
scenes[scene_restaurant_n_lock_index] = [options_restaurant_n_lock, desc_restaurant_n_lock]
scenes[scene_restaurant_am_index] = [options_restaurant_am, desc_restaurant_am]
scenes[scene_restaurant_am_talk_index] = [options_restaurant_am_talk, desc_restaurant_am_talk]
scenes[scene_restaurant_pm_index] = [options_restaurant_pm, desc_restaurant_pm]
scenes[scene_restaurant_pm_fugu_index] = [options_restaurant_pm_fugu, desc_restaurant_pm_fugu]
scenes[scene_restaurant_pm_company_index] = [options_restaurant_pm_company, desc_restaurant_pm_company]

var GetSceneOptions()
	var scene = scenes[scene_index]
	return scene[0]
end

array GetSceneDesc()
	if(scene_index == scene_court_pm_proofs_index)
		return GetProofs()
	else if(scene_index == scene_court_pm_verdict_index)
		return GetVerdict()
	else
		var scene = scenes[scene_index]
		return scene[1]
	end
end

string GetOptionText(var option, bool selected)
	if(selected)
		return option[1]
	else
		return option[0]
	end
end

number GetOptionDestination(var option)
	return option[2]
end

bool GetOptionResetChoice(var option)
	if (Count(option) >= 4)
		return option[3]
	else
		return true
	end
end

void MakeChoice()
	if (option_index < 0 || option_index >= Count(GetSceneOptions()))
		return
	end

	# Store what we say if we're in court.
	if(scene_index >= scene_court_pm_killer_index && scene_index <= scene_court_pm_motive_index)
		proof[scene_index] = option_index
	end

	var options = GetSceneOptions()
	var option = options[option_index]
	scene_index = GetOptionDestination(option)
	
	if(GetOptionResetChoice(option))
		option_index = 0
	end
end

array GetProofs()
	var scene = scenes[scene_court_pm_proofs_index]
	array res = CopyArray(scene[1])
	res[Count(res)] = "The killer is " + GetOptionText(options_court_pm_killer[proof[scene_court_pm_killer_index]], false)
	res[Count(res)] = "The cause of death was " + GetOptionText(options_court_pm_cause[proof[scene_court_pm_cause_index]], false)
	res[Count(res)] = "The motive was " + GetOptionText(options_court_pm_motive[proof[scene_court_pm_motive_index]], false)
	return res
end

array GetVerdict()
	var scene = scenes[scene_court_pm_verdict_index]
	array res = CopyArray(scene[1])
	if(proof[scene_court_pm_killer_index] == 0 && proof[scene_court_pm_cause_index] == 4 && proof[scene_court_pm_motive_index] == 1)
		res[Count(res)] = GetSuccessText()
	else
		res[Count(res)] = GetFailText()
	end
	return res
end

string GetSuccessText()
	return "GUILTY! HE'S GUILTY! Damn fine work!"
end

string GetFailText()
	return "NOT GUILTY! That evidence doesn't add up with that suspect..."
end

bool OnInput(number dt)
	bool res = false
	var lastIndex = option_index
	if(IsKeyPressed("space"))
		if(was_pressed_space == false)
			MakeChoice()
			res = true
		end
		was_pressed_space = true
	else
		was_pressed_space = false
	end
#	if(IsKeyPressed("left"))
#		if(was_pressed_left == false)
#			SetTimeOfDay(-1)
#			res = true
#		end
#		was_pressed_left = true
#	else
#		was_pressed_left = false
#	end
#	if(IsKeyPressed("right"))
#		if(was_pressed_right == false)
#			SetTimeOfDay(1)
#			res = true
#		end
#		was_pressed_right = true
#	else
#		was_pressed_right = false
#	end
	if(res == true)
		return true
	end
	option_index = HandleListInput(GetSceneOptions(), option_index)
	return lastIndex != option_index
end

void DrawTime()
	Print("                        " + GetTimeDesc())
	Print("")
	Print("")
	Print("")
end

void DrawSceneDesc()
	array desc = GetSceneDesc()
	loop line in desc
		Print(line)
	end
	Print("")
	Print("")
end

void DrawOptions(var options)
	number index = 0
	loop option in options
		Print(GetOptionText(option, index == option_index))
		index++
	end
end

bool modified = true
loop
	number dt = Time() - lastT
	lastT = Time()
	modified = OnInput(dt) || modified
	if modified
		modified = false
		ClearText()
		DrawTime()
		DrawSceneDesc()
		DrawOptions(GetSceneOptions())
	end
end
```