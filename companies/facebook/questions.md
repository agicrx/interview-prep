```java
// Tags: facebook, leetcode
// https://leetcode.com/problems/search-in-rotated-sorted-numsay/

/*
Suppose an numsay sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

You are given a target value to search. If found in the numsay return its index, otherwise return -1.

You may assume no duplicate exists in the numsay.
 */

private static int search(int[] nums, int target) {
    int low = 0;
    int high = nums.length - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        int midVal = nums[mid];

        if (midVal == target) {
            return mid;
        }

        if (nums[low] <= midVal) {
            if (target < midVal && target >= nums[low]) {
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }

        if (midVal <= nums[high]) {
            if (target > midVal && target <= nums[high]) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
    }
    return -1;
}
```

```java
// Tags: facebook, leetcode
// https://leetcode.com/problems/increasing-triplet-subsequence/

/*
Given an unsorted array return whether an increasing subsequence of length 3 exists or not in
 the array.

Formally the function should:
Return true if there exists i, j, k
such that arr[i] < arr[j] < arr[k] given 0 ≤ i < j < k ≤ n-1 else return false.
Your algorithm should run in O(n) time complexity and O(1) space complexity.

Examples:
Given [1, 2, 3, 4, 5],
return true.

Given [5, 4, 3, 2, 1],
return false.
 */

private static boolean increasingTriplet(int[] nums) {
    int max = Integer.MAX_VALUE;
    int min = Integer.MAX_VALUE;
    for (int n : nums) {
        if (n <= min) {
            min = n;
        } else if (n <= max) {
            max = n;
        } else {
            return true;
        }
    }
    return false;
}
```

```java
// Tags: Facebook, leetcode
// https://leetcode.com/problems/move-zeroes/

/*
Given an array nums, write a function to move all 0's to the end of it while maintaining the
relative order of the non-zero elements.

For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3,
12, 0, 0].

Note:
You must do this in-place without making a copy of the array.
Minimize the total number of operations.
Credits:
Special thanks to @jianchao.li.fighter for adding this problem and creating all test cases.
 */

private static void moveZeros_OptimizedWrites(int[] nums) {
    int insertPos = 0;
    while (insertPos < nums.length && nums[insertPos] != 0) {
        insertPos++;
    }

    int i = insertPos;
    boolean written = false;
    while (i < nums.length) {
        while (i < nums.length && nums[i] == 0) {
            i++;
        }

        if (i < nums.length) {
            written = true;
            nums[insertPos] = nums[i];
            insertPos++;
            i++;
        }
    }

    while (written && insertPos < nums.length) {
        nums[insertPos] = 0;
        insertPos++;
    }
}

private static void moveZeros(int[] arr) {
    int back = 0;
    int front = 0;
    while (front < arr.length) {
        while (front < arr.length && arr[front] == 0) {
            front++;
        }

        if (front < arr.length) {
            arr[back] = arr[front];
            front++;
            back++;
        }
    }

    while (back < arr.length) {
        arr[back] = 0;
        back++;
    }
}

private static void moveZeros_Swap(int[] arr) {
    int back = 0;
    int front = 0;
    while (front < arr.length) {
        while (front < arr.length && arr[front] == 0) {
            front++;
        }

        if (front < arr.length) {
            swap(arr, front, back);
            front++;
            back++;
        }
    }
}

private static void swap(int[] arr, int a, int b) {
    int tmp = arr[a];
    arr[a] = arr[b];
    arr[b] = tmp;
}
```

```java
/*
Given an array nums and a target value k, find the maximum length of a subarray that sums to
k. If there isn't one, return 0 instead.

Note:
The sum of the entire nums array is guaranteed to fit within the 32-bit signed integer range.

Example 1:
Given nums = [1, -1, 5, -2, 3], k = 3,
return 4. (because the subarray [1, -1, 5, -2] sums to 3 and is the longest)

Example 2:
Given nums = [-2, -1, 2, 1], k = 1,
return 2. (because the subarray [-1, 2] sums to 1 and is the longest)

Follow Up:
Can you do it in O(n) time?
 */
 public int maxSubArrayLen(int[] nums, int k) {
    Map<Integer, Integer> sumToIdx = new HashMap<>();
    sumToIdx.put(0, 0);
    int longest = 0;
    int sum = 0;
    for (int i = 0; i < nums.length; i++) {
        sum += nums[i];
        if (!sumToIdx.containsKey(sum)) {
            sumToIdx.put(sum, i + 1);
        }

        int candidate = sum - k;
        if (sumToIdx.containsKey(candidate)) {
            longest = Math.max(longest, i - sumToIdx.get(candidate) + 1);
        }
    }
    return longest;
}
```

```java
/*
Remove the minimum number of invalid parentheses in order to make the input string valid.
Return all possible results.

Note: The input string may contain letters other than the parentheses ( and ).

Examples:
"()())()" -> ["()()()", "(())()"]
"(a)())()" -> ["(a)()()", "(a())()"]
")(" -> [""]

 */
 private static List<String> removeInvalidParentheses(String s) {
     List<String> result = new ArrayList<>();
     remove(result, s, 0, 0, '(', ')');
     return result;
 }

 private static void remove(List<String> result, String s, int lastI, int lastJ, char opening,
                     char closing) {
     int count = 0;
     for (int i = lastI; i < s.length(); i++) {
         if (s.charAt(i) == opening) {
             count++;
         }
         if (s.charAt(i) == closing) {
             count--;
         }

         if (count < 0) {
             for (int j = lastJ; j <= i; j++) {
                 if (s.charAt(j) == closing &&
                     (j == lastJ || s.charAt(j - 1) != closing)) {
                     remove(result,
                            s.substring(0, j) + s.substring(j + 1, s.length()),
                            i, j, opening, closing);
                 }
             }
             return;
         }
     }

     String reversed = new StringBuilder(s).reverse().toString();
     if (opening == '(') {
         remove(result, reversed, 0, 0, closing, opening);
     } else {
         result.add(reversed);
     }
 }

 ///////////////////////////////////////////////////////

 private static List<String> removeInvalidParentheses(String s) {
    Set<String> visited = new HashSet<>();
    Deque<String> q = new LinkedList<>();
    q.add(s);
    visited.add(s);

    while (!q.isEmpty()) {
        Deque<String> newQ = new LinkedList<>();
        List<String> result =  new LinkedList<>();
        while (!q.isEmpty()) {
            String str = q.removeFirst();
            if (isValid(str)) {
                result.add(str);
            } else {
                List<String> combinations = generatePossibilities(str);
                for (String comb : combinations) {
                    if (!visited.contains(comb)) {
                        newQ.add(comb);
                        visited.add(comb);
                    }
                }
            }
        }

        if (!result.isEmpty()) {
            return result;
        }
        q = newQ;
    }
    return new LinkedList<>();
}

    private static List<String> generatePossibilities(String s) {
        List<String> result = new ArrayList<>();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(' || s.charAt(i) == ')') {
                result.add(s.substring(0, i) + s.substring(i + 1));
            }
        }
        return result;
    }


    private static boolean isValid(String s) {
        int count = 0;
        for (char c : s.toCharArray()) {
            if (c == '(') {
                count++;
            } else if (c == ')') {
                if (count == 0) {
                    return false;
                }
                count--;
            }
        }
        return count == 0;
    }

```

