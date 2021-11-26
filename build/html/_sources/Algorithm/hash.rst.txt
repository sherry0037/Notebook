=====================
Hash
=====================

.. contents::
    :depth: 2

----------------------------
41. First Missing Positive
----------------------------

Given an unsorted integer array nums, return the smallest missing positive integer.

You must implement an algorithm that runs in O(n) time and uses constant extra space.

.. topic:: Example 1

	Input: nums = [1,2,0]

	Output: 3


.. topic:: Example 2

	Input: nums = [3,4,-1,1]
	
	Output: 2

.. topic:: Example 3

	Input: nums = [7,8,9,11,12]

	Output: 1
 

.. topic:: Constraints

	1 <= nums.length <= 5 * 105

	-231 <= nums[i] <= 231 - 1

**Approach**: Given an array of length n, the missing positive integer must be 1 - n+1. For example, for length 5, to get the largest missing positive integer, the array must be [1,2,3,4,5], then the rst is 6. So we can create a array of length n, where index i means i+1 is not missing. For example, rst[0] = 1 means 1 is given. rst[1] = 0 means 2 is not given. Then we traverse nums once to fill in rst, then traverse rst to get the first index with 0. 

.. code-block:: java

    public int firstMissingPositive(int[] nums) {
        int n = nums.length;
        int[] rst = new int[n];
        
        for (int i : nums) {
            if (i<=n && i >= 1) {
                rst[i-1] = 1;
            }
        }
        
        for (int j=0; j<n; j++) {
            if (rst[j] == 0) {
                return j+1;
            }
        }
        
        return n+1;
    }

-----------------------------------
128. Longest Consecutive Sequence
-----------------------------------

Given an unsorted array of integers nums, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in O(n) time.

**Approach**: Add all the nums to a Hash Set. Then for each num, determine if it is the start of a sequence (n-1 is not in the set), then keep counting while n+1 is in the set.

 
.. topic:: Example 1

    Input: nums = [100,4,200,1,3,2]

    Output: 4

    Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.


.. topic:: Example 2

    Input: nums = [0,3,7,2,5,8,4,6,0,1]

    Output: 9
 

.. topic:: Constraints

    0 <= nums.length <= 105

    -109 <= nums[i] <= 109


.. code-block:: java

    public int longestConsecutive(int[] nums) {
        HashSet<Integer> count = new HashSet<Integer>();
        
        for (int n : nums) {
            count.add(n);
        }
        
        int maxL = 0;
        
        for (int n : nums) {
            // this step is very important: we only count the sequence that starts with n.
            if (count.contains(n-1)) {
                continue;
            }
            int current = n;
            int currentL = 1;
            while (count.contains(current+1)) {
                currentL++;
                current++;
            }
            
            maxL = Math.max(currentL, maxL);
            
        }
        
        return maxL;
    }

----------------------------------
825. Friends Of Appropriate Ages
----------------------------------

There are n persons on a social media website. You are given an integer array ages where ages[i] is the age of the ith person.

A Person x will not send a friend request to a person y (x != y) if any of the following conditions is true:

.. topic:: Conditions

    age[y] <= 0.5 * age[x] + 7

    age[y] > age[x]

    age[y] > 100 && age[x] < 100

Otherwise, x will send a friend request to y.

Note that if x sends a request to y, y will not necessarily send a request to x. Also, a person will not send a friend request to themself.

Return the total number of friend requests made.


.. topic:: Example 1

    Input: ages = [16,16]

    Output: 2

    Explanation: 2 people friend request each other.

.. topic:: Example 2

    Input: ages = [16,17,18]

    Output: 2

    Explanation: Friend requests are made 17 -> 16, 18 -> 17.

.. topic:: Example 3

    Input: ages = [20,30,100,110,120]

    Output: 3

    Explanation: Friend requests are made 110 -> 100, 120 -> 110, 120 -> 100.
 

