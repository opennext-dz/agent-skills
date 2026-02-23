---
name: Sage X3 L4G Development
description: Skill for developing in L4G (4th Generation Language) for Sage X3 ERP. Provides comprehensive assistance for writing L4G code including syntax, instructions, functions, system variables, table operations, and best practices.
---

# Sage X3 L4G Development Skill
You are an expert developer in **L4G (Langage de 4ème Génération)** for the **Sage X3 ERP**.
Your role is to write, read, and debug L4G code based on the user's requirements and the official Sage X3 documentation.


# Sage X3 L4G - 4th Generation Language

## Introduction to L4G

L4G (Language 4th Generation) is Sage X3's proprietary programming language. It is an interpreted, structured language oriented toward data processing, specifically designed for development within the Sage X3 ERP.

## Syntax Conventions

### General Rules

- Instructions and keywords are **case-insensitive**
- Multiple instructions can appear on the same line, separated by `:`
- Comments start with `#` or `REM`
- Variable names can contain letters, numbers, and underscores
- Character strings use double quotes `"`
- Instruction blocks typically end with `End` + keyword (e.g., `Endif`, `Endfor`)

### Program Structure

```l4g
# === DECLARATIONS ===
# Table declarations
File MYTABLE [MTB]

# Local variable declarations
Local Char NAME(50)
Local Integer COUNTER
Local Decimal AMOUNT(12,2)

# Global variable declarations
Global Char G_USER(20)

# === MAIN PROGRAM ===
# Main processing...

# === SUBPROGRAMS ===
Subprog MY_SUBPROGRAM(PARAM1, PARAM2)
    Value Char PARAM1
    Variable Integer PARAM2
    # Subprogram code
End

# === FUNCTIONS ===
Funprog MY_FUNCTION(A, B)
    Value Integer A
    Value Integer B
    End A + B
End
```

---

## Data Types

### Primitive Types

| Type | Description | Declaration Syntax |
|------|-------------|-------------------|
| `Char` | Alphanumeric string | `Char NAME(size)` or `Char NAME(size)(dim1:dim2)` |
| `Date` | Date | `Date MY_DATE` |
| `Decimal` | Decimal number | `Decimal AMOUNT(precision, decimals)` |
| `Double` | Double precision floating point | `Double VALUE` |
| `Integer` | Integer | `Integer COUNTER` |
| `Shortint` | Short integer | `Shortint FLAG` |
| `Schar` | Short character string | `Schar CODE(10)` |
| `Uuint` | Unsigned integer | `Uuint ID` |
| `Uuid` | Unique identifier | `Uuid GUID` |
| `Blbfile` | Binary large object file | `Blbfile DOCUMENT` |
| `Clbfile` | Character large object file | `Clbfile CONTENT` |

### Variable Declaration

```l4g
# Local variables (scope limited to current program)
Local Char NAME(50)
Local Integer COUNTER
Local Decimal PRICE(10,2)
Local Date DOC_DATE

# Global variables (accessible from all programs)
Global Char G_USER(20)
Global Integer G_NB_LINES

# Initialization
Local Integer NUMBER = 0
Local Char TEXT = "Initial value"

# Arrays
Local Char NAMES(30)(1:10)     # Array of 10 strings (30 chars max)
Local Integer MATRIX(1:5, 1:5)  # 5x5 matrix
Dim MY_ARRAY(1:MAX_SIZE)       # Dynamic array

# Constants
Const PI = 3.14159
```

---

## Control Structures

### IF Statement (Conditional)

**Syntax:**
```l4g
If logical_expression [Then]
    # Instructions
[Elsif logical_expression [Then]
    # Instructions]*
[Else
    # Instructions]
Endif
```

**Examples:**
```l4g
# Simple test
If I = 1
    Infbox "I equals 1"
Elsif I > 1
    Infbox "I is greater than 1"
Else
    Infbox "I is negative"
Endif

# Compact form
If CONDITION : ACTION : Endif

# If with only Else
If I = 1 : Infbox "I = 1" : Else Infbox "I not equal to 1" : Endif
```

### CASE Statement (Multiple Alternative)

**Syntax:**
```l4g
Case variable
    When value1
        # Instructions
    When value2, value3
        # Instructions
    When DEFAULT
        # Default instructions
Endcase
```

**Example:**
```l4g
Case CHOICE
    When 1
        Infbox "Option 1 selected"
    When 2, 3
        Infbox "Option 2 or 3"
    When DEFAULT
        Infbox "Other option"
Endcase
```

### FOR Loop on Variable

**Syntax 1 - Increment:**
```l4g
For num_variable = initial_value To final_value [Step step]
    # Instructions
Next [num_variable]
```

**Syntax 2 - Value List:**
```l4g
For variable = value_list
    # Instructions
Next [variable]
```

**Examples:**
```l4g
# Simple loop with step
For I = 1 To 13 Step 2.5
    Infbox num$(I)
Next I
# Displays: 1, 3.5, 6, 8.5, 11

# Descending loop
For I = 15 To 11 Step -1
    Infbox I
Next I
# Displays: 15, 14, 13, 12, 11

# List traversal
For CHN = "A", "EF", "X", "ZZZ"
    Infbox CHN
Next CHN
# Displays: A, EF, X, ZZZ

# Exit loop with Break
For I = 1 To 100
    If [MTB]FIELD = "FOUND" : Break : Endif
Next I
```

### FOR Loop on File (FORF)

**Syntax:**
```l4g
For key1 [hint-cl] [From key_start] [To key_end] [where-cl] [With Lock | With Stability]
    # Instructions
Next [key]
```

**Examples:**
```l4g
# Simple traversal
For [CLI]CODECLI
    # Process each customer
Next

# Traversal with conditions
For [AMZ]CODE Where CODMSK=[M]MASK & SAIAFF=1 & CODTYP<>"ABS"
    If !find([F:AMZ]CODZON,[M]ZONE(0..NOL-1))
        [M]ZONE(NOL) = [F:AMZ]CODZON
        NOL += 1
    Endif
Next

# Traversal with locking
For [COM]NUMCOM With Lock
    # Processing with lock
    Rewrite [COM]
Next

# Equivalent to Read + While
For [CCL]CLICLI From START To END Where CONDITION
    # Processing
Next
# Is equivalent to:
# Read [CCL]CLICLI >= START
# While [S]fstat <= 2 and [CCL]CLICLI <= END
#     If CONDITION
#         # Processing
#     Endif
#     Read [CCL]CLICLI Next
# Wend
```

### WHILE Loop

**Syntax:**
```l4g
While condition
    # Instructions
Wend

# or

While condition
    # Instructions
Endwhile
```

**Example:**
```l4g
I = 0
While I < 10
    I += 1
    Infbox num$(I)
Wend
```

### REPEAT Loop

**Syntax:**
```l4g
Repeat
    # Instructions
Until condition
```

**Example:**
```l4g
Repeat
    I += 1
    Infbox num$(I)
Until I >= 10
```

---

## Table Operations (Database)

### Table Declaration (FILE)

**Syntax:**
```l4g
[Local] File file [where_cl] [order_by_cl] [, file2 ...]
```

**Examples:**
```l4g
# Simple declaration
File HISTORY, CUSTOMER [CLI]

# Remote file
File "SERVER:1801@FOLDER.TABLE" [ABR]

# With Where and Order By clauses
File ORDERS [COM] Where STATUS = 1 Order By ORDDATE

# Reopen with same abbreviation
File [COM]  # Does not lose current record
```

