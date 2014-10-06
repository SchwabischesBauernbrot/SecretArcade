```
    ClearText()

    # this is a comment

    void Foo(string x)
        return x + "!"
    end

    array stuff = [1, 2, "three", false]

    loop x from 1 to 10
        loop y in stuff
            Print(y)
        end
    end

    if Foo("hej") == "hej!"
        number x = Input()
        number y = InputWithPrompt("Enter one more number: ")
        Print(x * y + 3.1415)
    end
```