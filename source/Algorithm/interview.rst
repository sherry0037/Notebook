===========
Interview
===========

---------------------
11/29/2021 - Google
---------------------

1. TransAddition
------------------

A word A is a trans-addition of another word B if by adding 1 letter and mutate the letters in B we can get A. Write a function f which takes two words and determines if the second word is a trans-addition of the first word.

.. topic:: Examples

    f("ACT", "TACK") -> true

    f("ACT", "TACT") -> true

    f("ACT", "CART") -> true

    f("TACK", "ACT") -> false

    f("ACE", "TACK") -> false

.. code-block:: java
  
    public boolean transAddition(String word1, String word2) {
        if (word2.length() - word1.length() != 1) {
            return false;
        }
        
        Set<Character> set = new HashSet<>();
        
        for(int i=0; i<word1.length(); i++) {
            set.add(word1.chatAt(i));
        }
        
        boolean notInSet = false;
        
        for(int i=0; i<word2.length(); i++) {
            if (!set.contains(word2.chatAt(i))) {
                // K
                if (!notInSet) {
                    notInSet = true;
                } else {
                    return false;
                }
                
            }   
        }
        
        return true;

    }

Follow up 1: Given two lists of words, return a list of words from the second list such that the words are trans-addition of the first list.

.. topic:: Example

    f(["ACT", "SET", "FOO"], ["TACK", "TEST", "BAR"]) -> ["TEST", "TACK"]


.. code-block:: java

    public List<String> transAdditionList(List<String> words1, List<String> words2) {
        //List<String> rst = new ArrayList<>();
        Set<String> set = new HashSet<>();
        for (String word1 : words1) {
            for (String word2 : words2) {
                if (transAddition(word1, word2)) {
                    set.add(word2);
                }
            }
        }
        
        List<String> rst = new ArrayList<>(set);
        
        return rst;
      
    }

    public List<String> transAdditionList2(List<String> words1, List<String> words2) {
        Map<String, Boolean> map = new HashMap<>();
        // word2, isAddedInResult
        
        for (String word2 : words2) {
            map.put(word2, false);
        }
        
        Set<String> set = new HashSet<>();
        for (String word1 : words1) {
            for (String word2 : words2) {
                if (map.get(word2)) {
                    continue;
                }
            
                if (transAddition(word1, word2)) {
                    map.put(word2, true);
                    set.add(word2);
                }
            }
        }
        
        List<String> rst = new ArrayList<>(set);
        
        return rst;
      
    }
      
    public List<String> transAdditionList3(List<String> words1, List<String> words2) {
        // Find all the trans-addition of the first list by adding all of the alphabet
        Set<String> set = new HashSet<>();
        for (String word1 : words1) {
            for (Character a : ALPHABET) {
                String newWord = word1 + a;
                newWord.sort();
                word1Set.add(newWord);
            }
        }
        
        List<String> rst = new ArrayList<>();
        
        for (String word2 : words2) {
            word2.sort();
            if (set.contains(word2)) {
                rst.append(word2);
            }
        }
        
        return rst;
    }

Follow up 2: Write a function to return the maximum length of trans-addition chain in a list.

.. topic:: Example

    f(["A", "AT" "ACT", "TACK", "CART", "GOOGLE"]) -> ["TACK", "CART"]

2. Shortest Path
------------------

Given N locations (named 0, 1, ... N-1) and a set of connections, find the length of the shortest path from the starting point S to the end point E.

.. topic:: Example
    
    N = 10
    connections = {(0, 1), (1, 2), (3, 4), (5,9), (7, 8)}
    S = 0
    E = 8

**Approach**: Generate a hash map with key being a location and value being a list of locations the key is connected to. Then do a BFS traversal (use queue to implement) to find the length from S to E.

3. Maze
---------

Write a program that takes a map of a maze as input, and outputs the length of the longest path from the top to the bottom.

Conditions and constraints:

  - The map is a two-dimensional, rectangular board.

  - You can only visit each cell once.

  - You are not allowed to go up, only left, right or down.

  - You can start on any cell at the top row that is not a wall.

  - You can exit on any cell at the bottom row that is not a wall.
    
# .  #  .  .  # 
# .  #  .  .  #
# .  #  #  .  #
# .  .  #  .  #
# .  .  #  .  #
# .  .  #  .  #

result: 9

