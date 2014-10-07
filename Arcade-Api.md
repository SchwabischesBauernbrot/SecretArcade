```
 #Set drawing color
 Color(r, g, b) 

 #Draw line between two points
 Line(x1, x2, y1, y2)

 #Draw Rect defined by two corner points
 Rect(x1, x2, y1, y2)

 #Swap drawing buffer, Use this to display your draw commands
 DisplayGraphics()

 #Convert Hue Saturation Value to a list contaning rgb (l[0] == r, l[1] == g, l[2] == b)
 HSVtoRGB(h,s,v) 
 
 #Convert color red green blue to a list contaning hsv (l[0] == h, l[1] == s, l[2] == v)
 RGBtoHSV(r,g,b) 

 #Wrap a number n within a size max
 Repeat(n, max)

 Time()
```