```java
// Tags: Facebook, leetcode
 // https://leetcode.com/problems/sparse-matrix-multiplication/

 /*
 Given two sparse matrices A and B, return the result of AB.

You may assume that A's column number is equal to B's row number.

Example:

A = [
[ 1, 0, 0],
[-1, 0, 3]
]

B = [
[ 7, 0, 0 ],
[ 0, 0, 0 ],
[ 0, 0, 1 ]
]


  |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
               | 0 0 1 |
  */
  public class Solution {
      public int[][] multiply(int[][] A, int[][] B) {
          int m = A.length, n = A[0].length, nB = B[0].length;
          int[][] C = new int[m][nB];

          for(int i = 0; i < m; i++) {
              for(int k = 0; k < n; k++) {
                  if (A[i][k] != 0) {
                      for (int j = 0; j < nB; j++) {
                          if (B[k][j] != 0) C[i][j] += A[i][k] * B[k][j];
                      }
                  }
              }
          }
          return C;   
      }
  }

  public class Solution2 {
    public int[][] multiply(int[][] A, int[][] B) {
    int m = A.length, n = A[0].length, nB = B[0].length;
    int[][] result = new int[m][nB];

    List[] indexA = new List[m];
    for(int i = 0; i < m; i++) {
        List<Integer> numsA = new ArrayList<>();
        for(int j = 0; j < n; j++) {
            if(A[i][j] != 0){
                numsA.add(j);
                numsA.add(A[i][j]);
            }
        }
        indexA[i] = numsA;
    }

    for(int i = 0; i < m; i++) {
        List<Integer> numsA = indexA[i];
        for(int p = 0; p < numsA.size() - 1; p += 2) {
            int colA = numsA.get(p);
            int valA = numsA.get(p + 1);
            for(int j = 0; j < nB; j ++) {
                int valB = B[colA][j];
                result[i][j] += valA * valB;
            }
        }
    }

    return result;   
}
  }

  private static int[][] multiply(int[][] A, int[][] B) {

      List<List<int[]>> compressedA = new ArrayList<>();
      for (int i = 0; i < A.length; i++) {
          List<int[]> row = new ArrayList<>();
          for (int j = 0; j < A[0].length; j++) {
              if (A[i][j] != 0) {
                  row.add(new int[]{j, A[i][j]});
              }
          }
          compressedA.add(row);
      }

      List<List<int[]>> compressedB = new ArrayList<>();
      for (int j = 0; j < B[0].length; j++) {
          List<int[]> col = new ArrayList<>();
          for (int i = 0; i < B.length; i++) {
              if (B[i][j] != 0) {
                  col.add(new int[]{i, B[i][j]});
              }
          }
          compressedB.add(col);
      }

      int[][] result = new int[A.length][B[0].length];

      for (int i = 0; i < A.length; i++) {
          for (int j = 0; j < B[0].length; j++) {

              List<int[]> row = compressedA.get(i);
              List<int[]> col = compressedB.get(j);

              int sum = 0;
              int rowI = 0;
              int colJ = 0;
              while (rowI < row.size() && colJ < col.size()) {
                  if (row.get(rowI)[0] == col.get(colJ)[0]) {
                      sum += row.get(rowI)[1] * col.get(colJ)[1];
                      rowI++;
                      colJ++;
                  } else if (row.get(rowI)[0] < col.get(colJ)[0]) {
                      rowI++;
                  } else {
                      colJ++;
                  }
              }
              result[i][j] = sum;
          }
      }

      return result;
  }
```

```java
// Tags: Facebook, leetcode
    // https://leetcode.com/problems/binary-tree-vertical-order-traversal/

    /*
    Given a binary tree, return the vertical order traversal of its nodes' values. (ie, from top
    to bottom, column by column).

If two nodes are in the same row and column, the order should be from left to right.

Examples:

Given binary tree [3,9,20,null,null,15,7],
   3
  /\
 /  \
 9  20
    /\
   /  \
  15   7
return its vertical order traversal as:
[
  [9],
  [3,15],
  [20],
  [7]
]
Given binary tree [3,9,8,4,0,1,7],
     3
    /\
   /  \
   9   8
  /\  /\
 /  \/  \
 4  01   7
return its vertical order traversal as:
[
  [4],
  [9],
  [3,0,1],
  [8],
  [7]
]
Given binary tree [3,9,8,4,0,1,7,null,null,null,2,5] (0's right child is 2 and 1's left child is 5),
     3
    /\
   /  \
   9   8
  /\  /\
 /  \/  \
 4  01   7
    /\
   /  \
   5   2
return its vertical order traversal as:
[
  [4],
  [9,5],
  [3,0,1],
  [8,2],
  [7]
]
     */
     public List<List<Integer>> verticalOrder(TreeNode root) {

         if (root == null) {
             return new ArrayList<>();
         }

         int minVert = 0;
         Map<Integer, List<Integer>> map = new HashMap<>();

         Deque<TreeNode> nodeQ = new LinkedList<>();
         Deque<Integer> vertQ = new LinkedList<>();
         nodeQ.addLast(root);
         vertQ.addLast(0);

         while (!nodeQ.isEmpty()) {

             TreeNode n = nodeQ.removeFirst();
             int vert = vertQ.removeFirst();
             minVert = Math.min(minVert, vert);

             if (!map.containsKey(vert)) {
                 map.put(vert, new ArrayList<>());
             }

             map.get(vert).add(n.val);

             if (n.left != null) {
                 nodeQ.addLast(n.left);
                 vertQ.addLast(vert - 1);
             }
             if (n.right != null) {
                 nodeQ.addLast(n.right);
                 vertQ.addLast(vert + 1);
             }
         }

         List<List<Integer>> result = new ArrayList<>();
         while (map.get(minVert) != null) {
             result.add(map.get(minVert));
             minVert++;
         }
         return result;
     }     
```

