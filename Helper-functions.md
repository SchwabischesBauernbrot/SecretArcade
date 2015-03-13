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
