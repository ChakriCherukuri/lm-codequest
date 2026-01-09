# LM CodeQuest Python Cheatsheet

## Rounding Function
```py
from decimal import Decimal, ROUND_HALF_UP

def round_half_away_from_zero(x: float, ndigits: int = 0) -> Decimal:
    """
    Round half away from zero using Decimal.
    Examples (ndigits=0):  1.5 -> 2,  -1.5 -> -2
    """
    d = Decimal(str(x))
    factor = Decimal("1").scaleb(ndigits)
    # scale, round half-up, unscale
    return (d * factor).to_integral_value(rounding=ROUND_HALF_UP) / factor

# Examples:
# round_half_away_from_zero("2.345", 2)   -> Decimal('2.35')
# round_half_away_from_zero("-2.345", 2)  -> Decimal('-2.35')
# round_half_away_from_zero("-1.5")       -> Decimal('-2')
```

## f-string formatting

| Use-case | f-string code | Example values → result |
|---|---|---|
| Insert a variable | `f"{name}"` | `name="Ava"` → `Ava` |
| Multiple variables | `f"{name} is {age}"` | `name="Ava", age=12` → `Ava is 12` |
| Expression inside `{}` | `f"{x+y}"` | `x=7, y=3` → `10` |
| Decimal places (rounding) | `f"{pi:.2f}"` | `pi=3.14159265` → `3.14` |
| 3 decimals with leading space for sign | `f"{n: .3f}"` | `n=2.5` → ` 2.500` ; `n=-2.5` → `-2.500` |
| Fixed width + left align | `f"{word:<10}"` | `word="cat"` → `'cat       '` |
| Fixed width + right align | `f"{num:>4}"` | `num=12` → `'  12'` |
| Center align | `f"{title:^8}"` | `title="Hi"` → `'   Hi   '` |
| Zero-pad integer | `f"{n:05d}"` | `n=42` → `00042` |
| Thousands separator | `f"{big:,}"` | `big=1234567890` → `1,234,567,890` |
| Percent | `f"{p:.1%}"` | `p=0.375` → `37.5%` |
| Scientific notation | `f"{x:.2e}"` | `x=12345.678` → `1.23e+04` |
| Always show sign | `f"{v:+d}"` | `v=5` → `+5` ; `v=0` → `+0` ; `v=-5` → `-5` |
| Binary / hex | `f"{n:b}"` / `f"{n:x}"` | `n=26` → `11010` / `1a` |
| Escape braces | `f"{{ {x} }}"` | `x=10` → `{ 10 }` |
| Debug style (Py 3.8+) | `f"{x=}, {y=}, {x+y=}"` | `x=7, y=2` → `x=7, y=2, x+y=9` |

---

## Input parsing

| Use-case | Code | Input | Output |
|---|---|---|---|
| Read one integer | `n = int(input())` | `7` | `n = 7` |
| Read two ints on one line | `a, b = [int(x) for x in input().split()]` | `3 9` | `a = 3, b = 9` |
| Read three ints on one line | `a, b, c = [int(x) for x in input().split()]` | `1 2 3` | `a = 1, b = 2, c = 3` |
| Read a list of ints | `arr = [int(x) for x in input().split()]` | `5 1 4` | `arr = [5, 1, 4]` |
| Read `N` then `N` lines (ints) | <pre>n = int(input())<br>nums = [int(input()) for _ in range(n)]</pre> | <pre>4<br>10<br>20<br>30<br>40</pre> | `n = 4, nums = [10, 20, 30, 40]` |
| Read `N` then a line with `N` ints | <pre>n = int(input())<br>arr = [int(x) for x in input().split()]</pre> | <pre>5<br>9 8 7 6 5</pre> | `n = 5, arr = [9, 8, 7, 6, 5]` |
| Read a whole line as string | `s = input()` | `hello world` | `s = "hello world"` |
| Read characters as a list | `chars = list(input())` | `abc` | `chars = ['a', 'b', 'c']` |
| Grid of digits (no spaces) | <pre>r, c = [int(x) for x in input().split()]<br>grid = [[int(ch) for ch in input()] for _ in range(r)]</pre> | <pre>3 4<br>0101<br>1110<br>0001</pre> | `r = 3, c = 4, grid = [[0,1,0,1],[1,1,1,0],[0,0,0,1]]` |
| Grid with spaces | <pre>r, c = [int(x) for x in input().split()]<br>grid = [[int(x) for x in input().split()] for _ in range(r)]</pre> | <pre>2 3<br>1 2 3<br>4 5 6</pre> | `r = 2, c = 3, grid = [[1,2,3],[4,5,6]]` |
| Multiple test cases `T` (two ints each) | <pre>T = int(input())<br>pairs = [tuple([int(x) for x in input().split()]) for _ in range(T)]</pre> | <pre>3<br>1 2<br>10 20<br>-5 8</pre> | `T = 3, pairs = [(1, 2), (10, 20), (-5, 8)]` |
| Dict → `dict[str, int]` (input `a:1, b:2, c:3`) | `d = {k: int(v) for k, v in (p.split(':') for p in input().split(', '))}` | `a:1, b:2, c:3` | `d = {'a': 1, 'b': 2, 'c': 3}` |
| Dict keys as `int` (input `1:10, 2:20`) | `d = {int(k): int(v) for k, v in (p.split(':') for p in input().split(', '))}` | `1:10, 2:20` | `d = {1: 10, 2: 20}` |
| Dict values as `float` (input `a:1.5, b:2.0`) | `d = {k: float(v) for k, v in (p.split(':') for p in input().split(', '))}` | `a:1.5, b:2.0, c:-3.25` | `d = {'a': 1.5, 'b': 2.0, 'c': -3.25}` |