### Record Reading (READ)

**Syntax:**
```l4g
Read [class] [key] [read_mode] [key_value]
```

**Read Modes:**
| Mode | Description |
|------|-------------|
| `First` | First record |
| `Last` | Last record |
| `Next` | Next record |
| `Prev` | Previous record |
| `=` | Equality |
| `>=` | Greater than or equal |
| `>` | Greater than |
| `<=` | Less than or equal |
| `<` | Less than |

**Examples:**
```l4g
# Read by equality
Read [CLI]CODECLI = "C001"
If fstat = 0
    # Record found
Endif

# Read first
Read [CLI]CODECLI First

# Read next
Read [CLI]CODECLI Next

# Read with locking
Read [CLI]CODECLI = "C001" With Lock

# Read greater than or equal
Read [CLI]CODECLI >= "C001"

# Read on partial key
Read [CLI]NAMCLI(1) = "DU"  # First on 2 first characters
```

### Read with Lock (READLOCK)

```l4g
Readlock [CLI]CODECLI = "C001"
If fstat = 0
    # Record locked, ready for modification
Endif
```

### Record Writing (WRITE)

```l4g
[CLI]CODECLI = "C001"
[CLI]NAMCLI = "SMITH"
[CLI]CITY = "NEW YORK"
Write [CLI]
If fstat <> 0
    # Error during write
Endif
```

### Record Modification (REWRITE)

```l4g
Readlock [CLI]CODECLI = "C001"
If fstat = 0
    [CLI]NAMCLI = "JONES"
    Rewrite [CLI]
Endif
```

### Record Deletion (DELETE)

```l4g
Readlock [CLI]CODECLI = "C001"
If fstat = 0
    Delete [CLI]
Endif
```

### Existence Check (LOOK)

```l4g
Look [CLI]CODECLI = "C001"
If fstat = 0
    Infbox "Customer exists"
Endif
```

### Table Join (LINK)

**Syntax:**
```l4g
Link main_class With link_list As link_class [where_cl] [order_by_cl]
```

**Example:**
```l4g
# Define a link between tables
File HISTORY [HIS], CUSTOMER [CLI], PRODUCT [PRO]

Link [HIS] With [PRO]CODPRO = NUMPRO, [CLI]NUMCLI = NUMCLI _
    As [LINK] _
    Order By Key KEY1 = [PRO]KEY;[CLI]KEY

# Use the link
For [LINK]KEY1
    # Classes [F:HIS], [F:CLI], [F:PRO] are loaded
    Infbox [F:CLI]NAMCLI + " - " + [F:PRO]DESCRIPTION
Next
```

**Join Types:**
- `=` : Left outer join (main records even without linked)
- `~=` : Strict join (main records with linked only)

---

## Transactions

### Transaction Management

```l4g
# Begin transaction
Trbegin [TABLE1], [TABLE2]

# Operations within transaction
[TABLE1]FIELD = VALUE
Write [TABLE1]

If CONDITION_OK
    # Commit
    Commit
Else
    # Rollback
    Rollback
Endif
```

**System Variables:**
- `[S]adxlog` : Indicates if a transaction is in progress

**Note:** A transaction can only be ended by the program that initiated it.

---

## Filtering and Sorting

### WHERE Clause

```l4g
# In File
File ORDERS [COM] Where STATUS = 1 And TOTAL > 1000

# In For
For [COM]NUMCOM Where STATUS = 1

# Separate filter
Filter [COM] Where STATUS = 1
```

### ORDER BY Clause

```l4g
File ORDERS [COM] Order By ORDDATE

# Descending order
File ORDERS [COM] Order By ORDDATE Desc
```

---

## SQL Queries

### SQL Instruction

```l4g
# Simple query
Sql "SELECT * FROM MYTABLE WHERE FIELD = 'VALUE'"

# Query with result
Local Char RESULT(100)
Sql "SELECT FIELD FROM TABLE WHERE ID = 1" As RESULT
```

### EXECSQL Instruction

```l4g
# For queries without return (INSERT, UPDATE, DELETE)
Execsql "UPDATE MYTABLE SET FIELD = 'VALUE' WHERE ID = 1"
```

### SQL Analysis (ANASQL)

```l4g
# Analyze a SQL query
Anasql "SELECT * FROM MYTABLE"
```

---

## Subprograms and Functions

### Subprogram (SUBPROG)

**Definition:**
```l4g
Subprog MY_SUBPROGRAM(PARAM1, PARAM2)
    Value Char PARAM1          # Passed by value
    Variable Integer PARAM2    # Passed by address (modifiable)
    
    # Subprogram code
    If PARAM1 = "TEST"
        PARAM2 = PARAM2 + 1
    Endif
End
```

**Call:**
```l4g
Call MY_SUBPROGRAM("TEST", MY_COUNTER)
```

### Function (FUNPROG)

**Definition:**
```l4g
Funprog CALC_X(A, B)
    Value Integer A
    Value Integer B
    
    # Returns the value
    End A * A / B
End

Funprog IS_ACTIVE(CODE)
    Value Char CODE
    
    If CODE = ""
        End 1
    Endif
    
    If clalev([F:ACV]) = 0
        Local File ACTIV [ACV]
    Endif
    
    Read [ACV]CODACT = CODE
    
    If fstat <> 0 Or [F:ACV]FLACT <> 2
        End 0
    Elsif [F:ACV]TYP = 2
        End [F:ACV]DIME
    Else
        End 1
    Endif
End
```

**Call:**
```l4g
Local Integer RESULT
RESULT = func CALC_X(3, 2)

# In an expression
If func IS_ACTIVE("CODE123") = 1
    Infbox "Code is active"
Endif
```

### Parameter Passing Modes

| Keyword | Description |
|---------|-------------|
| `Value` | Passed by value (copy, not modifiable) |
| `Variable` | Passed by address (modifiable) |
| `Const` | Passed by value (read-only, optimized) |

### Gosub (Internal Subroutine)

```l4g
# Call
Gosub LABEL

# ... main code ...

# Internal subroutine
LABEL:
    # Code
Return
```

---

## Mask Management (Screens)

### Mask Declaration

```l4g
Mask MYMASK [MM]
```

### Field Operations

```l4g
# Display a field
Affzo [MM]FIELD1

# Clear a field
Effzo [MM]FIELD1

# Force display
Envzo [MM]FIELD1

# Enable a field (ungray)
Enable [MM]FIELD1

# Disable a field (gray out)
Disable [MM]FIELD1

# Initialize a grayed zone
Grizo [MM]FIELD1

# Grayed zone on condition
Grizo [MM]FIELD1 Where CONDITION
```

### Left Lists

```l4g
# Fill a left list
Fillbox [MM]LIST With EXPRESSION

# Define a selection
Setlbox [MM]LIST, INDEX

# Position in a left list
Leftbox [MM]LIST

# Current left list
Local Integer SEL
SEL = currbox
```

---

## Sequential Files

### Open for Reading (OPENI)

```l4g
Local Integer FILEH
Openi "folder\file.txt" Using FILEH
If fstat = 0
    # File opened successfully
Endif
```

### Open for Writing (OPENO)

```l4g
Local Integer FILEH
Openo "folder\file.txt" Using FILEH
```

