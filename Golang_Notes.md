#### Types
* There are two kinds of types in Go: those that are values and those that are references to values.
* When a variable is assigned to another variables or passed to a function, the value of the variable is always copied.
* If the variable type is a reference, then the assignment or function parameter pass results in reference value copy and hence the pass by reference semantic.
* An interface type value can hold an actual value or a pointer to a value (depending on which one is assigned or passed to it as function parameter).  If the interface type value is holding pointer, then when this type value is assigned to another variable or passed around, the pointer value is copied.  Also, if it is holding an actual value then this whole value is copied when the interface type is passed around.
* The interface type representation is a two word value.  One word stores the type of the value and the other work points to the value (either an actual value or a pointer to the value)
* Methods can be defined on any type defined (not necessarily struct). e.g.:  `type Number int` methods can be defined for type `Number`
* The methods can be defind for the type or the type pointer `*type`.  The method set differs between the two:   
> continue
* the special type `interface{}` represent any value semantic while the type `struct{}` represents no value semantic.