```java
// Tags: Facebook, leetcode
// https://leetcode.com/problems/add-binary/

/*
Given two binary strings, return their sum (also a binary string).

For example,
a = "11"
b = "1"
Return "100".
 */

 public String addBinary(String a, String b) {
     int i = a.length() - 1;
     int j = b.length() - 1;
     int carry = 0;
     StringBuilder builder = new StringBuilder();
     while (i >= 0 || j >= 0) {
         char aChar = i >= 0 ? a.charAt(i) : '0';
         char bChar = j >= 0 ? b.charAt(j) : '0';
         int sum = carry + (aChar - '0') + (bChar - '0');
         builder.append(String.valueOf(sum % 2));
         carry = sum / 2;
         i--;
         j--;
     }

     if (carry != 0) {
         builder.append(String.valueOf(carry));
     }

     return builder.reverse().toString();
 }
```

```java
// Tags: facebook, leetcode
// https://leetcode.com/problems/integer-to-english-words/

/*
Convert a non-negative integer to its english words representation. Given input is guaranteed
 to be less than 231 - 1.

For example,
123 -> "One Hundred Twenty Three"
12345 -> "Twelve Thousand Three Hundred Forty Five"
1234567 -> "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"
Hint:

Did you see a pattern in dividing the number into chunk of words? For example, 123 and 123000.
Group the number by thousands (3 digits). You can write a helper function that takes a number
less than 1000 and convert just that chunk to words.
There are many edge cases. What are some good test cases? Does your code work with input such as
0? Or 1000010? (middle chunk is zero and should not be printed out)
 */

 private final String[] LESS_THAN_20 = {"", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
private final String[] TENS = {"", "Ten", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};
private final String[] THOUSANDS = {"", "Thousand", "Million", "Billion"};

public String numberToWords(int num) {
    if (num == 0) return "Zero";

    int i = 0;
    String words = "";

    while (num > 0) {
        if (num % 1000 != 0)
    	    words = helper(num % 1000) +THOUSANDS[i] + " " + words;
    	num /= 1000;
    	i++;
    }

    return words.trim();
}

private String helper(int num) {
    if (num == 0)
        return "";
    else if (num < 20)
        return LESS_THAN_20[num] + " ";
    else if (num < 100)
        return TENS[num / 10] + " " + helper(num % 10);
    else
        return LESS_THAN_20[num / 100] + " Hundred " + helper(num % 100);
}
```

```java
// Tags: facebook, leetcode
// https://leetcode.com/problems/decode-ways/

/*
A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -> 1
'B' -> 2
...
'Z' -> 26
Given an encoded message containing digits, determine the total number of ways to decode it.

For example,
Given encoded message "12", it could be decoded as "AB" (1 2) or "L" (12).

The number of ways decoding "12" is 2.
 */

private static int decodeWays(String digits) {

    if (digits.isEmpty()) {
        return 0;
    }

    int[] dp = new int[digits.length() + 1];
    dp[0] = 1;

    for (int i = 0; i < digits.length(); i++) {
        if (dp[i] > 0) {
            for (int j = 1; j <= 26; j++) {
                String rep = Integer.toString(j);
                if (i + rep.length() <= digits.length() &&
                    digits.substring(i, i + rep.length()).equals(rep)) {
                    dp[i + rep.length()] += dp[i];
                }
            }
        }
    }
    return dp[digits.length()];
}

////////////////////////////////////////////////////////////////////

public int numDecodings(String s) {

        if (s.length() == 0) {
            return 0;
        }

        int[] numWays = new int[s.length() + 1];
        numWays[0] = 1;

        for (int i = 1; i <= s.length(); i++) {
            int singleVal = s.charAt(i - 1) - '0';
            if (1 <= singleVal && singleVal <= 9) {
                numWays[i] += numWays[i-1];
            }

            if (i > 1) {
                int doubleVal = ((s.charAt(i - 2) - '0') * 10) + singleVal;
                if (10 <= doubleVal && doubleVal <= 26) {
                    numWays[i] += numWays[i-2];
                }
            }
        }
        return numWays[s.length()];
    }
```

```java
/*
Implement regular expression matching with support for '.' and '*'.

'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
*/

/*
1, If p.charAt(j) == s.charAt(i) :  dp[i][j] = dp[i-1][j-1];
2, If p.charAt(j) == '.' : dp[i][j] = dp[i-1][j-1];
3, If p.charAt(j) == '*':
   here are two sub conditions:
               1   if p.charAt(j-1) != s.charAt(i) : dp[i][j] = dp[i][j-2]  //in this case, a* only counts as empty
               2   if p.charAt(i-1) == s.charAt(i) or p.charAt(i-1) == '.':
                              dp[i][j] = dp[i-1][j]    //in this case, a* counts as multiple a
                           or dp[i][j] = dp[i][j-1]   // in this case, a* counts as single a
                           or dp[i][j] = dp[i][j-2]   // in this case, a* counts as empty
*/

public boolean isMatch(String s, String p) {

    boolean[][] dp = new boolean[s.length() + 1][p.length() + 1];
    dp[0][0] = true;
    for (int i = 2; i <= p.length(); i++) {
        if (p.charAt(i-1) == '*') {
            dp[0][i] = dp[0][i-2];    
        }
    }

    for (int i = 1; i <= s.length(); i++) {
        for (int j = 1; j <= p.length(); j++) {

            char sChar = s.charAt(i-1);
            char pChar = p.charAt(j-1);

            if (sChar == pChar || pChar == '.') {
                dp[i][j] = dp[i-1][j-1];
            } else if (pChar == '*') {
                if (sChar == p.charAt(j-2) || p.charAt(j-2) == '.') {
                    dp[i][j] = dp[i-1][j] || dp[i][j-2];
                } else {
                    dp[i][j] = dp[i][j-2];
                }
            }
        }
    }
    return dp[s.length()][p.length()];
}
```

