```
# A test of an animated stick-character
#
# W=512, H=256
ClearText()

var animStep = 0.1
var animate = true
number x=10
number y=130
number speed=50

var anim_walk = []
var man_ps = []
man_ps[0] = [15,0]  #HeadT
man_ps[1] = [0,10]  #HeadL
man_ps[2] = [30,10] #HeadR
man_ps[3] = [15,20] #Neck
man_ps[4] = [15,40] #Groin
man_ps[5] = [5,30]  #ElbowL
man_ps[6] = [20,30]  #ElbowR
man_ps[7] = [3,40]  #HandL
man_ps[8] = [30,38] #HandR
man_ps[9] = [10,50]  #KneeL
man_ps[10] = [20,45] #KneeR
man_ps[11] = [0,60]  #FootL
man_ps[12] = [30,60] #FootR
anim_walk[0] = man_ps

man_ps = []
man_ps[0] = [15,1]  #HeadT
man_ps[1] = [0,11]  #HeadL
man_ps[2] = [30,11] #HeadR
man_ps[3] = [15,21] #Neck
man_ps[4] = [15,40] #Groin
man_ps[5] = [7,30]  #ElbowL
man_ps[6] = [17,30]  #ElbowR
man_ps[7] = [10,40]  #HandL
man_ps[8] = [25,38] #HandR
man_ps[9] = [13,50]  #KneeL
man_ps[10] = [19,45] #KneeR
man_ps[11] = [0,55]  #FootL
man_ps[12] = [26,60] #FootR
anim_walk[1] = man_ps

man_ps = []
man_ps[0] = [15,2]  #HeadT
man_ps[1] = [0,12]  #HeadL
man_ps[2] = [30,12] #HeadR
man_ps[3] = [15,22] #Neck
man_ps[4] = [15,40] #Groin
man_ps[5] = [10,30]  #ElbowL
man_ps[6] = [14,30]  #ElbowR
man_ps[7] = [20,40]  #HandL
man_ps[8] = [20,38] #HandR
man_ps[9] = [16,50]  #KneeL
man_ps[10] = [17,45] #KneeR
man_ps[11] = [5,55]  #FootL
man_ps[12] = [22,60] #FootR
anim_walk[2] = man_ps


man_ps = []
man_ps[0] = [15,2]  #HeadT
man_ps[1] = [0,12]  #HeadL
man_ps[2] = [30,12] #HeadR
man_ps[3] = [15,22] #Neck
man_ps[4] = [15,40] #Groin
man_ps[5] = [15,30]  #ElbowL
man_ps[6] = [8,30]  #ElbowR
man_ps[7] = [25,40]  #HandL
man_ps[8] = [15,38] #HandR
man_ps[9] = [20,50]  #KneeL
man_ps[10] = [15,45] #KneeR
man_ps[11] = [15,56]  #FootL
man_ps[12] = [15,60] #FootR
anim_walk[3] = man_ps


man_ps = []
man_ps[0] = [15,1]  #HeadT
man_ps[1] = [0,11]  #HeadL
man_ps[2] = [30,11] #HeadR
man_ps[3] = [15,21] #Neck
man_ps[4] = [15,40] #Groin
man_ps[5] = [20,30]  #ElbowL
man_ps[6] = [3,30]  #ElbowR
man_ps[7] = [30,40]  #HandL
man_ps[8] = [10,38] #HandR
man_ps[9] = [25,50]  #KneeL
man_ps[10] = [14,45] #KneeR
man_ps[11] = [20,56]  #FootL
man_ps[12] = [8,60] #FootR
anim_walk[4] = man_ps


var man_ls = []
man_ls = [[0,1],[0,2],[1,3],[2,3],[3,4],[3,5],[5,7],[3,6],[6,8],[4,9],[9,11],[4,10],[10,12]]


number T = Time()
number dT
number aT = 0
number ai = 0
loop
 dT = Time()-T
 T = Time()

 if (Time()-aT) > animStep
  ai = ai + 1
  ai = Mod(ai,Count(anim_walk))
  if animate
   man_ps = anim_walk[ai]
  end
  aT = Time()
 end

 x = Mod(x + dT * speed,512)
 loop l in man_ls
  var p1 = man_ps[l[0]]
  var p2 = man_ps[l[1]] 
  Line(x+p1[0],y+p1[1],x+p2[0],y+p2[1])    
 end
 
 # Ground
 Line(0,y+61,512,y+61)
 
 DisplayGraphics()
end
```