### Open for Read/Write (OPENIO)

```l4g
Local Integer FILEH
Openio "folder\file.txt" Using FILEH
```

### Sequential Read (GETSEQ)

```l4g
Local Char LINE(200)
Getseq FILEH Into LINE
While fstat = 0
    # Process the line
    Getseq FILEH Into LINE
Wend
```

### Sequential Write (PUTSEQ)

```l4g
Putseq FILEH From "Content to write"
```

### Positioning (SEEK)

```l4g
# Absolute position
Seek FILEH, POSITION

# Current position
Local Integer POS
POS = adxseek(FILEH)
```

### Close

```l4g
Close FILEH
```

### Separator Variables

- `[S]adxifs` : Field separator (default TAB)
- `[S]adxirs` : Record separator (default CR/LF)
- `[S]adxium` : String encoding type

---

## Dialog Boxes

### Information (INFBOX)

```l4g
Infbox "Information message"
Infbox "Result: " + num$(VALUE)
```

### Error (ERRBOX)

```l4g
Errbox "Error message"
```

### User Input (ASKUI)

```l4g
Local Char RESPONSE(100)
Askui "Enter a value:" With RESPONSE
```

### Multiple Choice (CHOOSE)

```l4g
Local Integer CHOICE
Choose CHOICE, "Select an option:", _
    "Option 1", "Option 2", "Option 3"
```

---

## Error Handling

### ONERRGO

```l4g
Onerrgo ERROR_HANDLER

# Code likely to generate an error
# ...

ERROR_HANDLER:
    # Error processing
    Infbox "Error #" + num$(errn) + ": " + errmes$(errn)
    Resume
```

### Error Variables

| Variable | Description |
|----------|-------------|
| `errn` | Error number |
| `errm` | Second part of error message |
| `errmes$(n)` | Complete error message |
| `errp` | Program where error occurred |
| `errl` | Error line |
| `fstat` | Return status for file operations |

### Common Error Codes

| Code | Constant | Description |
|------|----------|-------------|
| 7 | ERCLAS | Abbreviation not found |
| 8 | ERINDI | Index out of bounds |
| 10 | ERMODE | Incorrect type |
| 20 | PAFIC | File not opened |
| 21 | ERPATH | Path not found |
| 22 | MODIN | Incorrect read mode |
| 28 | ERREOP | Abbreviation already used |
| 29 | TROFIC | Too many open files |
| 32 | ERRET | Incorrect loop nesting |
| 41 | ERLOOP | Step value is zero |
| 53 | ERNOM | Unknown variable |
| 55 | ERDIM | Incorrect number of dimensions |
| 61 | ERVEX | Incorrect expression type |

---

## System Variables

### Input Variables

| Variable | Description |
|----------|-------------|
| `[S]adxfmt` | Default input formats |
| `[S]adxsca` | Input characters |
| `[S]status` | Input return status |
| `[S]indice` | Current index in input |
| `[S]inpmode` | Mode used in input |

### Table Variables

| Variable | Description |
|----------|-------------|
| `[S]fstat` | Return status of table instruction |
| `[S]adxdlrec` | Number of records deleted by DELETE |
| `[S]adxuprec` | Number of records modified by UPDATE |
| `[S]currind` | Current key number |
| `[S]currlen` | Number of key parts used |
| `[S]keylen` | Key length |
| `[S]keyname` | Key name |
| `[S]nbrecord` | Number of records |
| `[S]nbind` | Number of indexes |

### Application Variables

| Variable | Description |
|----------|-------------|
| `[S]nomap` | Current and reference application |
| `[S]adxmother` | Reference applications |
| `[S]adxusr` | Connected user |
| `[S]adxuid` | User identifier |
| `[S]adxpid` | Adonix process number |
| `[S]datesyst` | Reference system date |
| `[S]adxdir` | Installation directory |
| `[S]adxrob` | Subdirectory names |

### Transaction Variables

| Variable | Description |
|----------|-------------|
| `[S]adxlog` | Indicates if a transaction is in progress |
| `[S]lockwait` | Lock wait time |

### Menu Variables

| Variable | Description |
|----------|-------------|
| `[S]menchoix` | Current function |
| `[S]adxpam` | Selected menu function parameter |
| `[S]adxpno` | Stack of OBJet names |

### Debugger Variables

| Variable | Description |
|----------|-------------|
| `[S]adxdbc` | Input subroutine |
| `[S]adxdbo` | Open subroutine |
| `[S]adxdbx` | Close subroutine |
| `[S]adxdpg` | Program |
| `[S]dbglong` | Long variable |
| `[S]dbgstr` | String variable |
| `[S]dbgmode` | Mode |

---

## Built-in Functions

### Numeric Functions

| Function | Description | Example |
|----------|-------------|---------|
| `abs(n)` | Absolute value | `abs(-5)` = 5 |
| `ar2(n)` | Round to 2 decimals | `ar2(3.456)` = 3.46 |
| `arr(n, p)` | Round to precision p | `arr(3.456, 2)` = 3.46 |
| `fix(n)` | Truncate | `fix(3.9)` = 3 |
| `int(n)` | Integer part | `int(3.9)` = 3 |
| `mod(n, d)` | Modulo | `mod(10, 3)` = 1 |
| `sgn(n)` | Sign | `sgn(-5)` = -1 |
| `sqr(n)` | Square root | `sqr(16)` = 4 |
| `exp(n)` | Exponential | `exp(1)` = e |
| `ln(n)` | Natural logarithm | `ln(e)` = 1 |
| `log(n)` | Base-10 logarithm | `log(100)` = 2 |
| `fac(n)` | Factorial | `fac(5)` = 120 |
| `rnd(n)` | Random number | `rnd(100)` |
| `anp(n, p)` | Arrangements | `anp(5, 3)` |
| `cnp(n, p)` | Combinations | `cnp(5, 3)` |
| `max(n1, n2, ...)` | Maximum | `max(1, 5, 3)` = 5 |
| `min(n1, n2, ...)` | Minimum | `min(1, 5, 3)` = 1 |
| `avg(n1, n2, ...)` | Average | `avg(1, 2, 3)` = 2 |
| `sigma(var)` | Cumulative sum in loop | Sum of values |

### Trigonometric Functions

| Function | Description |
|----------|-------------|
| `sin(n)` | Sine (radians) |
| `cos(n)` | Cosine (radians) |
| `tan(n)` | Tangent (radians) |
| `asin(n)` | Arc sine |
| `acos(n)` | Arc cosine |
| `atan(n)` | Arc tangent |
| `atan2(y, x)` | Arc tangent with quadrant |
| `sinh(n)` | Hyperbolic sine |
| `cosh(n)` | Hyperbolic cosine |
| `tanh(n)` | Hyperbolic tangent |
| `asinh(n)` | Hyperbolic arc sine |
| `acosh(n)` | Hyperbolic arc cosine |
| `atanh(n)` | Hyperbolic arc tangent |

### String Functions