```java
// Tags: Facebook, leetcode
// https://leetcode.com/problems/serialize-and-deserialize-binary-tree/

/*
Serialization is the process of converting a data structure or object into a sequence of bits
 so that it can be stored in a file or memory buffer, or transmitted across a network
 connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how
your serialization/deserialization algorithm should work. You just need to ensure that a binary
tree can be serialized to a string and this string can be deserialized to the original tree
structure.

For example, you may serialize the following tree

1
/ \
2   3
 / \
4   5
as "[1,2,3,null,null,4,5]", just the same as how LeetCode OJ serializes a binary tree. You do not
necessarily need to follow this format, so please be creative and come up with different
approaches yourself.
Note: Do not use class member/global/static variables to store states. Your serialize and
deserialize algorithms should be stateless.
 */

private static class TreeNode {

    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}

static String EMPTY = "#";
static String DIV = ",";

// Encodes a tree to a single string.
private static String serialize(TreeNode root) {
    StringBuilder builder = new StringBuilder();
    serialize(builder, root);
    return builder.toString();
}

private static void serialize(StringBuilder builder, TreeNode root) {
    if (root == null) {
        builder.append(EMPTY).append(DIV);
        return;
    }

    builder.append(root.val).append(DIV);
    serialize(builder, root.left);
    serialize(builder, root.right);
}

// Decodes your encoded data to tree.
private static TreeNode deserialize(String data) {

    if (data.isEmpty()) {
        return null;
    }

    String[] tokens = data.split(DIV);
    Iterator<String> it = Arrays.asList(tokens).iterator();
    return deserialize(it);
}

private static TreeNode deserialize(Iterator<String> tokens) {

    if (!tokens.hasNext()) {
        return null;
    }

    String token = tokens.next();
    if (token.equals(EMPTY)) {
        return null;
    }

    TreeNode node = new TreeNode(Integer.valueOf(token));
    node.left = deserialize(tokens);
    node.right = deserialize(tokens);
    return node;
}
```

```java
// Tags: Facebook, leetcode
// https://leetcode.com/problems/find-the-celebrity/

/*
Suppose you are at a party with n people (labeled from 0 to n - 1) and among them, there may
exist one celebrity. The definition of a celebrity is that all the other n - 1 people know
him/her but he/she does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you
are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information of
whether A knows B. You need to find out the celebrity (or verify there is not one) by asking as
few questions as possible (in the asymptotic sense).

You are given a helper function bool knows(a, b) which tells you whether A knows B. Implement a
function int findCelebrity(n), your function should minimize the number of calls to knows.

Note: There will be exactly one celebrity if he/she is in the party. Return the celebrity's label
if there is a celebrity in the party. If there is no celebrity, return -1.
 */

private static boolean knows(int a, int b) {
    // provided by leetcode
    return true;
}

private static int findCelebrity(int n) {
    int candidate = 0;
    for (int i = 1; i < n; i++) {
        if (knows(candidate, i)) {
            candidate = i;
        }
    }

    for (int i = 0; i < candidate; i++) {
        if (knows(candidate, i)) {
            return -1;
        }
    }

    for (int i = 0; i < n; i++) {
        if (!knows(i, candidate)) {
            return -1;
        }
    }
    return candidate;
}
```

```java
/*
The API: int read4(char *buf) reads 4 characters at a time from a file.

The return value is the actual number of characters read. For example, it returns 3 if there is only 3 characters left in the file.

By using the read4 API, implement the function int read(char *buf, int n) that reads n characters from the file.

Note:
The read function will only be called once for each test case.
*/
public int read(char[] buf, int n) {
        int numRead = 0;
        boolean eof = false;
        char[] tmp = new char[4];
        while (!eof && numRead < n) {
            int read = read4(tmp);
            eof = read < 4;
            read = Math.min(read, n - numRead);
            for (int i = 0; i < read; i++) {
                buf[numRead] = tmp[i];
                numRead++;
            }
        }
        return numRead;
    }

/*
The API: int read4(char *buf) reads 4 characters at a time from a file.

The return value is the actual number of characters read. For example, it returns 3 if there is only 3 characters left in the file.

By using the read4 API, implement the function int read(char *buf, int n) that reads n characters from the file.

Note:
The read function may be called multiple times.
*/

private int buffPtr = 0;
private int buffCnt = 0;
private char[] buff = new char[4];
public int read(char[] buf, int n) {
    int ptr = 0;
    while (ptr < n) {
        if (buffPtr == 0) {
            buffCnt = read4(buff);
        }
        if (buffCnt == 0) break;
        while (ptr < n && buffPtr < buffCnt) {
            buf[ptr++] = buff[buffPtr++];
        }
        if (buffPtr >= buffCnt) buffPtr = 0;
    }
    return ptr;
}
```

```java
/*
Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

Note: If the given node has no in-order successor in the tree, return null.
*/

public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {

    // Find leftmost child of right subtree
    if (p.right != null) {
        TreeNode n = p.right;
        while (n.left != null) {
            n = n.left;
        }
        return n;
    }

    // Find right parent for p
    TreeNode candidate = null;
    TreeNode n = root;
    while (!n.equals(p)) {
        if (p.val < n.val) {
            candidate = n;
            n = n.left;
        } else {
            n = n.right;
        }
    }
    return candidate;
}
```

```java
// Tags: Facebook, leetcode
// https://leetcode.com/problems/minimum-window-substring/

/*
Given a string S and a string T, find the minimum window in S which will contain all the
characters in T in complexity O(n).

For example,
S = "ADOBECODEBANC"
T = "ABC"
Minimum window is "BANC".

Note:
If there is no such window in S that covers all characters in T, return the empty string "".

If there are multiple such windows, you are guaranteed that there will always be only one unique
minimum window in S.
 */

private static String minWindow(String s, String t) {

    Map<Character, Integer> tCounts = new HashMap<>();
    for (Character c : t.toCharArray()) {
        tCounts.put(c, tCounts.getOrDefault(c, 0) + 1);
    }

    int min = Integer.MAX_VALUE;
    int minI = 0;
    int minJ = 0;
    int i = 0;
    int j = 0;
    Map<Character, Integer> sCounts = new HashMap<>();

    while (i < s.length()) {
        while (i < s.length() && !isSubset(tCounts, sCounts)) {
            Character c = s.charAt(i);
            if (tCounts.containsKey(c)) {
                sCounts.put(c, sCounts.getOrDefault(c, 0) + 1);
            }
            i++;
        }

        while (j < i && isSubset(tCounts, sCounts)) {
            if (i - j < min) {
                min = i - j;
                minI = i;
                minJ = j;
            }
            Character c = s.charAt(j);
            if (sCounts.containsKey(c)) {
                sCounts.put(c, sCounts.get(c) - 1);
            }
            j++;
        }
    }
    return s.substring(minJ, minI);
}

private static boolean isSubset(Map<Character, Integer> a, Map<Character, Integer> b) {
    if (b.keySet().containsAll(a.keySet())) {
        for (Character c : a.keySet()) {
            if (a.get(c) > b.get(c)) {
                return false;
            }
        }
        return true;
    }
    return false;
}
```

