=====================
Linked List
=====================

.. contents::
    :depth: 2

----------------------------
Useful Method
----------------------------

^^^^^^^^^^^^^^^^
Java LinkedList
^^^^^^^^^^^^^^^^

.. code-block:: java

  // add() add an element to the end of the list
  queue.add("1");
  queue.add("2");
  queue.add("3");
  // result: [1, 2, 3]

  // addFirst() add an element to the first of the list
  queue.addFirst("1");
  queue.addFirst("2");
  queue.addFirst("3");
  // result: [3, 2, 1]

  // pop() or poll() remove and return the first element
  queue.pop();
  // result: [2, 1]

  // pollLast() remove and return the last element
  queue.pollLast();

  // peek() look at the first element
  queue.peek(); // == 3


----------------------------
Overview
----------------------------

Definition

.. code-block:: java

    public class ListNode {
        // 结点的值
        int val;

        // 下一个结点
        ListNode next;

        // 节点的构造函数(无参)
        public ListNode() {
        }

        // 节点的构造函数(有一个参数)
        public ListNode(int val) {
            this.val = val;
        }

        // 节点的构造函数(有两个参数)
        public ListNode(int val, ListNode next) {
            this.val = val;
            this.next = next;
        }
    }

Printing

.. code-block:: java

    public void print() {
        ListNode curr = head;
        while (curr != null) {
            System.out.print(" " + curr.val);
            curr = curr.next;
        }
        System.out.println("");
    }

----------------------------------
203. Remove Linked List Elements
----------------------------------

Given the head of a linked list and an integer val, remove all the nodes of the linked list that has Node.val == val, and return the new head.

.. topic:: Example 1

    Input: head = [1,2,6,3,4,5,6], val = 6

    Output: [1,2,3,4,5]

.. topic:: Example 2

    Input: head = [], val = 1

    Output: []

.. topic:: Example 3

    Input: head = [7,7,7,7], val = 7

    Output: []

.. topic:: Constraints

    The number of nodes in the list is in the range [0, 104].

    1 <= Node.val <= 50

    0 <= val <= 50

.. code-block:: java

    // Directly delete

    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return head;
        }

        while (head.val == val) {
            head = head.next;
            if (head == null) {
                return head;
            }
        }

        ListNode prev = head;
        ListNode curr = head.next;
        while (curr != null) {
            if (curr.val == val) {
                prev.next = curr.next;
            } else {
                prev = curr;
            }
            curr = curr.next;
        }

        return head;
    }

.. code-block:: java

    // Delete with a dummy first node

    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return head;
        }
        ListNode first = new ListNode(-1, head);
        ListNode curr = head;
        ListNode prev = first;
        while (curr != null) {
            if (curr.val == val) {
                prev.next = curr.next;
            } else {
                prev = curr;
            }
            curr = curr.next;
        }

        return first.next;
    }

------------------------------------------------
707. Design Linked List
------------------------------------------------

Design your implementation of the linked list. You can choose to use a singly or doubly linked list.
A node in a singly linked list should have two attributes: val and next. val is the value of the current node, and next is a pointer/reference to the next node.
If you want to use the doubly linked list, you will need one more attribute prev to indicate the previous node in the linked list. Assume all nodes in the linked list are 0-indexed.

Implement the MyLinkedList class:

MyLinkedList() Initializes the MyLinkedList object.
int get(int index) Get the value of the indexth node in the linked list. If the index is invalid, return -1.
void addAtHead(int val) Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
void addAtTail(int val) Append a node of value val as the last element of the linked list.
void addAtIndex(int index, int val) Add a node of value val before the indexth node in the linked list. If index equals the length of the linked list, the node will be appended to the end of the linked list. If index is greater than the length, the node will not be inserted.
void deleteAtIndex(int index) Delete the indexth node in the linked list, if the index is valid.

.. topic:: Example 1

    Input

    ["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", "deleteAtIndex", "get"]

    [[], [1], [3], [1, 2], [1], [1], [1]]

    Output

    [null, null, null, null, 2, null, 3]

    Explanation

    MyLinkedList myLinkedList = new MyLinkedList();

    myLinkedList.addAtHead(1);

    myLinkedList.addAtTail(3);

    myLinkedList.addAtIndex(1, 2);    // linked list becomes 1->2->3

    myLinkedList.get(1);              // return 2

    myLinkedList.deleteAtIndex(1);    // now the linked list is 1->3

    myLinkedList.get(1);              // return 3

.. topic:: Constraints

    0 <= index, val <= 1000

    Please do not use the built-in LinkedList library.

    At most 2000 calls will be made to get, addAtHead, addAtTail, addAtIndex and deleteAtIndex.

.. code-block:: java

    /**
     * Your MyLinkedList object will be instantiated and called as such:
     * MyLinkedList obj = new MyLinkedList();
     * int param_1 = obj.get(index);
     * obj.addAtHead(val);
     * obj.addAtTail(val);
     * obj.addAtIndex(index,val);
     * obj.deleteAtIndex(index);
     */