| Function | Description | Example |
|----------|-------------|---------|
| `len(s)` | Length | `len("Hello")` = 5 |
| `left$(s, n)` | Left n characters | `left$("Hello", 2)` = "He" |
| `right$(s, n)` | Right n characters | `right$("Hello", 2)` = "lo" |
| `mid$(s, start, len)` | Substring | `mid$("Hello", 2, 3)` = "ell" |
| `seg$(s, start, end)` | Segment | `seg$("Hello", 2, 4)` = "ell" |
| `instr(start, s1, s2)` | Find position | `instr(1, "Hello", "ll")` = 3 |
| `toupper(s)` | Uppercase | `toupper("hello")` = "HELLO" |
| `tolower(s)` | Lowercase | `tolower("HELLO")` = "hello" |
| `vireblc(s)` | Remove spaces | `vireblc("  hi  ")` = "hi" |
| `space$(n)` | n spaces | `space$(3)` = "   " |
| `string$(n, c)` | n times character | `string$(3, "A")` = "AAA" |
| `chr$(n)` | ASCII character | `chr$(65)` = "A" |
| `ascii(s)` | ASCII code | `ascii("A")` = 65 |
| `val(s)` | String to number | `val("123")` = 123 |
| `num$(n)` | Number to string | `num$(123)` = "123" |
| `format$(fmt, val)` | Format value | `format$("K:999", 123)` |
| `ctrans(s)` | 7-bit conversion | Accented characters |

### Date Functions

| Function | Description | Example |
|----------|-------------|---------|
| `date$` | Current date | "20240115" |
| `[S]datesyst` | System date | current date |
| `time$` | Current time | "143025" |
| `day(date)` | Day (ordinal) | `day(date$)` |
| `dayn(date)` | Day of week (1-7) | `dayn(date$)` |
| `month(date)` | Month | `month(date$)` |
| `year(date)` | Year | `year(date$)` |
| `week(date)` | Week number | `week(date$)` |
| `addmonth(d, n)` | Add n months | `addmonth(date$, 3)` |
| `eomonth(d)` | End of month | `eomonth(date$)` |
| `gdat$(d, m, y)` | Create date | `gdat$(15, 1, 2024)` |
| `nday$(n)` | Day name | `nday$(1)` = "Monday" |
| `month$(n)` | Month name | `month$(1)` = "January" |
| `nday(date)` | Day number | Day of year |
| `aday$(n)` | Abbreviated day name | |
| `amonth$(n)` | Abbreviated month name | |

### Table/File Functions

| Function | Description |
|----------|-------------|
| `filinfo(file, type)` | Information about a file |
| `filelev(class)` | Locality level |
| `nbrecord(class)` | Number of records |
| `nbind(class)` | Number of indexes |
| `filename(class)` | Access path |
| `clalev(class)` | Class used or not |
| `clanam(class)` | Class name |
| `clanbs(class)` | Number of defined symbols |
| `clasiz(class)` | Number of allocated symbols |
| `clavar(class, index)` | Variable names in a class |
| `adxseek(file)` | Position in sequential file |

### Mask Functions

| Function | Description |
|----------|-------------|
| `inpmode()` | Mode used in field input |
| `varinit(mask, field)` | Mask variable initialized or not |
| `masklev(mask)` | Locality level |
| `masknam(mask)` | Mask name |
| `maskabr(mask)` | Mask abbreviation |
| `masknbf(mask)` | Number of linked files |
| `masksiz(mask)` | Mask size |
| `maskcou(mask)` | Mask color |

### Miscellaneous Functions

| Function | Description |
|----------|-------------|
| `evalue(expression)` | Formula evaluation |
| `parse(expression)` | Syntax analysis |
| `find(value, list)` | Search in list |
| `messname` | Message files |
| `getenv$("variable")` | Environment variable |

---

## Interruptions

### SLEEP

```l4g
Sleep 5  # Pause for 5 seconds
```

### INTER / NOINTER

```l4g
# Interruptible mode
Inter

# Non-interruptible mode
Nointer
```

### ONINTGO

```l4g
Onintgo LABEL

# ... code ...

LABEL:
    # Interruption handling
    Resume
```

---

## Events

### ONEVENT

```l4g
Onevent EVENT_NAME From MASK
    # Event handling
Endevent
```

### ONKEY

```l4g
Onkey BACKGROUND, "F1", "Help"
    # Action on F1 key
Endevent
```

---

## Reports

```l4g
Report MY_REPORT [RPT]
    # Report configuration
End
```

---

## Classes and Dynamic Variables

### Class Manipulation

```l4g
# Check if a class is loaded
If clalev([MTB]) <> 0
    # The class is used
Endif

# Get class name
Local Char NAME(20)
NAME = clanam([MTB])

# Number of variables in a class
Local Integer NB
NB = clanbs([MTB])
```

---

## 🏗️ Advanced Reminders & Naming Conventions
- **Actions vs Subprograms**: Use Actions (`ACT_`) to plug into standard dictionary UI events. Use Subprograms (`ADC_`) for background processing.
- **Entry Points (`AOE_`)**: Use standard entry points to inject custom behavior into standard X3 code without modifying the core files.
- **Reports (`ARP_`)**: Pertain to Crystal Reports connectivity and printing subprograms.
- **Transactions**: Avoid nested `Trbegin` blocks. Always ensure that every `Trbegin` is matched with a `Commit` or `Rollback`.
- **UI Context**: Direct database updates should not be performed mid-UI event (like field control) unless strictly necessary, to prevent locking issues.

---

## Best Practices

### Naming Conventions

- Use explicit variable names
- Prefix variables by their type: `i` for Integer, `s` for String, etc.
- Use consistent abbreviations for table cursors
- Limit names to 15 characters for compatibility

### Code Organization

1. Place all declarations (`File`, `Local`, `Global`) at the beginning
2. Group subprograms by functionality
3. Use comments extensively
4. Include error handling for all database operations

### Performance

1. Limit the number of open tables
2. Use appropriate indexes for reads
3. Close files when no longer needed
4. Use transactions for batch operations
5. Prefer `For` over `Read` + `While` for table traversal
6. Avoid reopening already open files

### Error Handling

```l4g
Onerrgo ERROR_HANDLER

# Risky operations
Read [MTB]KEY = VALUE
If fstat = 0
    # Success
Else
    # Failure - fstat contains the code
Endif

ERROR_HANDLER:
    Infbox "Error: " + errmes$(errn)
    Resume
```

### Transaction Safety

```l4g
Trbegin [MTB]

# Operations
[MTB]FIELD = VALUE
Write [MTB]

If EVERYTHING_OK
    Commit
Else
    Rollback
Endif
```

---

## Common Patterns

### CRUD Operations

```l4g
# CREATE - Creation
[CLI]CODECLI = "C001"
[CLI]NAMCLI = "SMITH"
Write [CLI]

# READ - Reading
Read [CLI]CODECLI = "C001"

# UPDATE - Modification
Readlock [CLI]CODECLI = "C001"
If fstat = 0
    [CLI]NAMCLI = "JONES"
    Rewrite [CLI]
Endif

# DELETE - Deletion
Readlock [CLI]CODECLI = "C001"
If fstat = 0
    Delete [CLI]
Endif
```

### Batch Processing

```l4g
Trbegin [COM]

For [COM]NUMCOM Where STATUS = 1 With Lock
    [COM]STATUS = 2
    Rewrite [COM]
Next

Commit
```

### Data Validation

```l4g
# Input validation
If [M]FIELD = ""
    Errbox "Field is mandatory"
    GERR FIELD  # Return to field
Endif
```

### Search with Default Value

