## Types
```
string s = "hello"
number x = 10.0
bool   b = true
array  a = [1,2,3]
```

## Array tricks
```
var a = ["yup", 10]
a[0] = "nope"
a["x"] = 50
Count(a)
```

## Comments
```
# this is a comment
```

## Functions
```
    ClearText()
    number x = Input("")

    void Foo(string x)
        return x + "!"
    end
```

## If statements
```
   if Something()

   else if Maybe()

   else

   end
```

## Loops
```
    loop x from 1 to 10
        sum += x
    end

    loop a in stuff
        Print(a)
    end

    loop
        # forever
    end
```

## Operators
```
==
!=
+ - * /
&& ||
and or
+= -= *= /=
++
--
```

## "KeyReleased" instead of "KeyPressed"
Sometimes, instead of `KeyPressed` event (which will keep repeating the action as long as you're pressing the key down), you might need a way to find out if the user clicked a key at all (which will fire only once). This can be handy in situations when you don't want to "accidentally" keep repeating the same action in case the user didn't pull his finger off of the key fast enough. Here is how you can do that.

```
#input
var keys = ["up","down","left","right","space"]
array input = []
array previous_input = []

InitializeKeys()
 
loop
    ClearText()
 
    loop key in keys
        input[key] = IsKeyPressed(key)
    end
 
    # game logic!
 
    loop key in keys
        previous_input[key] = input[key]
    end
 
    DisplayGraphics()    
end

void InitializeKeys()
    loop key in keys
        previousInput[key] = false
    end
end

void KeyReleased(string key)
    return input[key] == false and previous_input[key] == true
end
```