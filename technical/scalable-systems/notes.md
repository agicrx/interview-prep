# Scalable Systems

Rough whiteboard sketch summary
**Ask GOOD questions**

## Steps
1. Understand requirements
-- What are you solving for?
-- Inputs? Outputs?
-- Boundary conditions
    * Business
    -- User stories
    * Technical
    -- SLA (Latency)
    -- Uptime
    -- Limits (Api / Entity)
    -- Throughput
    -- Auth

2. Understand constraints
-- Read / Write heavy?
-- Availability?
-- What to optimize for?
-- Try numbers, get a sense of the problem
    * Back of the envelope calculation
    * Complexity
      -- *Data Storage Growth*
      -- *requests/sec*
      -- *network/throughput*
    * Essentially: **Scale, Throughput, Latency**

1. Design
   - Usage patterns
   - Consistency Levels
   - Sync vs Async
   - Procedure:
     * Figure out service interface
       - What is the MVP? What do we need to do?
       - What are our responsibilities?
       - How do we interface with other teams e.g.
         * Product / Sales / BD
         * Infrastructure / Monitoring
         * Data Science
         * Security / Legal
   - Outline workflows: Call Sequence diagrams
     - This will help build our entity relationship diagram
   - By the end of this you should have a sketch of:
     - Service Interface / External API
     - Call Sequence diagrams
     - Entity Relationship Diagrams (DB Schema too if needed)

4. Scale
   - Caching
   - Single point of failure?
   - Master - slave
   - Async and Sync?
   - Where is it fine to be slow?
   - Product hat!
   - Talk about geography, partitioning, sharding, monitoring, configuration etc

