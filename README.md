# SML/NJ Autograding Patch

This patch adds a setting, `Control.carryDeclaration`, which stores the name of an sml file with a single variable (val, val rec, or fun) declaration. When code is loaded in to the repl, if it contains any variables with the same name, the last declaration of such a variable will be replaced with the code in `carryDeclaration`. 

## Example 

Given two files,

map.sml
```
fun map f [] = []
  | map f (x::xs) = (f x) :: (map f xs)
```

student-code.sml
```
fun map f L = []

fun incr L = map (fn x => x + 1) L
val L = incr [1, 2]
```

The student's `incr` function will be incorrect because their map function is incorrect, not because their incr function is implemented wrong. We want to fix this by substituting the correct `map.sml` code for the student's map declaration.

```
Standard ML of New Jersey v110.82 [built: Wed Dec 13 23:38:01 2017]
- Control.carryDeclaration := "map.sml";
val it = () : unit
- use "student-code.sml"
[opening student-code.sml]
There was 1 occurrance of map
One was replaced.
val map = fn : ('a -> 'b) -> 'a list -> 'b list
val incr = fn : int list -> int list
val L = [2,3] : int list
val it = () : unit
- 
```

Different types of declarations can be freely interchanged - `fun`, `val`, and `val rec` are all treated the same way. 

This does not yet support code with structures, though I plan to add that at some point. 

## Building and Installing 

This patch uses Jacob Van Buren's install script from https://github.com/jvanburen/smlnj-warn-unused, with minor modifications. You should also check out his unused variable warning patch!

These instructions are also taken from his page and modified. (Thanks Jacob!)

### Building 

#### via curl

```shell
bash -c "$(curl -fsSL https://raw.githubusercontent.com/jluningp/smlnj-carry-error/master/install.sh)"
```

#### via wget

```shell
bash -c "$(wget https://raw.githubusercontent.com/jluningp/smlnj-carry-error/master/install.sh -O -)"
```

### Installing

Move the created directory into the appropriate location on your computer (`/usr/local/smlnj/` on macOS at least). Either add the `bin` folder to your PATH variable, or add a symlink to it the `smlnj-110.82/bin/sml` executable in `/usr/local/bin`.
