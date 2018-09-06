# Reading go.

# Package
  - every program starts with package
  - import can be done with groups
    - ``` go
      package main() {
        package
      }
  - import can also be single

# Function
  - ``` go
    func name(args) args {
      statement
    }
  - args can be written like 
    - `(x int, y int)`
    - `(x string, y int)`
    - `(x,y int)`
  - return formats 
    - `(int, int)` 
    - `(x int, y int)` 
    - `(x,y int)`
  - function can have `named return`
    - here `p, q` automatically returns
      ``` go
      func name(x, y int) (p, q int) {
        p = x
        q = y
      } 

# Variable
  - var statement declares a variable
  - `var name type`
  - `var x int`
  - var can be initialized
    -  `var x,y string  = "a", "b"`
  - `var x = "lasdl"`, it autometically detects the type to `string`
  - `:=` can be used as short variable assignment `x := 3` this is a delecration
  - default all variable initialized with zero, `nil`

# Basic types
  - bool
  - string
  - int  int8  int16  int32  int64
  - uint uint8 uint16 uint32 uint64 uintptr
  - byte // alias for uint8
  - rune // alias for int32 // represents a Unicode code point
  - float32 float64
  - complex64 complex128
  - ### Type conversion
    - `type_name(variable)`
    - while declaring a variable without type take the parent type

# Constants
  - Constants are declared in package should start with capital letter
  - otherwise it won't be imported
  - declaration `const Pi = 3.1416`
  - `const` could not use `:=` short assignment operator

# Loop
  - ## For
    - `for i := 1; i< 10; i++ {}`
    - `for ; i<10; {}` uninitialized loop
    - `for i<1 {}` while loop
    - `for {}` infinite loop
  - ## if statement
    - `if condition { statement }`
    - `if v:=f(); v<0 {}` assignment and condition check and `v` is block local, means invalid outside if of if block
    - `if condition {} else if condition {} else {}`
  - ## switch
    - `switch initialize; value { case: statement default: statement}`
    - `switch value { case: statement default: statement}`
    - There is no `break` statement in `go` switch like `c++`
    - another format with no variable just condition check condition work like `if else` 
      - ``` go 
        switch {
        case value>1:
          statement
        case value < 1:
          statement
        default:
          statement
        }
  - ## defer
    - run an statement after `parent function` or `block` `returns`
    - `defer` pushed in to `stack` one by one if a block contains multiple defer and they one by one from stack in reverse of their original order
  - ## Pointer
    - `var *p int`
    - p = &i
    - `*p = 12`
    - `*p` denotes value at address p
    - p denotes only address
  - ## struct
    - structure in c
      ``` go 
      type Name struct  {
        x int
        y int
      }
    - declared by `Name(2,3)` then `x = 2, y = 3`
    - `struct` field can be accessible through `.` notation Name.x, or Name.x = 3
    - can have pointer of struct, v = Name(2,3), p := &v
    - pointer can access member by . dot notaion p.x can be done
  - ## array
    - ```
       Go's arrays are values. An array variable denotes the entire array; it is not a pointer to the first array element (as would be the case in C). This means that when you assign or pass around an array value you will make a copy of its contents. (To avoid the copy you could pass a pointer to the array, but then that's a pointer to an array, not an array.) One way to think about arrays is as a sort of struct but with indexed rather than named fields: a fixed-size composite value.
    - ``` go
      /* A possible "gotcha"
       As mentioned earlier, re-slicing a slice doesn't make a copy of the underlying array. The full array will be kept in memory until it is no longer referenced. Occasionally this can cause the program to hold all the data in memory when only a small piece of it is needed.

      For example, this FindDigits function loads a file into memory and searches it for the first group of consecutive numeric digits, returning them as a new slice.
      */
      var digitRegexp = regexp.MustCompile("[0-9]+")

      func FindDigits(filename string) []byte {
          b, _ := ioutil.ReadFile(filename)
          return digitRegexp.Find(b)
      }
      /*
      This code behaves as advertised, but the returned []byte points into an array containing the entire file. Since the slice references the original array, as long as the slice is kept around the garbage collector can't release the array; the few useful bytes of the file keep the entire contents in memory.

      To fix this problem one can copy the interesting data to a new slice before returning it:
      */
      func CopyDigits(filename string) []byte {
          b, _ := ioutil.ReadFile(filename)
          b = digitRegexp.Find(b)
          c := make([]byte, len(b))
          copy(c, b)
          return c
      }
      /*
      A more concise version of this function could be constructed by using append. This is left as an exercise for the reader.
      */
  
    - array declaration `var name [size] type`
    - also can be done by `name := [size] type {values, values, ...}`
    - array slicing can be done `a[start: end]`
    - slice r kind of referrence, if you change anyting the other slice see the changes the original array too
    - initialize an array suing `q := [] type {values, .... }` or `q := [sz] type {v_1, .. ,v_sz-1, v_sz }`
    - if sz is greater than values then other values given zero
    - type can be struct `q = [] struct {int i bool j} {{1, true}, {2, true}, {3, false}}`
    - array length and capacity can be determined by `len()` and `cap()` function
    - slicing doens't change the capacity just change the length
    - only slicing from begining change the capacity
    - uninitialized array have a value nill `var a[]int` have len 0 and cap 0 and `a==nill` is True
    - make() function can create slice dinamically, make([]int, len, cap) make an array of len and cap, you can slice trough capaicity
    - append() fucntion add value to slice or array. `append(slice, value_to_add)`
    - range is an iterator which return index, value
    - example `for index,value := range iterable {}`
    - you can skip any values using `_`
    - `for _,value := range iterable {}` for only values
    - `for index := range iterable {}` for only index
  - ## Maps
    - initialize by `var name map[key_type] value_type`
    - initially declared map will be `nill` means `name == nill` will be true
    - type can be any type
    - after initialization need to make the map by `name = make(map[key_type]value_type)`
    - variable declartion of map doesn't allow us to use the map. we need to initialize it by make() function
    - map can be initialized through declaration `var name map[key_type] value_type { "key": value, "key": value}`
    - map literal is struct literal
    - get value name[key]
    - assign name[key] = value
    - delete `delete(name, key)`
    - `value, is_found := name[key]` where if is_found is false the value will be zero
  - ## sending function as params
    - send function as params `func (fn (float32, float32) float32) float32 { return fn(2,3);}`
    - function can return a function or closures `func adder() func(int)int { sum := 0 return func(x int ) int {return sum+=x;}}`
  - ## Method of struct
    - go doesn't have classes
    - method have a receiver in the middle of func keyword and keyword name
    - example `func (v Vertyx) name() return_type {}`
    - You can only declare a method with a receiver whose type is defined in the same package as the method. You cannot declare a method with a receiver whose type is defined in another package (which includes the built-in types such as int).
    - `type myFloat base64` then `f := MyFloat(anyvalues) fmt.Println(f.Abs())`
    - here the Abs would be `func (f myFloat) Abs() float64 { if f< 0 {return -f} return f}`
    - type pointer also can work method like `func (v *type) func_name(params ) {}`
    - you can also send pointer to change values of a params
    - for a function you can not send a params where pointer is received lile `func f(v *type) {}` give error if `f(v)` called but `f(&v)` will works
    - for struct or method both the pointer and params works
    - you can use pointer to avoid coping the values several times.
  - ## Interface
    - its a set of method signature
    - delecration `type name interface { func_name() float64 }`
    - Interface can be used to store values.
    - if the value or type store the method of interface then you can assing anytype to interface.
    - An interface can hold a value of any type if that type implements the method
    - if you use variable as receiver then both variable and pointer passing works
    - if you use pointer as receiver then just pointer works
    - INterfaces are implicitely implemented by struct types  `var name interface_type = struct_name {}` will works
    - interface under the hood is a value and type tuple
    - `Interface holding a nill value is not nill by itself`
    - empty interface hold a value of anytype
    - A type assertion provides access to an interface value's underlying concrete value.
    - `typename.(type)` gives you the underlying type
    - `switch v:= interface.(type)` build a switch case statement for type
  - ## Stringer interface
    - its an interface to override the output of the fmt.Println() function
    - fmt look for the function to override if not found then it use the default one
  - ## Error
    - error object also have a function name Error() to override
  - ## IO reader
    - io.reader took a buffer byte and send a tuple (len, error) by telling the data length in byte buffer
    - reader can wrap another reader to change the buffer in the midway

  - # Concurrency
    - ## goroutine
      - goroutine is a simple thread  used by go runtime
      - go function() run a function in another go runtime like thread
      - but value of the calling function will be in current runtime
    - ## Channel
      - channel is like data pipiline
      - insert data into channel through `<-`
      - and get the value value :<-c, ....
      - you need to make channel like map, struct `ch := make(chan type, nil)`
      - channel is thread safe, helps to getting data from multiple thread
      - ch -< public key,
      - sender should close the channel like `close(ch)`
      - `close()` helps the channel to show the end of capacity
    - ## select statement
      - its waits a go routine to wait on multiple statement to complete
      - if multiple works done then it selects a random one
      - syntax `select {case : statement, case : statement} `
      - You can use a default selection and syntax
        - `select {
          case <- :
          case :
          default:
            default will always execute if others case is faster
          }`
        - sync.Mutex used for locking and unlockng
        - use defer to ensure unlock a value