5. Deliver
   - External API
   - Call Sequence Diagrams
   - Entity Relationship Diagram
   - Schema DB
   - Architecture Diagrams

 ============================

 ```java
 public class TreeTraversal {

     private static class Node {
         Node left;
         Node right;
         String val;
         Node(String val) {
             this.val = val;
         }
     }

     private static void preorder_Recursive(Node n) {
         if (n == null) {
             return;
         }

         System.out.print(n.val);
         preorder_Recursive(n.left);
         preorder_Recursive(n.right);
     }

     private static void inorder_Recursive(Node n) {
         if (n == null) {
             return;
         }

         inorder_Recursive(n.left);
         System.out.print(n.val);
         inorder_Recursive(n.right);
     }

     private static void postorder_Recursive(Node n) {
         if (n == null) {
             return;
         }

         postorder_Recursive(n.left);
         postorder_Recursive(n.right);
         System.out.print(n.val);
     }

     private static void preorder_Iterative(Node root) {
         if (root == null) {
             return;
         }

         Stack<Node> s = new Stack<>();
         s.push(root);
         while (!s.isEmpty()) {
             Node n = s.pop();
             System.out.print(n.val);
             if (n.right != null) {
                 s.push(n.right);
             }

             if (n.left != null) {
                 s.push(n.left);
             }
         }
     }

     private static void inorder_Iterative(Node root) {
         if (root == null) {
             return;
         }

         Stack<Node> s = new Stack<>();

         Node n = root;
         while (n != null) {
             s.push(n);
             n = n.left;
         }

         while (!s.isEmpty()) {
             n = s.pop();
             System.out.print(n.val);
             if (n.right != null) {
                 n = n.right;
                 while (n != null) {
                     s.push(n);
                     n = n.left;
                 }
             }
         }
     }

     private static void postorder_Iterative2Stacks(Node root) {
         if (root == null) {
             return;
         }

         Stack<Node> s = new Stack<>();
         Stack<Node> output = new Stack<>();

         s.push(root);
         while (!s.empty()) {
             Node n = s.pop();
             output.push(n);
             if (n.left != null) {
                 s.push(n.left);
             }
             if (n.right != null) {
                 s.push(n.right);
             }
         }

         while (!output.isEmpty()) {
             System.out.print(output.pop().val);
         }
     }

     private static void postorder_Iterative1Stack(Node root) {
         if (root == null) {
             return;
         }

         Stack<Node> s = new Stack<>();
         s.push(root);

         Node prev = null;
         while (!s.isEmpty()) {
             Node n = s.peek();
             if (prev == null ||
                 prev.left == n ||
                 prev.right == n) {
                 if (n.left != null) {
                     s.push(n.left);
                 } else if (n.right != null) {
                     s.push(n.right);
                 } else {
                     s.pop();
                     System.out.print(n.val);
                 }
             } else if (n.left == prev) {
                 if (n.right != null) {
                     s.push(n.right);
                 } else {
                     s.pop();
                     System.out.print(n.val);
                 }
             } else if (n.right == prev) {
                 s.pop();
                 System.out.print(n.val);
             }
             prev = n;
         }
     }

////////////////////////

// Tags: Airbnb, leetcode
// https://leetcode.com/problems/contains-duplicate/
// https://leetcode.com/problems/contains-duplicate-ii/
// https://leetcode.com/problems/contains-duplicate-iii/

/*
Given an array of integers, find if the array contains any duplicates. Your function should
return true if any value appears at least twice in the array, and it should return false if
every element is distinct.
 */
private static boolean containsDuplicate(int[] nums) {
    Set<Integer> set = new HashSet<>();
    for (int n : nums) {
        if (set.contains(n)) {
            return true;
        }
        set.add(n);
    }
    return false;
}

/*
Given an array of integers and an integer k, find out whether there are two distinct indices
i and j in the array such that nums[i] = nums[j] and the difference between i and j is at
most k.
 */
private static boolean containsNearbyDuplicate(int[] nums, int k) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        if (map.containsKey(nums[i]) && Math.abs(map.get(nums[i]) - i) <= k) {
            return true;
        }
        map.put(nums[i], i);
    }
    return false;
}

/*
Given an array of integers, find out whether there are two distinct indices i and j in the
array such that the difference between nums[i] and nums[j] is at most t and the difference
between i and j is at most k.
 */
private static boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
    if (k < 1 || t < 0) {
        return false;
    }

    // Value buckets
    Map<Long, Long> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        long num = (long) nums[i] - Integer.MIN_VALUE;
        long bucket = num / ((long) t + 1);
        if (map.containsKey(bucket) ||
            (map.containsKey(bucket - 1) && num - map.get(bucket - 1) <= t) ||
            (map.containsKey(bucket + 1) && map.get(bucket + 1) - num <= t)) {
            return true;
        }
        if (map.size() >= k) {
            long lastBucket = ((long) nums[i - k] - Integer.MIN_VALUE) / ((long) t + 1);
            map.remove(lastBucket);
        }
        map.put(bucket, num);
    }
    return false;
}

///////////////////////////////

private static List<String> generatePermutations(String str) {
    List<String> result = new ArrayList<>();
    generatePermutations(result, str.toCharArray(), 0);
    return result;
}

private static void generatePermutations(List<String> result, char[] chars, int i) {
    if (i == chars.length - 1) {
        result.add(new String(chars));
        return;
    }

    for (int j = i; j < chars.length; j++) {
        swap(chars, i, j);
        generatePermutations(result, chars, i + 1);
        swap(chars, i, j);
    }
}

private static void swap(char[] chars, int a, int b) {
    char c = chars[a];
    chars[a] = chars[b];
    chars[b] = c;
}

/////////////////////////////

private static int edit_distance(String a, String b) {

    int[][] dp = new int[a.length() + 1][b.length() + 1];

    // Init stuff
    for (int i = 0; i <= a.length(); i++) {
        dp[i][0] = i;
    }
    for (int j = 0; j <= b.length(); j++) {
        dp[0][j] = j;
    }

    // Calculate edit distance
    for (int i = 1; i <= a.length(); i++) {
        for (int j = 1; j <= b.length(); j++) {
            if (a.charAt(i - 1) == b.charAt(j - 1)) {
                dp[i][j] = dp[i-1][j-1];
            } else {
                dp[i][j] = Math.min(dp[i-1][j], Math.min(dp[i][j-1], dp[i-1][j-1])) + 1;
            }
        }
    }
    return dp[a.length()][b.length()];
}

////////////////////////////

private static int partition(int[] arr, int low, int high) {
    int pivot = arr[low];
    int left = low;
    int right = high;
    while (left < right) {
        while (arr[left] <= pivot && left < high) {
            left++;
        }
        while (arr[right] > pivot) {
            right--;
        }
        if (left < right) {
            swap(arr, left, right);
        }
    }
    swap(arr, low, right);
    return right;
}

private static void swap(int[] arr, int a, int b) {
    int tmp = arr[a];
    arr[a] = arr[b];
    arr[b] = tmp;
}

private static int quickSelect(int[] arr, int k) {
    return quickSelect(arr, 0, arr.length - 1, k);
}

private static int quickSelect(int[] arr, int low, int high, int k) {
    while (low < high) {
        int pivotIdx = choosePartition(arr, low, high);
        swap(arr, low, pivotIdx);
        pivotIdx = partition(arr, low, high);
        if (k < pivotIdx) {
            high = pivotIdx - 1;
        } else if (pivotIdx < k) {
            low = pivotIdx + 1;
        } else {
            return arr[pivotIdx];
        }
    }
    return arr[low];
}

 ```


 # Palantir

