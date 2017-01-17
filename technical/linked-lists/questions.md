# Linked Lists, Stacks and Queues
<!-- TOC START min:1 max:3 link:true update:true -->
- [Linked Lists, Stacks and Queues](#linked-lists-stacks-and-queues)
    - [Delete Node in a Linked List](#delete-node-in-a-linked-list)
    - [Remove Linked List Elements](#remove-linked-list-elements)
    - [Reverse Linked List](#reverse-linked-list)
    - [Get a mid point of a Linked List](#get-a-mid-point-of-a-linked-list)
    - [Split a Linked List into two](#split-a-linked-list-into-two)
    - [Zip a linked list](#zip-a-linked-list)
    - [Sliding Window Maximum](#sliding-window-maximum)
    - [Clone a special list](#clone-a-special-list)
    - [Merge sort a linked list](#merge-sort-a-linked-list)
    - [Stack with increment](#stack-with-increment)
    - [Implement a LRU Cache](#implement-a-lru-cache)
    - [Alternative node split](#alternative-node-split)
    - [Middle value in linked list](#middle-value-in-linked-list)
    - [Check valid matching parans](#check-valid-matching-parans)
    - [Longest substring with matching parans](#longest-substring-with-matching-parans)
    - [Swap kth nodes in linked list](#swap-kth-nodes-in-linked-list)
    - [Reverse a linked list in groups](#reverse-a-linked-list-in-groups)
    - [Duplicates in a unsorted linked list](#duplicates-in-a-unsorted-linked-list)
    - [Implement min stack](#implement-min-stack)

<!-- TOC END -->
---

---



### Delete Node in a Linked List

#### Source
https://leetcode.com/problems/delete-node-in-a-linked-list/

#### Question
>Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.
Supposed the linked list is `1 -> 2 -> 3 -> 4` and you are given the third node with value
`3`, the linked list should become `1 -> 2 -> 4` after calling your function.

#### Idea
- Typically, to remove a node, we change the prev to point to n.next
- Alternatively, we can copy the next val over, and skip the next node

#### Solution
##### Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
private void deleteNode(ListNode node) {
    node.val = node.next.val;
    node.next = node.next.next;
}
```
##### Complexity
- Time: `O(1)`
- Space: `O(1)`

##### Things learnt
- Remember that removal can also be accomplished by *overwriting*

[Back to top](#linked-lists-stacks-and-queues)

---

---



### Remove Linked List Elements

#### Source
https://leetcode.com/problems/remove-linked-list-elements/

#### Question
>Remove all elements from a linked list of integers that have value val.
Example
Given: `1 --> 2 --> 6 --> 3 --> 4 --> 5 --> 6`, `val = 6`
Return: `1 --> 2 --> 3 --> 4 --> 5`

#### Idea
- We can't assume that the first node won't be removed
- We have two choices:
  - Use a temporary node (prehead)
  - Use recursion

#### Solution (Prehead)
##### Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
private ListNode removeElements(ListNode head, int val) {
    ListNode preHead = new ListNode(0);
    preHead.next = head;
    ListNode p = preHead;
    while (p.next != null) {
        if (p.next.val == val) {
            p.next = p.next.next;
        } else {
            p = p.next;
        }
    }
    return preHead.next;
}
```
##### Complexity
- Time: `O(n)`
- Space: `O(1)`

##### Things learnt
- Remember that you can always use an additional node

---

#### Solution (Recursion)
##### Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public ListNode removeElements(ListNode head, int val) {
    if (head == null) {
        return head;
    }
    ListNode tail = removeElements(head.next, val);
    if (head.val == val) {
        return tail;
    } else {
        head.next = tail;
        return head;
    }
}
```
##### Complexity
- Time: `O(n)`
- Space: `O(n)` due to implicit stack

[Back to top](#linked-lists-stacks-and-queues)

---

---



### Reverse Linked List

#### Source
https://leetcode.com/problems/reverse-linked-list/

#### Question
>Reverse a singly linked list

#### Idea
- We can do this iteratively or recursively. Iteratively uses less space,
  since we don't use an implicit stack

#### Solution (Iterative)
##### Approach
Assume that we have linked list `1 → 2 → 3 → Ø`, we would like to change it to `Ø ← 1 ← 2 ← 3`.
While you are traversing the list,
change the current node's next pointer to point to its previous element.
Since a node does not have reference to its previous node, you must store its
previous element beforehand. You also need another pointer to store the next
node before changing the reference. Do not forget to return the new head
reference at the end!

##### Code
```java
private ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}
```
##### Complexity
- Time: `O(n)`
- Space: `O(1)`

---

#### Solution (Recursion)
##### Approach
The recursive version is slightly trickier and the key is to work backwards. Assume that the rest of the list had already been reversed, now how do I reverse the front part? Let's assume the list is: `n1 → … → nk-1 → nk → nk+1 → … → nm → Ø`
Assume from node `nk+1` to `nm` had been reversed and you are at node nk.
`n1 → … → nk-1 → nk → nk+1 ← … ← nm`
We want `nk+1`’s next node to point to `nk`.
So,
`nk.next.next = nk;`
Be very careful that n1's next must point to Ø. If you forget about this, your linked list has a cycle in it. This bug could be caught if you test your code with a linked list of size 2.

##### Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
private ListNode reverseList(ListNode head) {
     if (head == null || head.next == null) {
         return head;
     }

     ListNode next = head.next;
     head.next = null;  // unlink from rest to prevent cycle
     ListNode newHead = reverseList(next);
     next.next = head; // join two links
     return newHead;
 }
```
##### Complexity
- Time: `O(n)`
- Space: `O(n)` due to implicit stack

[Back to top](#linked-lists-stacks-and-queues)

---

---

### Get a mid point of a Linked List

#### Source
Technique

#### Question
>Get the middle node in a singly linked list

#### Idea
- Use two pointers, one moving twice as fast as the other
- When fast pointer has reached the end, we got the mid point

#### Solution
##### Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
private ListNode findMidpoint(ListNode head) {
    ListNode mid = head;
    ListNode end = head;
    while (end != null && end.next != null) {
        mid = mid.next;
        end = end.next.next;
    }
    return mid;
}
```
##### Complexity
- Time: `O(n)`
- Space: `O(1)`

##### Things learnt
- Use multiple pointers when traversing singly linked list if you know proportions

[Back to top](#linked-lists-stacks-and-queues)

---

---

### Split a Linked List into two

#### Source
Technique

#### Question
>Split a linked list into two even halves

#### Idea
- Use two pointers, one moving twice as fast as the other
- When fast pointer has reached the end, we got the mid point
- Keep another pointer before mid to split

#### Solution
##### Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 * public class Result {
 *     ListNode first;
 *     ListNode second;
 * }
 */
private Result split(ListNode head) {
    ListNode mid = head;
    ListNode end = head;
    ListNode midPrev = null;
    while (end != null && end.next != null) {
        midPrev = mid;
        mid = mid.next;
        end = end.next.next;
    }
    if (midPrev != null) {
        midPrev.next = null;
    }
    return new Result(head, mid);
}
```
##### Complexity
- Time: `O(n)`
- Space: `O(1)`

##### Things learnt
- Use multiple pointers when traversing singly linked list
- Remember to keep previous if you need to split

[Back to top](#linked-lists-stacks-and-queues)

---

---

### Zip a linked list

#### Source
https://programming-puzzle.blogspot.com/2014/02/zip-of-linked-list.html

#### Question
![](/assets/scratch-be368.png)

#### Idea
- Split the list into two equal halves
- Reverse second list
- Merge both lists into one

#### Solution
##### Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
 static ListNode Zip(ListNode head) {
    if (head == null || head.next == null) {
         return head;
     }

     // Find mid point O(n)
     ListNode mid = pList;
     ListNode end = pList;
     ListNode midPrev = null;
     while (end != null && end.next != null) {
         midPrev = mid;
         mid = mid.next;
         end = end.next.next;
     }

     // Split into two lists
     if (midPrev != null) {
         midPrev.next = null;
     }

     // Reverse 2nd list O(n/2) => O(n)
     ListNode currNode = mid;
     ListNode prevNode = null;
     ListNode nextNode = null;
     while (currNode != null) {
         nextNode = currNode.next;
         currNode.next = prevNode;
         prevNode = currNode;
         currNode = nextNode;
     }

     // Merge our two lists, O(n)
     ListNode firstList = pList;
     ListNode secondList = prevNode;
     ListNode preHead = new ListNode(0);
     currNode = preHead;
     int index = 1;
     while (firstList != null && secondList != null) {
         if (index % 2 == 1) {
             currNode.next = firstList;
             currNode = firstList;
             firstList = firstList.next;
         } else {
             currNode.next = secondList;
             currNode = secondList;
             secondList = secondList.next;
         }
         index++;
     }

     if (firstList != null) {
         currNode.next = firstList;
     }
     if (secondList != null) {
         currNode.next = secondList;
     }
     return preHead.next;
 }
```
##### Complexity
- Time: `O(n)`
- Space: `O(1)`

##### Things learnt
- Reuse existing techniques

[Back to top](#linked-lists-stacks-and-queues)

---

---

### Sliding Window Maximum

#### Source
https://leetcode.com/problems/sliding-window-maximum/

#### Question
![](/assets/scratch-ce32c.png)

#### Idea
- Use a double ended queue to store indices of max items in window
- As you iterate through the input, check that elements in queue are in window
- Remove smallest elements from back in window on new input. We know we have a better option now

#### Solution
##### Code
```java
static long[] maxSlidingWindow(long[] input, int windowSize) {

    if (input == null || windowSize <= 0) {
        return new long[0];
    }

    long[] result = new long[input.length - windowSize + 1];
    int resultIndex = 0;

    // queue contains a queue of indexes for our sliding window
    Deque<Integer> queue = new LinkedList<>();

    for (int i = 0; i < input.length; i++) {

        long inputVal = input[i];
        int lowerWindowIndex = i - windowSize + 1;

        // Remove elements that are below our window
        while (!queue.isEmpty() && queue.peek() < lowerWindowIndex) {
            queue.poll();
        }

        // Remove smaller elements in our window
        while (!queue.isEmpty() && input[queue.peekLast()] < inputVal) {
            queue.pollLast();
        }

        // Add new index, update result
        queue.offer(i);
        if (i >= windowSize - 1) {
            result[resultIndex++] = input[queue.peek()];
        }
    }
    return result;
}
```
##### Complexity
- Time: `O(n)`
- Space: `O(n)` due to queue

##### Things learnt
- Think of queues for sliding windows (when you have to do time based eviction)
- Think about removing from back of deque too

[Back to top](#linked-lists-stacks-and-queues)

---

---

### Clone a special list

#### Source
http://www.geeksforgeeks.org/a-linked-list-with-next-and-arbit-pointer/

#### Question
![](/assets/scratch-a7be5.png)

#### Idea
- Create copy of nodes and insert into between original nodes
- Similarly, update random links
- Iterate copy list and split

#### Solution
##### Code
```java
/**
 * Definition for random-linked list.
 * private static class RandomListNode {
 *   int value;
 *   RandomListNode next, random;
 *   RandomListNode(int x) { this.value = x; }
 }
 */
private static RandomListNode copyRandomList(RandomListNode head) {
    if (head == null) {
       return null;
    }

    RandomListNode original = head;

    // Create copies of nodes and insert it after originals
    while (original != null) {
       RandomListNode copy = new RandomListNode(original.value);
       copy.next = original.next;
       original.next = copy;
       original = copy.next;
    }

    // Copy random links
    original = head;
    while (original != null) {
       original.next.random = original.random.next;
       original = original.next.next;
    }

    // Restore original and copy lists
    original = head;
    RandomListNode copy = original.next;
    RandomListNode copyHead = original.next;
    while (copy.next != null) {
       original.next = original.next.next;
       copy.next = copy.next.next;
       original = original.next;
       copy = copy.next;
    }
    original.next = null; // restore last original
    return copyHead;
}
```
##### Complexity
- Time: `O(n)`
- Space: `O(1)`

##### Things learnt
- Exploit structure of links

[Back to top](#linked-lists-stacks-and-queues)

---

---

### Merge sort a linked list

#### Source

#### Question
![](/assets/scratch-c2be2.png)

#### Idea
- Split list into two again, and recursively merge sort them

#### Solution
##### Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
static ListNode mergeSortList(ListNode head) {
    return helper(head);
}

static ListNode helper(ListNode head) {
    if (head == null || head.next == null) {
       return head;
    }

    // Find mid point O(n)
    ListNode mid = head;
    ListNode end = head;
    ListNode midPrev = null;
    while (end != null && end.next != null) {
       midPrev = mid;
       mid = mid.next;
       end = end.next.next;
    }

    // Split into two lists
    if (midPrev != null) {
       midPrev.next = null;
    }

    ListNode front = head;
    ListNode back = mid;
    return merge(front, back);
}

static ListNode merge(ListNode foo, ListNode bar) {
    foo = helper(foo);
    bar = helper(bar);

    // Merge our two lists, O(n)
    ListNode preHead = new ListNode(0);
    ListNode currNode = preHead;
    while (foo != null && bar != null) {
       if (foo.val <= bar.val) {
           currNode.next = foo;
           currNode = foo;
           foo = foo.next;
       } else {
           currNode.next = bar;
           currNode = bar;
           bar = bar.next;
       }
    }

    if (foo != null) {
       currNode.next = foo;
    }
    if (bar != null) {
       currNode.next = bar;
    }
    return preHead.next;
}
```
##### Complexity
- Time: `O(nlog(n))`
- Space: `O(1)`

##### Things learnt
- Use existing techniques
- Merge sort is preferred for Linked Lists
-- Insertion in the middle is `O(1)`, with `O(1)` space. We don't need extra space unlike arrays.
-- Merge sort accesses memory sequentially, we don't need random access (unlike quicksort)

[Back to top](#linked-lists-stacks-and-queues)

---

---

### Stack with increment

#### Source

#### Question
![](/assets/scratch-3033c.png)

#### Idea
- Maintain a second stack that will store increments

#### Solution
##### Code
```java
/**
 * Definition for Delta.
 * private static class Delta {
 *     int fromBottom;
 *     int increment;
 *
 *     Delta(int fromBottom, int increment) {
 *         this.fromBottom = fromBottom;
 *         this.increment = increment;
 *     }
 * }
 */
private static class SuperStack {

    // Stack contains the actual values
    private Stack<Integer> stack = new Stack<>();

    // IncrementStack contains deltas. This allows us to keep track of how much to add for each
    // pop
    private Stack<Delta> incrementStack = new Stack<>();


    // O(1)
    public void push(int a) {
        stack.push(a);
    }

    // O(1)
    public int peek() {
        int result = stack.peek();
        if (!incrementStack.isEmpty() && stack.size() <= incrementStack.peek().fromBottom) {
           result += incrementStack.peek().increment;
        }
        return result;
    }


    // O(1)
    public int pop() {
        int result = stack.pop();
        // If our stack size goes below the size recorded affected increments, decrease
        // index in affected increments
        if (!incrementStack.isEmpty() && stack.size() + 1 <= incrementStack.peek().fromBottom) {
           result += incrementStack.peek().increment;
           incrementStack.peek().fromBottom--;
           if (incrementStack.peek().fromBottom == 0) {
               incrementStack.pop();
           }
        }
        return result;
    }


    // O(number of increments)
    public void incr(int addition, int numElementFromBottom) {

        Stack<Delta> tmp = new Stack<>();

        // Remove deltas that aren't affected
        while (!incrementStack.isEmpty()
              && incrementStack.peek().fromBottom > numElementFromBottom) {
           tmp.push(incrementStack.pop());
        }

        // Add new delta
        if (!incrementStack.isEmpty()
           && incrementStack.peek().fromBottom == numElementFromBottom) {
           Delta d = incrementStack.pop();
           d.increment += addition;
           tmp.push(d);
        } else {
           tmp.push(new Delta(numElementFromBottom, addition));
        }

        // Increment affected deltas
        while (!incrementStack.isEmpty()) {
           Delta d = incrementStack.pop();
           d.increment += addition;
           tmp.push(d);
        }

        // Rebuild delta stack
        while (!tmp.isEmpty()) {
           incrementStack.push(tmp.pop());
        }
    }
}
```
##### Complexity
- Time: See above
- Space: See above

##### Things learnt
- Think about using auxiliary stacks if you need to store information

[Back to top](#linked-lists-stacks-and-queues)

---

---

### Implement a LRU Cache

#### Source

#### Question
![](/assets/scratch-becfc.png)

#### Idea
- Use hashmap for storing keys and values
- Use linked list for knowing what to evict

#### Solution
##### Code
```java
private static class Cache<K, V> {

    private static class CacheNode<K, V> {
        private K key;
        private V val;
        private CacheNode<K, V> newer;
        private CacheNode<K, V> older;

        CacheNode(K key, V val) {
            this.key = key;
            this.val = val;
        }
    }

    private final int capacity;
    private CacheNode<K, V> newest = null;
    private CacheNode<K, V> oldest = null;
    private Map<K, CacheNode<K, V>> cache = new HashMap<>();

    Cache(int capacity) {
        this.capacity = capacity;
    }

    void add(K key, V val) {

        if (cache.containsKey(key)) {
            bump(cache.get(key));
            return;
        }

        if (cache.size() == capacity) {
            evict();
        }

        CacheNode<K, V> newNode = new CacheNode<>(key, val);
        if (newest != null) {
            newest.newer = newNode;
            newNode.older = newest;
        }
        if (oldest == null) {
            oldest = newNode;
        }
        cache.put(key, newNode);
        newest = newNode;
    }

    V get(K key) {
        if (!cache.containsKey(key)) {
            return null;
        }

        CacheNode<K, V> node = cache.get(key);
        bump(node);
        return node.val;
    }

    private void bump(CacheNode<K, V> node) {
        if (node.newer != null) {
            node.newer.older = node.older;
        }
        if (node.older != null) {
            node.older.newer = node.newer;
        }
        if (oldest == node && node.newer != null) {
            oldest = node.newer;
        }
        node.newer = null;
        node.older = newest;
        newest = node;
    }

    private void evict() {
        CacheNode<K, V> n = oldest;
        if (oldest.newer != null) {
            oldest.newer.older = null;
        }
        oldest = oldest.newer;
        cache.remove(n.key);
    }
}
```
##### Complexity
- Time: `O(1)`
- Space: `O(n)`

##### Things learnt
- Use a combination of data structures

[Back to top](#linked-lists-stacks-and-queues)

---

---

### Alternative node split

#### Source

#### Question
![](/assets/scratch-edca7.png)

#### Idea
- Use prehead and split list

#### Solution
##### Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
private static void alternativeSplit(ListNode head) {

    ListNode preHead1 = new ListNode(0);
    ListNode preHead2 = new ListNode(0);

    ListNode currHead1 = preHead1;
    ListNode currHead2 = preHead2;

    int index = 1;
    while (head != null) {
        if (index % 2 == 1) {
            currHead1.next = head;
            currHead1 = head;
        } else {
            currHead2.next = head;
            currHead2 = head;
        }
        head = head.next;
        index++;
    }
    currHead1.next = null;
    currHead2.next = null;

    print(preHead1.next);
    print(preHead2.next);
    // Two lists are preHead1.next and preHead2.next
}
```
##### Complexity
- Time: `O(n)`
- Space: `O(1)`

##### Things learnt
- Remember to use a prehead if it simplifies things

[Back to top](#linked-lists-stacks-and-queues)

---

---

### Middle value in linked list

#### Source

#### Question
![](/assets/scratch-7ddc0.png)

#### Idea
- Use previous technique

#### Solution
##### Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
 static int findMiddleNode(ListNode head) {

     if (head == null) {
         return 0;
     }

     if (head.next == null) {
         return head.val;
     }

     // Find mid point O(n)
     ListNode mid = head;
     ListNode end = head;
     while (end != null && end.next != null) {
         mid = mid.next;
         end = end.next.next;
     }
     return mid.val;
 }
```
##### Complexity
- Time: `O(n)`
- Space: `O(1)`

##### Things learnt
- Nothing really

[Back to top](#linked-lists-stacks-and-queues)

---

---

### Check valid matching parans

#### Source

#### Question
![](/assets/scratch-f4454.png)

#### Idea
- Use a stack to match parans

#### Solution
##### Code
```java
static boolean hasMatchingParantheses(String strExpression) {

    if (strExpression.isEmpty()) {
        return false;
    }

    Stack<Character> stack = new Stack<>();
    for (int i = 0; i < strExpression.length(); i++) {
        Character c = strExpression.charAt(i);
        if (c.equals('{') || c.equals('(') || c.equals(']')) {
            stack.push(c);
        }
        if (c.equals('}')) {
            if (!stack.isEmpty() && stack.peek().equals('{')) {
                stack.pop();
                continue;
            }
            return false;
        }
        if (c.equals(')')) {
            if (!stack.isEmpty() && stack.peek().equals('(')) {
                stack.pop();
                continue;
            }
            return false;
        }
        if (c.equals(']')) {
            if (!stack.isEmpty() && stack.peek().equals('[')) {
                stack.pop();
                continue;
            }
            return false;
        }
    }
    return stack.isEmpty();
}
```
##### Complexity
- Time: `O(n)`
- Space: `O(n)`

##### Things learnt
- Use stack to store matching parans

[Back to top](#linked-lists-stacks-and-queues)

---

---

### Longest substring with matching parans

#### Source

#### Question
![](/assets/scratch-10bb8.png)

#### Idea
- We can use a stack to measure matching parans, but this time we need max length
- Instead, store the last *index* of a opening parans

#### Solution
##### Code
```java
private static int longestParans(String parans) {

    int maxLength = 0;
    int last = -1;
    Stack<Integer> s = new Stack<>(); // used to store last index of '(
    for (int i = 0; i < parans.length(); i++) {
        if (parans.charAt(i) == '(') {
            s.push(i);
        } else {
            if (s.isEmpty()) {
                last = i;
            } else {
                s.pop();
                if (s.isEmpty()) {
                    maxLength = Math.max(maxLength, i - last);
                } else {
                    maxLength = Math.max(maxLength, i - s.peek());
                }
            }
        }
    }
    return maxLength;
}
```
##### Complexity
- Time: `O(n)`
- Space: `O(n)`

##### Things learnt
- Stacks can also store indices for given condition

[Back to top](#linked-lists-stacks-and-queues)

---

---

### Swap kth nodes in linked list

#### Source

#### Question
![](/assets/scratch-316bc.png)

#### Idea
- Use existing techniques to find kth nodes from start and end
- Swap them, taking care if they are next to each other

#### Solution
##### Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
private static ListNode getKthFromEnd(ListNode root, int k) {
    ListNode end = root;
    while (end != null && k > 0) {
        end = end.next;
        k--;
    }

    if (end == null) {
        return null; // LL is too short
    }

    ListNode n = root;
    while (end != null) {
        end = end.next;
        n = n.next;
    }
    return n;
}

private static ListNode getKthFromStart(ListNode root, int k) {
    ListNode n = root;
    while (n != null && k > 1) { // using 1 based indexing
        n = n.next;
        k--;
    }
    return n;
}

static ListNode swapKthNodes(ListNode root, int k) {

    ListNode tmpHead = new ListNode(0);
    tmpHead.next = root;

    ListNode prevStart = getKthFromStart(tmpHead, k);
    ListNode prevEnd = getKthFromEnd(tmpHead, k + 1);

    if (prevStart == null || prevEnd == null) {
        return null;
    }

    ListNode start = prevStart.next;
    ListNode end = prevEnd.next;

    if (start == prevEnd) {
        start.next = start.next.next;
        prevStart.next = end;
        end.next = start;
        return tmpHead.next;
    } else if (end == prevStart) {
        end.next = end.next.next;
        prevEnd.next = start;
        start.next = end;
        return tmpHead.next;
    }

    ListNode startNext = start.next;
    start.next = end.next;
    end.next = startNext;

    prevStart.next = end;
    prevEnd.next = start;

    return tmpHead.next;
}
```
##### Complexity
- Time: `O(n)`
- Space: `O(1)`

##### Things learnt
- Combine existing techniques

[Back to top](#linked-lists-stacks-and-queues)

---

---

### Reverse a linked list in groups

#### Source
http://www.geeksforgeeks.org/reverse-a-list-in-groups-of-given-size/

#### Question
![](/assets/scratch-07643.png)

#### Idea
- Reverse the first sub-list of size k.
- While reversing keep track of the next node and previous node.
- Let the pointer to the next node be next and pointer to the previous node be prev.
- recursively call for the remainder of the list and link them both up
- return prev as part of recursive call

#### Solution
##### Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
 static ListNode reverseKNodes(ListNode head, int k) {
    int count = k;
    ListNode curr = head;
    ListNode next = null;
    ListNode prev = null;
    while (curr != null && count > 0) {
       next = curr.next;
       curr.next = prev;
       prev = curr;
       curr = next;
       count--;
    }

    if (next != null) {
       head.next = reverseKNodes(next, k);
    }
    return prev;
 }
```
##### Complexity
- Time: `O(n)`
- Space: `O(1)`

##### Things learnt
- Use return values from recursive calls to link up linked lists

[Back to top](#linked-lists-stacks-and-queues)

---

---

### Duplicates in a unsorted linked list

#### Source
http://algorithms.tutorialhorizon.com/remove-duplicates-from-an-unsorted-linked-list/

#### Question
![](/assets/scratch-82a9b.png)

#### Idea
- Use hash table to dedup (without it, `O(n^2)`)
- Remove nodes if already present

#### Solution
##### Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
 static ListNode removeDuplicates(ListNode head) {
    Set<Integer> vals = new HashSet<>();

    if (head == null) {
       return null;
    }

    ListNode node = head;
    while (node.next != null) {
       vals.add(node.val);
       while (node.next != null && vals.contains(node.next.val)) {
           node.next = node.next.next;
       }
       node = node.next;

       if (node == null) {
           break;
       }
    }
    return head;
 }
```
##### Complexity
- Time: `O(n)`
- Space: `O(n)`

##### Things learnt
- Remove removal in singly linked list requires two pointers

[Back to top](#linked-lists-stacks-and-queues)

---

---


### Implement min stack

#### Source
http://articles.leetcode.com/stack-that-supports-push-pop-and-getmin/
http://www.geeksforgeeks.org/design-and-implement-special-stack-data-structure/
http://www.geeksforgeeks.org/design-a-stack-that-supports-getmin-in-o1-time-and-o1-extra-space/

#### Question
![](/assets/scratch-8be0f.png)

#### Idea
- Use another stack to keep track of mins

#### Solution
##### Code
```java
private static class MininumStack {
    Stack<Integer> stack = new Stack<>();
    Stack<Integer> minStack = new Stack<>();

    int getMinimum() {
        if (minStack.isEmpty()) {
            throw new IllegalStateException("min stack is empty");
        }
        return minStack.peek();
    }

    void push(int foo) {
        stack.push(foo);
        if (minStack.isEmpty()) {
            minStack.push(foo);
        } else if (foo < minStack.peek()) {
            minStack.push(foo);
        }
    }

    int pop() {
        if (stack.isEmpty()) {
            throw new IllegalStateException("stack empty");
        }

        if (stack.peek().equals(minStack.peek())) {
            minStack.pop();
        }
        return stack.pop();
    }
}
```
##### Complexity
- Time: See above
- Space: See above

##### Things learnt
- Use auxiliary stacks

[Back to top](#linked-lists-stacks-and-queues)

---

---