```l4g
Read [PAR]CODE = "MY_PARAM"
If fstat <> 0
    # Parameter not found, use default value
    VALUE = DEFAULT_VALUE
Else
    VALUE = [F:PAR]VALUE
Endif
```

### Null and Empty String Handling

```l4g
If null([CLI]EMAIL) Or vireblc([CLI]EMAIL) = ""
    # No email
Endif
```

---

## STATUS Values

The `status` variable returns the result of an input operation:

| Value | Global Variable | Description |
|-------|-----------------|-------------|
| 0 | | No input (display) |
| 1 | | Normal validation |
| 2 | | Escape key / Cancellation |
| 3 | | Input interrupted |
| 7+ | | Specific actions depending on context |

---

## FSTAT Values

The `fstat` variable returns the status of file operations:

| Value | Description |
|-------|-------------|
| 0 | Success |
| 1 | End of file reached |
| 2 | Record not found |
| 3 | Record locked |
| 4+ | Other errors |

---

## Instructions Limited or Prohibited in Web Mode

Certain instructions are limited or prohibited in Web mode:
- `Openi` - Open for reading
- `Openo` - Open for writing
- `Openio` - Open for read/write
- `Rdseq` - Sequential read
- `Wrseq` - Sequential write
- `Getseq` - Sequential read (limited)
- `Putseq` - Sequential write (limited)
- Specific interactive dialog instructions

---

## Instructions Encapsulated by Supervisor

These instructions are rarely used directly as they are managed by the supervisor:
- `Enable` - Button ungraying
- `Disable` - Button graying
- `Addmen` - Add a menu
- `Setmok` - Mask validation

---

## OBJet Actions (Standard Event Handlers)

### Overview

OBJet Actions are entry points in the Sage X3 supervisor that allow developers to inject custom code at specific moments during the lifecycle of an object (OBJet). These actions are defined in the Dictionary and call specific subprograms in the associated processing code.

### Action Types and Naming Conventions

| Prefix | Type | Description |
|--------|------|-------------|
| `ACT_` | Standard Action | Main actions triggered by user operations |
| `AOE_` | Entry Point | Standard entry points for injecting custom behavior |
| `ADC_` | Subprogram | Background processing subprograms |
| `ARP_` | Report | Crystal Reports related subprograms |

### OBJet Types

| Type | Description |
|------|-------------|
| Simple | Single record management |
| Table (Tableau) | Detail lines management |
| Combined (Combiné) | Header + Lines management |

---

### Creation Actions

Actions executed during the creation process:

| Action | Transaction | Description |
|--------|-------------|-------------|
| `RAZCRE` | No | When creating a new record (screen reset) |
| `VERIF_CRE` | No | Before creation transaction begins - can cancel by setting `OK = 0` |
| `DEBUT_CRE` | Yes | At beginning of creation transaction (combined OBJet only) |
| `INICRE` | Yes | During creation, just before Write |
| `CREATION` | Yes | During creation transaction, after successful Write |
| `APRES_CRE` | No | After creation (transaction completed) |
| `AB_CREATION` | No | On creation abort due to error (after Rollback) |

**Example - Creation Action:**
```l4g
$ACTION
Case ACTION
    When "CREATION"
        # Additional updates after record creation
        [LNK]CODREF = [F:HDR]CODHDR
        Write [LNK]
        If fstat <> 0
            GOK = 0  # Cancel transaction
        Endif
Endcase
Return
```

---

### Modification Actions

Actions executed during the modification process:

| Action | Transaction | Description |
|--------|-------------|-------------|
| `LIENS` | No | After reading a record |
| `STYLE` | No | After reading and displaying a record |
| `AVANT_MOD` | No | When switching to edit mode (after field modification) |
| `GRISE` | No | When key fields are grayed out (after entering modification) |
| `VERIF_MOD` | No | Before modification transaction - can cancel by setting `OK = 0` |
| `AVANT_MODFIC` | Yes | During modification (transaction start) |
| `DEBUT_MOD` | Yes | At beginning of modification transaction (combined OBJet only) |
| `INIMOD` | Yes | During modification, just before Rewrite |
| `MODIF` | Yes | During modification transaction, after successful Rewrite |
| `APRES_MOD` | No | After modification (transaction completed) |
| `APRES_MODIF` | No | After any field modification |
| `AB_MODIF` | No | On modification abort due to error (after Rollback) |

**Example - Modification Action:**
```l4g
$ACTION
Case ACTION
    When "VERIF_MOD"
        # Validation before modification
        If [M]AMOUNT < 0
            OK = 0  # Cancel modification
            Errbox "Amount must be positive"
        Endif
    When "MODIF"
        # Update related records after modification
        [LNK]CODHDR = [F:HDR]CODHDR
        Rewrite [LNK]
Endcase
Return
```

---

### Deletion/Annulment Actions

Actions executed during the deletion process:

| Action | Transaction | Description |
|--------|-------------|-------------|
| `AV_VERF_ANU` | No | Before annulment (before dictionary link controls) |
| `AP_VERF_ANU` | No | Before annulment (after dictionary link controls) |
| `VERF_ANU` | No | Before deletion transaction - can cancel by setting `OK = 0` |
| `AV_ANNULE` | Yes | At beginning of annulment transaction |
| `ANNULE` | Yes | During annulment transaction, before Delete |
| `APRES_ANNULE` | No | After annulment (transaction completed) |
| `AP_ANNULE` | No | After annulment of record |

**Example - Deletion Action:**
```l4g
$ACTION
Case ACTION
    When "VERF_ANU"
        # Check if deletion is allowed
        If [F:HDR]STATUS = 2
            OK = 0
            Errbox "Cannot delete validated record"
        Endif
    When "ANNULE"
        # Delete related records
        For [LNK]CODHDR = [F:HDR]CODHDR
            Delete [LNK]
        Next
Endcase
Return
```

---

### Code Change Actions

Actions executed during code change operations:

| Action | Transaction | Description |
|--------|-------------|-------------|
| `AVANT_CHG` | No | Before code change (can cancel) |
| `AP_CHANGE` | No | After code change completed |
| `VERF_CHG` | No | Before code change transaction |
| `CHANGE` | Yes | During code change transaction |

---

### Locking/Unlocking Actions

| Action | Transaction | Description |
|--------|-------------|-------------|
| `AVANT_VERROU` | No | Before locking a record |
| `VERROU` | No | When a record is locked (for modification or deletion) |
| `AVANT_DEVERROU` | No | Before unlocking a record |
| `DEVERROU` | No | When a record is unlocked (modification completed) |

---

### Opening/Closing Actions

| Action | Transaction | Description |
|--------|-------------|-------------|
| `AVANT_OUVRE` | No | First action executed |
| `OUVRE` | No | After opening tables and screens |
| `OUVRE_BOITE` | No | Before opening the window |
| `BOITE` | No | Before opening window with tabs and left lists |
| `AFFMASK` | No | At first screen display |
| `EFFMASK` | No | When clearing mask (creation abort, etc.) |
| `TITRE` | No | Before opening the window |
| `FERME` | No | Last action executed |
| `FIN` | No | After "FIN" button, before unlocking record |

---

### Left List Actions

