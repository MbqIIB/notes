#### Types
* There are two kinds of types in Go: those that are values and those that are references to values.
* When a variable is assigned to another variables or passed to a function, the value of the variable is always copied.
* If the variable type is a reference, then the assignment or function parameter pass results in reference value copy and hence the pass by reference semantic.
* An interface type value can hold an actual value or a pointer to a value (depending on which one is assigned or passed to it as function parameter).  If the interface type value is holding pointer, then when this type value is assigned to another variable or passed around, the pointer value is copied.  Also, if it is holding an actual value then this whole value is copied when the interface type is passed around.
* The interface type representation is a two word value.  One word stores the type of the value and the other word the value (either an actual value or a pointer to the value).
* interface type has special handling when comes to checking nil.  if both type and value are nil, then checking `== nil` returns true.  However, if type is not nil and value is nil, then checking for nil returns false.
  [reference](http://golang.org/doc/faq#nil_error)
* Methods can be defined on any type (not necessarily struct). e.g.:  `type Number int` methods can be defined for type `Number`
* The methods can be defind for the type or the type pointer `*type`.  The method set differs between the two:   
> continue
* the special type `interface{}` represent any value semantic while the type `struct{}` represents no value semantic.
* `nil` can be used with *pointer* and *reference types* .  Reference types: interface type, slice type, map type and channel type. The values of slice, map and channel types are constructed with `make`.

#### Comparison to Erlang:
* Erlang has soft real-time scheduling through expression reduction count.
* Erlang does not share memory but pass(copy) values between processes and hence garbage collection can be done individually on processes whitout affecting other processes.  Go share memory among goroutines (two goroutines can access the same variable a the same time and there is no guarantee on the outcome) and for this reason, the garbage collection has 'stop the world' semantics.
