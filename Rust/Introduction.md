- Rust uses *rust compiler* to compile the code to binary (Just like c++ and c use gcc and clang)
- To Compile a rust program use `rustc filename` (rustc is the compiler)
- Rust also have *Cargo* a package manager.
- For managing dependencies and managing the project, we normally use *Cargo* 
- To create a new project with *Cargo* use `cargo new folder-name`
- To build a project use `cargo build`
- To run a project with build and use `cargo run`
- To check if the code can compile without building use `cargo check`

## Syntax
- Just like c++ rust also have a *main function*
```rust
fn main(){
	println!("Hello");
}
```
- *println!* is not a function, it's a macro.
- If it ends with a *!* then its a macro.

### Defining a variable
```rust
let mut guess = String::new();
```
- Here *let* used to initialized a variable.
- In rust variable are normal *immutable* (mean we cannot change the value of them later), to make the *mutable* `mut` is used to make them mutable.
- `guess` is the variable name.
- *String* is a string type provided by the standard library that is growable, UTF-8 encoded text.
- *String::new()* is a function which return new instance of string.

### Receiving use input
```rust
use std::io
```
- `std::io` is a standard module for input and output. Which is called in the scope with the help of use.Just like we use `#include <iostream>` in c++ and c.

```rust
fn main(){
	io::stdin.read_line(&mut guess).except("Failed to get a input");
}
```
- read_line is a method which is called to get input from the user.
- `&` is a reference. Reference is used to use the same data in the program without making extra copies of the data.
- In rust `&` are *immutable* to make them *mutable* use `mut` .
- *read_line* returns a result (result is a enum) which have a value of *Ok* and *Err*. If the result is *Err* it will crash the program and show what kind of error the program getting. If the result if *Ok* it will return the value read from the user to the variable.

### Printing the variable
```rust
println!("You gussed {}" , guess);
```
- For printing the received value, the *{}* brackets are used to place the value in the string. After that, place the variable after the comma.