**Approach 1: singly linked list**

.. code-block:: java

    class ListNode {
        int val;
        ListNode next;

        public ListNode(int val) {
            this.val = val;
        }
    }

    class MyLinkedList {
        int size;
        ListNode head;

        public MyLinkedList() {
            this.size = 0;
            this.head = new ListNode(-1);
        }

        public int get(int index) {
            if (index >= size || index < 0) {
                return -1;
            }

            ListNode curr = head;
            for (int i=0; i<=index; i++) {
                curr = curr.next;
            }

            return curr.val;
        }

        public void addAtHead(int val) {
            addAtIndex(0, val);
        }

        public void addAtTail(int val) {
            addAtIndex(this.size, val);
        }

        public void addAtIndex(int index, int val) {
            if (index > size || index < 0) {
                return;
            }

            ListNode prev = head;
            ListNode newNode = new ListNode(val);

            for (int i=0; i<index; i++) {
                prev = prev.next;
            }

            newNode.next = prev.next;
            prev.next = newNode;
            size++;
        }

        public void deleteAtIndex(int index) {
            if (index >= size || index < 0) {
                return;
            }

            ListNode prev = head;

            for (int i=0; i<index; i++) {
                prev = prev.next;
            }

            size--;
            prev.next = prev.next.next;
        }
    }

**Approach 2: doubly linked list**

.. code-block:: java

    class ListNode {
        int val;
        ListNode next;
        ListNode prev;

        public ListNode(int val) {
            this.val = val;
        }
    }

    class MyLinkedList {
        int size;
        ListNode head;
        ListNode tail;

        public MyLinkedList() {
            this.size = 0;
            this.head = new ListNode(-1);
            this.tail = new ListNode(-1);
            this.head.next = this.tail;
            this.tail.prev = this.head;
        }

        public int get(int index) {
            if (index < 0 || index >= this.size) {
                return -1;
            }

            if (index < size/2) {
                ListNode curr = head;
                for (int i=0; i<=index; i++) {
                    curr = curr.next;
                }
                return curr.val;
            } else {
                ListNode curr = tail;
                for (int i=size-1; i>=index; i--) {
                    curr = curr.prev;
                }
                return curr.val;
            }

        }

        public void addAtHead(int val) {
            addAtIndex(0, val);
        }

        public void addAtTail(int val) {
            addAtIndex(this.size, val);
        }

        public void addAtIndex(int index, int val) {
            if (index < 0 || index > this.size) {
                return;
            }

            ListNode curr = head;
            for (int i=0; i<index; i++) {
                curr = curr.next;
            }

            ListNode newNode = new ListNode(val);
            newNode.next = curr.next;
            if (newNode.next != null) {
                newNode.next.prev = newNode;
            }

            curr.next = newNode;
            newNode.prev = curr;

            size++;

            //print();
        }

        public void deleteAtIndex(int index) {
            if (index < 0 || index >= this.size) {
                return;
            }

            ListNode curr = head;
            for (int i=0; i<index; i++) {
                curr = curr.next;
            }

            curr.next = curr.next.next;
            if (curr.next != null) {
                curr.next.prev = curr;
            }

            size--;
            //print();
        }


    }

------------------------------------------------
206. Reverse Linked List
------------------------------------------------

Given the head of a singly linked list, reverse the list, and return the reversed list.

.. topic:: Example 1

    Input: head = [1,2,3,4,5]

    Output: [5,4,3,2,1]

.. topic:: Example 2

    Input: head = [1,2]

    Output: [2,1]

.. topic:: Example 3

    Input: head = []

    Output: []

.. topic:: Constraints

    The number of nodes in the list is the range [0, 5000].

    -5000 <= Node.val <= 5000

.. topic:: Follow up

    A linked list can be reversed either iteratively or recursively. Could you implement both?

.. code-block:: java

    // Iterative
    class Solution {
        public ListNode reverseList(ListNode head) {
            ListNode prev = null;
            ListNode curr = head;
            ListNode temp;
            while (curr != null) {
                temp = curr.next;
                curr.next = prev;
                prev = curr;
                curr = temp;
            }
            return prev;
        }
    }

    // Recursive
    class Solution {
        public ListNode reverseList(ListNode head) {
            return _reverse(null, head);
        }

        public ListNode _reverse(ListNode prev, ListNode curr) {
            if (curr == null) {
                return prev;
            }
            ListNode temp = curr.next;
            curr.next = prev;
            return _reverse(curr, temp);
        }
    }

------------------------------------------------
24. Swap Nodes in Pairs
------------------------------------------------

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

.. topic:: Example 1

    Input: head = [1,2,3,4]

    Output: [2,1,4,3]

.. topic:: Example 2

    Input: head = []

    Output: []

.. topic:: Example 3

    Input: head = [1]

    Output: [1]