| Action | Transaction | Description |
|--------|-------------|-------------|
| `TIROIR` | No | Before displaying any left list |
| `HINT` | No | Before left list display (except last read) to indicate traversal key |
| `CLE_GAUCHE` | No | Before left list display to modify sort key characteristics |
| `FILGAUCHE` | No | To filter records in left list (except last read) |
| `AP_FILGAUCHE` | No | During display of different left list |
| `GAUCHE` | No | When selecting a simple or hierarchical left list element |
| `GAUCHE9` | No | When selecting from last read left list |
| `PRE_GAUCHE` | No | Previous page when paging on left list |
| `SUI_GAUCHE` | No | Next page when paging on left list |
| `REMP_DERLU` | No | Before displaying last read list |

---

### Button Actions (Standard Buttons)

For each standard button, there are before and after actions:

| Button | Before Action | After Action |
|--------|---------------|--------------|
| Finish | `AVANT_END` | `END` |
| Save | `AVANT_ENR` | `ENR` |
| Create | `AVANT_CRE` | `CRE` |
| Delete | `AVANT_SUP` | `SUP` |
| Abandon | `AVANT_ABA` | `ABA` |
| OK | `AVANT_OK` | `OK` |
| New | `AVANT_NEW` | `NEW` |
| First | `AVANT_FIR` | `FIR` |
| Last | `AVANT_LAS` | `LAS` |
| Previous | `AVANT_PRE` | `PRE` |
| Next | `AVANT_SUI` | `SUI` |
| Selection | `AVANT_SEL` | `SEL` |
| Change Code | `AVANT_CHG` | `CHG` |
| Print | `AVANT_EDI` | `EDI` |
| List | `AVANT_LIS` | `LIS` |
| Attachments | `AVANT_JOI` | `JOI` |
| Comments | `AVANT_COM` | `COM` |
| Properties | `AVANT_PRO` | `PRO` |

**Note:** For custom buttons (XXX), actions are `AVANT_XXX` and `XXX`.

**Example - Button Action:**
```l4g
$ACTION
Case ACTION
    When "AVANT_CRE"
        # Validation before creation button
        If [M]TYPE = ""
            OK = 0
            Errbox "Type is mandatory"
        Endif
    When "CRE"
        # After creation button (if STD or SPE action not specified)
        Infbox "Record created"
        Fin = 0  # Stay on window
Endcase
Return
```

---

### Printing Actions

| Action | Transaction | Description |
|--------|-------------|-------------|
| `IMPRIME` | No | Before Crystal Report print (for GEODE) |
| `AV_IMPRIME` | No | Before Crystal Report document print |
| `AP_IMPRIME` | No | After Crystal Report document print |
| `AV_LISTE` | No | Before Crystal Report list print |
| `AP_LISTE` | No | After Crystal Report list print |

---

### Menu/Action Execution Actions

| Action | Transaction | Description |
|--------|-------------|-------------|
| `AVANT_ACT` | No | Before executing a predefined button |
| `EXEACT` | No | After predefined button execution, before supervisor processing |
| `AVANTBOUT` | No | Before executing a button or menu defined in window |
| `EXEBOUT` | No | After executing a button defined in window |
| `AVANT_CHOI` | No | After button/menu/left list selection |
| `APRES_CHOI` | No | After button/menu/left list selection |
| `FIN_ACTION` | No | After button/menu/left list execution |
| `STATUT` | No | After executing a menu defined in window |

---

### Picking Actions

| Action | Transaction | Description |
|--------|-------------|-------------|
| `DEB_PICK` | No | When selecting an element in picking list |
| `PICKE` | No | After selecting an element in picking list |
| `DEPICK` | No | After deselecting an element in picking list |
| `FIN_PICK` | No | End of picking selection process |

---

### Browser Actions

| Action | Transaction | Description |
|--------|-------------|-------------|
| `LECTURE` | No | During browser record reading |
| `SET_DERLU` | No | To set last read list |
| `REMP_DERLU` | No | Before displaying last read list |

---

### Detail Table Actions (Tableau)

For OBJet with detail lines:

| Action | Transaction | Description |
|--------|-------------|-------------|
| `LIENS0` | No | Before reading a group of records |
| `LIENS` | No | After reading a record |
| `LIENS2` | No | After reading a group of records |
| `INICRE_LIG` | Yes | During line creation, before Write |
| `INIMOD_LIG` | Yes | During line modification, before Rewrite |
| `DEFLIG` | No | When deleting a line |
| `EXCLIG` | No | When excluding a line |
| `VALLIG` | No | When validating a line |
| `FIN_TABLE` | No | After table processing |

---

### Initialization Actions

| Action | Transaction | Description |
|--------|-------------|-------------|
| `DEFTRANS` | No | Beginning of OBJet window analysis |
| `SETTRANS` | No | End of OBJet window analysis |
| `VARIANTE` | No | OBJet window analysis (for each window) |
| `INIT` | No | Initialization |
| `INIT_DIA` | No | Dialog initialization |
| `INIPAR` | No | Parameter initialization |
| `INIPAR2` | No | Additional parameter initialization |
| `EXCINI` | No | Parameter exclusion |

---

### Filter/Search Actions

| Action | Transaction | Description |
|--------|-------------|-------------|
| `FILTRE` | No | Define criteria for filter on main table |
| `FILTRE_CNS` | No | Filter for consultation |
| `FILTRE_HIS` | No | Filter for history |

---

### Environment Actions

| Action | Transaction | Description |
|--------|-------------|-------------|
| `DROIT` | No | Before mask population from main table |
| `ENV` | No | On environment change (GEODE & LOAN) |
| `ENV_FEN` | No | On window environment change |

---

### Icon Actions

| Action | Transaction | Description |
|--------|-------------|-------------|
| `ICONE` | No | On double-click on icon at bottom right of screen |

---

### Variables Used in Actions

| Variable | Description |
|----------|-------------|
| `ACTION` | Current action name |
| `OK` | Set to 0 to cancel the action |
| `GOK` | Global OK for transaction control |
| `Fin` | 0 = stay on window, 1 = exit window |
| `status` | Input status return |
| `fstat` | File operation status |
| `[M]` | Mask class for current window |
| `[F:ABR]` | Main table class |

---

### Transaction and OK Variable

**Important Rules:**
- Actions with "Transaction: No" cannot perform database Write/Rewrite/Delete operations
- Setting `OK = 0` in VERIF_* actions cancels the operation
- Setting `GOK = 0` during transaction cancels the entire transaction

**Example - Action Template:**
```l4g
$ACTION
Case ACTION
    When "OUVRE"
        # Initialization on object opening
        GSTATUT = 0
        
    When "VERIF_CRE"
        # Validation before creation
        If [M]CHAMP = ""
            OK = 0
            Errbox "Field is mandatory"
        Endif
        
    When "CREATION"
        # During creation transaction
        [L:LNK]CODREF = [F:HDR]CODHDR
        Call LNK_CREATE([L:LNK])
        
    When "APRES_CRE"
        # After creation completed
        Infbox "Record created: " + [F:HDR]CODHDR
        
    When "FERME"
        # Cleanup on object closing
        Close
Endcase
Return
```

---

### Standard Processing Subprogram Structure