```java

// facebook, leetcode
   // https://leetcode.com/problems/expression-add-operators/

   /*
   Given a string that contains only digits 0-9 and a target value, return all possibilities to
   add binary operators (not unary) +, -, or * between the digits so they evaluate to the target
    value.

Examples:
"123", 6 -> ["1+2+3", "1*2*3"]
"232", 8 -> ["2*3+2", "2+3*2"]
"105", 5 -> ["1*0+5","10-5"]
"00", 0 -> ["0+0", "0-0", "0*0"]
"3456237490", 9191 -> []
    */

   private static List<String> addOperators(String num, int target) {
       Set<String> result = new HashSet<>();
       addOperators(result, target, num, 0, "");
       List<String> resultList = new ArrayList<>();
       for (String s : result) {
           resultList.add(s);
       }
       return resultList;
   }

   private static void addOperators(Set<String> result, int target, String num, int i,
                                    String expr) {
       if (i == num.length()) {
           if (evaluateString(expr) == target) {
               result.add(expr);
           }
           return;
       }

       if (i == num.length() - 1) {
           addOperators(result, target, num, i + 1, expr + num.charAt(i));
       } else {
           /*
           if (num.charAt(i) != '0') {
               addOperators(result, target, num, i + 1, expr + num.charAt(i));
           }
           */
           if (num.charAt(i) != '0') {
               int j = i;
               String newExpr = expr;
               while (j < num.length() - 1) {
                   newExpr += num.charAt(j);
                   addOperators(result, target, num, j + 1, newExpr);
                   j++;
               }
           }
           addOperators(result, target, num, i + 1, expr + num.charAt(i) + '+');
           addOperators(result, target, num, i + 1, expr + num.charAt(i) + '-');
           addOperators(result, target, num, i + 1, expr + num.charAt(i) + '*');
       }
   }


   private static int evaluateString(String s) {

       boolean subtraction = false;
       boolean multiply = false;
       Stack<Integer> stack = new Stack<>();

       int i = 0;
       while (i < s.length()) {
           Character c = s.charAt(i);
           if (Character.isDigit(c)) {
               int num = 0;
               while (i < s.length() && Character.isDigit(s.charAt(i))) {
                   num = num * 10 + (s.charAt(i) - '0');
                   i++;
               }
               if (subtraction) {
                   stack.push(-num);
                   subtraction = false;
               } else if (multiply) {
                   int prev = stack.pop();
                   stack.push(prev * num);
                   multiply = false;
               } else {
                   stack.push(num);
               }
           } else {
               if (c == '-') {
                   subtraction = true;
               } else if (c == '*') {
                   multiply = true;
               }
               i++;
           }
       }

       int sum = 0;
       while (!stack.isEmpty()) {
           sum += stack.pop();
       }
       return sum;
   }
 ```

```java
// Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode mergeKLists(ListNode[] lists) {

        PriorityQueue<ListNode> pq = new PriorityQueue<>((l1, l2) -> l1.val - l2.val);
        for (int i = 0; i < lists.length; i++) {
            if (lists[i] != null) {
                pq.add(lists[i]);
            }
        }

        ListNode preHead = new ListNode(0);
        ListNode curr = preHead;
        while (!pq.isEmpty()) {
            ListNode n = pq.poll();
            curr.next = n;
            curr = n;
            if (n.next != null) {
                pq.add(n.next);
            }
        }

        return preHead.next;
    }
}
```

```java
/*
Given a collection of intervals, merge all overlapping intervals.

For example,
Given [1,3],[2,6],[8,10],[15,18],
return [1,6],[8,10],[15,18].
*/
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
public class Solution {
    public List<Interval> merge(List<Interval> intervals) {

        if (intervals.size() == 0) {
            return intervals;
        }

        intervals.sort((i1, i2) -> i1.start - i2.start);

        Interval curr = intervals.get(0);
        int i = 1;
        while (i < intervals.size()) {
            Interval next = intervals.get(i);
            if (next.start <= curr.end) {
                curr.end = Math.max(next.end, curr.end);
                intervals.remove(i);
            } else {
                curr = next;
                i++;
            }
        }
        return intervals;
    }
}
```

```java
/*
Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Note:
You are not suppose to use the library's sort function for this problem.
*/

public void sortColors(int[] nums) {
    int low = 0;
    int mid = 0;
    int high = nums.length - 1;
    while (mid <= high) {
        if (nums[mid] == 0) {
            swap(nums, low, mid);
            low++;
            mid++;
        } else if (nums[mid] == 2) {
            swap(nums, mid, high);
            high--;
        } else {
            mid++;
        }
    }
}

private static void swap(int[] nums, int a, int b) {
    int tmp = nums[a];
    nums[a] = nums[b];
    nums[b] = tmp;
}
```