.. topic:: Constraints

    The number of nodes in the list is in the range [0, 100].

    0 <= Node.val <= 100

.. code-block:: java

    public ListNode swapPairs(ListNode head) {
        if (head == null) {
            return head;
        }

        ListNode first = new ListNode(-1, head);

        ListNode prevprev = first;
        ListNode prev = head;
        ListNode curr = head.next;
        ListNode temp;

        while (curr != null) {
            temp = curr.next;

            prevprev.next = curr;
            prev.next = curr.next;
            curr.next = prev;

            if (temp == null) {
                break;
            }

            prevprev = prev;
            curr = temp.next;
            prev = temp;
        }
        return first.next;
    }

------------------------------------------------
160. Intersection of Two Linked Lists
------------------------------------------------

Given the heads of two singly linked-lists headA and headB, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null.

For example, the following two linked lists begin to intersect at node c1:


The test cases are generated such that there are no cycles anywhere in the entire linked structure.

Note that the linked lists must retain their original structure after the function returns.

.. code-block:: java

    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        HashSet<ListNode> visited = new HashSet<>();

        ListNode currA = headA;
        ListNode currB = headB;

        while (currA != null || currB != null) {
            if (currA != null) {
                if (visited.contains(currA)) {
                    return currA;
                }

                visited.add(currA);
                currA = currA.next;
            }

            if (currB != null) {
                if (visited.contains(currB)) {
                    return currB;
                }

                visited.add(currB);
                currB = currB.next;
            }
        }

        return null;
    }

------------------------------------------------
19. Remove Nth Node From End of List
------------------------------------------------

Given the head of a linked list, remove the nth node from the end of the list and return its head.

.. topic:: Example 1

    Input: head = [1,2,3,4,5], n = 2

    Output: [1,2,3,5]

.. topic:: Example 2

    Input: head = [1], n = 1

    Output: []

.. topic:: Example 3

    Input: head = [1,2], n = 1

    Output: [1]

.. topic:: Constraints

    The number of nodes in the list is sz.

    1 <= sz <= 30

    0 <= Node.val <= 100

    1 <= n <= sz

.. topic:: Follow up

    Could you do this in one pass?

**Approach 1**: calculate the total length of the list, then find the previous node of the nth node from the end and do the deletion.

.. code-block:: java


    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head == null) {
            return null;
        }

        int size = 0;

        ListNode curr = head;
        while (curr != null) {
            curr = curr.next;
            size++;
        }

        ListNode first = new ListNode(-1, head);
        curr = first;
        for (int i=0; i<size-n; i++) {
            curr = curr.next;
        }
        curr.next = curr.next.next;

        return first.next;
    }

**Approach 2**: Use a fast pointer and a slow pointer. Fast moves n+1 steps first. Then both pointers move until fast is null. Then slow is exactly the previous node of the nth node from the end.

.. code-block:: java

    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head == null) {
            return null;
        }

        ListNode first = new ListNode(-1, head);
        ListNode fast = first;
        ListNode slow = first;

        for (int i=0; i<=n; i++) {
            fast = fast.next;
        }

        while (fast != null) {
            slow = slow.next;
            fast = fast.next;
        }

        slow.next = slow.next.next;

        return first.next;
    }

------------------------------------------------
2390. Removing Stars From a String (Medium)
------------------------------------------------

You are given a string s, which contains stars \*.

In one operation, you can:

    Choose a star in s.
    Remove the closest non-star character to its left, as well as remove the star itself.

Return the string after all stars have been removed.

Note:

    The input will be generated such that the operation is always possible.
    It can be shown that the resulting string will always be unique.

.. topic:: Example 1

  Input: s = "leet\*\*cod\*e"
  Output: "lecoe"
  Explanation: Performing the removals from left to right:
  - The closest character to the 1st star is 't' in "leet\*\*cod\*e". s becomes "lee\*cod\*e".
  - The closest character to the 2nd star is 'e' in "lee\*cod\*e". s becomes "lecod\*e".
  - The closest character to the 3rd star is 'd' in "lecod\*e". s becomes "lecoe".
  There are no more stars, so we return "lecoe".

.. topic:: Example 2

  Input: s = "erase\*\*\*\*\*"
  Output: ""
  Explanation: The entire string is removed, so we return an empty string.

.. topic:: Constraints

    1 <= s.length <= 105
    s consists of lowercase English letters and stars \*.
    The operation above can be performed on s.

.. code-block:: java

  class Solution {
    public String removeStars(String s) {
      LinkedList<String> queue = new LinkedList<>();

      for (int i = 0; i < s.length(); i++) {
          if (s.charAt(i) == '*') {
              if (queue.peek() != "*") {
                  queue.pop();
              }
          } else {
              queue.addFirst(String.valueOf(s.charAt(i)));
          }
      }

      String rst = "";
      while (!queue.isEmpty()) {
          rst = queue.pop() + rst;
      }

      return rst;
    }
  }