```l4g
Subprog OBJET_MAIN
# Standard OBJet processing subprogram

# Declarations
Local Char ACTION(20)

# Main action handler
$ACTION
Case ACTION
    When "OUVRE"
        Gosub OUVRE
    When "FERME"
        Gosub FERME
    When "CREATION"
        Gosub CREATION
    When "MODIF"
        Gosub MODIF
    When "ANNULE"
        Gosub ANNULE
    When Default
        # Handle other actions
Endcase
Return

# Opening
$OUVRE
    # Initialization code
Return

# Closing
$FERME
    # Cleanup code
Return

# Creation
$CREATION
    # Creation transaction code
Return

# Modification
$MODIF
    # Modification transaction code
Return

# Deletion
$ANNULE
    # Deletion transaction code
Return
End
```

---

## Field Actions (Screen Field Events)

### Overview

Field actions are entry points defined at the screen field level in the dictionary. They allow custom code to be executed at specific moments during field interaction (display, input, validation, etc.). These actions are defined in the Screen dictionary under each field's properties.

### Field Action Types

| Action | Trigger | Usage |
|--------|---------|-------|
| `AVANT_ZONE` | Before any input or display | Define field format, initialize display properties |
| `INIT_BOUTON` | On context menu initialization | Define context menu button labels |
| `INIT` | On field initialization | Initialize field value |
| `AVANT_SAISIE` | Before each input | Set mkstat, prevent input, control field state |
| `CONTROLE` | After input, before validation | Test field validity |
| `APRES_ZONE` | After valid control | Affect or display other fields, trigger calculations |
| `APRES_MODIF` | After field modification | Same as APRES_ZONE but only if field was modified |
| `SELECTION` | F12 key press | Open selection list, zoom, lookup |
| `BOUTON_1` | F9 key press | Tunnel navigation (reserved) |
| `BOUTON_2` to `BOUTON_20` | F4 key press (context menu) | Custom context menu actions |
| `AVANT_LIGNE` | On line entry (scrolling tables) | Actions when entering line modification |
| `APRES_LIGNE` | After line input (scrolling tables only) | Actions after each line is entered |
| `CLIC` | On icon click (icon fields only) | Trigger action when clicking an icon |

---

### AVANT_ZONE (Before Field)

Executed before any input or display of the field.

**Usage:**
- Define field format
- Set field properties dynamically
- Control field visibility or state

**Example:**
```l4g
$ACTION
Case ACTION
    When "AVANT_ZONE"
        # Set date format based on context
        If [M]TYPE = "INT"
            [M]DATE格式 = "DD/MM/YYYY"
        Else
            [M]DATE格式 = "MM/DD/YYYY"
        Endif
Endcase
Return
```

---

### INIT_BOUTON (Initialize Button)

Allows defining context menu button labels dynamically.

**Usage:**
- Set dynamic button labels based on context
- Enable/disable context menu options

**Example:**
```l4g
$ACTION
Case ACTION
    When "INIT_BOUTON"
        # Define context menu labels
        MKBOUT(1) = "Add Line"
        MKBOUT(2) = "Delete Line"
        MKBOUT(3) = "Copy Line"
Endcase
Return
```

---

### INIT (Initialize Field)

Executed to initialize a field value.

**Usage:**
- Set default field values
- Initialize calculated fields
- Set initial field state

**Example:**
```l4g
$ACTION
Case ACTION
    When "INIT"
        # Initialize with current date
        [M]DATECRE = date$
        [M]STATUS = 1
Endcase
Return
```

---

### AVANT_SAISIE (Before Input)

Executed before each field input. The `mkstat` variable controls input behavior.

**mkstat Values:**
| Value | Behavior |
|-------|----------|
| 0 | Normal input |
| 1 | Display only (no input) |
| 2 | Skip field |

**Usage:**
- Prevent input based on conditions
- Set field to display-only mode
- Skip field automatically

**Example:**
```l4g
$ACTION
Case ACTION
    When "AVANT_SAISIE"
        # Prevent modification if record is validated
        If [F:HDR]STATUS = 2
            mkstat = 1  # Display only
        Endif
        
        # Skip field if type requires it
        If [M]TYPE = "SIMPLE"
            mkstat = 2  # Skip field
        Endif
Endcase
Return
```

---

### CONTROLE (Control/Validation)

Executed after input to test field validity. Setting `OK = 0` cancels and returns to the field.

**Usage:**
- Validate field format
- Check business rules
- Perform lookups and cross-validation
- Display error messages

**Example:**
```l4g
$ACTION
Case ACTION
    When "CONTROLE"
        # Validate customer code exists
        If [M]CODCLI <> ""
            Read [CLI]CODCLI = [M]CODCLI
            If fstat <> 0
                OK = 0
                Errbox "Customer code not found: " + [M]CODCLI
                GERR CODCLI  # Return to field
            Endif
        Endif
        
        # Validate amount is positive
        If [M]AMOUNT <= 0
            OK = 0
            Errbox "Amount must be positive"
            GERR AMOUNT
        Endif
Endcase
Return
```

---

### APRES_ZONE (After Field)

Executed after the control if validation passed.

**Usage:**
- Update related fields
- Perform calculations
- Display dependent information
- Trigger cascading updates

**Example:**
```l4g
$ACTION
Case ACTION
    When "APRES_ZONE"
        # When customer changes, update name and address
        If [M]CODCLI <> ""
            Read [CLI]CODCLI = [M]CODCLI
            If fstat = 0
                [M]NOMCLI = [F:CLI]NOMCLI
                [M]ADRCLI = [F:CLI]ADRCLI
                [M]VILCLI = [F:CLI]VILCLI
            Endif
        Endif
Endcase
Return
```

---

### APRES_MODIF (After Modification)

Same as APRES_ZONE but triggered only if the field was actually modified.

**Usage:**
- React only to actual changes
- Avoid unnecessary recalculation
- Track modification history
- Update totals only when needed

**Example:**
```l4g
$ACTION
Case ACTION
    When "APRES_MODIF"
        # Recalculate total only if quantity changed
        [M]TOTLIN = [M]QTY * [M]PRICE
        
        # Apply discount if applicable
        If [M]DISCOUNT > 0
            [M]TOTLIN = [M]TOTLIN * (1 - [M]DISCOUNT / 100)
        Endif
Endcase
Return
```

---

### SELECTION (F12 Key)

Triggered by the F12 key. Used for zoom/lookup functionality.

**Usage:**
- Open selection window
- Display lookup list
- Navigate to related object
- Perform search

**Example:**
```l4g
$ACTION
Case ACTION
    When "SELECTION"
        # Open customer selection
        Local Char SELCLI(20)
        Call CHOIX_CLI(SELCLI) From TABCUST
        If SELCLI <> ""
            [M]CODCLI = SELCLI
            # Trigger field update
            Affzo [M]CODCLI
        Endif
        
        # Alternative: use standard lookup
        # Call ZONE([M]CODCLI, "CLI", "CODCLI")
Endcase
Return
```

---

### BOUTON_1 (F9 Key - Tunnel)

Reserved for tunnel navigation. F9 key triggers this action.

**Usage:**
- Navigate to detail/related screens
- Open transaction tunnel
- Drill-down functionality

**Example:**
```l4g
$ACTION
Case ACTION
    When "BOUTON_1"
        # Tunnel to customer detail
        If [M]CODCLI <> ""
            Call ZONE([M]CODCLI, "BPCUSTOMER", "", "CODCLI")
        Endif
Endcase
Return
```

---

### BOUTON_2 to BOUTON_20 (Context Menu)

F4 key displays context menu with these options. BOUTON_2 through BOUTON_20 are available.

**Usage:**
- Custom actions via context menu
- Quick access to related functions
- Field-specific operations

