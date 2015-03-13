```
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
```