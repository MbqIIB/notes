-   Tcl script is a sequence of commands separated by new line or comma
-   A command a sequence of words separated by white space
-   Everything in Tcl is a string.  Every word is a string.  The result of
    command execution is also a string.  A word can contain any character and
    only white space mark the end of beginning and end of a word
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    set a 4+
    set b 5
    expr $a$b
    # Tcl will do variable substiution inside the word string $a$b and this results in string 4+5 then this string is passed as argument to expr command which returns string 9 as result

    set x [expr 1 + 2]hello
    # x: 3hello
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
-   If a word string need to contain white spaces then
    -   surround it with “” : Tcl does variable substitution and command
        substitution for the string
    -   surround it with {} : Tcl treats the string as it is with not
        substituion

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    set x 24
    set y 18
    set z "$x + $y is [expr $x + $y]"
    # z: 24 + 18 is 42
    set z {$x + $y is [expr $x + $y]} 
    # z: $x + $y is [expr $x + $y]
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
-   Certain parts a word string can be substituted with other strings using:
    -   Variable substitution: `$myvar`
    -   Command substitution: `[expr 4 + 5]`