**Example:**
```l4g
$ACTION
Case ACTION
    When "INIT_BOUTON"
        # Define context menu labels
        MKBOUT(2) = "Calculate Total"
        MKBOUT(3) = "Apply Template"
        MKBOUT(4) = "Clear Line"
        
    When "BOUTON_2"
        # Calculate total action
        [M]TOTLIN = [M]QTY * [M]PRICE
        Affzo [M]TOTLIN
        
    When "BOUTON_3"
        # Apply template action
        Call APPLIQUE_MODELE([M])
        
    When "BOUTON_4"
        # Clear line action
        [M]QTY = 0
        [M]PRICE = 0
        [M]TOTLIN = 0
        Effzo [M]QTY, [M]PRICE, [M]TOTLIN
Endcase
Return
```

---

### AVANT_LIGNE (Before Line - Scrolling Tables Only)

Executed when entering modification of a line in a scrolling table.

**Usage:**
- Initialize line-specific variables
- Store current line state
- Prepare for line modification
- Set line-specific controls

**Example:**
```l4g
$ACTION
Case ACTION
    When "AVANT_LIGNE"
        # Store original quantity for comparison
        S_QTY_ORIG = [M]QTY
        
        # Set line-specific properties
        If [M]LINSTA = "CLOSED"
            mkstat = 1  # Display only
        Endif
Endcase
Return
```

---

### APRES_LIGNE (After Line - Scrolling Tables Only)

Executed after each line input in a scrolling table.

**Usage:**
- Update line totals
- Validate line data
- Update header totals from lines
- Calculate derived values

**Example:**
```l4g
$ACTION
Case ACTION
    When "APRES_LIGNE"
        # Calculate line total
        [M]TOTLIN = [M]QTY * [M]PRICE
        
        # Update header total from all lines
        Call CALC_TOTAL([M]TOTHEAD)
        
        # Calculate difference if modified
        If S_QTY_ORIG <> [M]QTY
            [M]QTYDIFF = [M]QTY - S_QTY_ORIG
        Endif
Endcase
Return
```

---

### CLIC (Click - Icon Fields Only)

Triggered when clicking on an icon field.

**Usage:**
- Open URL or document
- Display popup or dialog
- Toggle icon state
- Execute custom action

**Example:**
```l4g
$ACTION
Case ACTION
    When "CLIC"
        # Open document link
        If [M]DOCLINK <> ""
            Call OPEN_DOCUMENT([M]DOCLINK)
        Else
            Infbox "No document attached"
        Endif
Endcase
Return
```

---

### Field Action Flow

The following diagram shows the order of field action execution:

```
1. AVANT_ZONE     (Before field display/input)
       ↓
2. AVANT_SAISIE   (Before input - can skip or set display-only)
       ↓
3. [User Input]
       ↓
4. CONTROLE       (Validation - can reject with OK=0)
       ↓
5. APRES_ZONE     (After valid input - always runs)
       ↓
6. APRES_MODIF    (After valid input - only if modified)
```

---

### Variables Used in Field Actions

| Variable | Description |
|----------|-------------|
| `mkstat` | Field input mode (0=normal, 1=display, 2=skip) |
| `OK` | Set to 0 to reject input and return to field |
| `ACTION` | Current action name |
| `zonmod` | Indicates if field was modified (1=modified) |
| `nolign` | Current line number in scrolling table |
| `nblig` | Total number of lines in scrolling table |
| `GERR` | Return to specific field on error |
| `Affzo` | Display/refresh field |
| `Effzo` | Clear field |
| `Grizo` | Gray out field (display only) |
| `Dezo` | Enable field for input |

---

### Complete Field Action Example

```l4g
$ACTION
Case ACTION
    # Initialize field with default value
    When "INIT"
        If [M]QTY = 0
            [M]QTY = 1
        Endif
        
    # Control input mode based on status
    When "AVANT_SAISIE"
        If [F:HDR]STATUS = 2
            mkstat = 1  # Display only for validated records
        Endif
        
    # Validate the field
    When "CONTROLE"
        # Check product exists
        If [M]ITMREF <> ""
            Read [ITM]ITMREF = [M]ITMREF
            If fstat <> 0
                OK = 0
                Errbox "Product not found"
                GERR ITMREF
            Endif
        Endif
        
    # Update related fields after valid input
    When "APRES_ZONE"
        # Always update description
        If [M]ITMREF <> ""
            [M]ITMDES = [F:ITM]ITMDES
            [M]PRICE = [F:ITM]PRICE
        Endif
        
    # Calculate only if quantity actually changed
    When "APRES_MODIF"
        [M]TOTLIN = [M]QTY * [M]PRICE
        
    # F12 - Selection list
    When "SELECTION"
        Call SELECT_PRODUCT([M]ITMREF) From TABITEM
        
    # Context menu button
    When "BOUTON_2"
        Call SHOW_PRODUCT_INFO([M]ITMREF)
        
Endcase
Return
```

---

### Scrolling Table Line Actions Example

```l4g
$ACTION
Case ACTION
    # Before entering line modification
    When "AVANT_LIGNE"
        # Store original values
        S_QTY_ORIG = [M]QTY
        S_PRICE_ORIG = [M]PRICE
        
    # After line input
    When "APRES_LIGNE"
        # Calculate line total
        [M]TOTLIN = [M]QTY * [M]PRICE
        
        # If quantity changed, check stock
        If S_QTY_ORIG <> [M]QTY
            Call CHECK_STOCK([M]ITMREF, [M]QTY)
        Endif
        
        # Update header total
        Gosub CALC_HEADER_TOTAL
Endcase
Return

$CALC_HEADER_TOTAL
    Local Decimal TOTAL(12,2)
    TOTAL = 0
    For I = 1 To nblig
        TOTAL += [M]TOTLIN(I)
    Next I
    [M:HDR]TOTAL = TOTAL
Return
```

---

## Keyword Reference

### Declaration Keywords
- `Char`, `Date`, `Decimal`, `Double`, `Integer`, `Shortint`, `Schar`, `Uuint`, `Uuid`, `Blbfile`, `Clbfile`
- `Dim`, `Local`, `Global`, `Const`, `Value`, `Variable`
- `File`, `Mask`, `Report`

### Control Keywords
- `If`, `Then`, `Elsif`, `Else`, `Endif`
- `Case`, `When`, `Default`, `Endcase`
- `For`, `To`, `Step`, `Next`, `Break`
- `While`, `Wend`, `Endwhile`
- `Repeat`, `Until`

### Function/Subprogram Keywords
- `Subprog`, `Funprog`, `Func`, `End`, `Return`
- `Call`, `Gosub`

### Database Keywords
- `Read`, `Readlock`, `Write`, `Rewrite`, `Delete`, `Look`
- `Trbegin`, `Commit`, `Rollback`
- `Sql`, `Execsql`, `Anasql`
- `Lock`, `Unlock`

### Error Handling Keywords
- `Onerrgo`, `Resume`

### File Keywords
- `Openi`, `Openo`, `Openio`, `Close`
- `Getseq`, `Putseq`, `Seek`

### UI Keywords
- `Affzo`, `Effzo`, `Envzo`, `Grizo`, `Enable`, `Disable`
- `Infbox`, `Errbox`, `Askui`, `Choose`
- `Fillbox`, `Setlbox`, `Leftbox`