# PointsScript Documentation

## Overview
PointsScript (PS) is a simple markup language designed to define custom scoring rules for form fields in your forms.
It allows you to award points based on conditions like the number of selected options or the presence of specific substrings in text fields.
It’s designed for anyone to use, letting you give points based on answers like text or lists with easy conditions.
This documentation covers the syntax, rules, and examples to help you get started.

## Syntax Rules

### Conditions
Use `if (expression) give points` to set rules. The first matching rule decides the points. You can combine multiple expressions in one line using `and` or `or`. For example:
- `if (length(>= 10)) give 5` gives 5 points if the answer is 10 or more characters long.
- `if (length(>= 10) and contains("good")) give 5` gives 5 points if the answer is 10+ characters *and* contains "good".

### Expressions
These are the checks you can use in conditions:
- `count(options operator number)`: Counts items in a list (e.g., `count(options >= 5)` checks for 5 or more items).
- `length(operator number)`: Checks length of text or list (e.g., `length(>= 10)`), or `!length()` to reverse it (e.g., `!length(< 5)` for 5 or more).
- `contains("text")`: Looks for text in an answer (e.g., `contains("good")`), or `!contains()` if it’s not there (e.g., `!contains("bad")`).
- `equals("text")`: Checks if the answer matches exactly (e.g., `equals("yes")`), or `!equals()` if it doesn’t (e.g., `!equals("no")`).
- `matches("text1", "text2", ..., operator number)`: Counts specific answers (e.g., `matches("yes", "maybe", >= 2)` checks for 2 or more matches).
- `missing()`: Checks if no answer was given.
- `nothing()`: Checks if the answer has nothing in it (like "" or an empty list).
- Operators: `==` (equals), `>=` (at least), `<=` (at most), `>` (more than), `<` (less than), `!=` (not equal).
- Combine with `and` (both must be true) or `or` (at least one must be true). `and` is checked before `or` (e.g., `a and b or c` means `(a and b) or c`).

### Default
Use `give points` alone as a fallback if no conditions match (e.g., `give 0` sets a default of 0 points).

### Comments
Lines starting with `#` are ignored (e.g., `# This is a note`).

## Examples

### Give points based on how many options are picked
```ps
if (count(options >= 5)) give 7
if (count(options >= 4)) give 3
if (count(options == 1)) give 1
give 0
```

### Give points for specific combinations of answers
```ps
if (matches("Proprietary tech/IP", "cost reduction", "exclusive partnerships", == 3)) give 5
if (matches("Proprietary tech/IP", "cost reduction", "exclusive partnerships", >= 2)) give 3
if (equals("No clear advantage")) give 0
give 0
```

### Give points for text answers with combined conditions
```ps
if (length(>= 10) and contains("good")) give 5
if (contains("good") or contains("great")) give 4
if (!contains("bad")) give 3
give 0
```

## Validation
PointsScript makes sure your rules work right:
- Rules must be written correctly (e.g., proper syntax for conditions, expressions, and points).
- Points must be 0 or higher (no negative points allowed).
- There needs to be a `give` fallback unless your conditions cover all possibilities.
- No rules can be hidden by earlier ones (e.g., `matches("a", "b", >= 2)` before `matches("a", "b", >= 3)` would make the second unreachable).
