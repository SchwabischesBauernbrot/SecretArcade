```
# sstar b*rd!
# for best results, screwdriver the arcade to 500mhz!

DisplayGraphics()#clear the buffer

var colors = []
colors[0] = [0.1627, 0.1078,0] # brown red
colors[1] = [0.6027, 0.4,0] #gold
colors[2] = [0,0,0] #black
colors[3] = [0.2627, 0.2778,0] #red
colors[4] = [0.7026, 0.5, 0.3] #cream
var blue = [0.1,0.1,0.8]

var bg_size = [512,256]

array game_objects = []

array inactive = []
inactive["bullet"] = []
inactive["enemy"] = []
array enemies = []

#intro scene
var intro = [[58,71,43,76],[43,76,59,85],[59,85,47,98],[68,68,71,101],[64,68,77,68],[88,68,83,104],[91,69,122,98],[88,87,103,83],[124,64,124,107],[125,65,155,80],[155,80,134,83],[134,83,156,112],[188,52,193,142],[193,142,217,124],[217,124,197,119],[247,95,226,117],[222,96,247,117],[238,97,238,116],[269,109,269,147],[271,113,290,112],[290,112,290,112],[290,112,297,119],[337,122,310,113],[310,113,310,138],[310,138,336,142],[336,142,337,85]]
var intro_guy = [[417,146,389,149],[389,149,368,170],[368,170,383,193],[384,193,412,191],[412,191,429,176],[429,176,415,147],[381,158,395,165],[411,156,396,161],[401,160,402,166],[386,162,386,167],[381,181,388,175],[388,175,402,175],[402,175,410,177],[407,192,415,221],[415,221,405,246],[414,221,429,241],[405,194,376,206],[376,206,358,199],[409,195,441,199],[441,199,458,213],[361,191,351,209],[351,209,344,205],[344,205,355,188],[355,188,359,191],[359,191,336,173],[336,173,329,183],[329,183,351,192],[342,189,340,192],[340,192,348,196],[352,194,346,192]]

array story_scene = []
story_scene[0] = [[84,22.5,55,33.5],[55,33.5,71,47.5],[71,47.5,105,45],[105,45,113,33],[113,33,82,24.5],[70,30,79,37.5],[88,28,93,33.5],[81,41,88,36],[88,36,103,36],[103,36,99,39],[99,39,89,38.5],[89,38.5,86,44.5],[79,40.5,88,43.5],[100,46,139,80],[120,61.5,43,56],[43,56,53,36.5],[115,59,155,34.5],[163,34.5,89,20.5],[70,21.5,66,33],[72,21.5,108,28],[137,78,110,109],[138,78,186,115.5]]
story_scene[1] = [[155,12,183,7.5],[167,6,170,18],[192,3.5,186,19],[183,19,192,10.5],[192,10.5,204,18.5],[214,12.5,228,13.5],[227,13.5,219,8],[219,8,207,13],[207,13,221,19.5],[265,12,255,6.5],[255,6.5,246,13.5],[246,13.5,264,16],[264,16,254,21.5],[253,21.5,246,18],[284,8.5,278,20.5],[273,13.5,292,11],[302,14,291,16],[291,16,299,22],[299,22,310,19],[310,19,301,14.5],[309,17.5,315,22.5],[324,14.5,323,21],[327,16.5,333,14],[359,10,350,7.5],[350,7.5,343,13.5],[343,13.5,354,15.5],[354,15.5,349,22.5],[349,22.5,342,20],[373,18.5,383,14],[383,14,389,20],[389,20,378,25.5],[377,25.5,374,18.5],[387,18,394,23.5],[410,13,409,23],[411,17,419,14],[419,14,432,14],[434,16,450,16.5],[450,16.5,444,11.5],[444,11.5,431,19.5],[431,19.5,443,23.5],[196,39.5,190,35],[190,35,179,41.5],[179,41.5,192,49],[192,49,195,40.5],[195,40.5,204,44.5],[216,31.5,219,45.5],[206,38,227,34.5],[243,29.5,239,43],[226,33.5,248,33.5],[254,39.5,264,32.5],[264,32.5,280,36.5],[280,36.5,269,44],[269,44,255,39],[279,37,287,40.5],[301,32.5,294,37],[294,37,308,41.5],[315,31,317,43.5],[320,37,329,33],[320,38,329,42],[342,37.5,343,44.5],[344,33.5,345,35],[354,39.5,353,46],[354,42,361,36],[361,36,377,45.5],[400,38,388,37],[388,37,392,44.5],[392,44.5,401,42.5],[401,42.5,401,37.5],[401,37.5,402,50.5],[402,50.5,386,54.5],[424,30.5,418,43.5],[416,48.5,415,51.5],[439,30.5,434,44.5],[434,50.5,430,57],[138,54.5,163,51],[163,51,197,67],[197,67,453,70.5],[453,70.5,477,13.5],[477,13.5,283,2],[283,2,126,3.5],[126,3.5,162,42],[162,42,140,52]]
story_scene[2] = [[184,157,102,159],[102,159,54,192],[54,192,88,230],[88,230,209,233.5],[209,233.5,250,198.5],[250,198.5,175,156.5],[113,180.5,108,204],[168,180.5,173,206],[100,174,123,185.5],[183,174,150,185.5],[151,231,152,253],[154,236.5,226,255],[325,210.5,291,255.5],[291,255.5,291,255.5],[300,203.5,346,217],[301,204,312,176],[312,176,336,178.5],[336,178.5,318,199],[318,199,359,205],[359,205,345,217],[324,189.5,342,193],[342,193,337,200.5],[321,199,336,197]]
story_scene[3] = [[248,130,246,139],[247,131,262,130.5],[262,130.5,265,140.5],[282,130,274,133.5],[274,133.5,283,139.5],[283,139.5,291,138],[291,138,297,131.5],[297,131.5,280,129],[309,124.5,309,136.5],[301,128,317,128],[347,129.5,332,133],[332,133,338,139],[339,139,351,140.5],[351,140.5,352,133.5],[352,133.5,344,130.5],[367,132.5,362,139.5],[366,133,378,133],[378,133,389,138],[413,137.5,421,131],[421,131,428,134.5],[428,134.5,431,130.5],[431,130.5,447,138],[450,130.5,453,137],[453,137,464,134.5],[464,134.5,464,129.5],[464,129.5,464,140],[463,140,456,143],[261,148,274,153],[274,153,287,149],[288,148.5,294,152.5],[294,152.5,306,147],[328,148,317,149.5],[317,149.5,320,155],[320,155,334,155.5],[334,155.5,338,152],[338,152,329,148],[339,150.5,338,158],[337,158,347,158.5],[357,147,353,155],[351,149.5,357,149.5],[364,150.5,351,150],[386,148,376,151.5],[376,151.5,374,157],[374,157,383,159],[403,146.5,397,156],[397,156,408,152.5],[408,152.5,417,157.5],[428,155.5,426,159],[445,157.5,445,159],[465,158.5,462,161.5],[474,122,291,107.5],[291,107.5,219,131],[219,131,244,157.5],[243,158.5,233,171],[233,171,268,163],[268,163,473,172.5],[473,172.5,503,140.5],[503,140.5,473,122]]
story_scene["progress"] = 0
story_scene["setup"] = false
story_scene["t"] = "scene"

var game_bg = []
game_bg["data"] = [0, 240, 512, 242]
game_bg["color"] = colors[3]

var game_scene = CreateObject()
game_scene["rect"] = game_bg
game_scene["setup"] = false
game_scene["t"] = "scene"

var badguy_lines = []
badguy_lines["data"] = [[34,86.5,71,76.5],[71,76.5,80,50],[81,50,102,71],[102,71,151,60],[151,60,120,84.5],[120,84.5,158,104],[158,104,102,93],[102,93,66,117],[66,117,80,89.5],[80,89.5,37,87],[79,74.5,86,81],[91,71,94,81],[77,69.5,85,73.5],[94,65.5,88,74.5],[80,81.5,102,84],[102,84,90,90.5],[90,90.5,81,82.5],[318,74,377,94],[377,94,420,56],[420,56,418,103.5],[418,103.5,485,125],[486,125,421,135.5],[422,135.5,433,183.5],[432,183.5,384,132],[384,132,308,154.5],[308,154.5,365,113],[365,113,314,75.5],[391,91,381,111.5],[408,91,401,114.5],[390,84.5,395,92],[412,82,402,93],[373,110,383,124.5],[383,124.5,427,119],[55,14.5,75,25],[75,25,75,16.5],[75,16.5,89,16.5],[89,16.5,90,27],[90,27,72,23],[102,14.5,105,24],[105,24,116,18],[116,18,125,24.5],[125,24.5,133,14.5],[137,19.5,145,15],[145,15,155,20],[155,20,144,24],[144,24,138,19.5],[156,19.5,162,24],[171,8,172,24],[174,19.5,183,15],[183,15,201,23.5],[217,14.5,202,19.5],[202,19.5,215,26.5],[215,26.5,225,22],[225,21.5,217,15],[224,22,231,28.5],[248,8.5,247,21.5],[247,21.5,247,28],[247,18.5,257,17.5],[257,17.5,272,26.5],[277,21.5,288,16.5],[288,16.5,304,24],[304,24,292,29],[292,29,276,21],[303,22.5,306,29],[324,10,320,26],[319,28.5,317,32],[97,33.5,100,44],[96,32.5,109,34.5],[109,34.5,112,42],[132,31,119,36.5],[119,36.5,127,43],[127,43,137,39],[137,39,132,32],[146,32.5,153,42.5],[153,42.5,163,37.5],[163,37.5,168,45],[168,45,182,35.5],[196,31,201,43.5],[192,35,202,34.5],[210,31.5,213,43],[212,39.5,221,35.5],[221,35.5,224,41],[235,37.5,246,38],[246,38,240,33],[240,33,230,36],[230,36,237,42.5],[266,34.5,272,42.5],[273,42.5,279,39],[279,39,286,44.5],[286,44.5,293,36],[307,38,298,41],[299,41.5,306,48],[309,38,316,41.5],[316,41.5,301,46.5],[330,38.5,325,44],[330,39,341,38.5],[355,32,349,45.5],[352,44.5,360,44],[379,40,367,39],[367,39,368,45.5],[368,45.5,377,46.5],[377,46.5,379,29.5],[404,35.5,402,45.5],[405,30,403,31.5],[432,34,415,38],[415,38,426,41],[426,41,414,45.5],[185,55,173,62],[173,62,184,68],[184,68,196,61],[196,61,185,56.5],[203,58.5,203,67],[203,67,221,69],[221,69,221,60.5],[229,59.5,227,69.5],[229,62,239,58.5],[239,58.5,245,61.5],[261,58,250,62.5],[250,62.5,258,67],[258,67,243,72.5],[273,53.5,268,66.5],[267,70.5,263,75.5],[333,10.5,330,24.5],[330,28.5,328,31],[228,3.5,452,10],[453,10,471,46],[471,46,297,65.5],[297,65.5,285,84],[285,84,173,80],[173,80,155,71],[103,54,101,80],[101,78,124,50.5],[124,50.5,150,53.5],[150,53.5,157,72.5],[101,55,26,15.5],[26,15.5,115,4],[115,4,230,4],[52,13,78,11.5],[78,11.5,74,16]]
badguy_lines["color"] = colors[4]
var lose_badguys = []
lose_badguys["lines"] = badguy_lines
lose_badguys["t"] = "graphic"
lose_badguys["position"] = [0,0]

var deadguy_lines = []
deadguy_lines["data"] = [[231,158.5,181,158],[181,158,164,168.5],[163,168.5,201,187.5],[197,187.5,257,186],[254,185,252,172],[252,172,226,159],[210,163,185,165.5],[189,160,203,167],[236,169.5,215,173],[216,169,232,174.5],[188,172.5,223,182.5],[203,178,202,182.5],[202,182.5,214,182.5],[185,179.5,117,203.5],[118,203.5,72,195],[72,195,36,196.5],[116,203,127,219],[127,219,72,235.5],[152,188,126,179.5],[126,179.5,174,152],[154,190.5,247,205],[247,205,220,225],[177,215,203,224.5],[203,224.5,179,240.5],[179,240.5,166,235.5],[166,235.5,186,228],[186,228,171,221],[171,221,177,218],[177,223.5,172,227],[172,227,175,231],[184,228.5,176,228.5]]
deadguy_lines["color"] = blue
var lose_deadguy = []
lose_deadguy["lines"] = deadguy_lines
lose_deadguy["t"] = "graphic"
lose_deadguy["position"] = [0,0]

var winguy_lines = []
winguy_lines["data"] = [[130,2,118,86],[118,86,194,142.5],[194,142.5,300,158],[300,158,384,125],[384,124.5,412,40],[412,40,410,3],[230,106.5,273,105],[273,105,307,97],[306,97.5,300,109.5],[299,109.5,281,118.5],[281,118.5,242,118.5],[242,118.5,205,111.5],[205,111.5,234,106.5],[285,105,355,114],[355,113.5,342,123.5],[342,123.5,286,108.5],[159,21.5,254,36],[254,35.5,207,56.5],[207,56.5,157,23],[157,23,118,12],[253,36,367,36],[367,36,326,61],[326,61,263,35.5],[363,35.5,410,18.5],[367,38,415,29.5],[160,27,132,23],[251,17,250,64.5],[250,64.5,234,72],[234,72,246,80],[246,80,261,80],[261,80,283,75],[283,75,270,67],[270,67,264,21.5],[175,9.5,246,30.5],[282,31.5,371,19],[141,58,140,89.5],[140,87.5,178,114],[405,65,374,109.5],[373,110.5,344,133.5],[90,0,76,95.5],[76,93.5,123,111],[123,111,113,146],[113,146,65,144],[65,144,76,92.5],[68,142.5,285,172],[285,172,285,172],[285,172,257,220],[257,220,26,182.5],[26,182.5,29,2],[68,142.5,106,122],[94,246.5,88,133],[280,179,333,186.5],[333,186.5,342,171],[342,171,312,230.5],[317,217,265,209.5],[221,108,237,111],[237,111,247,107],[247,107,260,112],[260,112,268,104.5],[268,104.5,282,111],[282,111,293,101.5],[236,117.5,251,114.5],[251,114.5,262,117.5],[263,117,276,112],[276,112,285,117],[318,149.5,511,179.5],[304,205.5,508,209],[189,140,150,154.5],[139,199.5,108,249.5],[198,27,245,38],[312,37,358,40],[277,187,303,191],[303,191,288,204.5],[288,204.5,271,192.5],[346,105.5,333,100.5],[333,100.5,348,85.5],[347,69,365,85],[364,85,351,96.5],[339,111.5,342,123.5],[346,112.5,343,122]]
winguy_lines["color"] = blue
var winguy = []
winguy["lines"] = winguy_lines
winguy["t"] = "graphic"
winguy["position"] = [0,0]

var yeah_lines = []
yeah_lines["data"] = [[39,-30.5,17,-21.5],[17,-21.5,25,-12.5],[40,-30.5,69,-30],[67,-29.5,75,-21],[74,-21,64,-13.5],[64,-13.5,24,-13],[36,-24,35,-17],[54,-24.5,50,-16],[29,-25.5,40,-24],[59,-25.5,47,-23.5],[35,-53,25,-49.5],[25,-49.5,30,-44.5],[30,-44.5,38,-45.5],[38,-45.5,35,-51],[43,-57,43,-45.5],[43,-45.5,48,-49.5],[48,-50,52,-46],[20,-43,23,-39.5],[23,-39.5,32,-41],[32,-41.5,21,-33.5],[36,-39.5,52,-39],[52,-39,43,-44],[43,-44,35,-40],[35,-40,44,-35],[53,-37.5,61,-41.5],[61,-41.5,68,-37],[68,-37,56,-35],[56,-35,51,-37.5],[68,-37,68,-34],[74,-46.5,72,-35],[72,-35,76,-37.5],[76,-37.5,84,-32],[90,-45.5,90,-35.5],[92,-30.5,91,-30.5],[15,-12.5,0,-33],[0,-33,28,-60.5],[28,-60.5,107,-50.5],[107,-50.5,90,-15.5],[90,-15.5,46,-5],[46,-5,24,-8],[24,-8,0,0],[0,0,16,-13]]
yeah_lines["color"] = blue
var yeah = []
yeah["lines"] = yeah_lines
yeah["t"] = "graphic"
yeah["position"] = [0,0]

#[time before spawn, payload, param]
var levels =  [ [[[2, 2],[2.5, 2],[2.9, 2]],[[1, 2, 1],[1.3, 2, 1],[1.6, 2, 1]],[[2, 2],[2.3, 2, 1],[2.9, 2],[2.9, 2, 1]] ],  [[[2.5, 2],[2.8, 2, 1],[3.4, 2],[3.4, 2, 1],[3.4,3],[3.7,3],[4,3]],[[2, 2],[2, 2, 1],[2,3],[3, 2],[3, 2, 1],[3,3]]],  [[[2, 4],[2.3, 2, 1],[2.9, 2],[2.9, 3, 1],[2.9,3],[3.2,4],[3.5,3]],[[2, 2],[2, 2, 1],[2,3],[3, 4],[3, 2, 1],[3,4]]] ]
var current_level = 0
var current_level_data = []
var current_bucket=-1
var enemies_left_in_bucket=0

var level_text = [[115,86,118,110.5],[118,110.5,128,113.5],[145,108.5,168,108.5],[168,108.5,155,100.5],[155,100.5,145,109.5],[145,109.5,158,119.5],[182,104.5,190,119.5],[190,119.5,203,101.5],[218,108.5,261,108],[261,107.5,238,98],[238,98,216,116.5],[216,116.5,235,122],[277,89.5,268,122],[268,122,293,120.5]]
var numbers = [[[347,92,363,79.5],[363,79.5,357,114.5],[358,114.5,331,113],[332,113,379,116.5]],[[349,86,392,77.5],[392,77.5,426,84],[426,84,356,112],[356,112,405,126.5]],    [[337,85,364,79],[364,79,379,90.5],[379,90.5,353,96.5],[353,96.5,376,99],[376,99,376,108.5],[376,108.5,351,115.5],[390,73.5,397,96.5],[399,104.5,396,111],[408,70,413,93.5],[415,101,415,110]],    [[337,78.5,323,99],[323,99,349,102.5],[341,95,333,106],[365,79,374,76.5],[374,76,384,82],[383,82,368,89.5],[368,89.5,368,98.5],[370,104.5,370,105],[395,69,395,90.5],[393,100,393,103.5],[408,68.5,411,95],[415,98.5,416,104.5]],    [[408,75.5,358,77],[358,77,359,92],[359,92,394,91],[394,91,387,111],[387,111,352,109.5],[344,124.5,361,122],[377,121,396,119],[355,123,354,127.5],[380,121,382,123.5],[348,136,359,130.5],[359,130.5,386,128.5],[386,128.5,392,136],[351,115,340,122],[396,113,405,118],[351,134.5,359,141],[366,141,386,138.5],[417,72.5,418,86.5],[416,97.5,416,103]]]

var timers = []
var timer_id = 0
#pixels per second
var PLAYER_SPEED = 250
var BULLET_SPEED = 200
var SP_SPEED = 170
var FALL_SPEED = 120

var ENEMY_HEIGHT = 15

var STORY_DELAY = 1.5
var PREGAME_DELAY = 1.5
var START_POSITION = [100,230]

var state = 0

var alive_enemies = 0
var score = 0
var killed = 0
var lives = 0
var max_lives = 5

#input 
var keys = ["up","down","left","right","space"]
array input = []
array previous_input = []
loop key in keys
    previous_input[key] = false
end

var time = Time()
number dt = 0

#some temp vars to prevent allocations happening everywhere
array object_position 
array kinematic_data
array rect_info
array data
array color
array offset
array em_data

array r1
array r2
array rd1
array rd2
array p1
array p2

#debug stuff
array test_attr = [[0,0], [0,0], [0,0],["n/a","n/a"]]
array test_label = ["position", "speed","acceleration", "create enemy"]

number selected_attr = 0
number selected_index = 0
var active_index = [false]

bool debug = false
## Game Loop
loop       
    loop key in keys
        input[key] = IsKeyPressed(key)
    end

    dt = Time() - time
    time = Time()

    if debug
        #Print("dt " + dt + " fps " + Round(1/dt))#should average this
        #Print("GameObjects " + Count(game_objects))
       # Print(game_objects)
    else
        ClearText()
    end

    #background
    Color(0.1627, 0.1078,0)
    Rect(0,0,bg_size[0], bg_size[1])
    
    if state == 0
        Intro()
    else if state == 1
        Story()
    else if state == 2
        Game(dt)
    else if state == 3   
        Lose()
    else if state == 4
        Win()  
    end 

    loop object in game_objects
        if object["active"]
            UpdateObject(object, dt)
            DrawObject(object)
        end
    end

    if dt < 0.032 #lol
        Sleep(0.032 - dt)
    end

    loop key in keys
        previous_input[key] = input[key]
    end

    DrawHud()
    DisplayGraphics()    
end

void UpdateObject(array object, number dt)    
    object_position = object["position"]

    if HasIndex(object, "kinematic")
        kinematic_data = object["kinematic"]
        kinematic_data["x_vel"] = kinematic_data["x_vel"] + (kinematic_data["x_acc"] * dt)
        kinematic_data["y_vel"] = kinematic_data["y_vel"] + (kinematic_data["y_acc"] * dt)
        object_position[0] = object_position[0] + (kinematic_data["x_vel"] * dt)
        object_position[1] = object_position[1] + (kinematic_data["y_vel"] * dt)
        object["position"] = object_position
    end

    if object_position[1] < -10 or object_position[1] > 270
        AddToPool(object)
    end
    
    if HasIndex(object, "enemy_movement")
        UpdateEnemy(object)
    end

    if HasIndex(object, "t") and object["t"] == "bullet" 
       if enemies_left_in_bucket > 0
            loop enemy in enemies
                if enemy["active"] and Collision(object, enemy)
                    PlaySound("Powerup 3")

                    Kill(enemy)
                    Kill(object)
                end 
            end
        end
    end
end     

void DrawObject(array object)
    object_position = object["position"]
    if HasIndex(object, "rect")    
        rect_info = object["rect"]
        data = rect_info["data"]
        color = rect_info["color"]        
        Color(color[0],color[1],color[2])
        Rect(data[0] + object_position[0], data[1] + object_position[1], data[2] + object_position[0] ,data[3] + object_position[1])
    else if HasIndex(object, "lines")
        var line_info = object["lines"]
        color = line_info["color"]
        Color(color[0],color[1],color[2])
        loop line in line_info["data"]
            Line(line[0] + object_position[0], line[1] + object_position[1], line[2] + object_position[0] ,line[3] + object_position[1])
        end
    end
end

void DrawHud()
    if state == 2
        if lives > 0 #draw lives
            var red = colors[3]
            loop i in from 0 to lives - 1
                Color(red[0], red[1], red[2])
                Rect(485 - ( i * 21), 5, 505 - ( i * 21), 15) 
            end
        end
    end 
end

## States
void Intro()
    if KeyReleased("space")
        PlaySound("Coin 3")
        Sleep(0.5)

        NextState()
        return
    end
    color = colors[4]
    Color(color[0],color[1],color[2])
    Rect(0,100,512,145)

    color = colors[1]
    Color(color[0],color[1],color[2])

    loop line in intro
        Line(line[0],line[1],line[2],line[3])
    end

    Color(blue[0], blue[1], blue[2])
    loop line in intro_guy
        Line(line[0],line[1],line[2],line[3])
    end
end

void Story()  
    if story_scene["setup"] != true
        story_scene["setup"] = true
    else
        Sleep(STORY_DELAY)
    end

    if story_scene["progress"] >= 4
        game_objects = []
        NextState()
        return
    end

    var step
    var data

    data =[]

    if story_scene["progress"] >= 2
        data["color"] = blue
    else
        data["color"] = colors[1]
    end

    data["data"] = story_scene[story_scene["progress"]]
    step = CreateObject()
    step["lines"] = data
    step["t"] = "step"
    story_scene["progress"] = story_scene["progress"] + 1
end

void Game(number dt)
    var position

    if game_scene["setup"] != true
        SetupGame()
    else
        if debug
            DebugMenu() 
        else
            if current_bucket < 0
                current_bucket = 0
                enemies_left_in_bucket = 0
                current_level_data = levels[current_level]
                Print("setup bucket" + current_level_data[current_bucket])
                SetupBucket(current_level_data[current_bucket])
            end     
            Print("Score : " + score)
            UpdatePlayer()
            if KeyReleased("space") 
                FireBullet()
            end
        end    
        UpdateTimers(dt)     
    end    
end

void SetupGame()
    if debug == false
        DisplayPreLevelScreen()
    end
    killed = 0   
    lives = max_lives
    current_bucket = -1
    if HasIndex(game_scene, "player") != true
        Append(game_objects, game_scene)

        var player = Player()
        game_scene["player"] = player
        score = 0
    end
    StartPosition(game_scene["player"])
    game_scene["setup"] = true
end

void DisplayPreLevelScreen()
    var pre_title = []
    var lines = []

    pre_title["position"] = [0,0]
    lines["data"] = level_text
    lines["data"] = lines["data"] + numbers[current_level]
    lines["color"] = colors[1]

    pre_title["lines"] = lines

    DrawObject(pre_title)
    DisplayGraphics()    

    Sleep(PREGAME_DELAY)
end

void DebugMenu()
    if active_index[0] == false
        if KeyReleased("up")
            selected_attr = selected_attr - 1
            if selected_attr < 0
                selected_attr = Count(test_attr) - 1
            end
            DebugUpdateLabels()
        end 
        if KeyReleased("down")
            selected_attr = selected_attr + 1
            if selected_attr > Count(test_attr) - 1
                selected_attr = 0
            end
            DebugUpdateLabels()
        end    
    else
        var attr = test_attr[selected_attr]
        if IsKeyPressed("up")
            attr[selected_index] = attr[selected_index] + 1
        end 
        if IsKeyPressed("down")
            attr[selected_index] = attr[selected_index] - 1
        end
        DebugUpdateLabels()
    end

    if KeyReleased("left")
        selected_index = selected_index - 1
        if selected_index < 0
            selected_index = 1
        end
        DebugUpdateLabels()    
    end

    if KeyReleased("right")
        selected_index = selected_index + 1
        if selected_index > 1
            selected_index = 0
        end
        DebugUpdateLabels()
    end

    if KeyReleased("space")
        if active_index[0] == false
            if selected_attr == Count(test_attr) - 1
                DebugCreateTimer()
                #DebugCreateEnemy()
            else
                active_index = [selected_attr]
            end
        else
            active_index = [false]
        end
        DebugUpdateLabels()
    end
end

void DebugUpdateLabels()
    ClearText()
    loop i from 0 to Count(test_attr) - 1
        var attr = test_attr[i]
        var attr_label = " - ["
        if i == active_index[0]
            attr_label = " *** - ["
        end
        attr_label = attr_label + attr[0]
        if i == selected_attr and  selected_index == 0
            attr_label = attr_label + "*"
        end
        attr_label = attr_label + "," + attr[1]
        if i == selected_attr and selected_index == 1
            attr_label = attr_label + "*"
        end
        attr_label = attr_label + "]"

        Print(test_label[i] + attr_label)
    end
end

void DebugCreateTimer()
    var t = Timer(2,[2, 1])
    t["active"] = true
end

void DebugCreateEnemy()
    DebugUpdateLabels()

    var position = test_attr[0]
    var speed = test_attr[1]
    var acc = test_attr[2]

    var level = levels[current_level]
    level[0] = 999 #
    var enemy = Enemy()
    var em = enemy["enemy_movement"]
    em["sp"] = false

    o_setPosition([position[0],position[1]],enemy)
    k_setSpeed(speed[0],speed[1], enemy)
    k_setAcc(acc[0],acc[1] enemy)
end

void Lose()
    loop from 0 to 12
        Print("")
    end
    Print("                  GAME OVER")
    Print("                  SCORE:" + score)

    game_objects = []
    DrawObject(lose_badguys)
    DrawObject(lose_deadguy)
end

void Win()
    var text = ["YOU","WIN!","the world is", "safe from those", "darn stars"," "," ","peace and love", "are now free once again", " "," ","reload your weapon and", "go home, sir"," "," ","the time of", "violence is over now"]

    loop t in text
        RPrint(t)
    end
    Print("")
    Print("")
    Print("")
    Print("")
    Print("")
    Print("                  SCORE:" + score + "!!!!")

    game_objects = []
    DrawObject(winguy)
    PlaySound("Arcade music")
    DisplayGraphics()
    Sleep(100)
end

## Logic 

void UpdatePlayer()
    var player = game_scene["player"]
    var position = player["position"]

    if IsKeyPressed("left") 
        k_setSpeed(-PLAYER_SPEED,0,player)
    else if IsKeyPressed("right")
        k_setSpeed(PLAYER_SPEED,0,player)
    else
        k_setSpeed(0,0,player)
    end

    if position[0] < 2
        o_setPosition([2, position[1]], player)
    end

    if position[0] > 510
        o_setPosition([510, position[1]], player)
    end
end

void FireBullet()
    var bullet = Bullet()

    var player = game_scene["player"]
    var position = player["position"]

    bullet["position"] = [position[0], position[1]]
    k_setSpeed(0, -BULLET_SPEED, bullet)

    PlaySound("Hit 2")
end

void Kill(array object)   
    if object["t"] == "enemy"
        score = score + 100 + Round(Random() * 100)

        killed = killed + 1
        DecreaseBucketCount()
    end

    AddToPool(object)     
end

void NextLevel()
    var player = game_scene["player"]
    yeah["position"] = player["position"]

    DrawObject(yeah)
    DisplayGraphics()
    Sleep(2)

    killed = 0
    current_level = current_level + 1
    if current_level >= Count(levels)
        state = 4 #win game!
    end
    game_scene["setup"] = false
    loop object in game_objects
       AddToPool(object)
    end
end

## Util
void KeyReleased(string key)
    return input[key] == false and previous_input[key] == true
end

void NextState()
    state++
    ClearText()
end

void RPrint(string msg)
    var char_length = 56
    var str = ""
    loop from 0 to char_length-Count(msg)-1
        str = str + " "
    end

    Print(str + msg)
end

void Collision(array o1, array o2)
    r1 = o1["rect"]
    r2 = o2["rect"]
    rd1 = r1["data"]
    rd2 = r2["data"]
    p1 = o1["position"]
    p2 = o2["position"]

    return Intersection([rd1[0] + p1[0], rd1[1] + p1[1], rd1[2] + p1[0], rd1[3] + p1[1]], [rd2[0] + p2[0], rd2[1] + p2[1], rd2[2] + p2[0], rd2[3] + p2[1]])
end

void Intersection(array rect1, array rect2)
    if rect1[3] < rect2[1] or rect1[1] > rect2[3] or rect1[0] > rect2[2] or rect1[2] < rect2[0]
        return false
    end

    return true
end

void AddToPool(array object)
    if HasIndex(inactive, object["t"])
        var pool = inactive[object["t"]]
        if object["active"] == true
            object["active"] = false
            Append(pool, object)
        end
    end
end

void ObjectFromPool(array pool)
    var object
    object = pool[Count(pool) - 1]

    if object["active"] != false
        Remove(pool, Count(pool) - 1)
        return [false]
    end

    object["active"] = true
    Remove(pool, Count(pool) - 1)   
    return [true, object]
end
    
void StartPosition(array object)
    object["position"] = [START_POSITION[0], START_POSITION[1]]
end

void Timer(number seconds_til_active, array payload)
    var timer = []
    timer["remaining"] = seconds_til_active
    timer["payload"] = payload
    timer["active"] = false
    timer["id"] = timer_id
    timer_id = timer_id + 1
    Append(timers, timer)
    return timer
end

void t_update(array timer, number dt)
    if timer["active"] == false
        return
    end
    timer["remaining"] = timer["remaining"] - dt
    if timer["remaining"] <= 0
        return true
    end
    return false
end

void t_trigger(array timer)
    DecodePayload(timer["payload"])
end

void UpdateTimers(number dt)
    if Count(timers) == 0
        return
    end
    #loop over each timer and update
    var active_timers = []
    var timer
    loop i from 0 to Count(timers) - 1
        timer = timers[i]
  
        var result = t_update(timer, dt)
        if result == true
            t_trigger(timer)
        else
            Append(active_timers, timer)
        end
    end

    timers = active_timers
end

void DecodePayload(array payload)
    var enemy
    if payload[0] == 1
        enemy = Enemy()
        o_setPosition([Random()*500 + 5, -5], enemy)
        Fall(enemy)
    else if payload[0] == 2
        enemy = Enemy()
        var em = enemy["enemy_movement"]
        if HasIndex(payload, 1)
            em["sweep_right"] = true
        end
        em["sweep_set"] = false
        Sweep(enemy)    
    else if payload[0] == 3
        #create sp
        enemy = Enemy() 
        o_setPosition([Random()*500 + 5, 0], enemy)
        SpaceInvaders(enemy)
        if Random() < 0.5
            k_setSpeed(SP_SPEED, 0, enemy)
        else
            k_setSpeed(-SP_SPEED, 0, enemy)
        end
    else if payload[0] == 4
        #create wiggler
    end 
end

void SetupBucket(array bucket)
    loop b in bucket
        enemies_left_in_bucket = enemies_left_in_bucket + 1 
        var payload = [b[1]]
        if HasIndex(b, 2)
            Append(payload, b[2])
        end
        var timer = Timer(b[0], payload)
        timer["active"] = true
    end
end

void DecreaseBucketCount()

    if enemies_left_in_bucket > 0
        enemies_left_in_bucket = enemies_left_in_bucket - 1
    end

    if enemies_left_in_bucket == 0
        current_bucket = current_bucket + 1

         if HasIndex(current_level_data, current_bucket)#next bucket
            SetupBucket(current_level_data[current_bucket])
        else
            NextLevel()
        end
    end
end

## game objects
void CreateObject()
    var new_object = []
    new_object["position"] = [0,0]
    new_object["active"] = true

    Append(game_objects, new_object)

    return new_object
end

void o_setPosition(array position, array object)
    object_position = object["position"]

    object_position[0] = position[0]
    object_position[1] = position[1]

    object["position"] = position
end

void o_movePosition(array delta, array object)
    object_position = object["position"]

    object_position[0] = object_position[0] + delta[0]
    object_position[1] = object_position[1] + delta[1]

    object["position"] = object_position
end

void Enemy()
    if Count(inactive["enemy"]) != 0
        var recycled = ObjectFromPool(inactive["enemy"])
        if recycled[0] == true
            em_reset(recycled[1])
            return recycled[1]
        end
    end

    var enemy = CreateObject()
    var rect = []
 
    rect["color"] = colors[4]
    rect["data"] = [0,0, 30, ENEMY_HEIGHT]
    rect["offset"] = [15,7.5]

    enemy["rect"] = rect
    enemy["t"] = "enemy"
    enemy["hp"] = 3
    enemy["size"] = 10

    Kinematic(enemy)
    EnemyMovement(enemy)

    Append(enemies, enemy)

    return enemy
end

void Bullet()
    if Count(inactive["bullet"]) != 0
       var recycled = ObjectFromPool(inactive["bullet"])
        if recycled[0] == true
            return recycled[1]
        end
    end
    
    array bullet = CreateObject()
    var rect = []
 
    rect["color"] = colors[1]
    rect["data"] = [0,0, 5,10]
    
    bullet["rect"] = rect
    bullet["t"] = "bullet"

    Kinematic(bullet)

    return bullet
end

void Player()
    array player = CreateObject()
    var rect = []

    rect["color"] = blue
    rect["data"] = [0,0, 5,10]
    
    player["rect"] = rect
    player["t"] = "player"

    StartPosition(player)

    Kinematic(player)
    return player
end

# components
void Kinematic(array object)
    var k = []
    k["x_vel"] = 0
    k["y_vel"] = 0
    k["x_acc"] = 0
    k["y_acc"] = 0
    object["kinematic"] = k
end

void k_setSpeed(number x, number y, array object)
    var kinematic_data = object["kinematic"]

    kinematic_data["x_vel"] = x
    kinematic_data["y_vel"] = y
end

void k_setAcc(number x, number y, array object)
    var kinematic_data = object["kinematic"]

    kinematic_data["x_acc"] = x
    kinematic_data["y_acc"] = y
end

void k_addSpeed(number dx, number dy, array object)
    var kinematic_data = object["kinematic"]

    kinematic_data["x_vel"] = kinematic_data["x_vel"] + dx
    kinematic_data["y_vel"] = kinematic_data["y_vel"] + dy
end

void EnemyMovement(array enemy)
    var em = []
    enemy["enemy_movement"] = em

    em_reset(enemy)
end

void em_reset(array enemy)
    var em = enemy["enemy_movement"]

    em["sp"] = false
    em["fall"] = false
    em["sweep"] = false
    em["sweep_set"] = false
    em["sweep_right"] = false 
end

void em_setType(string type, array enemy)
    var types = ["sp","fall","sweep"]
    var em_data = enemy["enemy_movement"]

    loop t in types
        if t == type
            em_data[t] = true
        else
            em_data[t] = false
        end
    end
end

void UpdateEnemy(array enemy)
    em_data = enemy["enemy_movement"]

    if em_data["sp"] == true
        SpaceInvaders(enemy)
    else if em_data["sweep"] == true
        Sweep(enemy)
    else if em_data["fall"] == true
        Fall(enemy)
    end 

    #lose life when at bottom
    if object_position[1] > 215
        AddToPool(enemy)
        if debug
            return
        end 
        PlaySound("Lose")
        lives = lives - 1

        if lives <= 0
            PlaySound("Error")
            state = 3
        else
            DecreaseBucketCount()
        end 
    end
end

void SpaceInvaders(array object)
    em_setType("sp", object)
    object_position = object["position"]
    if object_position[0] <= 5
        k_setSpeed(SP_SPEED, 0, object)
        o_setPosition([5, object_position[1] + 2 * ENEMY_HEIGHT + 5], object)
    end
    if object_position[0] >= 470
        k_setSpeed(-SP_SPEED, 0, object)    
        o_setPosition([470, object_position[1] + 2 * ENEMY_HEIGHT + 5], object)
    end
end

void Fall(array enemy)
    em_setType("fall", enemy)
    k_setSpeed(0, FALL_SPEED, enemy)
end

void Sweep(array enemy)
    object_position = enemy["position"]

    em_data = enemy["enemy_movement"]
    em_setType("sweep", enemy)

    if em_data["sweep_set"] == false
        em_data["sweep_set"] = true
        if em_data["sweep_right"] == true
            o_setPosition([512,57], enemy)
            k_setSpeed(-141,104, enemy)
            k_setAcc(0, -127, enemy)
        else
            o_setPosition([0,57], enemy)
            k_setSpeed(141,104, enemy)
            k_setAcc(0, -127, enemy)
        end
    end

    if object_position[1] <= -5
        em_setType("nothing", enemy)
        var t = Timer(0.5, [1])
        t["active"] = true
    end
end
```