.. topic:: Constraints

    n == ages.length

    1 <= n <= 2 * 104

    1 <= ages[i] <= 120

.. code-block:: java

    public int numFriendRequests(int[] ages) {
        int[] people = new int[121];
        
        for (int a : ages) {
            if (people[a] != 0) {
                people[a] = people[a]+1;
            } else {
                people[a] = 1;
            }
        }
        
        int counts = 0;
        
        for (int x : ages) {
            //System.out.println("x: " + x);
            int min = x/2 + 7 + 1;
            int max = x<100? Math.min(100, x):x;
            for (int y=min; y<=max; y++) {
                //System.out.println("y: " + y);
                counts += people[y];
                if (x==y) {
                    counts--;
                }
                //System.out.println("C: " + counts);
            }
        }
        
        return counts;
    }

------------------------
846. Hand of Straights
------------------------

Alice has some number of cards and she wants to rearrange the cards into groups so that each group is of size groupSize, and consists of groupSize consecutive cards.

Given an integer array hand where hand[i] is the value written on the ith card and an integer groupSize, return true if she can rearrange the cards, or false otherwise.
 

.. topic:: Example 1

    Input: hand = [1,2,3,6,2,3,4,7,8], groupSize = 3

    Output: true

    Explanation: Alice's hand can be rearranged as [1,2,3],[2,3,4],[6,7,8]

.. topic:: Example 2

    Input: hand = [1,2,3,4,5], groupSize = 4

    Output: false

    Explanation: Alice's hand can not be rearranged into groups of 4.

.. topic:: Constraints

    1 <= hand.length <= 104

    0 <= hand[i] <= 109

    1 <= groupSize <= hand.length

.. code-block:: java

    public boolean isNStraightHand(int[] hand, int groupSize) {
        
        if (hand.length%groupSize != 0) {
            return false;
        }
        TreeMap<Integer, Integer> counts = new TreeMap<>();
        
        for (int card : hand) {
            if (counts.containsKey(card)) {
                int k = counts.get(card) + 1;
                counts.put(card, k);
            } else {
                counts.put(card, 1);
            }
        }
        // print(counts);
        
    
        while (counts.keySet().size() > 0) {
            int i = counts.keySet().iterator().next();
            // System.out.println("i: "+i);
            // print(counts);
            for (int k=1; k<groupSize; k++) {
                if (!counts.containsKey(i+k)) {
                    return false;
                }
            }
        
            for (int k=0; k<groupSize; k++) {
                if (counts.get(i+k) == 1) {
                    counts.remove(i+k);
                } else {
                    counts.put(i+k, counts.get(i+k)-1);
                }
            }
        }
        
        return true;
    }
    
    private void print(TreeMap<Integer, Integer> counts) {
        for (int k : counts.keySet()) {
            System.out.println("Key: "+k+" value: "+counts.get(k));
        }
    }

--------------------------
359. Logger Rate Limiter
--------------------------

Design a logger system that receives a stream of messages along with their timestamps. Each unique message should only be printed at most every 10 seconds (i.e. a message printed at timestamp t will prevent other identical messages from being printed until timestamp t + 10).

All messages will come in chronological order. Several messages may arrive at the same timestamp.

Implement the Logger class:

Logger() Initializes the logger object.
bool shouldPrintMessage(int timestamp, string message) Returns true if the message should be printed in the given timestamp, otherwise returns false.
 