## Overview
* Data analytics company working with enterprise corporations
* Value prop: unlock data insights for new markets etc
* Tooling for human driven analysis of real-world data

## Notes
* Strong engineering culture
* Missing driven engineering

## Contacts
Alex Layman (Technical Recruiter)
Melanie Block (Technical Recruiter)

## Why palantir?
* Strong engineering culture - mission driven
  * I want to develop a skill set around breaking down client problems and solving them with technology
* Work sounds interesting
  * Scope of problems
* Challenging

## Things I want to find out
* What is the day to day like for palantirians?
* What does support look like?
* What is the FDE role like?
  * Types of projects?
* Ability to rotate?
* How true are staff departures, loss of corporate clients?

## Interview Experiences
See Palantir.java

## Previous Interview Questions
### Behavioural

### Technical
* [ ] parse connect 4 notation to a 6x7 board where the input string can be invalid
* [ ] implementation of an interpreter for a small language that does multiplication / addition
* [ ] import a csv and print out content
* [x] fraud detection
* [ ] check valid sudoku problem
* [x] rotate array by k place
* [ ] Given a list of strings, return a list of strings which each element in the result can uniquely identify the element in the input
* [ ] Find duplicate entry in an integer array.
  * [ ] Only if within some index range?
  * [ ] Not duplicate. Only if within some value range.
  * [x] Find duplicates within distance k
  * [x] Find fuzzy duplicates within distance k. Two integers are fuzzy duplicates if their difference is at most d.
* [x] Given an array as input, find running median at each position i.
  * [x] Without heaps? Constant space?
* [x] Sort an array from 1-n (unordered) in linear time
* [ ] Make all the words in a sentence capitalized
* [x] Tree traversal
  * [x] Pre order
    * [x] Recursive
    * [x] Iterative
  * [x] In order
    * [x] Recursive
    * [x] Iterative
  * [x] Post order
    * [x] Recursive
    * [x] Iterative
* [ ] Design a route table
* [ ] Flipping columns of a matrix to get maximum benefit
* [ ] 2 Glass bottles. Most efficient way to figure out which floor will break the glass.
* [x] Parentheses matching. Minimum number of edits to balance parentheses.
* [ ] Min stack
* [ ] Find top k numbers in max heap
* [ ] Find integers in an array
  * [ ] __variants__
* [ ] Flip a matrix
* [x] Convert a tree to a linked list
  * [x] Convert a sorted LL to a tree
* [ ] String compress function. RRGB => R2G1B1
  * [ ] __variants__
* [x] Find if path exists between two vertices in graph
* [x] Determine if cycle exists in graph
* [ ] Find duplicates between two arrays
* [x] 3 distinct algorithms to find the Kth largest values in a list of N items
* [ ] Arcane data structures
* [ ] Print binary tree using BFS.
  * [x] Print level by level
  * [ ] Print as tree https://stackoverflow.com/questions/4965335/how-to-print-binary-tree-diagram
* [ ] Two lists, each have an identical number in them except for one unique number. How do you find the unique number?
* [ ] TSP Problem derivative.
* [ ] Stack with fixed size
* [ ] Returns whether there are two duplicates within k indices of each other.
  * [ ] k indices and within plus or minus l of each other. O(n) running time, O(k) space
* [ ] External mergesort
* [ ] Farmer elevation problem
* [ ] Trie, Ternary Trie
* [ ] DFS Locking
* [ ] Returns numbers in an unordered list that can be represented as a sum of 3 other numbers in the list
* [ ] Function that finds all the different ways you can split up a word into a concatenation of two other words
* [ ] Given an unordered list of numbers ranging rom 1 to n, and 1 of the numbers is removed. How do you find that number? What if two numbers are removed?
* [ ] Given a string of integers of undefined length, how would you choose an element uniformly at random using only a constant amount of space? Prove its correct.
* [ ] Given a list of +ve and -ve ints, how would you determine if any 3 elements sum to 0? What about for k elements summing to 0?
* [ ] Write a function that determines the intersection of two arrays (indentical elements)
* [ ] Do a binary tree traversal with constant memory (no stacks)
* [ ] Given a list of ints, write an algorithm to return all pairs of values that sum to 0
* [ ] Model chess
* [ ] Model poker
* [ ] Stock value aggregate. Missing numbers for all days.
* [ ] Decomposition interview.
* [ ] Given a series of stock prices and trades made by traders, find the rogue trader based on some given conditions.
* [ ] LRU Cache
* [ ] How would you design Netflix
* [ ] Replace letters in a string
* [ ] Cutting stock problem
* [ ] Implement a hash map
* [ ] Find minimum element in a sorted rotated array in less than O(n)
* [ ] DP (revise)
* [ ] Write a function that is given an array of integers and an integer k. It should return true if and only if there are two distinct indices i and j into the array such that arr[i] = arr[j] and the difference between i and j is at most k. [1, 0, 2, 1] k = 2 -- false [1, 0, 2, 1] k = 3 -- true
* [ ] Test for 4 collinear points in dataset of coordinates
* [ ] Design a persistent stack
* [ ] Find the max difference in an array
* [ ] Implement jQuery
* [ ] DOM structure
* [ ] Return all anagrams of a word, given a dictionary
* [ ] Sum of n numbers is equal to a certain number?
* [ ] Find smallest 100 numbers in an array.
  * [ ] What if the array is too large to fit into memory
