```
 # Set drawing color
 Color(r, g, b) 

 # Draw line between two points
 Line(x1, x2, y1, y2)

 # Draw Rect defined by two corner points
 Rect(x1, x2, y1, y2)

 # Swaps drawing buffer, Use this to display your draw commands
 DisplayGraphics()

 # Convert (hue, saturation, value) to an array [red, green, blue]
 HSVtoRGB(h,s,v) 
 
 # Convert color (red, green, blue) to an array [hue, saturation, value]
 RGBtoHSV(r,g,b) 

 # Wrap a number (n) within a size (max)
 Repeat(n, max)

 # Returns the world time in seconds.
 Time()

 # Checks if a key is pressed, ("left", "right", "up", "down", "space")
 IsKeyPressed("right")
```