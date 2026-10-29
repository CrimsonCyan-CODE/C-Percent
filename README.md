# C%

**C% is an esoteric programming language with a mix of Assembly, C and Python-like syntax.** Everything is built around `<<` chaining and short, memorable keywords like `ptx`, `stin` and `gback`.

```
ptx << "Hello, world!"
```

The interpreter is a single Python file with no dependencies.

## Quick start

You need Python 3. Then:

```
python cpercent.py examples/demo.cpc
```

C% programs can have any file extension. The examples use `.cpc`.

## A taste of C%

```
ptx << input << "Whats the capital of France?" << stin "answer" ver upper
is vari: answer == "PARIS":
    ptx << "Correct!"
els:
    ptx << "Wrong!"
```

## The basics

| Thing | Syntax |
|---|---|
| Comment | `% this is a comment` |
| Print | `ptx << "Hello " << vari: name << "!"` |
| New line inside text | `\nline` |
| Wait | `float << 1` (seconds) |
| Stop the program | `ndsc` |
| Do nothing | `ps` |

`ptx` glues everything together with no spaces, so add your own: `ptx << "a" << " " << "b"`.

### Variables

```
vari << x << 10
vari << x << vari: x - 2
ptx << "x is " << vari: x
```

Use `vari: name` to read a variable. (Bare names like `x` work too, but `vari:` keeps things clear.)

### Input

```
ptx << input << "How old are you?" << stin "age"
```

`stin` means **ST**ore **IN**put. Numbers are turned into real numbers automatically. Add `ver upper` to shout the answer back, with `upper`, `lower`, `rev` or `trim`.

### Choices

```
is vari: age < 13:
    ptx << "kid"
elseis vari: age >= 13 && vari: age < 20:
    ptx << "teen"
els:
    ptx << "adult"
```

Blocks use **indentation**. Comparisons: `==`  `=/=` (not equal)  `<`  `>`  `<=`  `>=`  `ct` (contains). Combine with `&&` (and) and `||` (or).

### Loops

```
loop << 5:
    ptx << "hi"

loop << forever:
    ptx << input << "Type quit" << stin "x"
    is vari: x == "quit":
        brk

each << fruit << list:
    ptx << vari: fruit
```

`brk` leaves a loop and `skp` skips to the next round.

### Lists

```
list == << "apple", "banana", "foo", "bar" <<
content == << <<                 % an empty list

push << "cherry" << list         % add
pull itm that == "foo" from list % remove (without "from" it uses a list called content)
len << list << stin "size"       % how many items
ptx << lst:list                  % print a list
```

### Dice and random

```
rld << 1 t 6 << stin "roll"           % rld = RoLL Dice, t = to
rld << pick << list << stin "choice"  % random item
rld << shuf << list                   % shuffle
```

### Math

`+  -  *  /  ^` plus `sqrt`, `3rt` (cube root), `sin`, `cos` and `tan` (degrees).

```
ptx << sqrt 16 << " " << 2 ^ 3 << " " << 3rt(27)
```

### Custom commands

`cucom` means **CU**stom **COM**mand and `ndcom` ends it.

```
cucom << "double" << n:
    rtn << vari: n * 2
ndcom

double << 21 << stin "result"
ptx << vari: result
```

A command with no inputs is called just by its name. All variables are global.

### Going back

`gback << input` jumps back to the last question that was asked, and `gback << input << 2` jumps back two questions. You can also jump to a label:

```
lbl << "menu":
ptx << "Back at the menu"
gback << "menu"
```

### Saving and loading

```
sv << list << "list.txt"
ld << "list.txt" << stin "list"
```

### Errors

```
try:
    ptx << 10 / 0
catch:
    ptx << "Oops: " << vari: error

ptx << 10 / 0 << oops "Can't divide by zero!"   % short version
```

### Timer

```
tmr << "start"
float << 1
tmr << "stop" << stin "secs"
```

## Example: number guessing game

```
cucom << "randnumber":
rld << 0 t 100 << stin "answer"
ndcom

randnumber
ptx << input << "I'm thinking of a number 0 - 100, can you guess it?" << stin "guess"
is vari: guess < vari: answer:
    ptx << "Greater than that!"
    gback << input
elseis vari: guess > vari: answer:
    ptx << "Less than that!"
    gback << input
els:
    ptx << "You got it! :)"
```

More programs are in [`examples/`](examples/): `demo.cpc` tries every feature and `menu.cpc` is a list manager.

## Keyword cheat sheet

| Keyword | Meaning |
|---|---|
| `ptx` | print |
| `input` | ask a question (inside `ptx`) |
| `stin` | store the result in a variable |
| `vari` | set a variable, or `vari: name` to read one |
| `is` / `elseis` / `els` | if / else if / else |
| `ct` | contains |
| `loop` / `each` | repeat / go through a list |
| `brk` / `skp` | break / skip |
| `rld` | roll dice (also `pick`, `shuf`) |
| `cucom` / `ndcom` / `rtn` | custom command / end it / return a value |
| `lbl` / `gback` | label / go back |
| `push` / `pull` / `len` / `lst:` | list tools |
| `sv` / `ld` | save / load a file |
| `try` / `catch` / `oops` | handle errors |
| `tmr` | timer |
| `float` | wait |
| `ps` | pass |
| `ndsc` | end the script |

## License

MIT. See [LICENSE](LICENSE).
