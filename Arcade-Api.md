```
 # Return a random number between 0.0 and 1.0
 Random()

 # Set drawing color
 Color(r, g, b) 

 # Draw line between two points
 Line(x1, y1, x2, y2)

 # Draw Rect defined by two corner points
 Rect(x1, y1, x2, y2)

 # Draw text in a specific position, IS CLEARED WHEN NORMAL Print() IS USED
 Text(x, y, string)

 # Remove all text from the screen
 ClearText ()

 # Print text to the screen (terminal style)
 Print (string text)
NOTE THAT THIS WILL CLEAR TEXT PRINTED BY THE Text() FUNCTION!

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

 # Play a sound
 PlaySound (string soundName)

 # Set the pitch of the sound
 Pitch (float pitch)

 # The sinus function
 Sin(float x)

 # The cosinus function
 Cos(float x)

```