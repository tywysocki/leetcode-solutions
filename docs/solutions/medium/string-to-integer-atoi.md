---
comments: true

tags:
    - Medium
    - Swift
    - String
---

# String to Integer (atoi)

---

# Description

!!! question "LeetCode Problem 8: String to Integer (atoi)"

    Implement the `myAtoi(string s)` function, which converts a string to a 32-bit signed integer (similar to C/C++'s `atoi` function).

    The algorithm for `myAtoi(string s)` is as follows:

    1. Read in and ignore any leading whitespace.

    2. Check if the next character (if not already at the end of the string) is `'-'` or `'+'`. Read this character in if it is either. This determines if the final result is negative or positive respectively. Assume the result is positive if neither is present.

    3. Read in next the characters until the next non-digit character or the end of the input is reached. The rest of the string is ignored.

    4. Convert these digits into an integer (i.e. `"123" -> 123`, `"0032" -> 32`). If no digits were read, then the integer is `0`. Change the sign as necessary (from step 2).

    5. If the integer is out of the 32-bit signed integer range `[-2^31, 2^31 - 1]`, then clamp the integer so that it remains in the range. Specifically, integers less than `-2^31` should be clamped to `-2^31`, and integers greater than `2^31 - 1` should be clamped to `2^31 - 1`.

    6. Return the integer as the final result.

    Note:

    - Only the space character `' '` is considered a whitespace character.

    - **Do not ignore** any characters other than the leading whitespace or the rest of the string after the digits.
    ```

# Examples

```{ .lang title="Example 1" }

Input: s = "42"
Output: 42

Explanation: The underlined characters are what is read in, the caret is the current reader position.
Step 1: "42" (no characters read because there is no leading whitespace)
            
Step 2: "42" (no characters read because there is neither a '-' nor '+')
            
Step 3: "42" ("42" is read in)

The parsed integer is 42.
Since 42 is in the range [-2^31, 2^31 - 1], the final result is 42.
```

```{ .lang title="Example 2" }

Input: s = "-42"
Output: -42

Explanation:
Step 1: "    -42" (leading whitespace is read and ignored)
                
Step 2: "   -42" ('-' is read, so the result should be negative)
                
Step 3: "   -42" ("42" is read in)
                
The parsed integer is -42.
Since -42 is in the range [-2^31, 2^31 - 1], the final result is -42.
```

```{ .lang title="Example 3" }

Input: s = "4193 with words"
Output: 4193

Explanation:
Step 1: "4193 with words" (no characters read because there is no leading whitespace)
            
Step 2: "4193 with words" (no characters read because there is neither a '-' nor '+')
            
Step 3: "4193 with words" ("4193" is read in; reading stops because the next character is a non-digit)
                
The parsed integer is 4193.
Since 4193 is in the range [-2^31, 2^31 - 1], the final result is 4193.
```

--

# Solution


## Intuition

The problem requires parsing a given string to convert it into a 32-bit signed integer while considering leading whitespace, signs, and integer overflow. The solution involves careful handling of each step of the algorithm.

## Approach

1. Trim leading whitespace from the input string using `trimmingCharacters(in:)`.

2. Check for signs ('+' or '-') and set the sign variable accordingly. Remove the sign character if present.

3. Iterate through the remaining characters, checking if they are digits. Convert them into an integer and update the result.

4. Handle integer overflow by checking whether the result exceeds the 32-bit signed integer range during each iteration. If it does, return the clamped result.

5. Return the final integer result with the appropriate sign.

## Complexity

- **Time complexity**: $O(n)$ where $n$ is the length of the input string. We iterate through the string once.

- **Space complexity**: $O(1)$ as we use a constant amount of extra space for variables.

## Code

```swift
class Solution {
    func myAtoi(_ s: String) -> Int {
        // Step 1: Trim leading whitespace
        let str = s.trimmingCharacters(in: .whitespaces)
        // If the string is empty after trimming, return 0
        guard !str.isEmpty else { return 0 }
        
        var sign = 1
        var result = 0
        var i = str.startIndex
        
        // Step 2: Check for signs ('+' or '-') and set the sign variable
        if str[i] == "+" || str[i] == "-" {
            sign = str[i] == "-" ? -1 : 1
            i = str.index(after: i) // Move to the next character
        }
        
        // Step 3: Iterate through the remaining characters
        while i < str.endIndex, str[i].isNumber {
            let digit = Int(String(str[i]))!
            
            // Step 4: Handle integer overflow
            if result > (Int(Int32.max) - digit) / 10 {
                return sign == 1 ? Int(Int32.max) : Int(Int32.min)
            }
            
            // Convert the digit to an integer and update the result
            result = result * 10 + digit
            i = str.index(after: i) // Move to the next character
        }
        
        // Step 5: Return the final integer result with the appropriate sign
        return sign * result
    }
}

```