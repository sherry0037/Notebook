=======
String
=======

----------------
Useful Methods
----------------

Length of a string: `str1.length();`

Trim a string: `str1 = str1.trim();`

Get char: `char c = str1.charAt(1);`

SubString (start inclusive, end exclusive): `str1.substring(start, end);`

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Convert a list of strings to string
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: java

    String rst = String.join(", ", list);


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
StringBuilder
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: java

    StringBuilder strBuilder = new StringBuilder();
    strBuilder.append("AAA");

    StringBuilder strBuilder = new StringBuilder("AAAA");

    strBuilder.length();

    //Delete char at index
    strBuilder.deleteCharAt(1);


---------------------------------------------
1768. Merge Strings Alternately (Easy)
---------------------------------------------

You are given two strings word1 and word2. Merge the strings by adding letters in alternating order, starting with word1. If a string is longer than the other, append the additional letters onto the end of the merged string.

Return the merged string.

.. topic:: Example 1

    Input: word1 = "abc", word2 = "pqr"
    Output: "apbqcr"
    Explanation: The merged string will be merged as so:
    word1:  a   b   c
    word2:    p   q   r
    merged: a p b q c r

.. topic:: Example 2

    Input: word1 = "ab", word2 = "pqrs"
    Output: "apbqrs"
    Explanation: Notice that as word2 is longer, "rs" is appended to the end.
    word1:  a   b
    word2:    p   q   r   s
    merged: a p b q   r   s

.. topic:: Example 3

    Input: word1 = "abcd", word2 = "pq"
    Output: "apbqcd"
    Explanation: Notice that as word1 is longer, "cd" is appended to the end.
    word1:  a   b   c   d
    word2:    p   q
    merged: a p b q c   d

.. topic:: Constraints:

        1 <= word1.length, word2.length <= 100
        word1 and word2 consist of lowercase English letters.

.. code-block:: java

    class Solution {
        public String mergeAlternately(String word1, String word2) {
            int i = 0;
            int j = 0;
            StringBuilder strBuilder = new StringBuilder();

            while (i < word1.length() || j < word2.length()) {
                if (i < word1.length()) {
                    strBuilder.append(word1.charAt(i));
                }

                if (j < word2.length()) {
                    strBuilder.append(word2.charAt(j));
                }

                i++;
                j++;
            }

            return strBuilder.toString();
        }
    }

.. code-block:: java

    class Solution {
        public String mergeAlternately(String word1, String word2) {
            int n1 = word1.length();
            int n2 = word2.length();
            int n = Math.max(n1, n2);
            StringBuilder strBuilder = new StringBuilder();

            for (int i = 0; i < n; i++) {
                if (i < n1) {
                    strBuilder.append(word1.charAt(i));
                }

                if (i < n2) {
                    strBuilder.append(word2.charAt(i));
                }
            }
            return strBuilder.toString();
        }
    }


---------------------------------------------
151. Reverse Words in a String (Medium)
---------------------------------------------

Given an input string s, reverse the order of the words.

A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.

Return a string of the words in reverse order concatenated by a single space.

Note that s may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.



.. topic:: Example 1

    Input: s = "the sky is blue"
    Output: "blue is sky the"

.. topic:: Example 2

    Input: s = "  hello world  "
    Output: "world hello"
    Explanation: Your reversed string should not contain leading or trailing spaces.

.. topic:: Example 3

    Input: s = "a good   example"
    Output: "example good a"
    Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.

.. topic:: Constraints

    1 <= s.length <= 104
    s contains English letters (upper-case and lower-case), digits, and spaces ' '.
    There is at least one word in s.

.. topic:: Follow-up

    If the string data type is mutable in your language, can you solve it in-place with O(1) extra space?

.. code-block:: java

    class Solution {
        public String reverseWords(String s) {
            List<String> words = new ArrayList<>();
            for (int i = 0; i < s.length(); i++) {
                if (s.charAt(i) != ' ') {
                    String word = "";
                    while (i < s.length() && s.charAt(i) != ' ') {
                        word += s.charAt(i);
                        i++;
                    }
                    i--;
                    words.add(word);
                }
            }
            String result = "";
            for (int i = words.size()-1; i > 0; i--) {
                result += words.get(i) + " ";
            }
            result += words.get(0);
            return result;
        }
    }
