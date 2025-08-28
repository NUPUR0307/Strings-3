# Strings-3

## Problem1 
TC: O(n)
SC: O(1)

Approach:-

1. Break the number into groups of three digits starting from the right and work on each group individually.
2. Convert each group into words using straightforward rules: directly map small numbers, combine tens and ones where needed, and include “Hundred” if applicable.
3. Attach the appropriate scale word such as “Thousand” or “Million,” then join everything together neatly with proper spacing to form the final sentence.

 Integer to English Words (https://leetcode.com/problems/integer-to-english-words/)
 
class Solution {
    String[] thousands = new String[]{"", " Thousand ", " Million ", " Billion "};
    String[] below_20 = new String[]{"", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
    String[] tens = {"", "", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};

    public String numberToWords(int num) {

        if (num == 0) return "Zero";

        int i = 0;
        StringBuilder result = new StringBuilder();

        while (num > 0) {
            int triplet = num % 1000;

            if (triplet > 0) {
                result.insert(0, helper(triplet).trim() + thousands[i]);
            }

            num = num / 1000;
            i++;
        }

        return result.toString().trim();
    }

    private String helper(int num) {
        StringBuilder result = new StringBuilder();

        if (num < 20) {
            result.append(below_20[num]);
        } else if (num < 100) {
            result.append(tens[num / 10]).append(" ").append(below_20[num % 10]);
        } else {
            result.append(below_20[num / 100]).append(" Hundred ").append(helper(num % 100));
        }

        return result.toString();
    }
}

## Problem2 
Tc: O(n)
SC: O(n)

Approach:-
1.	We scan the expression using a shared index, forming numbers and calling the function recursively when encountering ‘(’.
2.	On each operator or closing parenthesis, we apply the previous operator with the help of tail to keep operator precedence correct. When a ‘)’ is  found, the result of that subexpression is returned right away to the outer context.

Basic Calculator || (https://leetcode.com/problems/basic-calculator-ii/)

class Solution {

    int idx = 0; 

    public int calculate(String s) {
        
        int n = s.length();

        int calc = 0;
        int tail = 0;
        char lastSign = '+';
        int currNum = 0;

        while (idx < n) {
            char c = s.charAt(idx++);
            if (Character.isDigit(c)) {
                currNum = currNum * 10 + c - '0';
            }
            else if (c == '(') {
                currNum = calculate(s);
            }  

            if ((!Character.isDigit(c) && c != ' ')
                || idx == n || c == ')') 
            {
                if (lastSign == '+') {
                    calc = calc + currNum;
                    tail = currNum;
                } else if (lastSign == '-') {
                    calc = calc - currNum;
                    tail = -currNum;
                } else if (lastSign == '*') {
                    calc = (calc - tail) + (tail * currNum);
                    tail = tail * currNum;
                } else if (lastSign == '/') {
                    calc = (calc - tail) + (tail / currNum);
                    tail = tail / currNum;
                }

                currNum = 0;
                lastSign = c;

                if (c == ')') {
                    return calc;
                }
            }
        }
        return calc;
    }
}

