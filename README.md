# Luhn-s-Algorithm
Luhn's Algorithm is a very popular and widely used algorithm that is used to make, rather generate card numbers. eg. 5555 5555 5555 4444.
I made this program while I was working on my cs50 cert and thought to publish this cool little algorithm.

# Luhn's Algorithm – Card Number Validator

A small command-line tool that checks whether a credit card number is valid using **Luhn's algorithm** (the mod 10 checksum) and identifies the card brand: **American Express**, **MasterCard** or **Visa**.

It started as a problem from my CS50 course and is available in two implementations:

| File | Language | Notes |
|------|----------|-------|
| `Algorithm_Checker.py` | Python 3 | No dependencies |
| `Algorithm_Checker.c`  | C        | Requires the CS50 library (`cs50.h`) |

> **Note:** Luhn's algorithm *validates* a number's checksum – it does not generate real, usable card numbers. Test numbers such as `5555 5555 5555 4444` simply happen to pass the check.

## How Luhn's algorithm works

1. Starting from the **second-to-last digit** and moving left, double every other digit.
2. If doubling gives a two-digit number, add its digits together (e.g. `7 × 2 = 14 → 1 + 4 = 5`).
3. Add up all the results from step 2.
4. Add the digits that were **not** doubled.
5. If the total ends in `0` (total % 10 == 0), the number is valid.

**Example:** `4003600000000014`

```
Digits doubled (second-to-last, going left): 1, 6, 0, 4 -> 2, 12, 0, 8
Add their digits:  2 + (1+2) + 0 + 8            = 13
Add the other digits: 4 + 3 + zeros              = 7
```
Total = 13 + 7 = 20 → ends in 0 → **valid**.

## Card brand rules

| Brand | Length | Starting digits |
|-------|--------|-----------------|
| AMEX | 15 | 34, 37 |
| MASTERCARD | 16 | 51–55 |
| VISA | 13 or 16 | 4 |

If the checksum fails or the number matches none of these patterns, the output is `INVALID`.

## Usage

### Python

```bash
python3 Algorithm_Checker.py
```

```
Number : 378282246310005
AMEX
```

Non-numeric input is rejected and you are prompted again.

### C

Requires the [CS50 library](https://github.com/cs50/libcs50) (or the CS50 Codespace / `check50` environment).

```bash
clang -o Algorithm_Checker Algorithm_Checker.c -lcs50
./Algorithm_Checker
```

```
Enter Card Number : 4003600000000014
Number : 4003600000000014
VISA
```

## Sample test numbers

| Number | Expected output |
|--------|-----------------|
| `378282246310005` | AMEX |
| `371449635398431` | AMEX |
| `5555555555554444` | MASTERCARD |
| `5105105105105100` | MASTERCARD |
| `4111111111111111` | VISA |
| `4012888888881881` | VISA |
| `4222222222222` | VISA (13 digits) |
| `1234567890` | INVALID |
| `4111111111111112` | INVALID (bad checksum) |

## Known limitations

- Only AMEX, MasterCard (51–55) and Visa are recognised; newer MasterCard ranges (2221–2720), Discover, etc. are not.
- The C version uses `long`, so it is limited to numbers that fit in a `long`, and its output for non-matching brands can print extra `INVALID` lines before the brand name.

## License

Free to use and learn from.
