# nim-cheat-sheet
A cheat sheet for nim
# Nimrod Notes #

## Links ##

- [Home](http://force7.de/nimrod/)
- [GitHub](http://github.com/Araq/Nimrod) ([Issues](http://github.com/Araq/Nimrod/issues), [Wiki](http://github.com/Araq/Nimrod/wiki))
- Tutorial: [Part 1](http://force7.de/nimrod/tut1.html) [Part 2](http://force7.de/nimrod/tut2.html)
- [Forum](http://force7.de/heimdall)

### Reference ###

- [Index](http://force7.de/nimrod/theindex.html) [Language](http://force7.de/nimrod/manual.html) [Library](http://force7.de/nimrod/lib.html)
- [Compiler](http://force7.de/nimrod/nimrodc.html) [Tools](http://force7.de/nimrod/tools.html)
- [Internals](http://force7.de/nimrod/intern.html)

# Cheatsheet #

## Syntax ##

### Types ###

Built in:

```nim
char: 'c' '\l'
int8: 1_024'i8
int16: 0xbeef'i16
int32: 0o71'i32 
int64: 0b10'i64
float32: 1_000.5e-10'f64
float64: 1_000.5e-10'f64
string: "a\"b", r"a""b", """a"b"""
        # with \n: new line (platform dependant)
        #     \l: LF (10)  \r: CR (13)
        # var s: string = newString(4)
```

#### Declarations ####

```nim
type
  TDirection = enum
    north, east, south, west
  TSubrange = range[0..5]
  TDollar = distinct int
  TEuro = distinct int

type
  TIntArray = array[0..5, int] # an array that is indexed with 0..5
  TIntSeq = seq[int]
x = [1, 2, 3, 4, 5, 6]  # [] is the array constructor
low(x) -> high(x)
y = @[1, 2, 3, 4, 5, 6] # the @ turns the array into a sequence
0 -> len(y) - 1, init with proc newSeq*[T](s: var seq[T], len: int)

type TPerson = tuple[name: string, age: int]
var person : TPerson = (name: "Peter", age: 30)

type
  TCallback = proc (x: int) {.cdecl.}
```

#### Objects ####

```nim
type
  TPerson = object
      name*: string   # the * means that `name` is accessible from other modules
      age: int        # no * means that the field is hidden
  TStudent = object of TPerson # a student is a person
    id: int                    # with an id field
  PNode = ref TNode
  TNode = object
    case kind: TNodeKind  # the ``kind`` field is the discriminator
    of nkInt: intVal: int
    of nkFloat: floatVal: float
    of nkString: strVal: string
    of nkAdd, nkSub:
      leftOp, rightOp: PNode
    of nkIf:
      condition, thenPart, elsePart: PNode

`assert(student is TStudent)`
```

#### Sets ####

```nim
{} # emtpy set
{'a'..'z', '0'..'9'} # This constructs a set that contains the
                     # letters from 'a' to 'z' and the digits
                     # from '0' to '9'

a + b	# union of two sets
a * b	# intersection of two sets
a - b	# difference of two sets (a without b's elements)
a == b	# set equality
a <= b	# subset relation (a is subset of b or equal to b)
a < b	# strong subset relation (a is a real subset of b)
e in a	# set membership (a contains element e)
a -+- b	# symmetric set difference (= (a - b) + (b - a))
card(a)	# the cardinality of a (number of elements in a)
```
#### Pointers ####

Traced references are declared with the `ref` keyword, untraced references are declared with the `ptr` keyword.
The `^` operator can be used to derefer a reference, the `addr` procedure returns the address of an item.
An address is always an untraced reference.

#### Generics ####

```nim
type
  TBinaryTree[T] = object      # TBinaryTree is a generic type with
                               # with generic param ``T``
    le, ri: ref TBinaryTree[T] # left and right subtrees; may be nil
    data: T                    # the data stored in a node
  PBinaryTree[T] = ref TBinaryTree[T] # a shorthand for notational convenience

proc newNode[T](data: T): PBinaryTree[T] = # constructor for a node
  new(result)
  result.data = data
```

### Procedures ###

```nim
proc myWriteln(f: TFile, a: openarray[string]) =
  for s in items(a):
    write(f, s)
  write(f, "\n")
proc callme(x, y: int, s: string = "", c: char, b: bool = false) = ...

callme(0, 1, "abc", '\t', true)  # (x=0, y=1, s="abc", c='\t', b=true)
callme(y=1, x=0, "abd", '\t')    # (x=0, y=1, s="abd", c='\t', b=false)
callme(c='\t', y=1, x=0)         # (x=0, y=1, s="", c='\t', b=false)
callme 0, 1, "abc", '\t'

proc `$` (x: int): string =
  return intToStr(x)
```

### Control flow ###

```nim
if name == "Andreas":
  echo("What a nice name!")
elif name == "":
  echo("Don't you have a name?")
else:
  echo("Boring name...")

case readline(stdin)
of "delete-everything", "restart-computer":
  echo("permission denied")
of "go-for-a-walk":     echo("please yourself")
else:                   echo("unknown command")

when isMainModule:
  when defined(win32):
  elif defined(macosx):
  else:

var f: TFile
if open(f, "numbers.txt"):
  try:
    var a = readLine(f)
    raise newEOS("operating system failed")
  except EIO:
    echo("IO error!")
  except:
    echo("Unknown exception!")
  finally:
    close(f)

var found = false
block myblock:
  for i in 0..3:
    for j in 0..3:
      if a[j][i] == 7:
        found = true
        break myblock # leave the block, in this case both for-loops

while pw != "12345":
  echo("Wrong password! Next try: \n")
  continue
  pw = readLine(stdin)

iterator items*(a: string): char {.inline.} =
  var i = 0
  while i < len(a):
    yield a[i]
    inc(i)
for ch in items("hello world"): # `ch` is an iteration variable
  echo(ch)

ternary: p(if x > 8: 9 else: 10)
```

### Templates ###

```nim
template declareInNewScope(x: expr, t: typeDesc): stmt =
  # open a new scope:
  block:
    var x: t
declareInScope(a, int)
a = 42  # works, `a` is known here
```

## Interop ##

```nim
proc printf(formatstr: cstring) {.importc: "printf", varargs.}
proc callme(formatstr: cstring) {.exportc: "callMe", varargs.}
proc Tcl_Eval(interp: pTcl_Interp, script: cstring): int {.cdecl,
  importc, dynlib: "libtcl(|8.5|8.4|8.3).so.(1|0)".}
```

When building a library with `--app:lib`:

```nim
proc exportme(): int {.cdecl, export, dynlib.}
```

## Compiler ##
### Commands ###
      compile, c                compile project with default code generator (C)
      doc                       generate the documentation for inputfile
      i                         start Nimrod in interactive mode (limited)
      compileToC, cc            compile project with C code generator
      compileToOC, oc           compile project to Objective C code
      rst2html                  convert a reStructuredText file to HTML
      rst2tex                   convert a reStructuredText file to TeX
      run                       run the project (with Tiny C backend; buggy!)
      pretty                    pretty print the inputfile
      genDepend                 generate a DOT file containing the
                                module dependency graph
      dump                      dump all defined conditionals and search paths
      check                     checks the project for syntax and semantic

### Arguments ###
      -p, --path:PATH           add path to search paths
      -d, --define:SYMBOL       define a conditional symbol
      -u, --undef:SYMBOL        undefine a conditional symbol
      -f, --forceBuild          force rebuilding of all modules
      --stackTrace:on|off       turn stack tracing on|off
      --lineTrace:on|off        turn line tracing on|off
      --threads:on|off          turn support for multi-threading on|off
      -x, --checks:on|off       turn all runtime checks on|off
      --objChecks:on|off        turn obj conversion checks on|off
      --fieldChecks:on|off      turn case variant field checks on|off
      --rangeChecks:on|off      turn range checks on|off
      --boundChecks:on|off      turn bound checks on|off
      --overflowChecks:on|off   turn int over-/underflow checks on|off
      -a, --assertions:on|off   turn assertions on|off
      --floatChecks:on|off      turn all floating point (NaN/Inf) checks on|off
      --nanChecks:on|off        turn NaN checks on|off
      --infChecks:on|off        turn Inf checks on|off
      --deadCodeElim:on|off     whole program dead code elimination on|off
      --opt:none|speed|size     optimize not at all or for speed|size
      --app:console|gui|lib     generate a console|GUI application|dynamic library
      -r, --run                 run the compiled program with given arguments
      --advanced                show advanced command line switches
      -h, --help                show this help
        --track:FILE,LINE,COL   track a file/cursor position
        --suggest               suggest all possible symbols at position
        --def                   list all possible symbols at position
        --context               list possible invokation context

## Debugger ##

Compile with `--debugger:on`, without `--app:gui`. Commands:

    s: step_into    n: step_over    f: skip_current
    c: continue     i: ignore

### Breakpoint Commands ###

    b <identifier> [fromline [toline]] [file]    or   {.breakpoint: "before_write_2".}
    breakpoints: Display the entire breakpoint list.  
    disable: <identifier>             enable: <identifier>

# Opinionated #

- Compiler / Parser is not built into the core libs

## Want ##

- Interruptible/Resettable iterators
- Examples of OO common cases
- DSLs, moaaard DSLs

# This cheatsheet #

Rebuild argument list with:

```nim
    {:help => [2,4], :advanced => [1,2]}.map { |k, v| `nimrod --#{k}`.split(/^[^\n]*:\n/m).values_at(*v) }.flatten.values_at(0,2,1,3).each_with_index { |v, i| puts [["### Commands ###\n", "", "\n### Arguments ###\n", ""][i],v.rstrip.gsub(/^(?!#)/, "    ")].join("")  } #.gsub(/^( +)(.*?)( *)$/m, "\\1`\\2`\\3")
```