```java
/*
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words. You may assume the dictionary does not contain duplicate words.

For example, given
s = "leetcode",
dict = ["leet", "code"].

Return true because "leetcode" can be segmented as "leet code".

UPDATE (2017/1/4):
The wordDict parameter had been changed to a list of strings (instead of a set of strings). Please reload the code definition to get the latest changes.
*/

public class Solution {
    private static class TrieNode {

        boolean terminal;
        Map<Character, TrieNode> map = new HashMap<>();

        boolean isTerminal() {
            return terminal;
        }

        void setTerminal() {
            terminal = true;
        }

        boolean containsEdge(Character c) {
            return map.containsKey(c);
        }

        TrieNode next(Character c) {
            return map.get(c);
        }

        TrieNode addEdge(Character c) {
            TrieNode n = new TrieNode();
            map.put(c, n);
            return n;
        }
    }

    private static class Trie {
        TrieNode root = new TrieNode();

        void insertWord(String s) {
            TrieNode n = root;
            for (Character c : s.toCharArray()) {
                if (!n.containsEdge(c)) {
                    n = n.addEdge(c);
                } else {
                    n = n.next(c);
                }
            }
            n.setTerminal();
        }
    }

    public static boolean wordBreak(String s, List<String> wordDict) {

        Trie trie = new Trie();
        for (String word : wordDict) {
            trie.insertWord(word);
        }

        boolean[] b = new boolean[s.length() + 1];
        b[0] = true;

        for (int i = 0; i < s.length(); i++) {
            if (b[i]) {
                TrieNode n = trie.root;
                int j = i + 1;
                while (j <= s.length() && n.containsEdge(s.charAt(j - 1))) {
                    n = n.next(s.charAt(j - 1));
                    if (n.isTerminal()) {
                        b[j] = true;
                    }
                    j++;
                }
            }
        }

        return b[s.length()];
    }
}

/////////////////////////////////////////////

/*
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, add spaces in s to construct a sentence where each word is a valid dictionary word. You may assume the dictionary does not contain duplicate words.

Return all such possible sentences.

For example, given
s = "catsanddog",
dict = ["cat", "cats", "and", "sand", "dog"].

A solution is ["cats and dog", "cat sand dog"].

UPDATE (2017/1/4):
The wordDict parameter had been changed to a list of strings (instead of a set of strings). Please reload the code definition to get the latest changes.
*/

public class Solution {

    private static class FatTrieNode {

        String val;
        boolean terminal;
        Map<Character, FatTrieNode> map = new HashMap<>();

        boolean isTerminal() {
            return terminal;
        }

        void setTerminal() {
            terminal = true;
        }

        boolean containsEdge(Character c) {
            return map.containsKey(c);
        }

        FatTrieNode next(Character c) {
            return map.get(c);
        }

        FatTrieNode addEdge(Character c) {
            FatTrieNode n = new FatTrieNode();
            map.put(c, n);
            return n;
        }
    }

    private static class FatTrie {

        FatTrieNode root = new FatTrieNode();

        void insertWord(String s) {
            FatTrieNode n = root;
            for (Character c : s.toCharArray()) {
                if (!n.containsEdge(c)) {
                    n = n.addEdge(c);
                } else {
                    n = n.next(c);
                }
            }
            n.setTerminal();
            n.val = s;
        }
    }

    private static void dfs(List<String> result, List<String>[] l, int i, List<String> curr) {
        if (i == 0) {
            StringJoiner joiner = new StringJoiner(" ");
            for (int j = curr.size() - 1; j >= 0; j--) {
                joiner.add(curr.get(j));
            }
            result.add(joiner.toString());
            return;
        }

        List<String> candidates = l[i];
        for (String candidate : candidates) {
            curr.add(candidate);
            dfs(result, l, i - candidate.length(), curr);
            curr.remove(curr.size() - 1);
        }
    }


    public static List<String> wordBreak(String s, List<String> wordDict) {

        FatTrie trie = new FatTrie();
        for (String word : wordDict) {
            trie.insertWord(word);
        }

        List<String>[] l = new List[s.length() + 1];
        l[0] = new ArrayList<>();

        for (int i = 0; i < s.length(); i++) {
            if (l[i] != null) {
                FatTrieNode n = trie.root;
                int j = i + 1;
                while (j <= s.length() && n.containsEdge(s.charAt(j - 1))) {
                    n = n.next(s.charAt(j - 1));
                    if (n.isTerminal()) {
                        if (l[j] == null) {
                            l[j] = new ArrayList<>();
                        }
                        l[j].add(n.val);
                    }
                    j++;
                }
            }
        }

        List<String> result = new ArrayList<>();
        if (l[s.length()] != null) {
            dfs(result, l, s.length(), new LinkedList<>());
        }
        return result;
    }
}

```

```java
/*
Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.

Example:

nums = [1, 2, 3]
target = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is 7.
Follow up:
What if negative numbers are allowed in the given array?
How does it change the problem?
What limitation we need to add to the question to allow negative numbers?
*/

public int combinationSum4(int[] nums, int target) {

        Arrays.sort(nums);

        int[] dp = new int[target + 1];
        for (int i = 0; i < nums.length; i++) {
            int numVal = nums[i];
            if (numVal > target) {
                break;
            }
            dp[numVal] = 1;
        }

        for (int i = 0; i <= target; i++) {
            for (int j = 0; j < nums.length; j++) {
                int numVal = i - nums[j];
                if (numVal >= 0) {
                    dp[i] += dp[numVal];
                } else {
                    break;
                }
            }
        }
        return dp[target];
    }
```

```java
// Skyline
public List<int[]> getSkyline(int[][] buildings) {
    List<int[]> result = new ArrayList<>();
    List<int[]> height = new ArrayList<>();
    for(int[] b:buildings) {
        height.add(new int[]{b[0], -b[2]});
        height.add(new int[]{b[1], b[2]});
    }
    Collections.sort(height, (a, b) -> {
            if(a[0] != b[0])
                return a[0] - b[0];
            return a[1] - b[1];
    });
    Queue<Integer> pq = new PriorityQueue<>((a, b) -> (b - a));
    pq.offer(0);
    int prev = 0;
    for(int[] h:height) {
        if(h[1] < 0) {
            pq.offer(-h[1]);
        } else {
            pq.remove(h[1]);
        }
        int cur = pq.peek();
        if(prev != cur) {
            result.add(new int[]{h[0], cur});
            prev = cur;
        }
    }
    return result;
}
```

```java
/*
Implement wildcard pattern matching with support for '?' and '*'.

'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "*") → true
isMatch("aa", "a*") → true
isMatch("ab", "?*") → true
isMatch("aab", "c*a*b") → false
*/

boolean comparison(String str, String pattern) {
        int s = 0, p = 0, match = 0, starIdx = -1;            
        while (s < str.length()){
            // advancing both pointers
            if (p < pattern.length()  && (pattern.charAt(p) == '?' || str.charAt(s) == pattern.charAt(p))){
                s++;
                p++;
            }
            // * found, only advancing pattern pointer
            else if (p < pattern.length() && pattern.charAt(p) == '*'){
                starIdx = p;
                match = s;
                p++;
            }
           // last pattern pointer was *, advancing string pointer
            else if (starIdx != -1){
                p = starIdx + 1;
                match++;
                s = match;
            }
           //current pattern pointer is not star, last patter pointer was not *
          //characters do not match
            else return false;
        }

        //check for remaining characters in pattern
        while (p < pattern.length() && pattern.charAt(p) == '*')
            p++;

        return p == pattern.length();
}

///////////////////////////////

public boolean isMatch(String s, String p) {
        boolean[][] m = new boolean[s.length() + 1][p.length() + 1];
        m[0][0] = true;
        for (int i = 1; i <= p.length(); i++) {
            if (p.charAt(i-1) == '*') {
                m[0][i] = m[0][i-1];
            }
        }

        for (int i = 1; i <= s.length(); i++) {
            for (int j = 1; j <= p.length(); j++) {
                char sChar = s.charAt(i-1);
                char pChar = p.charAt(j-1);
                if (sChar == pChar || pChar == '?') {
                    m[i][j] = m[i-1][j-1];
                } else if (pChar == '*') {
                    m[i][j] = m[i-1][j] || m[i][j-1];
                }
            }
        }
        return m[s.length()][p.length()];
    }
```

