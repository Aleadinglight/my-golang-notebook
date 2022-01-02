# My Golang Notes  
My personal notes on the Go programming language.
## Getting Started

- Enable dependency tracking. 
	```bash
	$ go mod init golang-note
	```
	This will create a **go.mod** file which contains description of our **project** and its **go version**. 
	
- Added the file [helloworld.go](basic/helloworld.go)
- Add new module requirements
	```bash
	$ go mod tiny
	go: finding module for package rsc.io/quote
	go: found rsc.io/quote in rsc.io/quote v1.5.2
	```
	This will also create a **go.sum** file for authenticating the module.
	
- Run the program
	```bash
	$ go run ./basic/helloworld.go
	Hello World
	```

## Basic concept and syntax

### Variables
```go
	// Manually declare the variable
	var a int
	// Declaring multible variables and let Go infer the types
	var b, c = 1, 2
	// Short hand for declaring and initializing a variable
	d := true

	// Normal output
	fmt.Println("b =", b, "and c =", c)
	// Formatted output
	fmt.Printf("a = %v (%T) and d = %v (%T)", a, a, d, d)
```
- The inner scope variable is stronger than the ourter scope (shadowing)

### User Input
```go
	var  n  int
	fmt.Println("Input n")
	fmt.Scan(&n)
	fmt.Println("n = ", n)
```
- Refer to [input.go](basic/input.go)

### Maps
```go
	// Use make keyword
	m := make(map[string]int)

	// Update values via keys
	m["Alice"] = 1
	m["Bob"] = 2

	// Delete value for a specific key
	delete(m, "Alice")

	// Check if a value exist
	_, ok := m["Bob"]
	if ok {
		// do something
	}

	// Get the number of key/value pairs
	fmt.Println("len: ", len(m))
```
- Collections of value types that are accesssed via keys
- Created via literals or via `make` function
- Accessed via `[key]` syntax
- Check for present with a second variable `_, ok = map[key]`
- Map are reference type

### Struct
```go
	type person struct {
		age int
		name string
	}
	
	func main() {
		p:= person {
			name: "Bob",
			age: 20
		}
		fmt.Println(p)
	}
```

- Collections of disparate data types that describe a single concept
- Keyed by named fields
- Anonymous structs are allowed
- No inheritance, but can use composition via embedding
- Tags can be added to struct fields to describe field

### If Statement
```go 
	peopleAge := map[string]int{
		"Trung Duong": 23,
		"Janie":       21,
	}

	// Initialize p, ok before entering the check
	if p, ok := peopleAge["Janie"]; ok {
		fmt.Println(p)
	} else { // Else need to be on the same line of the closed curly brace
		fmt.Println("Janie not found!")
	}
```

- Has it own initializer block
- `if` statement does not execute the unnecessary statement (short circuiting).
- Equality with `float` need to be check differently using own defined `error()` function
```go
	n := 0.5
	// Failed due to floating point
	if math.Pow(math.Sqrt(n), 2) == n {
		fmt.Println("true")
	} else {
		fmt.Println("false")
	}

	// Use a custom checking
	if math.Pow(math.Sqrt(n), 2)-n < 0.00001 {
		fmt.Println("true")
	} else {
		fmt.Println("false")
	}
```

## Control flow 

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