.. code-block:: java
  
    public int getLongestPath(int[][] maze) {
      // 1 = wall
      // 0 = cell
      int n = maze.size; // number of rows
      int m = maze[0].size; // number of columns
      int[] firstRow = maze[0];
      
      int rst = Integer.MIN_VALUE;
      
      for (int y : firstRow) {
          if (maze[0][y] == 0) {
              int[][] visited = new int[n][m];
              int[][] lengths = new int[n][m];
              int length = DFS(maze, visited, 0, i, lengths);
              rst = Math.max(length, rst);
          }
      }
      
      return rst;
    }

    private int DFS(int[][] maze, int[][] visited, int sx, int sy, int[][] lengths) {
        int n = maze.size; // number of rows
        int m = maze[0].size; // number of columns
        Stack<List<Integer>> stack = new Stack<>()'
        ArrayList<Integer> first = new ArrayList<>();
        first.add(sx);
        first.add(sy);
      
        stack.add(first);
      
        while(!stack.isEmpty()) {
              ArrayList<Integer> current = stack.pop();
              visited[current.get(0)][current.get(1)] = 1;
          
              List<List<Integer>> nextSteps = getNextStep(current, maze);
          
              if (nextSteps.length() == 0) {
                   if (current.get(1) == n) {
                        return lengths[current.get(0)][current.get(1)];
                   } else {
                        continue;
                   }
              }
          
              for (List<Integer> point : nextSteps) {
                  stack.add(point);
                  lengths[point.get(0)][point.get(1)] = lengths[current.get(0)][current.get(1)] + 1;
              }          
        }
      
        return -1;
    }

    private List<List<Integer>> getNextStep(ArrayList<Integer> current, int[][] maze) {
        int n = maze.size; // number of rows
        int m = maze[0].size; // number of columns
        List<List<Integer>> rst = new ArrayList<>();
        // left
        if ((current.get(0) - 1) >=0 && (current.get(0) - 1)<n && maze[current.get(0) - 1][current.get(1)]) {
            List<Integer> point = new ArrayList<>();
            rst.add(current.get(0) - 1);
            rst.add(current.get(1));
        }
      
        // right
        if ((current.get(0) + 1) >=0 && (current.get(0) + 1)<n && maze[current.get(0) + 1][current.get(1)]) {
            List<Integer> point = new ArrayList<>();
            rst.add(current.get(0) + 1);
            rst.add(current.get(1));
        }
      
        // down
        if ((current.get(1) + 1) >=0 && (current.get(1) + 1)<m && maze[current.get(0)][current.get(1) + 1]) {
            List<Integer> point = new ArrayList<>();
            rst.add(current.get(0));
            rst.add(current.get(1) + 1);
        }
      
        return rst;
    }

time complexity O(mn)
space           O(mn)

4. Gliding keyboard
---------------------
For phone keyboard, there is a gliding feature that user can start with one letter and without lifting the finger, finishing typing the whole word. Write a function that given a user input string and a dictionary, output all the possible words the user wants to type.

.. topic:: Example

    Dictionary:

    'apple'

    orange'

    'swipe'

    'app'

    'ale'
     
    input = "sewertyuiopoiuytre", output = "swipe".
            
.. topic:: Example
    
    input = "apccccple" -> 'apple', 'ale'
  
Assumption: start of the input will be the start of the word, end will be the end  
 
.. code-block:: java

    Class WordNode() {
      Character value;
      List<WordNode> nexts;
      WordNode previous;
      boolean isEnd;
      
      public WordNode(Character c){
          this.value = c;
          this.next = new ArrayList<>();
          this.isEnd = false;
          this.previous = null;
      }
      
      public String toString() {
          // return the word as String
      }
    }

    public List<String> main(String input, List<String> dictionary) {
        List<WordNode> wordDict = new ArrayList<>();
        List<String> rst = new ArrayList<>();
      
      // construct the wordDict
        
      
      // find matching
        for (WordNode root : wordDict) {
            rst += findMatchWords(root, input);
        }
    }

    public List<String> findMatchWords(WordNode root, String word, List<String> rst) {
        //WordNode current = root;
        //List<String> rst = new ArrayList<>();
      
        //for (int i=0; i<word.length(); i++) {
        int i = 0;
        if (i>=word.length()) {
            return new ArrayList<>();
        }
      
        Character c = word.charAt(i);
        while (i<word.length) {
            if (root.value == c) {
                if (root.isEnd && i==word.length()-1) {
                    // got a match word
                    rst.append(root.toString());
                }

                i++;

                //Stack<Pair<WordNode, Integer>> stack = new Stack<>();
                //for (WordNode next : current.next) {
                    // 1. none of the nexts match i
                    // 2. some of the nexts match i (1, multiple)
                    //stack.add(new Pair<>(next, i));
                //}
                for (WordNode next : root.next) {
                    // Find matching for current.next, word[i, end];
                    rst.addAll(findMatchWords(next, word.subString(i, word.length()), rst));
                }
            } else {
                i++;
            }
        }  
      return rst;
    }