```java
/*
Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

For example,
Given [100, 4, 200, 1, 3, 2],
The longest consecutive elements sequence is [1, 2, 3, 4]. Return its length: 4.

Your algorithm should run in O(n) complexity.
*/

public class Solution {

    private static class UF {
        Map<Integer, Integer> vals = new HashMap<>();
        Map<Integer, Integer> rank = new HashMap<>();
        Map<Integer, Integer> size = new HashMap<>();

        void insert(int i) {
            if (!vals.containsKey(i)) {
                vals.put(i, i);
                rank.put(i, 1);
                size.put(i, 1);
            }
        }

        int find(int i) {
            if (i != vals.get(i)) {
                vals.put(i, find(vals.get(i)));
            }
            return vals.get(i);
        }

        void union(int i, int j) {

            if (!vals.containsKey(i) || !vals.containsKey(j)) {
                return;
            }

            int ri = find(i);
            int rj = find(j);

            if (ri == rj) {
                return;
            }

            int rankI = rank.get(i);
            int rankJ = rank.get(j);
            if (rankI < rankJ) {
                vals.put(ri, rj);
                size.put(rj, size.get(ri) + size.get(rj));
            } else if (rankJ < rankI) {
                vals.put(rj, ri);
                size.put(ri, size.get(ri) + size.get(rj));
            } else {
                vals.put(ri, rj);
                size.put(rj, size.get(ri) + size.get(rj));
                rank.put(rj, rank.get(rj) + 1);
            }
        }

        int size(int i) {
            int ri = find(i);
            return size.get(ri);
        }

        List<Integer> roots() {
            List<Integer> result = new ArrayList<>();
            for (int v : vals.keySet()) {
                if (vals.get(v) == v) {
                    result.add(v);
                }
            }
            return result;
        }
    }


    public static int longestConsecutive(int[] nums) {
        UF uf = new UF();

        for (int num : nums) {
            uf.insert(num);
            uf.union(num, num - 1);
            uf.union(num, num + 1);
        }

        int maxSize = 0;
        for (int r : uf.roots()) {
            maxSize = Math.max(uf.size(r), maxSize);
        }
        return maxSize;
    }
}

``java
/*
Follow up for problem "Populating Next Right Pointers in Each Node".

What if the given tree could be any binary tree? Would your previous solution still work?

Note:

You may only use constant extra space.
For example,
Given the following binary tree,
         1
       /  \
      2    3
     / \    \
    4   5    7
After calling your function, the tree should look like:
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \    \
    4-> 5 -> 7 -> NULL
*/

// Use queues etc

public class Solution {
    public void connect(TreeLinkNode root) {
        if (root == null) {
            return;
        }

        TreeLinkNode curr = root;
        TreeLinkNode nextLevel = null;
        TreeLinkNode nextLevelHead = null;


        while (curr != null) {

            while (curr != null) {

                if (curr.left != null) {
                    if (nextLevel != null) {
                        nextLevel.next = curr.left;
                    } else {
                        nextLevelHead = curr.left;
                    }
                    nextLevel = curr.left;
                }

                if (curr.right != null) {
                    if (nextLevel != null) {
                        nextLevel.next = curr.right;
                    } else {
                        nextLevelHead = curr.right;
                    }
                    nextLevel = curr.right;
                }
                curr = curr.next;
            }

            curr = nextLevelHead;
            nextLevel = null;
            nextLevelHead = null;
        }
    }
}
```

```java
//pow
public double myPow(double x, int n) {

    if (n < 0) {
        n = -n;
        x = 1 / x;
    }

    if (n == 0) {
        return 1;
    }

    if (n == 1) {
        return x;
    }

    if (n % 2 == 1) {
        return myPow(x * x, n / 2) * x;
    } else {
        return myPow(x * x, n / 2);
    }
}
```

```java
// sqrt
public int sqrt(int x) {
    if (x == 0)
        return 0;
    int left = 1, right = Integer.MAX_VALUE;
    while (true) {
        int mid = left + (right - left)/2;
        if (mid > x/mid) {
            right = mid - 1;
        } else {
            if (mid + 1 > x/(mid + 1))
                return mid;
            left = mid + 1;
        }
    }
}

// Also
long r = x;
while (r*r > x)
    r = (r + x/r) / 2;
return (int) r;
```

```java
/*
Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

You may not alter the values in the nodes, only nodes itself may be changed.

Only constant memory is allowed.

For example,
Given this linked list: 1->2->3->4->5

For k = 2, you should return: 2->1->4->3->5

For k = 3, you should return: 3->2->1->4->5
*/
public ListNode reverseKGroup(ListNode head, int k) {
        int i = k;
        ListNode curr = head;
        while (curr != null && i > 0) {
            curr = curr.next;
            i--;
        }

        if (i == 0) {
            curr = reverseKGroup(curr, k);
            while (i < k) {
                ListNode tmp = head.next;
                head.next = curr;
                curr = head;
                head = tmp;
                i++;
            }
            head = curr;
        }
        return head;
    }

```

```java
/*
There are a row of n houses, each house can be painted with one of the k colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a n x k cost matrix. For example, costs[0][0] is the cost of painting house 0 with color 0; costs[1][2] is the cost of painting house 1 with color 2, and so on... Find the minimum cost to paint all houses.

Note:
All costs are positive integers.

Follow up:
Could you solve it in O(nk) runtime?
*/

public class Solution {
    public int minCostII(int[][] costs) {

        if (costs.length == 0) {
            return 0;
        }

        int prevMinCost = Integer.MAX_VALUE;
        int prevMinCostK = 0;
        int prevNextMinCost = Integer.MAX_VALUE;
        for (int i = 0; i < costs.length; i++) {

            int currMinCost = Integer.MAX_VALUE;
            int currMinCostK = 0;
            int currNextMinCost = Integer.MAX_VALUE;

            for (int k = 0; k < costs[0].length; k++) {
                if (i == 0) {
                    if (costs[0][k] < currMinCost) {
                        currNextMinCost = currMinCost;
                        currMinCost = costs[0][k];
                        currMinCostK = k;
                    } else if (costs[0][k] < currNextMinCost) {
                        currNextMinCost = costs[0][k];
                    }
                } else {
                    int candidateCost = (k == prevMinCostK ? prevNextMinCost : prevMinCost) + costs[i][k];
                    if (candidateCost < currMinCost) {
                        currNextMinCost = currMinCost;
                        currMinCost = candidateCost;
                        currMinCostK = k;
                    } else if (candidateCost < currNextMinCost) {
                        currNextMinCost = candidateCost;
                    }

                }
            }
            prevMinCost = currMinCost;
            prevMinCostK = currMinCostK;
            prevNextMinCost = currNextMinCost;
        }
        return prevMinCost;
    }
}
```