.. topic:: Example 1

    Input

    ["Logger", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage"]

    [[], [1, "foo"], [2, "bar"], [3, "foo"], [8, "bar"], [10, "foo"], [11, "foo"]]

    Output

    [null, true, true, false, false, false, true]

    Explanation

    Logger logger = new Logger();

    logger.shouldPrintMessage(1, "foo");  // return true, next allowed timestamp for "foo" is 1 + 10 = 11

    logger.shouldPrintMessage(2, "bar");  // return true, next allowed timestamp for "bar" is 2 + 10 = 12

    logger.shouldPrintMessage(3, "foo");  // 3 < 11, return false

    logger.shouldPrintMessage(8, "bar");  // 8 < 12, return false

    logger.shouldPrintMessage(10, "foo"); // 10 < 11, return false

    logger.shouldPrintMessage(11, "foo"); // 11 >= 11, return true, next allowed timestamp for "foo" is 11 + 10 = 21

.. topic:: Constraints

    0 <= timestamp <= 109

    Every timestamp will be passed in non-decreasing order (chronological order).

    1 <= message.length <= 30

    At most 104 calls will be made to shouldPrintMessage.

.. code-block:: java

    class Logger {
        
        HashMap<String, Integer> logs;

        public Logger() {
            this.logs = new HashMap<String, Integer>();
        }
        
        public boolean shouldPrintMessage(int timestamp, String message) {
            if (this.logs.containsKey(message) && timestamp < this.logs.get(message) + 10) {
                return false;
            } else {
                logs.put(message, timestamp);
                return true;
            }
        }
    }

----------------
146. LRU Cache
----------------

Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.

Implement the LRUCache class:

LRUCache(int capacity) Initialize the LRU cache with positive size capacity.

int get(int key) Return the value of the key if the key exists, otherwise return -1.

void put(int key, int value) Update the value of the key if the key exists. Otherwise, add the key-value pair to the 
cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.

The functions get and put must each run in O(1) average time complexity.

.. topic:: Example 1

    Input

    ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]

    [[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]

    Output

    [null, null, null, 1, null, -1, null, -1, 3, 4]

    Explanation

    LRUCache lRUCache = new LRUCache(2);

    lRUCache.put(1, 1); // cache is {1=1}

    lRUCache.put(2, 2); // cache is {1=1, 2=2}

    lRUCache.get(1);    // return 1

    lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}

    lRUCache.get(2);    // returns -1 (not found)

    lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}

    lRUCache.get(1);    // return -1 (not found)

    lRUCache.get(3);    // return 3

    lRUCache.get(4);    // return 4
 
.. topic:: Constraints

    1 <= capacity <= 3000

    0 <= key <= 104

    0 <= value <= 105

    At most 2 * 105 calls will be made to get and put.

**Approach** use HashMap and doubly-linked list 

.. code-block:: java

    class LRUCache {
        
        class Node {
            int key;
            int value;
            Node prev;
            Node next;
            
            public Node(int key, int value) {
                this.key = key;
                this.value = value;
            }
            
            public Node() {
                this(0, 0);
            }
        }
        
        int capacity;
        int count;
        Node head;
        Node tail;
        Map<Integer, Node> map;

        public LRUCache(int capacity) {
            this.capacity = capacity;
            this.count = 0;
            this.head = new Node();
            this.tail = new Node();
            head.next = tail;
            tail.prev = head;
            this.map = new HashMap<>();
        }
        
        public int get(int key) {
            if (map.containsKey(key)) {
                Node node = map.get(key);
                update(node);
                return node.value;
            } else {
                return -1;
            }
        }
        
        public void put(int key, int value) {
            if (map.containsKey(key)) {
                Node node = map.get(key);
                update(node);
                node.value = value;
            } else {
                Node node = new Node(key, value);
                add(node);
                map.put(key, node);
                count++;
            }
            if (count > capacity) {
                    Node toDelete = tail.prev;
                    remove(toDelete);
                    map.remove(toDelete.key);
                    count--;
                }
        }
        
        private void update(Node node) {
            remove(node);
            add(node);
        }
        
        private void add(Node node) {
            Node temp = head.next;
            head.next = node;
            node.prev = head;
            node.next = temp;
            temp.prev = node;
        }
        
        private void remove(Node node) {
            Node before = node.prev;
            Node after = node.next;
            before.next = after;
            after.prev = before;
        }
    }