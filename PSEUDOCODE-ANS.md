# 📘 PSEUDOCODE - ANS
### *The Complete Guide to Crack Placement MCQ Rounds (TCS, Infosys, Accenture, Cognizant)*

> **How to use this guide:** Read top-to-bottom once, then use as a reference. Every section maps directly to real placement exam patterns. Dry-run tables are your best friends — practice tracing by hand.

---

## Table of Contents

1. [Number Systems & Memory Units](#1-number-systems--memory-units)
2. [Binary Arithmetic & Complements](#2-binary-arithmetic--complements)
3. [Bitwise Operators — Deep Dive](#3-bitwise-operators--deep-dive)
4. [Pseudocode Syntax Reference](#4-pseudocode-syntax-reference)
5. [Control Structures](#5-control-structures)
6. [Algorithm Patterns — Classic Problems](#6-algorithm-patterns--classic-problems)
7. [Placement Question Bank — Solved with Dry Run](#7-placement-question-bank--solved-with-dry-run)
8. [Common Pseudocode Traps](#8-common-pseudocode-traps)
9. [Quick Reference Cheat Sheet](#9-quick-reference-cheat-sheet)

---

## 1. Number Systems & Memory Units

### 1.1 Memory Hierarchy

| Unit    | Definition              | Size                                       |
|---------|-------------------------|--------------------------------------------|
| **Bit** | Smallest memory unit    | 0 or 1                                     |
| **Nibble** | Group of bits        | 4 bits                                     |
| **Byte** | Group of bits          | 8 bits                                     |
| **Word** | Machine-dependent      | 16, 32, or 64 bits                         |
| **KB**  | Kilobyte                | $2^{10}$ = 1024 Bytes, Address Bus = 10    |
| **MB**  | Megabyte                | $2^{20}$ Bytes, Address Bus = 20           |
| **GB**  | Gigabyte                | $2^{30}$ Bytes, Address Bus = 30           |
| **TB**  | Terabyte                | $2^{40}$ Bytes, Address Bus = 40           |

> 💡 **Exam Tip:** "Address Bus = n" means the system can address $2^n$ unique memory locations.

---

### 1.2 Number System Conversion

#### Decimal → Binary
Repeatedly divide by 2 and record remainders (read bottom-up).

**Example:** $(19)_{10} = (?)_2$

| Division | Quotient | Remainder |
|----------|----------|-----------|
| 19 ÷ 2   | 9        | **1**     |
| 9 ÷ 2    | 4        | **1**     |
| 4 ÷ 2    | 2        | **0**     |
| 2 ÷ 2    | 1        | **0**     |
| 1 ÷ 2    | 0        | **1**     |

**Result:** $(19)_{10} = (10011)_2$ ✅

#### Binary → Decimal
Use the positional weight table: $128\ \ 64\ \ 32\ \ 16\ \ 8\ \ 4\ \ 2\ \ 1$

**Example:** $(110110)_2 = (?)_{10}$

$$= 1{\times}32 + 1{\times}16 + 0{\times}8 + 1{\times}4 + 1{\times}2 + 0{\times}1 = 32 + 16 + 4 + 2 = (54)_{10}$$

---

## 2. Binary Arithmetic & Complements

### 2.1 Binary Addition Rules

| A | B | Sum | Carry |
|---|---|-----|-------|
| 0 | 0 |  0  |   0   |
| 0 | 1 |  1  |   0   |
| 1 | 1 |  0  |   1   |
| 1 | 0 |  1  |   0   |

### 2.2 1's Complement
**Rule:** Invert every bit (0 → 1, 1 → 0).

```
Original:  1101
1's Comp:  0010
```

### 2.3 2's Complement
**Rule:** 1's Complement **+1**. Used to represent negative integers in memory.

```
Original (e.g. 0010 1101):
Step 1 - 1's Comp:  1101 0010
Step 2 - Add 1:     1101 0011   ← This is the 2's complement
```

> 💡 **Quick Trick:** Starting from the rightmost bit, keep all bits the same until you hit the first `1`. After the first `1`, invert everything to the left.

---

## 3. Bitwise Operators — Deep Dive

### 3.1 Operator Table

| Operator      | Symbol | Rule                                                    |
|---------------|--------|---------------------------------------------------------|
| **AND**       | `&`    | Result is `1` only if **both** bits are `1`             |
| **OR**        | `\|`   | Result is `1` if **at least one** bit is `1`            |
| **XOR**       | `^`    | Result is `1` if bits are **different** (`0,0→0`, `1,1→0`, `0,1→1`, `1,0→1`) |
| **NOT / Complement** | `~` | Inverts all bits                                  |
| **Left Shift**  | `<<` | Shifts bits left; fills right with `0`                  |
| **Right Shift** | `>>` | Shifts bits right; fills left with `0` (for positives)  |

### 3.2 Worked Example — AND, OR, XOR

```
x = 30  →  Binary: 1 1 1 1 0
y = 12  →  Binary: 0 1 1 0 0
           ─────────────────
x & y   →          0 1 1 0 0  =  18  (AND)
x | y   →          1 1 1 1 0  =  30  (OR)  ← Wait, let's recalculate
x ^ y   →          1 0 0 1 0  =  18  (XOR)
```

> Verify: `30 & 12 = 18`, `30 | 12 = 30`, `30 ^ 12 = 18`.

### 3.3 ⚡ Shift Operator Tricks (EXAM FAVOURITE)

#### Right Shift → Divide by $2^n$

$$x >> n \equiv \lfloor x / 2^n \rfloor$$

```
x = 5   →  0 0 0 0 0 1 0 1
x >> 1  →  0 0 0 0 0 0 1 0  =  2   (5 / 2 = 2, integer division)

x = 16  →  0 0 0 1 0 0 0 0
x >> 2  →  0 0 0 0 0 1 0 0  =  4   (16 / 4 = 4)
```

#### Left Shift → Multiply by $2^n$

$$x << n \equiv x \times 2^n$$

```
x = 1   →  0 0 0 0 0 0 0 1
x << 3  →  0 0 0 0 1 0 0 0  =  8   (1 × 8 = 8)
```

### 3.4 Bitwise Complement — Signed Integers

The `~` operator on a **signed integer** follows the formula:

$$\sim x = -(x + 1)$$

**Example: `int x = 5`**

```
Step 1: Binary of 5         →  0 0 0 0 0 1 0 1
Step 2: Invert all bits     →  1 1 1 1 1 0 1 0   (This is -1 if negative)
Step 3: Sign bit is 1, so negative → find magnitude using 2's complement
        Invert:              0 0 0 0 0 1 0 1
        Add 1:               0 0 0 0 0 1 1 0  =  6
Result: ~5 = -6
```

**Example: `int x = -8`**

```
-8 in binary (2's complement for 8-bit):
8   =  0 0 0 0 1 0 0 0
~8  =  1 1 1 1 0 1 1 1  → 2's comp → 0 0 0 0 1 0 0 0 + 1 = Not needed
Actually: ~(-8) = -(-8 + 1) = -(-7) = +7
```

> 📌 **Formula to memorize:** `~x = -(x + 1)` for all integers.

---

## 4. Pseudocode Syntax Reference

### 4.1 Variable Declaration

```pseudocode
DECLARE variableName : DataType
```

| Data Type   | Use For              | Example                        |
|-------------|----------------------|--------------------------------|
| `INTEGER`   | Whole numbers        | `DECLARE count : INTEGER`      |
| `REAL`      | Decimal numbers      | `DECLARE s : REAL`             |
| `STRING`    | Text values          | `DECLARE uid : STRING`         |
| `BOOLEAN`   | True/False flags     | `DECLARE found : BOOLEAN`      |

### 4.2 Assignment

```pseudocode
SET variable := value       // Formal assignment
variable = expression       // Also accepted in most placement exams
```

### 4.3 Input / Output

```pseudocode
READ variableName           // Take input from user
PRINT "message"             // Display output
WRITE "message"             // Same as PRINT
```

### 4.4 Operators

| Category   | Operators                     |
|------------|-------------------------------|
| Arithmetic | `+  -  *  /  MOD  ^` (power)  |
| Comparison | `== != > < >= <=`             |
| Logical    | `AND  OR  NOT`                |
| Bitwise    | `& \| ^ ~ << >>`              |

> ⚠️ **Critical:** `MOD` = modulus (remainder). `DIV` or `/` between integers = **integer division** (floor).

---

## 5. Control Structures

### 5.1 Simple IF

```pseudocode
IF condition THEN
    statement(s)
END IF
```

### 5.2 IF-ELSE

```pseudocode
IF condition THEN
    S1
ELSE
    S2
END IF
```

**Example — Check Positive/Negative:**

```pseudocode
DECLARE num : INTEGER
READ num
IF num > 0 THEN
    PRINT "Number is +ve"
ELSE
    PRINT "Number is -ve"
END IF
```

### 5.3 ELSE-IF Ladder

```pseudocode
IF condition1 THEN
    S1
ELSE IF condition2 THEN
    S2
ELSE IF condition3 THEN
    S3
ELSE
    Default_S
END IF
```

**Example — Quadrant Detection:**

```pseudocode
DECLARE x, y : INTEGER
READ x, y
IF x > 0 AND y > 0 THEN
    PRINT "1st Quadrant"
ELSE IF x > 0 AND y < 0 THEN
    PRINT "4th Quadrant"
ELSE IF x < 0 AND y > 0 THEN
    PRINT "2nd Quadrant"
ELSE IF x < 0 AND y < 0 THEN
    PRINT "3rd Quadrant"
ELSE
    PRINT "Origin"
END IF
```

### 5.4 Nested IF

```pseudocode
// Login Check (from notes - TIT system)
DECLARE uid, pass : STRING
READ uid, pass
IF uid == "TIT" THEN
    IF pass == "TIT" THEN
        PRINT "Welcome to TIT"
    ELSE
        PRINT "Wrong Password"
    END IF
ELSE
    PRINT "Invalid UID"
END IF
```

---

### 5.5 FOR Loop

```pseudocode
FOR variable := START TO END STEP value
    statements
END FOR
```

**Default STEP = 1** if not specified.

```pseudocode
// Print 1 to 10
FOR i := 1 TO 10
    PRINT i
END FOR

// Print 10 to 1 (reverse)
FOR i := 10 TO 1 STEP -1
    PRINT i
END FOR
```

---

### 5.6 WHILE Loop *(Entry-Controlled)*

> Checks condition **before** executing. May execute **0 times** if condition is false initially.

```pseudocode
// Structure
WHILE condition DO
    statements
    INCREMENT/DECREMENT
END WHILE

// Example: Print 1 to 10
DECLARE i : INTEGER
SET i := 1
WHILE i <= 10 DO
    PRINT i
    INCREMENT i       // i := i + 1
END WHILE
```

---

### 5.7 REPEAT-UNTIL / DO-WHILE *(Exit-Controlled)*

> Checks condition **after** executing. Always executes **at least once**.

```pseudocode
// Structure
REPEAT
    statements
    INCREMENT/DECREMENT
UNTIL condition

// Example: Count down from 10
DECLARE i : INTEGER
SET i := 10
REPEAT
    PRINT i
    DECREMENT i
UNTIL i <= 0
```

**Comparison Table:**

| Feature           | WHILE          | REPEAT-UNTIL    |
|-------------------|----------------|-----------------|
| Check Position    | **Before** body | **After** body  |
| Min Executions    | **0**           | **1**           |
| Use When...       | Might skip      | Always runs once|

---

### 5.8 Jump Statements

| Statement  | Effect                                            |
|------------|---------------------------------------------------|
| `BREAK`    | Immediately exits the **entire loop**             |
| `CONTINUE` | Skips **current iteration**, continues next       |
| `RETURN`   | Exits function, optionally returns a value        |
| `GOTO`     | Jumps to a labelled line (avoid in modern code)   |

**Example 1 — BREAK (print 1 to 4, stop at 5):**

```pseudocode
FOR i := 1 TO 10
    IF i == 5 THEN
        BREAK
    END IF
    PRINT i
END FOR
// Output: 1 2 3 4
```

**Example 2 — CONTINUE (print only even numbers):**

```pseudocode
FOR i := 1 TO 10
    IF i MOD 2 != 0 THEN
        CONTINUE
    END IF
    PRINT i
END FOR
// Output: 2 4 6 8 10
```

**Example 3 — CONTINUE (print only odd numbers):**

```pseudocode
FOR i := 1 TO 10
    IF i MOD 2 == 0 THEN
        CONTINUE
    END IF
    PRINT i
END FOR
// Output: 1 3 5 7 9
```

---

### 5.9 Functions / Procedures

```pseudocode
FUNCTION functionName(param1 : Type, param2 : Type) : ReturnType
    DECLARE localVar : Type
    // body
    RETURN value
END FUNCTION

// Call it:
PRINT functionName(arg1, arg2)
```

---

## 6. Algorithm Patterns — Classic Problems

### 6.1 Reverse a Number

**Logic:** Extract last digit via `MOD 10`, build reversed number, shrink original via integer division by 10.

```pseudocode
FUNCTION reverse(n : INTEGER) : INTEGER
    DECLARE a, b : INTEGER
    SET b := 0
    WHILE n > 0 DO
        a := n MOD 10       // Extract last digit
        n := n / 10         // Remove last digit (integer division)
        b := b * 10 + a     // Append digit to reversed number
    END WHILE
    RETURN b
END FUNCTION

PRINT reverse(21538)    // Output: 83512
```

**Dry Run for `n = 21538`:**

| Iteration | n     | a (n MOD 10) | n (after /10) | b (b*10 + a) |
|-----------|-------|--------------|---------------|--------------|
| 1         | 21538 | 8            | 2153          | 8            |
| 2         | 2153  | 3            | 215           | 83           |
| 3         | 215   | 5            | 21            | 835          |
| 4         | 21    | 1            | 2             | 8351         |
| 5         | 2     | 2            | 0             | 83512        |

**Result:** `83512` ✅

---

### 6.2 Palindrome Check

**Logic:** A number is a palindrome if `reverse(n) == n`.

```pseudocode
FUNCTION palindrome(n : INTEGER) : BOOLEAN
    DECLARE a, b : INTEGER
    DECLARE num : INTEGER
    SET num := n                // Save original
    SET b := 0
    WHILE num > 0 DO
        a := num MOD 10
        num := num / 10
        b := b * 10 + a
    END WHILE
    IF n == b THEN
        RETURN TRUE
    ELSE
        RETURN FALSE
    END IF
END FUNCTION

PRINT palindrome(283382)    // Output: TRUE
PRINT palindrome(12345)     // Output: FALSE
```

---

### 6.3 Armstrong Number

**Definition:** A number where the sum of each digit raised to the power of the **total digit count** equals the number itself.

$$153 = 1^3 + 5^3 + 3^3 = 1 + 125 + 27 = 153 \checkmark$$

```pseudocode
// (Notes use 3-digit Armstrong — cubes)
DECLARE n, y, b, x : INTEGER
SET b := 0
SET x := n          // Save original
WHILE n > 0 DO
    y := n MOD 10
    b := b + (y * y * y)    // Sum of cubes of digits
    n := n / 10
END WHILE
IF x == b THEN
    PRINT "Armstrong"
ELSE
    PRINT "Not Armstrong"
END IF
```

**Dry Run for `n = 153`:**

| Iter | n   | y (n MOD 10) | n (/10) | b (b + y³)       |
|------|-----|--------------|---------|------------------|
| 1    | 153 | 3            | 15      | 0 + 27 = 27      |
| 2    | 15  | 5            | 1       | 27 + 125 = 152   |
| 3    | 1   | 1            | 0       | 152 + 1 = **153**|

`x = 153 == b = 153` → **Armstrong** ✅

---

### 6.4 Digit Frequency (Count Occurrences)

**Task:** Count how many times digit `d` appears in number `num`.

```pseudocode
DECLARE num, x, d : INTEGER
DECLARE count : INTEGER
SET count := 0
SET d := 2          // digit to find
WHILE num > 0 DO
    x := num MOD 10
    num := num / 10
    IF x == d THEN
        INCREMENT count
    ELSE
        CONTINUE
    END IF
END WHILE
PRINT count
```

---

### 6.5 Digit Search (Does Digit Exist?)

**Task:** Check if digit `d` exists in `num`. Uses a Boolean flag.

```pseudocode
// Find digit d=2 in num=12345
DECLARE num, x, d : INTEGER
DECLARE found : BOOLEAN
SET d := 2
SET found := FALSE
WHILE num > 0 DO
    x := num MOD 10
    num := num / 10
    IF x == d THEN
        found := TRUE
        BREAK           // Stop searching once found
    END IF
END WHILE

IF found == TRUE THEN
    PRINT "Search Successful"
ELSE
    PRINT "Search Unsuccessful"
END IF
```

---

### 6.6 Sum of Series: $1 + \frac{1}{2} + \frac{1}{3} + \dots + \frac{1}{n}$

> ⚠️ **Key:** Must use `REAL` type — integer division of `1/i` would give `0` for all `i > 1`!

```pseudocode
FUNCTION sum(n : INTEGER) : REAL
    DECLARE S : REAL
    SET S := 0.0
    FOR i := 1 TO n
        S := S + 1/i        // REAL division — critical!
    END FOR
    RETURN S
END FUNCTION

PRINT sum(5)    // Output: 1 + 0.5 + 0.33 + 0.25 + 0.2 = 2.283...
```

---

### 6.7 Sum of Series: $1 + 2 + 3 + \dots + n$

```pseudocode
FUNCTION SumN(n : INTEGER) : INTEGER
    DECLARE S : INTEGER
    SET S := 0
    FOR i := 0 TO n
        S := S + i
    END FOR
    RETURN S
END FUNCTION

PRINT SumN(5)   // Output: 15
```

---

### 6.8 Multiplication Table

```pseudocode
DECLARE n : INTEGER
DECLARE sum : INTEGER
READ n
SET sum := 0
FOR i := 1 TO n
    sum := sum + (i * n)    // Or just PRINT n * i
END FOR
```

---

### 6.9 Count All Digits of a Number

```pseudocode
// Count digits in 123489 = 6
// Reuse the reverse/digit-extract loop, but just increment a counter
DECLARE count : INTEGER
SET count := 0
WHILE num > 0 DO
    num := num / 10
    INCREMENT count
END WHILE
PRINT count
```

---

## 7. Placement Question Bank — Solved with Dry Run

---

### 🔴 Problem 1 — Tracing `Integer fun(a, b, c)` [Page 9 Style]

**Given:** `a = 4, b = 4, c = 7`

```pseudocode
INTEGER fun(Integer a, Integer b, Integer c)
    FOR each c FROM 3 TO 5
        a := (c + c) MOD c          // Line A
        IF (0 + c) < (c + a) THEN   // Line B
            b := (a + 1) + c
        ELSE
            c := b + b
            a := 3 + b
            CONTINUE
        END IF
    END FOR
    RETURN a + b
```

**Step-by-step Dry Run:**

> Initial values: `a = 4, b = 4`, loop variable `c` goes from 3 to 5.

| c | Line A: `a = (c+c) MOD c` | Condition `(0+c) < (c+a)`? | Action | a | b |
|---|--------------------------|---------------------------|--------|---|---|
| 3 | `a = (3+3) MOD 3 = 6 MOD 3 = 0` | `(0+3) < (3+0)` → `3 < 3` → **FALSE** | `c = b+b = 8`, `a = 3+b = 7`, CONTINUE | 7 | 4 |
| 4 | `a = (4+4) MOD 4 = 8 MOD 4 = 0` | `(0+4) < (4+0)` → `4 < 4` → **FALSE** | `c = b+b = 8`, `a = 3+b = 7`, CONTINUE | 7 | 4 |
| 5 | `a = (5+5) MOD 5 = 10 MOD 5 = 0` | `(0+5) < (5+0)` → `5 < 5` → **FALSE** | `c = b+b = 8`, `a = 3+b = 7`, CONTINUE | 7 | 4 |

**RETURN** `a + b = 7 + 4 = **11**`

---

### 🔴 Problem 2 — Tracing `Integer fun(a, b, c)` [Page 13 Style]

**Given:** `a = 6, b = 9, c = 2`

```pseudocode
INTEGER fun(Integer a, Integer b, Integer c)
    FOR each c FROM 4 TO 8
        a := (a + 1) + b        // Line 1
        a := (c + 3) + b        // Line 2 (overwrites Line 1!)
    END FOR

    b := (5 + 10) + a           // After first loop
    a := (10 + 8) + a           // Updates a

    FOR each c FROM 0 TO 5
        a := (10 + 2) & a       // Line 3: Bitwise AND
        b := (3 + 4) + a        // Line 4
    END FOR

    RETURN a + b
```

**Dry Run — Loop 1 (c: 4 to 8), initial `a=6, b=9`:**

| c | a = (a+1)+b | a = (c+3)+b | a (final) | b |
|---|-------------|-------------|-----------|---|
| 4 | 6+1+9 = 16  | (4+3)+9=16  | 16        | 9 |
| 5 | 16+1+9=26   | (5+3)+9=17  | 17        | 9 |
| 6 | 17+1+9=27   | (6+3)+9=18  | 18        | 9 |
| 7 | 18+1+9=28   | (7+3)+9=19  | 19        | 9 |
| 8 | 19+1+9=29   | (8+3)+9=20  | 20        | 9 |

After Loop 1: `a = 20, b = 9`

**Between loops:**
- `b = (5 + 10) + a = 15 + 20 = 35`
- `a = (10 + 8) + a = 18 + 20 = 38`

State: `a = 38, b = 35`

**Dry Run — Loop 2 (c: 0 to 5):**

> `a = (10 + 2) & a = 12 & a`  
> `12` in binary = `0...01100`

| c | a = 12 & a                 | b = (3+4)+a = 7+a | a   | b   |
|---|----------------------------|-------------------|-----|-----|
| 0 | 12 & 38 = `01100 & 100110` = `000100` + `001000` = only shared bits...  | Let's compute: 38=100110, 12=001100; AND=000100=4 | 4 | 7+4=11 |
| 1 | 12 & 4 = 4 (`01100 & 00100 = 00100`) | 7+4=11 | 4 | 11 |
| 2 | 12 & 4 = 4 | 7+4=11 | 4 | 11 |
| 3 | 12 & 4 = 4 | 7+4=11 | 4 | 11 |
| 4 | 12 & 4 = 4 | 7+4=11 | 4 | 11 |
| 5 | 12 & 4 = 4 | 7+4=11 | 4 | 11 |

**RETURN** `a + b = 4 + 11 = **15**`

---

### 🔴 Problem 3 — Nested IF Login System

**Question:** What is the output when `uid = "ABC"` and `pass = "TIT"`?

```pseudocode
DECLARE uid, pass : STRING
READ uid, pass

IF uid == "TIT" THEN
    IF pass == "TIT" THEN
        PRINT "Welcome to TIT"
    ELSE
        PRINT "Wrong Password"
    END IF
ELSE
    PRINT "INVALID UID"
END IF
```

**Trace:**
- `uid = "ABC"` → `uid == "TIT"` is **FALSE**
- Goes to outer `ELSE`
- **Output: `INVALID UID`**

---

### 🔴 Problem 4 — REPEAT-UNTIL (Count Up & Down)

**Count up from 1 to 10, then count down:**

```pseudocode
// Count Up
DECLARE i : INTEGER
SET i := 1
REPEAT
    PRINT i
    INCREMENT i
UNTIL i > 10
// Output: 1 2 3 4 5 6 7 8 9 10

// Count Down from 10
SET i := 10
REPEAT
    PRINT i
    DECREMENT i
UNTIL i <= 0
// Output: 10 9 8 7 6 5 4 3 2 1
```

---

### 🔴 Problem 5 — Even/Odd Check

```pseudocode
DECLARE num : INTEGER
READ num
IF num MOD 2 == 0 THEN
    PRINT "num is even"
ELSE
    RETURN     // or PRINT "num is odd"
END IF
```

---

### 🔴 Problem 6 — Palindrome via Function (from Notes)

```pseudocode
FUNCTION palindrome(n : INTEGER) : BOOLEAN
    DECLARE a, b : INTEGER
    DECLARE num : INTEGER
    SET num := n
    SET b := 0
    WHILE num > 0 DO
        a := num MOD 10
        num := num / 10
        b := b * 10 + a
    END WHILE
    IF n == b THEN
        RETURN TRUE
    ELSE
        RETURN FALSE
    END IF
END FUNCTION

PRINT palindrome(283382)    // TRUE
```

**Dry Run for `n = 283382`:**

| Iter | num    | a    | num (/10) | b          |
|------|--------|------|-----------|------------|
| 1    | 283382 | 2    | 28338     | 2          |
| 2    | 28338  | 8    | 2833      | 28         |
| 3    | 2833   | 3    | 283       | 283        |
| 4    | 283    | 3    | 28        | 2833       |
| 5    | 28     | 8    | 2         | 28338      |
| 6    | 2      | 2    | 0         | 283382     |

`n = 283382 == b = 283382` → **TRUE** ✅

---

## 8. Common Pseudocode Traps

These are the **most common mistake areas** in placement exams. Read and memorize every one.

---

### ⚠️ Trap 1: Integer Division (NOT Real Division)

```pseudocode
DECLARE x : INTEGER
SET x := 1/3       // x = 0, NOT 0.333!

DECLARE y : REAL
SET y := 1/3       // y = 0.333 ✅
```

> **Rule:** Division between two integers always **floors** the result. To get decimals, at least one operand must be `REAL` type.

---

### ⚠️ Trap 2: Loop Variable Mutation Inside Loop

```pseudocode
FOR i := 1 TO 10
    i := i + 1      // ⚠️ DANGEROUS! Skips iterations
    PRINT i
END FOR
// Prints: 2 4 6 8 10  (not 1 to 10!)
```

---

### ⚠️ Trap 3: CONTINUE vs BREAK Confusion

```pseudocode
FOR i := 1 TO 5
    IF i == 3 THEN
        CONTINUE    // Skips 3, continues loop → Output: 1 2 4 5
    END IF
    PRINT i
END FOR

FOR i := 1 TO 5
    IF i == 3 THEN
        BREAK       // Stops at 3 → Output: 1 2
    END IF
    PRINT i
END FOR
```

---

### ⚠️ Trap 4: REPEAT-UNTIL vs WHILE — Condition Direction

```pseudocode
// WHILE stops when condition becomes FALSE
WHILE i <= 10 DO ... END WHILE    // Runs while i ≤ 10

// REPEAT-UNTIL stops when condition becomes TRUE
REPEAT ... UNTIL i > 10           // Stops when i > 10
```

> These two are **logically equivalent** in this example, but in edge cases (when the condition is false from the start), WHILE executes **0 times** and REPEAT executes **1 time**.

---

### ⚠️ Trap 5: SET d := 2 Means "digit to find is 2", NOT array index

In the frequency/search problems, `d` is initialized as the target digit value. Don't confuse it with index or loop variable.

---

### ⚠️ Trap 6: MOD vs Integer Division

```pseudocode
17 MOD 5  = 2        // Remainder after dividing 17 by 5 (5×3=15, remainder 2)
17 / 5    = 3        // Integer quotient (floor)
17.0 / 5  = 3.4      // Real division
```

---

### ⚠️ Trap 7: Nested Loop Output Count

```pseudocode
FOR i := 1 TO n
    FOR j := 1 TO n
        PRINT "*"
    END FOR
END FOR
// Total prints = n × n = n²
```

> Always multiply outer × inner iteration counts to find total iterations.

---

### ⚠️ Trap 8: Bitwise vs Logical Operators

| Expression       | Meaning                                          |
|------------------|--------------------------------------------------|
| `a AND b`        | **Logical AND** — True/False result              |
| `a & b`          | **Bitwise AND** — operates on individual bits    |
| `a OR b`         | **Logical OR**                                   |
| `a \| b`         | **Bitwise OR**                                   |

> In placement exams, mixing these is a common trap! `5 & 3 = 1` but `5 AND 3 = TRUE`.

---

### ⚠️ Trap 9: Shift Operator on Negative Numbers

```pseudocode
x = -8
x >> 1     // In most languages: -4 (arithmetic right shift, preserves sign)
           // NOT simply -8 / 2 in all contexts!
```

> Right shift on **negative numbers** fills with the **sign bit** (1), not 0. This is arithmetic shift.

---

### ⚠️ Trap 10: Function Return Doesn't Exit in Pseudocode Unless Explicit

```pseudocode
FUNCTION check(x : INTEGER) : STRING
    IF x > 0 THEN
        RETURN "Positive"
    END IF
    RETURN "Non-Positive"    // Reached only if x <= 0
END FUNCTION
```

---

## 9. Quick Reference Cheat Sheet

### Memory Units
$$\text{Bit} < \text{Nibble (4)} < \text{Byte (8)} < \text{Word (16/32)} < \text{KB} (2^{10}) < \text{MB} (2^{20}) < \text{GB} (2^{30}) < \text{TB} (2^{40})$$

### Key Formulas
| Formula | Value |
|---------|-------|
| `~x` | $-(x+1)$ |
| `x << n` | $x \times 2^n$ |
| `x >> n` | $\lfloor x / 2^n \rfloor$ |
| `x MOD 2 == 0` | x is even |
| `x MOD 2 != 0` | x is odd |

### Loop Choice Guide

```
Know exact iterations?  → FOR loop
Condition-based entry?  → WHILE loop
Must execute at once?   → REPEAT-UNTIL
```

### 2's Complement Quick Steps
1. Write binary of the number
2. Invert all bits (1's complement)
3. Add 1

### Palindrome/Reverse Template
```pseudocode
SET b := 0
WHILE n > 0 DO
    a := n MOD 10
    n := n / 10
    b := b * 10 + a
END WHILE
```

### Armstrong Template
```pseudocode
SET b := 0, SET x := n
WHILE n > 0 DO
    y := n MOD 10
    b := b + (y * y * y)
    n := n / 10
END WHILE
IF x == b → Armstrong
```

---

> 📌 **Final Tip for Placement Exams:**
> 1. Always trace through code with a dry-run table — never guess.
> 2. Watch for `INTEGER` vs `REAL` in division problems.
> 3. Know `~x = -(x+1)` and shift tricks cold.
> 4. The REPEAT-UNTIL loop always runs at least once — easy marks if you remember this.
> 5. Nested IFs: trace the **outermost** condition first, then go inward.

---

*Document compiled from classroom notes | Topics: Number Systems, Bitwise Ops, Pseudocode Control Structures, Classic Algorithms*