```java
/*
Given an array which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these m subarrays.

Note:
If n is the length of array, assume the following constraints are satisfied:

1 ≤ n ≤ 1000
1 ≤ m ≤ min(50, n)
Examples:

Input:
nums = [7,2,5,10,8]
m = 2

Output:
18

Explanation:
There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8],
where the largest sum among the two subarrays is only 18.
*/
private static class SumArr {
        int[] sums;
        SumArr(int[] arr) {
            this.sums = new int[arr.length + 1];
            for (int i = 1; i <= arr.length; i++) {
                sums[i] = sums[i-1] + arr[i-1];
            }
        }

        // inclusive
        int sum(int from, int to) {
            return sums[to + 1] - sums[from];
        }

    }

    public int splitArray(int[] nums, int m) {

        SumArr sa = new SumArr(nums);
        int[][] minSums = new int[nums.length][m];

        for (int p = 0; p < m; p++) {
            for (int i = 0; i < nums.length; i++) {
                if (p == 0) {
                    minSums[i][p] = sa.sum(i, nums.length - 1);
                } else {
                    int minSum = Integer.MAX_VALUE;
                    for (int j = i; j < nums.length - 1; j++) {
                        int prefixSum = sa.sum(i, j);
                        int partitionSum = minSums[j+1][p-1];
                        int maxSum = Math.max(prefixSum, partitionSum);
                        minSum = Math.min(minSum, maxSum);
                    }
                    minSums[i][p] = minSum;
                }
            }
        }
        return minSums[0][m-1];
    }
    ```

```java
/*
Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

For example:

Given n = 5 and edges = [[0, 1], [0, 2], [0, 3], [1, 4]], return true.

Given n = 5 and edges = [[0, 1], [1, 2], [2, 3], [1, 3], [1, 4]], return false.
*/
public class Solution {
    private static class UF {
        Map<Integer, Integer> map = new HashMap<>();

        void insert(int i) {
            if (!map.containsKey(i)) {
                map.put(i, i);
            }
        }

        int find(int i) {

            if (!map.containsKey(i)) {
                return -1;
            }

            while (i != map.get(i)) {
                i = map.get(i);
            }
            return i;
        }

        void union(int i, int j) {
            i = find(i);
            j = find(j);
            if (i != j) {
                map.put(i, j);
            }
        }

    }

    public static boolean validTree(int n, int[][] edges) {
        UF uf = new UF();
        for (int i = 0; i < n; i++) {
            uf.insert(i);
        }


        for (int[] edge : edges) {
            int source = edge[0];
            int dest = edge[1];
            if (uf.find(source) == uf.find(dest)) {
                return false;
            }
            uf.union(source, dest);
        }

        boolean rootFound = false;
        for (int i = 0; i < n; i++) {
            if (uf.find(i) == i) {
                if (rootFound) {
                    return false;
                }
                rootFound = true;
            }
        }
        return true;
    }
}
```

```java
/*
Follow up for "Remove Duplicates":
What if duplicates are allowed at most twice?

For example,
Given sorted array nums = [1,1,1,2,2,3],

Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3. It doesn't matter what you leave beyond the new length.
*/
public class Solution {
    public int removeDuplicates(int[] nums) {

        if (nums.length < 2) {
            return nums.length;
        }

        int readIdx = 1;
        int writeIdx = 1;
        int count = 1;
        while (readIdx < nums.length) {
            if (nums[readIdx] != nums[readIdx - 1]) {
                count = 1;
                nums[writeIdx] = nums[readIdx];
                writeIdx++;
            } else {
                if (count < 2) {
                    nums[writeIdx] = nums[readIdx];
                    writeIdx++;
                    count++;
                }
            }
            readIdx++;
        }
        return writeIdx;
    }
}
```


```java
maximal rect

The DP solution proceeds row by row, starting from the first row. Let the maximal rectangle area at row i and column j be computed by [right(i,j) - left(i,j)]*height(i,j).

All the 3 variables left, right, and height can be determined by the information from previous row, and also information from the current row. So it can be regarded as a DP solution. The transition equations are:

left(i,j) = max(left(i-1,j), cur_left), cur_left can be determined from the current row
right(i,j) = min(right(i-1,j), cur_right), cur_right can be determined from the current row
height(i,j) = height(i-1,j) + 1, if matrix[i][j]=='1';
height(i,j) = 0, if matrix[i][j]=='0'
The code is as below. The loops can be combined for speed but I separate them for more clarity of the algorithm.

class Solution {public:
int maximalRectangle(vector<vector<char> > &matrix) {
    if(matrix.empty()) return 0;
    const int m = matrix.size();
    const int n = matrix[0].size();
    int left[n], right[n], height[n];
    fill_n(left,n,0); fill_n(right,n,n); fill_n(height,n,0);
    int maxA = 0;
    for(int i=0; i<m; i++) {
        int cur_left=0, cur_right=n;
        for(int j=0; j<n; j++) { // compute height (can do this from either side)
            if(matrix[i][j]=='1') height[j]++;
            else height[j]=0;
        }
        for(int j=0; j<n; j++) { // compute left (from left to right)
            if(matrix[i][j]=='1') left[j]=max(left[j],cur_left);
            else {left[j]=0; cur_left=j+1;}
        }
        // compute right (from right to left)
        for(int j=n-1; j>=0; j--) {
            if(matrix[i][j]=='1') right[j]=min(right[j],cur_right);
            else {right[j]=n; cur_right=j;}    
        }
        // compute the area of rectangle (can do this from either side)
        for(int j=0; j<n; j++)
            maxA = max(maxA,(right[j]-left[j])*height[j]);
    }
    return maxA;
}
};

If you think this algorithm is not easy to understand, you can try this example:

0 0 0 1 0 0 0
0 0 1 1 1 0 0
0 1 1 1 1 1 0
The vector "left" and "right" from row 0 to row 2 are as follows

row 0:

l: 0 0 0 3 0 0 0
r: 7 7 7 4 7 7 7
row 1:

l: 0 0 2 3 2 0 0
r: 7 7 5 4 5 7 7
row 2:

l: 0 1 2 3 2 1 0
r: 7 6 5 4 5 6 7
The vector "left" is computing the left boundary. Take (i,j)=(1,3) for example. On current row 1, the left boundary is at j=2. However, because matrix[1][3] is 1, you need to consider the left boundary on previous row as well, which is 3. So the real left boundary at (1,3) is 3.

I hope this additional explanation makes things clearer.
```