* [ ] Given the price of a stock in a period of time. Find the dates to buy and sell once for most profit.
* [ ] Use stack to implement a queue
* [ ] You are given a pyramid; the numbers for example is 2 on the first level, 3 -1 on the second level, 4 7 8 on the third, etc. How do you calculate the maximum sub sequence of any path traversing the pyramid?
* [ ] 2 sum, 3 sum
* [ ] What is synchronized. How to avoid deadlocks?
* [ ] How to get phone numbers from many files in directory
* [ ] Print every other level of a BST
* [ ] Determine if a LL is a palindrome
* [ ] Given a n*n matrix of 1's and 0's, figure out if all of the 1s are connected
* [ ] You have two arrays with numbers in them. You have to stop the program once you've found the largest difference between one number in the 1st array and one number in the 2nd array. You do not have to return the numbers, just stop once you've found it.
* [ ] Devise a representation of an NxN game board, with the variation you only need tokens M in a row to win (in any direction), s.t. a win can be verified in O(1) time.
* [ ] Efficiently delete a large number of duplicate files on a linux server.
* [ ] Given a set of events [e1, e2, e3, ... , eN] occuring at given times and a set of timeframes [t1, t2, .., tM] each having a start timestamp and an end timestamp, give an algorithm for each event the number of timeframes it occurs within
* [ ] Given a set of cabs navigating within a city, where would be the best place for all of them to meet?
* [ ] Code a math expression parser (subtraction, multiplication), any language, consider parentheses
* [ ] How would you store email adress for a web service?
* [ ] Given an integer, return its english word-format, e.g. -10,203 -> "negative ten thousand two hundred and three"
* [ ] Design make (Given a list of dependencies, determine the order to compile)
* [ ] Max sum subsequence of an array
* [ ] Sample a stream of integers with uniform distribution
* [ ] Suppose you have a rectangle embedded on the plane, and a disk with the points contained within the rectangle. If you randomly throw a fixed triangle into the rectangle, what is the probability all its endpoints are contained within the circle?
* [ ] 100 hat puzzle.
* [ ] Proof by induction why data structure has property
* [ ] Program a computer to shuffle a deck of cards. Generate random permutation.
* [ ] Verify that all words in the solution of a crossword puzzle are valid words. Puzzle is represented as a 2D array of single character strings
* [ ] What happens when you load a webpage.
* [ ] Rotate the letters of a word by k, using only 1 extra byte of storage. abcd -> deabc for k = 2
* [ ] Given an input string, print out the N most common characters in that string
* [ ] Find the running subset of an array that sums to the highest value
* [ ] Describe an algorithm for finding a duplicate character in a string
* [ ] Check that in a grid of 1s, all are connected
* [ ] Delete element from a singly linked-list
* [ ] Given a set of events and a set of time ranges. How can I find in how many time ranges a single event is in? Events are expressed as single points in time(single timestamp), time ranges are expressed as starting point and end point.
* [ ] 25 horses 3 fastest 5 races
* [ ] Iterating through a list of intervals without interval overlaps.
* [ ] Design event logging system
* [ ] I have 100 bottles of wine. 1 is poisoned. I have 10 rats. My guests are coming in an hour but the poison takes 45 minutes to kick in and "kill" a rat. How do I test for and find the 1 bottle of poisoned wine?
* [ ] Job scheduler
### System

## Scheduled Interviewers
N/A

## Things to prep for
Coding, Algorithms, Decomp, Intel

## What I want from this experience
Practice onsite in NYC
An offer

## Sources
glassdoor palantir

Last Updated at: 3/1/2017
