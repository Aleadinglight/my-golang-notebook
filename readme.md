Important Concepts


### Basic concept and syntax

0. Varable
  - The inner scope variable is stronger than the ourter scope (shadowing)

1. Maps 
  - Collections of value types that are accesssed via keys
  - Created via literals or via `make` function
  - Accessed via `[key]` syntax
  - Check for present with `value, ok = map[key]` 
  - Map are reference type

2. Structs
  - Collections of disparate data types that describe a single concept
  - Keyed by named fields
  - Anonymous structs are allowed
  - No inheritance, but can use composition via embedding
  - Tags can be added to struct fields to describe field

3. If statement
  - Has it own initializer block
  - Short circuiting: the if statement does not execute the unecessary statement.
  - Make some error 

### Control flow 

1. Defer
  - Delay the execution of a statement until function exits. Last in first out. Helpful when you need to open/close a resource (I/O socket) in the same block of code.
  - Defer statement is evaluated at time defer is executed, not at time of called function execution. For changing value of defer function  multiple times, the value is evaluate at the time the defer is called.

2. Panic 
  - Act as run-time error. Panic is used when an application get into a state that it cannot recover from. Call `panic(error)` as a way to `throw(error)`.
  - No longer execute the rest of that function. But will still fire defer function.

3. Recover
  - Used to recover from panic.
  - Because `panic` doesn't execute the rest of the function, the only place to use `recover` is inside a `defer`. 
  - Current function will not attempt to continue, but the higher function in call stack will.
  - If the higher function cannot handle that `panic`, then the `panic` can be rethrow again, propagate the error up the call stack.

### Important concepts and syntax

1. Pointer
  - Store the address of a memory location.
  - Uninitilized pointer is assigned a `nil` value.
  - Syntactic sugar: no need to deference to get the field of a pointer. 
  - All assigment operation in Go are copy operation except for `array` and `map` because they contain internal pointers. When sharing `array` or `map`, you share a pointer point to the underlying data.

2. Function 
  - Can return pointer. (Unlike C++ where heap and stack is important to keep in mind). The returned address automatically promoted from local memory (stack) to shared memory (heap).
  - Can declare the return variable.
  - Can return multiple value.
  - Can return an `error` object for checking purposes.
  - Can be treated as type (anonymous function - first class function).

### Question
  1. First class function: a language that support first class function allows function to be assign to variables, pass as arguments to other functions and returned from other functions. Go has support for first class function.

  2. Anynomous fuction: function that is assigned to a variable and does not have a name. The only way to call the function is by calling the variable that it assigned to, or to call it immediately right after created.

  3. Closure: a special case of anonymous function that access variable defined outside of its body.

  4. Higher-other function: a function that takes one or more functions as arguments, or a function that return a function as its result.

