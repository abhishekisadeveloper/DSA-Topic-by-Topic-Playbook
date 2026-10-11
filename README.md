# DSA Topic-by-Topic Playbook

For every topic: must-know concepts + patterns to solve problems + algorithms needed + pro tips from the trenches. Follow in order.

## 📑 Index

| # | Topic | What you'll get |
| --- | --- | --- |
| 1 | Arrays | Core concepts + Sliding Window, Two Pointers, Prefix Sum, Kadane, Binary Search patterns + algos + pro tips |
| 2 | Strings | Core concepts + Sliding Window on chars, KMP, Two Pointers + algos + pro tips |
| 3 | Hashing | Core concepts + Frequency Count, Grouping, Complement patterns + algos + pro tips |
| 4 | Linked Lists | Core concepts + Fast & Slow, Reversal, Merge patterns + algos + pro tips |
| 5 | Stack | Core concepts + Monotonic Stack, Expression Eval, Bracket patterns + algos + pro tips |
| 6 | Queue & Deque | Core concepts + BFS, Sliding Window Max patterns + algos + pro tips |
| 7 | Binary Tree | Core concepts + DFS, BFS, Path Sum, LCA patterns + algos + pro tips |
| 8 | Binary Search Tree | Core concepts + Inorder property patterns + algos + pro tips |
| 9 | Heap | Core concepts + Top-K, Two Heaps, K-way Merge patterns + algos + pro tips |
| 10 | Trie | Core concepts + Prefix Search, Bitmask Trie patterns + algos + pro tips |
| 11 | Graphs | Core concepts + BFS, DFS, Dijkstra, Union-Find, Topo Sort patterns + algos + pro tips |
| 12 | Dynamic Programming | Core concepts + all 6 DP families + algos + pro tips |

## 1️⃣ Arrays

### 📌 Must-Know Concepts

- How arrays sit in memory — contiguous blocks, index = base address + (i × size)
- Static vs dynamic arrays — dynamic arrays (like Python lists, JS arrays) double in size when full → amortized O(1) push
- Time complexity: access O(1), search O(n), insert/delete at end O(1), insert/delete at middle O(n)
- Prefix Sum — precompute cumulative sums so range queries go from O(n) to O(1)
- Difference Array — for range update queries in O(1) instead of O(n)
- In-place operations — modify without extra space using index tricks
- Subarray vs subsequence — subarray is contiguous, subsequence is not

---

### 🧩 Patterns + When to Use Them

**Pattern 1 — Sliding Window** Use when: contiguous subarray/substring + you want max/min/count with a constraint Trigger words: longest, shortest, maximum sum of window of size K, at most K distinct

```python
# Fixed window (size K)
window_sum = sum(arr[:k])
max_sum = window_sum
for i in range(k, len(arr)):
    window_sum += arr[i] - arr[i - k]
    max_sum = max(max_sum, window_sum)

# Variable window
left = 0
for right in range(len(arr)):
    # add arr[right] to window
    while window_violates_condition:
        # remove arr[left] from window
        left += 1
    result = max(result, right - left + 1)
```

Key problems: Max sum subarray of size K, Longest subarray with sum ≤ K, Fruits into baskets, Minimum size subarray sum

---

**Pattern 2 — Two Pointers** Use when: sorted array + pair/triplet problem, or reduce from both ends Trigger words: pair with target sum, remove duplicates in-place, reverse, container with most water

```python
left, right = 0, len(arr) - 1
while left < right:
    current = arr[left] + arr[right]
    if current == target:
        return [left, right]
    elif current < target:
        left += 1
    else:
        right -= 1
```

Key problems: Two Sum II, Three Sum, Four Sum, Trapping Rain Water, Container with Most Water, Dutch National Flag (sort 0s 1s 2s)

---

**Pattern 3 — Prefix Sum** Use when: range sum queries, subarray sum equals K, count subarrays with given sum Trigger words: "sum of elements from index i to j", "subarray with sum = K"

```python
# Build prefix sum
prefix = [0] * (len(arr) + 1)
for i in range(len(arr)):
    prefix[i+1] = prefix[i] + arr[i]

# Range query [l, r] in O(1)
range_sum = prefix[r+1] - prefix[l]

# Count subarrays with sum = K (use hashmap)
count, curr_sum = 0, 0
seen = {0: 1}
for num in arr:
    curr_sum += num
    count += seen.get(curr_sum - k, 0)
    seen[curr_sum] = seen.get(curr_sum, 0) + 1
```

Key problems: Subarray Sum Equals K, Contiguous Array (0s and 1s), Range Sum Query, Product of Array Except Self

---

**Pattern 4 — Kadane's Algorithm (Max Subarray)** Use when: maximum sum contiguous subarray Trigger words: "maximum subarray sum", "largest sum contiguous"

```python
max_sum = arr[0]
curr_sum = arr[0]
for i in range(1, len(arr)):
    curr_sum = max(arr[i], curr_sum + arr[i])  # extend or restart
    max_sum = max(max_sum, curr_sum)
```

Key problems: Maximum Subarray, Maximum Product Subarray (track both min and max), Best Time to Buy and Sell Stock

---

**Pattern 5 — Binary Search on Arrays** Use when: sorted array + find target, OR minimize/maximize a value Trigger words: "search in sorted", "find first/last position", "rotated sorted array"

```python
# Standard binary search
lo, hi = 0, len(arr) - 1
while lo <= hi:
    mid = (lo + hi) // 2
    if arr[mid] == target: return mid
    elif arr[mid] < target: lo = mid + 1
    else: hi = mid - 1

# Find FIRST position of target
while lo < hi:
    mid = (lo + hi) // 2
    if arr[mid] < target: lo = mid + 1
    else: hi = mid  # don't exclude mid yet
```

Key problems: Binary Search, Search in Rotated Sorted Array, Find First and Last Position, Find Peak Element, Search a 2D Matrix

---

**Pattern 6 — Matrix Traversal** Use when: 2D array problems — spiral, diagonal, layer-by-layer

```python
# Spiral order traversal
top, bottom, left, right = 0, rows-1, 0, cols-1
while top <= bottom and left <= right:
    for col in range(left, right+1): result.append(matrix[top][col])  # →
    top += 1
    for row in range(top, bottom+1): result.append(matrix[row][right])  # ↓
    right -= 1
    if top <= bottom:
        for col in range(right, left-1, -1): result.append(matrix[bottom][col])  # ←
        bottom -= 1
    if left <= right:
        for row in range(bottom, top-1, -1): result.append(matrix[row][left])  # ↑
        left += 1
```

Key problems: Spiral Matrix, Rotate Image, Set Matrix Zeroes, Search a 2D Matrix

---

### ⚙️ Algorithms You Need

| Algorithm | What it does | Complexity |
| --- | --- | --- |
| Kadane's | Max subarray sum | O(n) time, O(1) space |
| Prefix Sum | Precompute cumulative sums for range queries | O(n) build, O(1) query |
| Dutch National Flag | Sort 0s, 1s, 2s in one pass | O(n) time, O(1) space |
| Binary Search | Find target in sorted array | O(log n) time |
| Merge Sort | Sort + count inversions | O(n log n) time, O(n) space |
| Quick Select | Find Kth largest in O(n) avg | O(n) avg, O(n²) worst |

---

### 🏆 Pro Tips

**Standard pro tips:**

- Always ask: is the array sorted? That unlocks binary search and two pointers immediately.
- When you see "subarray" think sliding window or prefix sum first — not brute O(n²).
- Prefix sum + hashmap combo solves 80% of subarray sum problems — burn this into your memory.
- For 2D matrix problems, treat (row, col) as a single integer: `idx = row * cols + col`. This helps with binary search on matrices.
- Dutch National Flag pattern (3 pointers) is the answer for any "sort array with only 3 distinct values" problem.

**🔥 Untold tips from big tech engineers:**

- **Amazon engineer:** "In interviews I always ask the input range first. If n ≤ 10⁵, O(n log n) is fine. If n ≤ 10⁸, you need O(n) or less. This single question eliminates half the wrong approaches before you even start."
- **Google engineer:** "Most candidates jump to sliding window immediately. I look for engineers who first ask: can this be solved with a prefix sum in one line? Kadane's, prefix sum, and two pointers solve the same class of problems — the best engineers know which one gives the cleanest code."
- **Razorpay SDE:** "When a problem says 'in-place' and O(1) space, immediately think: can I use the array indices themselves as a hashmap? Negative marking trick — mark arr\[abs(arr\[i\])-1\] as negative to encode seen values without extra space."
- **Uber engineer:** "Rotate array problems, rotated sorted array problems — they're all the same idea: find the break point. Binary search on the pivot. Once you see that, all rotation problems become trivial."
- **Meta/Facebook engineer:** "Product of array except self is asked in almost every first round. The trick is: left products array + right products in one pass. Knowing this cold — not just the answer but WHY two passes are needed — signals you think in terms of passes over data, which is exactly how we think about large-scale data pipelines."

## 2️⃣ Strings

### 📌 Must-Know Concepts

- Strings are immutable in most languages — concatenation in a loop is O(n²), always use a list/StringBuilder
- ASCII values — 'a' = 97, 'A' = 65, '0' = 48. Char to int: `ord(c) - ord('a')`
- Character frequency array of size 26 replaces a HashMap for lowercase English strings
- Palindrome — reads same forwards and backwards. Every single character is a palindrome.
- Anagram — same characters, different order. Two strings are anagrams if their sorted form is equal OR their frequency maps match.
- Substring — contiguous part of string. Subsequence — not necessarily contiguous.

---

### 🧩 Patterns + When to Use Them

**Pattern 1 — Sliding Window on Strings** Use when: longest/shortest substring with constraint on character counts Trigger words: longest substring without repeating, minimum window substring, at most K distinct characters

```python
# Longest substring without repeating characters
left = 0
seen = {}
result = 0
for right, char in enumerate(s):
    if char in seen and seen[char] >= left:
        left = seen[char] + 1  # shrink window past last occurrence
    seen[char] = right
    result = max(result, right - left + 1)
```

Key problems: Longest Substring Without Repeating, Minimum Window Substring, Fruit into Baskets, Longest Substring with At Most K Distinct

---

**Pattern 2 — Two Pointers on Strings** Use when: palindrome check, reverse string, check if string is valid palindrome with deletions

```python
# Check palindrome
def is_palindrome(s):
    left, right = 0, len(s) - 1
    while left < right:
        if s[left] != s[right]: return False
        left += 1; right -= 1
    return True

# Expand around center for longest palindromic substring
def expand(s, l, r):
    while l >= 0 and r < len(s) and s[l] == s[r]:
        l -= 1; r += 1
    return s[l+1:r]
```

Key problems: Valid Palindrome, Palindromic Substrings, Longest Palindromic Substring, Valid Palindrome II

---

**Pattern 3 — Frequency Map / Anagram Pattern** Use when: anagram check, group anagrams, find all anagrams in a string

```python
# Check anagram
from collections import Counter
return Counter(s) == Counter(t)

# Find all anagrams (sliding window + freq map)
def findAnagrams(s, p):
    p_count = Counter(p)
    window = Counter(s[:len(p)])
    result = [0] if window == p_count else []
    for i in range(len(p), len(s)):
        window[s[i]] += 1
        old = s[i - len(p)]
        window[old] -= 1
        if window[old] == 0: del window[old]
        if window == p_count: result.append(i - len(p) + 1)
    return result
```

Key problems: Valid Anagram, Group Anagrams, Find All Anagrams in a String, Permutation in String

---

**Pattern 4 — KMP Pattern Matching** Use when: find pattern in text efficiently — O(n+m) instead of O(n×m)

```python
def kmp_search(text, pattern):
    # Build failure function
    lps = [0] * len(pattern)
    j = 0
    for i in range(1, len(pattern)):
        while j > 0 and pattern[i] != pattern[j]: j = lps[j-1]
        if pattern[i] == pattern[j]: j += 1
        lps[i] = j
    # Search
    j = 0
    for i in range(len(text)):
        while j > 0 and text[i] != pattern[j]: j = lps[j-1]
        if text[i] == pattern[j]: j += 1
        if j == len(pattern):
            print(f'Found at index {i - j + 1}')
            j = lps[j-1]
```

Key problems: Implement strStr(), Repeated Substring Pattern, Shortest Palindrome

---

### ⚙️ Algorithms You Need

| Algorithm | What it does | Complexity |
| --- | --- | --- |
| KMP | Pattern search in text | O(n+m) time |
| Rabin-Karp | Rolling hash for pattern search | O(n+m) avg |
| Manacher's | Longest palindromic substring | O(n) time |
| Z-Algorithm | Pattern matching via Z-array | O(n+m) time |

---

### 🏆 Pro Tips

**Standard pro tips:**

- Use a frequency array of size 26 instead of a HashMap when dealing with lowercase English — it's faster and cleaner.
- Sliding window + two HashMap comparison for fixed-length window anagram problems is the cleanest O(n) solution.
- "Expand around center" for palindromes is O(n²) but very clean to code — for most interviews this is the expected answer, not Manacher's.

**🔥 Untold tips from big tech engineers:**

- **Google engineer:** "String problems are almost always array problems in disguise. The moment you mentally convert a string to an array of characters, half the patterns from arrays apply directly — especially sliding window."
- **Amazon SDE2:** "Group Anagrams is asked in almost every first round at Amazon. The trick interviewers look for: sorting each string as the key is O(n log n) per word. The better solution is using a tuple of 26 character counts as the key — O(n) per word. Knowing both and stating why the second is faster is a green flag."
- **Flipkart engineer:** "When you see 'minimum window' or 'smallest substring containing', always think shrinkable sliding window with two pointers. The trick is: expand right until you satisfy the condition, then shrink left as much as possible while condition holds. The window when left can't shrink anymore is a candidate answer."

## 3️⃣ Hashing (HashMap & HashSet)

### 📌 Must-Know Concepts

- HashMap: key → value mapping. Average O(1) insert, lookup, delete. Worst case O(n) due to collisions.
- HashSet: just keys, no values. Used for existence checks in O(1).
- Hash function maps a key to a bucket index. Collision handling: chaining (linked list per bucket) or open addressing.
- Load factor — when HashMap gets \~75% full, it resizes (doubles capacity, rehashes all keys) → amortized O(1)
- HashMap vs TreeMap: HashMap is O(1) but unordered. TreeMap/SortedDict is O(log n) but ordered.

---

### 🧩 Patterns + When to Use Them

**Pattern 1 — Frequency Count** Use when: count occurrences, find most frequent, check if valid given frequency constraints

```python
from collections import Counter
freq = Counter(nums)  # or defaultdict(int)
for num in nums:
    freq[num] += 1
```

Key problems: Top K Frequent Elements, Sort Characters by Frequency, First Unique Character, Valid Anagram

---

**Pattern 2 — Two Sum / Complement Pattern** Use when: find pair with given sum — don't loop twice, store complements

```python
seen = {}
for i, num in enumerate(nums):
    complement = target - num
    if complement in seen:
        return [seen[complement], i]
    seen[num] = i
```

Key problems: Two Sum, Two Sum II, Four Sum II (4 arrays), Pairs of Songs with Total Duration Divisible by 60

---

**Pattern 3 — Grouping / Bucketing** Use when: group elements that share a property

```python
# Group anagrams by sorted key
from collections import defaultdict
groups = defaultdict(list)
for word in words:
    key = tuple(sorted(word))  # or ''.join(sorted(word))
    groups[key].append(word)
```

Key problems: Group Anagrams, Group Shifted Strings, Find Duplicate File in System

---

**Pattern 4 — Sliding Window + Hash (Distinct Count)** Use when: count distinct elements in a window, maintain window validity

```python
from collections import defaultdict
window = defaultdict(int)
left = 0
for right in range(len(s)):
    window[s[right]] += 1
    while len(window) > k:  # more than k distinct
        window[s[left]] -= 1
        if window[s[left]] == 0:
            del window[s[left]]
        left += 1
    result = max(result, right - left + 1)
```

Key problems: Longest Substring with At Most K Distinct, Subarrays with K Different Integers

---

**Pattern 5 — Prefix Sum + Hash (Subarray Sum)** Use when: count subarrays with exact sum, find longest subarray with given sum

```python
prefix_sum = 0
seen = {0: 1}  # sum 0 seen once before index 0
count = 0
for num in nums:
    prefix_sum += num
    count += seen.get(prefix_sum - k, 0)
    seen[prefix_sum] = seen.get(prefix_sum, 0) + 1
```

Key problems: Subarray Sum Equals K, Contiguous Array (equal 0s and 1s), Longest Subarray with Sum K

---

### ⚙️ Algorithms You Need

| Algorithm | What it does | Complexity |
| --- | --- | --- |
| HashMap insert/lookup | Store and retrieve key-value pairs | O(1) avg |
| Frequency counting | Count occurrences of elements | O(n) |
| Prefix sum + hash | Count subarrays with target sum | O(n) |

---

### 🏆 Pro Tips

**Standard pro tips:**

- Whenever you're about to write a nested loop to check all pairs — stop and think: can a HashMap reduce this to O(n)? Usually yes.
- defaultdict(int) and defaultdict(list) from Python's collections save you from KeyError and make code 2x cleaner.
- For problems involving "consecutive" numbers (like Longest Consecutive Sequence), HashSet + check `num-1 not in set` = O(n) solution that beats sorting.

**🔥 Untold tips from big tech engineers:**

- **Google SDE:** "The HashMap is the Swiss Army knife of interviews. If you can't immediately see a pattern, ask yourself: what if I stored each element in a map as I traverse? What would I look up? This single question unlocks Two Sum, Subarray Sum = K, LRU Cache, and dozens more."
- **PhonePe engineer:** "Two Sum is the baby problem. But Pairs Divisible by 60, or 4Sum II — these are the same idea applied to harder constraints. If you understand WHY Two Sum works (store complement, check existence), you can adapt it to any constraint. Memorizing the solution doesn't help — understanding the WHY does."
- **Swiggy SDE2:** "Longest Consecutive Sequence is a perfect example of the HashSet trick. The brute approach is O(n²). Sorting is O(n log n). The HashSet approach is O(n). In a big-data system, that difference is the difference between a pipeline running in 1 second vs crashing. That's why we ask it."

## 4️⃣ Linked Lists

### 📌 Must-Know Concepts

- Each node holds data + pointer to next (singly) or next+prev (doubly)
- No random access — to get node at index i, you must traverse from head: O(n)
- Insert/delete at head: O(1). Insert/delete at tail: O(1) if you have tail pointer, else O(n)
- No built-in size — you need a counter or traverse to find length
- Dummy/sentinel node trick — add a fake head node before real head to handle edge cases cleanly
- Drawing the pointer state before and after every operation prevents bugs

---

### 🧩 Patterns + When to Use Them

**Pattern 1 — Fast & Slow Pointers (Floyd's)** Use when: detect cycle, find middle, find cycle start, check if palindrome

```python
# Detect cycle
slow = fast = head
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
    if slow == fast:
        return True  # cycle exists

# Find middle (slow stops at middle when fast hits end)
slow = fast = head
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
# slow is now the middle node
```

Key problems: Linked List Cycle I & II, Middle of Linked List, Happy Number, Palindrome Linked List

---

**Pattern 2 — Reversal** Use when: reverse entire list, reverse in groups of K, check palindrome

```python
# Iterative reversal
def reverse(head):
    prev, curr = None, head
    while curr:
        next_node = curr.next
        curr.next = prev
        prev = curr
        curr = next_node
    return prev  # new head

# Reverse in groups of K
def reverseKGroup(head, k):
    dummy = ListNode(0)
    dummy.next = head
    group_prev = dummy
    while True:
        kth = get_kth(group_prev, k)  # find kth node
        if not kth: break
        group_next = kth.next
        # reverse the group
        prev, curr = group_next, group_prev.next
        while curr != group_next:
            tmp = curr.next
            curr.next = prev
            prev = curr
            curr = tmp
        tmp = group_prev.next
        group_prev.next = kth
        group_prev = tmp
    return dummy.next
```

Key problems: Reverse Linked List, Reverse in K Groups, Reverse between positions L and R

---

**Pattern 3 — Two Pointers (Remove Nth from End)** Use when: remove Nth from end without knowing length — one pass

```python
dummy = ListNode(0)
dummy.next = head
fast = slow = dummy
# move fast n+1 steps ahead
for _ in range(n + 1):
    fast = fast.next
# move both until fast hits end
while fast:
    slow = slow.next
    fast = fast.next
# slow.next is the node to remove
slow.next = slow.next.next
return dummy.next
```

Key problems: Remove Nth Node from End, Delete Middle Node

---

**Pattern 4 — Merge Pattern** Use when: merge two sorted lists, merge K sorted lists

```python
# Merge two sorted lists
def mergeTwoLists(l1, l2):
    dummy = ListNode(0)
    curr = dummy
    while l1 and l2:
        if l1.val <= l2.val:
            curr.next = l1; l1 = l1.next
        else:
            curr.next = l2; l2 = l2.next
        curr = curr.next
    curr.next = l1 or l2
    return dummy.next
```

Key problems: Merge Two Sorted Lists, Merge K Sorted Lists (use MinHeap), Sort List

---

**Pattern 5 — HashMap on Linked List** Use when: clone with random pointer, detect intersection, LRU Cache

```python
# Clone with random pointer
old_to_new = {}
for node in original:
    old_to_new[node] = Node(node.val)
for node in original:
    old_to_new[node].next = old_to_new.get(node.next)
    old_to_new[node].random = old_to_new.get(node.random)
return old_to_new[original[0]]
```

Key problems: Copy List with Random Pointer, Intersection of Two Linked Lists, LRU Cache

---

### ⚙️ Algorithms You Need

| Algorithm | What it does | Complexity |
| --- | --- | --- |
| Floyd's Cycle Detection | Detect cycle + find entry point | O(n) time, O(1) space |
| In-place reversal | Reverse list iteratively | O(n) time, O(1) space |
| Merge two sorted lists | Combine two sorted LLs | O(n+m) time |
| Merge K sorted lists | Use MinHeap of (val, node) | O(n log k) time |

---

### 🏆 Pro Tips

**Standard pro tips:**

- Always use a dummy head node. It eliminates special cases for empty list or deleting the head node.
- Draw the pointers on paper before coding reversal problems. Getting it right in your head first saves 10 minutes of debugging.
- For "find middle" — if even length list, slow pointer gives the first middle. To get second middle, initialize fast = head.next.

**🔥 Untold tips from big tech engineers:**

- **Amazon SDE:** "LRU Cache is the most important linked list problem in industry. It combines Doubly LL + HashMap. At Amazon, we use LRU-style caching everywhere in distributed systems. If you can implement LRU in an interview completely clean — O(1) get and put — it signals you understand how real systems cache data."
- **Uber engineer:** "When I review code, junior engineers always handle the head and tail nodes as special cases with if-else blocks. Senior engineers use a dummy node and the code looks the same for every node. That one trick makes linked list code 3x more readable and 3x less buggy."
- **Microsoft SDE2:** "Palindrome Linked List — the clean solution is: find middle with slow/fast, reverse second half in-place, compare both halves. Interviewers love it because it combines 3 patterns in one solution. If you can code that without hints, you've shown mastery of linked lists."

## 5️⃣ Stack

### 📌 Must-Know Concepts

- LIFO — Last In First Out. push() and pop() both O(1).
- Call stack in programming is literally a stack — recursive calls use it.
- Balanced parentheses, expression evaluation, undo/redo — all stack problems.
- Monotonic stack — a stack where elements are kept in increasing or decreasing order. This is the secret weapon for "next greater/smaller" class of problems.
- Stack can be implemented with an array (append/pop) or a linked list (push/pop from head).

---

### 🧩 Patterns + When to Use Them

**Pattern 1 — Balanced Brackets** Use when: validate parentheses, check if brackets are balanced

```python
def isValid(s):
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}
    for char in s:
        if char in mapping:
            top = stack.pop() if stack else '#'
            if mapping[char] != top:
                return False
        else:
            stack.append(char)
    return not stack
```

Key problems: Valid Parentheses, Minimum Remove to Make Valid Parentheses, Longest Valid Parentheses

---

**Pattern 2 — Monotonic Stack (Decreasing) — Next Greater Element** Use when: for each element, find the NEXT element that is greater/smaller Trigger words: next greater, next warmer, stock span, daily temperatures

```python
# Next Greater Element — monotonic decreasing stack
result = [-1] * len(nums)
stack = []  # stores indices
for i in range(len(nums)):
    # while current element is greater than stack top, we found the answer
    while stack and nums[stack[-1]] < nums[i]:
        idx = stack.pop()
        result[idx] = nums[i]
    stack.append(i)
# elements left in stack have no next greater → result stays -1
```

Key problems: Next Greater Element I & II, Daily Temperatures, Stock Span Problem, Online Stock Plan

---

**Pattern 3 — Monotonic Stack (Increasing) — Largest Rectangle** Use when: find the largest area in a histogram, or problems involving "nearest smaller" elements

```python
# Largest Rectangle in Histogram
def largestRectangleArea(heights):
    stack = []  # monotonic increasing stack of indices
    max_area = 0
    heights.append(0)  # sentinel to flush remaining stack
    for i, h in enumerate(heights):
        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)
    return max_area
```

Key problems: Largest Rectangle in Histogram, Maximal Rectangle (2D), Trapping Rain Water

---

**Pattern 4 — Min Stack** Use when: design a stack with O(1) getMin operation

```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []  # parallel stack tracking current minimum

    def push(self, val):
        self.stack.append(val)
        min_val = min(val, self.min_stack[-1] if self.min_stack else val)
        self.min_stack.append(min_val)

    def pop(self):
        self.stack.pop()
        self.min_stack.pop()

    def getMin(self):
        return self.min_stack[-1]
```

Key problems: Min Stack, Maximum Frequency Stack, Design Browser History

---

**Pattern 5 — Stack for Expression Evaluation / Decode** Use when: evaluate arithmetic expressions, decode nested strings

```python
# Decode String — e.g., "3[a2[c]]" → "accaccacc"
def decodeString(s):
    stack = []
    curr_str, curr_num = '', 0
    for char in s:
        if char.isdigit():
            curr_num = curr_num * 10 + int(char)
        elif char == '[':
            stack.append((curr_str, curr_num))
            curr_str, curr_num = '', 0
        elif char == ']':
            prev_str, num = stack.pop()
            curr_str = prev_str + num * curr_str
        else:
            curr_str += char
    return curr_str
```

Key problems: Decode String, Evaluate Reverse Polish Notation, Basic Calculator I & II

---

### ⚙️ Algorithms You Need

| Algorithm | What it does | Complexity |
| --- | --- | --- |
| Monotonic decreasing stack | Next greater element for all elements | O(n) time, O(n) space |
| Monotonic increasing stack | Largest rectangle / nearest smaller | O(n) time, O(n) space |
| Two-stack min stack | O(1) getMin with standard push/pop | O(1) all ops |

---

### 🏆 Pro Tips

**Standard pro tips:**

- The monotonic stack processes every element exactly once (each element pushed and popped once) → always O(n) even though there's a while loop inside a for loop.
- Whenever you see "next greater element" or "previous smaller" — draw the stack state on paper. Once you visualize it once, you'll never forget it.
- Trapping Rain Water has 3 solutions: O(n²) brute, O(n) prefix max arrays, O(n) two pointers. Know all three — interviewers often ask you to optimize.

**🔥 Untold tips from big tech engineers:**

- **Google SDE2:** "Monotonic stack is the most underrated pattern. I ask it in every interview. Most candidates know the brute force but the O(n) monotonic stack solution separates good candidates from great ones. The key insight: we don't need to look forward — we process from left to right, and the stack tells us when we found the answer for a previous element."
- **Amazon engineer:** "Largest Rectangle in Histogram — I've seen candidates solve it with O(n²) and feel proud. The O(n) stack solution is what Amazon's image processing pipelines use conceptually for finding dominant regions. If you understand why the stack stays monotone, you understand lazy evaluation — a core concept in big data systems."
- **Razorpay SDE:** "Valid Parentheses is the warm-up. The real stack question at Razorpay is Minimum Remove to Make Valid Parentheses — it's a two-pass stack problem. First pass marks invalid ')' characters. Second pass (right to left) marks extra '(' characters. Most candidates only do one pass and get wrong answers. Two-pass thinking is a senior engineer trait."

## 6️⃣ Queue & Deque

### 📌 Must-Know Concepts

- Queue: FIFO — First In First Out. Enqueue at back, dequeue from front. O(1) both ends.
- Deque (double-ended queue): insert and remove from BOTH ends in O(1). More powerful than a queue.
- In Python: `collections.deque` is the go-to — do NOT use a list as a queue (list.pop(0) is O(n)).
- BFS uses a queue — process nodes level by level, guaranteeing shortest path in unweighted graphs.
- Sliding Window Maximum uses a deque — maintain a monotonic decreasing deque of indices.

---

### 🧩 Patterns + When to Use Them

**Pattern 1 — BFS with Queue** Use when: shortest path in unweighted graph/grid, level-order traversal, minimum steps

```python
from collections import deque

def bfs(start, target, graph):
    queue = deque([start])
    visited = {start}
    steps = 0
    while queue:
        for _ in range(len(queue)):  # process entire level
            node = queue.popleft()
            if node == target:
                return steps
            for neighbor in graph[node]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)
        steps += 1
    return -1
```

Key problems: Binary Tree Level Order Traversal, Rotten Oranges, Word Ladder, 01 Matrix, Shortest Path in Binary Matrix, Walls and Gates

---

**Pattern 2 — Sliding Window Maximum (Monotonic Deque)** Use when: maximum in every sliding window of size K

```python
from collections import deque

def maxSlidingWindow(nums, k):
    dq = deque()  # stores indices, values in decreasing order
    result = []
    for i in range(len(nums)):
        # remove indices outside window
        while dq and dq[0] < i - k + 1:
            dq.popleft()
        # remove indices whose values are smaller than current
        while dq and nums[dq[-1]] < nums[i]:
            dq.pop()
        dq.append(i)
        if i >= k - 1:
            result.append(nums[dq[0]])  # front is always the max
    return result
```

Key problems: Sliding Window Maximum, Jump Game VI, Constrained Subsequence Sum

---

**Pattern 3 — Multi-Source BFS** Use when: multiple starting points, spread from all sources simultaneously

```python
# Start BFS from ALL source nodes at once
queue = deque()
for i in range(rows):
    for j in range(cols):
        if grid[i][j] == source_value:
            queue.append((i, j))
            visited.add((i, j))

while queue:
    r, c = queue.popleft()
    for dr, dc in [(0,1),(0,-1),(1,0),(-1,0)]:
        nr, nc = r + dr, c + dc
        if is_valid(nr, nc) and (nr, nc) not in visited:
            visited.add((nr, nc))
            queue.append((nr, nc))
```

Key problems: Rotten Oranges, 01 Matrix (distance from nearest 0), Pacific Atlantic Water Flow

---

### ⚙️ Algorithms You Need

| Algorithm | What it does | Complexity |
| --- | --- | --- |
| BFS | Shortest path in unweighted graph | O(V+E) time |
| Multi-source BFS | BFS from multiple starting nodes | O(V+E) time |
| Monotonic Deque | Sliding window max/min | O(n) time |

---

### 🏆 Pro Tips

**Standard pro tips:**

- Always use `collections.deque` for BFS in Python. `list.pop(0)` is O(n) — this is a subtle bug that causes TLE on large inputs.
- Multi-source BFS is just regular BFS where you add ALL source nodes to the queue before starting. Rotten Oranges is the canonical example — add all rotten oranges first, then spread.
- The level-size trick (`for _ in range(len(queue))`) is how you track depth/steps in BFS without using extra space.

**🔥 Untold tips from big tech engineers:**

- **Google engineer:** "In system design interviews, BFS is how we think about propagation in distributed systems — broadcasting a message, cache invalidation, or event streaming. When I see a candidate naturally reach for BFS in a graph problem, it tells me they have the distributed systems intuition already."
- **Uber SDE2:** "Sliding window maximum with a deque is asked in Uber interviews because it's a real pattern in our pricing engine — computing rolling max surge scores over a window of time. Most candidates use a sorted structure (O(n log n)). The deque is O(n). That difference on millions of rides per hour matters enormously."

## 7️⃣ Binary Tree

### 📌 Must-Know Concepts

- Binary tree: each node has at most 2 children (left, right)
- Height of tree: longest path from root to leaf. Empty tree height = -1 or 0 depending on definition.
- Depth of node: distance from root to that node
- Full binary tree: every node has 0 or 2 children. Complete binary tree: all levels full except last, last level filled left to right.
- DFS on trees: Preorder (root → left → right), Inorder (left → root → right), Postorder (left → right → root)
- BFS on trees: Level order traversal using a queue
- The recursive call stack depth = height of tree = O(log n) for balanced, O(n) for skewed

---

### 🧩 Patterns + When to Use Them

**Pattern 1 — DFS Traversals** Use when: visit all nodes, path problems, validate properties

```python
# Recursive DFS (preorder)
def dfs(node):
    if not node: return
    print(node.val)       # preorder: process before children
    dfs(node.left)
    dfs(node.right)
    # postorder: move print here
    # inorder: move print between left and right calls

# Iterative inorder (important for BST problems)
def inorder(root):
    stack, result = [], []
    curr = root
    while curr or stack:
        while curr:
            stack.append(curr)
            curr = curr.left
        curr = stack.pop()
        result.append(curr.val)
        curr = curr.right
    return result
```

Key problems: Binary Tree Inorder/Preorder/Postorder Traversal, Flatten Binary Tree to Linked List

---

**Pattern 2 — BFS Level Order** Use when: level-by-level processing, right/left view, zigzag, average of levels

```python
from collections import deque

def levelOrder(root):
    if not root: return []
    queue = deque([root])
    result = []
    while queue:
        level = []
        for _ in range(len(queue)):  # process entire level
            node = queue.popleft()
            level.append(node.val)
            if node.left: queue.append(node.left)
            if node.right: queue.append(node.right)
        result.append(level)
    return result
```

Key problems: Binary Tree Level Order Traversal, Zigzag Level Order, Right Side View, Average of Levels, Maximum Width

---

**Pattern 3 — Path Sum Problems** Use when: find if path with given sum exists, find all root-to-leaf paths, max path sum

```python
# Root to leaf path sum check
def hasPathSum(root, target):
    if not root: return False
    if not root.left and not root.right:  # leaf node
        return root.val == target
    return hasPathSum(root.left, target - root.val) or \
           hasPathSum(root.right, target - root.val)

# Max path sum (any path — the hard version)
def maxPathSum(root):
    max_sum = float('-inf')
    def dfs(node):
        nonlocal max_sum
        if not node: return 0
        left = max(dfs(node.left), 0)   # ignore negative contributions
        right = max(dfs(node.right), 0)
        max_sum = max(max_sum, node.val + left + right)  # path through node
        return node.val + max(left, right)  # return best single branch
    dfs(root)
    return max_sum
```

Key problems: Path Sum I & II & III, Binary Tree Maximum Path Sum, Sum Root to Leaf Numbers

---

**Pattern 4 — LCA (Lowest Common Ancestor)** Use when: find the deepest node that is ancestor of both given nodes

```python
def lowestCommonAncestor(root, p, q):
    if not root or root == p or root == q:
        return root  # found one of the targets
    left = lowestCommonAncestor(root.left, p, q)
    right = lowestCommonAncestor(root.right, p, q)
    if left and right:
        return root  # p and q are in different subtrees → root is LCA
    return left or right  # both in same subtree
```

Key problems: LCA of Binary Tree, LCA of BST, Smallest Common Region, Distance Between Two Nodes

---

**Pattern 5 — Tree Construction** Use when: build tree from traversal arrays, serialize/deserialize

```python
# Build tree from preorder + inorder
def buildTree(preorder, inorder):
    if not preorder: return None
    root = TreeNode(preorder[0])
    mid = inorder.index(preorder[0])  # root splits inorder
    root.left = buildTree(preorder[1:mid+1], inorder[:mid])
    root.right = buildTree(preorder[mid+1:], inorder[mid+1:])
    return root
```

Key problems: Construct Binary Tree from Preorder+Inorder, Serialize and Deserialize Binary Tree, Symmetric Tree

---

### ⚙️ Algorithms You Need

| Algorithm | What it does | Complexity |
| --- | --- | --- |
| DFS (recursive) | Traversal, path problems | O(n) time, O(h) space |
| BFS (level order) | Level processing, views | O(n) time, O(n) space |
| Morris Traversal | Inorder without stack/recursion | O(n) time, O(1) space |
| Euler Tour | Preprocess for LCA queries | O(n) time |

---

### 🏆 Pro Tips

**Standard pro tips:**

- Postorder is the answer for problems where you need info from children before processing parent — height, diameter, max path sum all use postorder DFS.
- Return values from recursive calls carry information upward. Return type could be a tuple — `(height, is_balanced)` for the balanced tree check.
- Diameter of binary tree = max of (left height + right height + 2) across all nodes. Update a global max inside the DFS.

**🔥 Untold tips from big tech engineers:**

- **Google SDE3:** "Binary Tree Maximum Path Sum is the hardest non-graph tree problem asked at Google. The key insight most miss: you return the best SINGLE branch upward (to parent), but you update the global max considering BOTH branches at the current node. If you understand that distinction, you can solve it. If you don't, you'll never get it right."
- **Amazon SDE2:** "LCA is the tree problem asked most often at Amazon. We use LCA in our product graph to find relationships between categories, products, and taxonomies. When you understand that LCA works by the 'found one or both in subtrees' logic, you can adapt it to dozens of variants — lowest common ancestor of deepest leaves, smallest region, distance between nodes."
- **Microsoft engineer:** "Serialize and Deserialize Binary Tree is the complete package — it tests DFS, string parsing, and tree construction in one problem. Preorder serialization with '#' for null nodes is the cleanest approach. If you can write clean serialize/deserialize in 20 lines, you've shown mastery of tree recursion."

## 8️⃣ Binary Search Tree

### 📌 Must-Know Concepts

- BST property: all nodes in left subtree < root < all nodes in right subtree. This holds recursively.
- Inorder traversal of a BST gives elements in sorted ascending order — this is the most used property.
- Search, insert, delete: O(log n) for balanced BST, O(n) for skewed (like a linked list)
- BST doesn't have to be balanced — a skewed BST is just a linked list. Balanced BSTs (AVL, Red-Black) maintain O(log n) height.

---

### 🧩 Patterns + When to Use Them

**Pattern 1 — Inorder = Sorted** Use when: Kth smallest/largest, validate BST, convert BST to sorted array

```python
# Kth smallest in BST — inorder gives sorted order
def kthSmallest(root, k):
    stack, curr, count = [], root, 0
    while curr or stack:
        while curr:
            stack.append(curr)
            curr = curr.left
        curr = stack.pop()
        count += 1
        if count == k: return curr.val
        curr = curr.right
```

Key problems: Kth Smallest in BST, Kth Largest in BST, Convert BST to Greater Sum Tree, Inorder Successor

---

**Pattern 2 — BST Validation / Range Check** Use when: validate if a binary tree is a valid BST

```python
def isValidBST(root, min_val=float('-inf'), max_val=float('inf')):
    if not root: return True
    if root.val <= min_val or root.val >= max_val:
        return False
    return isValidBST(root.left, min_val, root.val) and \
           isValidBST(root.right, root.val, max_val)
```

Key problems: Validate Binary Search Tree, Recover BST (two nodes swapped)

---

**Pattern 3 — BST Search / Insert / Delete** Use when: standard BST operations

```python
# Search
def search(root, target):
    if not root or root.val == target: return root
    if target < root.val: return search(root.left, target)
    return search(root.right, target)

# Delete (hardest — 3 cases)
def deleteNode(root, key):
    if not root: return None
    if key < root.val: root.left = deleteNode(root.left, key)
    elif key > root.val: root.right = deleteNode(root.right, key)
    else:  # found the node
        if not root.left: return root.right
        if not root.right: return root.left
        # has both children: replace with inorder successor (min of right subtree)
        successor = root.right
        while successor.left: successor = successor.left
        root.val = successor.val
        root.right = deleteNode(root.right, successor.val)
    return root
```

Key problems: Search in BST, Insert into BST, Delete Node in BST, Floor/Ceiling in BST

---

### ⚙️ Algorithms You Need

| Algorithm | What it does | Complexity |
| --- | --- | --- |
| BST inorder traversal | Get sorted elements | O(n) time |
| BST search | Find value by exploiting ordering | O(log n) avg |
| BST insert/delete | Maintain BST property | O(log n) avg |

---

### 🏆 Pro Tips

**Standard pro tips:**

- Always pass `min` and `max` range bounds in the BST validation recursion — not just comparing with parent. Common mistake: comparing only with parent fails for deeper nodes.
- Inorder successor of a node: if it has a right child, go right then all the way left. If it doesn't, it's the nearest ancestor where you came from the left.

**🔥 Untold tips from big tech engineers:**

- **Google engineer:** "BST delete is the hardest basic operation to code correctly. The 3-case analysis (no child, one child, two children) is what separates candidates who truly understand trees from those who just read about them. At Google, database indexing uses BST-like structures. Getting delete right signals you can reason about pointer manipulation under constraints — critical for systems work."
- **Adobe SDE2:** "In interviews, whenever I see a Binary Tree problem, I immediately ask myself: is it actually a BST? If yes, the inorder = sorted trick and the range-based property unlock O(log n) solutions to problems that would otherwise need O(n) full traversals."

## 9️⃣ Heap & Priority Queue

### 📌 Must-Know Concepts

- Heap = complete binary tree stored as an array. Parent at index i → children at 2i+1 and 2i+2.
- Min-Heap: parent ≤ children → root is always the minimum. Max-Heap: parent ≥ children → root is always the maximum.
- Insert (heapify up): O(log n). Remove root (heapify down): O(log n). Get min/max: O(1).
- Python's `heapq` is a min-heap by default. For max-heap, negate values: push -x, pop and negate back.
- Priority Queue = Heap in disguise. "Next most important" problems = heap.

---

### 🧩 Patterns + When to Use Them

**Pattern 1 — Top K Elements** Use when: K largest, K smallest, K most frequent — don't sort the whole array

```python
import heapq

# K largest elements — use min-heap of size K
def kLargest(nums, k):
    heap = []
    for num in nums:
        heapq.heappush(heap, num)
        if len(heap) > k:
            heapq.heappop(heap)  # remove the smallest
    return list(heap)  # heap contains K largest

# K most frequent
from collections import Counter
def topKFrequent(nums, k):
    freq = Counter(nums)
    return heapq.nlargest(k, freq.keys(), key=freq.get)
```

Key problems: Kth Largest Element, Top K Frequent Elements, K Closest Points to Origin, Sort Characters by Frequency

---

**Pattern 2 — Two Heaps (Median of Stream)** Use when: find median from a stream, split data into two halves

```python
import heapq

class MedianFinder:
    def __init__(self):
        self.small = []  # max-heap (negate) — lower half
        self.large = []  # min-heap — upper half

    def addNum(self, num):
        heapq.heappush(self.small, -num)  # always push to small first
        # balance: ensure small's max <= large's min
        if self.small and self.large and (-self.small[0]) > self.large[0]:
            heapq.heappush(self.large, -heapq.heappop(self.small))
        # balance sizes: small can have at most 1 more than large
        if len(self.small) > len(self.large) + 1:
            heapq.heappush(self.large, -heapq.heappop(self.small))
        if len(self.large) > len(self.small):
            heapq.heappush(self.small, -heapq.heappop(self.large))

    def findMedian(self):
        if len(self.small) > len(self.large): return -self.small[0]
        return (-self.small[0] + self.large[0]) / 2
```

Key problems: Find Median from Data Stream, Sliding Window Median

---

**Pattern 3 — K-Way Merge** Use when: merge K sorted arrays/lists — use heap to always pick minimum

```python
import heapq

def mergeKLists(lists):
    heap = []
    for i, node in enumerate(lists):
        if node:
            heapq.heappush(heap, (node.val, i, node))
    dummy = curr = ListNode(0)
    while heap:
        val, i, node = heapq.heappop(heap)
        curr.next = node
        curr = curr.next
        if node.next:
            heapq.heappush(heap, (node.next.val, i, node.next))
    return dummy.next
```

Key problems: Merge K Sorted Lists, Smallest Range Covering Elements from K Lists, Kth Smallest in Sorted Matrix

---

### ⚙️ Algorithms You Need

| Algorithm | What it does | Complexity |
| --- | --- | --- |
| Heapify | Build heap from array | O(n) time |
| Heap insert | Add element maintaining heap property | O(log n) |
| Heap extract-min/max | Remove root, reheapify | O(log n) |
| K-way merge with heap | Merge K sorted sequences | O(n log k) |

---

### 🏆 Pro Tips

**Standard pro tips:**

- For "K largest" use a min-heap of size K. Counterintuitive — you keep the K largest by kicking out the small ones.
- For "K smallest" use a max-heap of size K. Same logic reversed.
- Heap sort is O(n log n) but not stable and not cache-friendly — rarely used in practice but good to know.

**🔥 Untold tips from big tech engineers:**

- **Amazon SDE2:** "Heaps are fundamental to how Amazon's recommendation system works at the data layer — we're constantly finding Top-K items from a stream. The Two Heaps pattern for median is asked specifically because it mirrors how we handle streaming metrics. If you implement MedianFinder clean, that tells me you understand streaming data architecture."
- **Zepto SDE:** "A common trick in heap problems: if you need the Kth largest in an unsorted array and can't sort, use QuickSelect instead of a heap. QuickSelect is O(n) average vs O(n log k) for heap. Knowing BOTH approaches and their trade-offs — and when to use which — is the senior engineer answer."

## 🔟 Trie

### 📌 Must-Know Concepts

- Trie (prefix tree): each node represents one character. Root is empty. Path from root to a node spells a prefix.
- TrieNode has: `children` (dict or array of 26), `is_end` (bool — marks complete word)
- Insert, search, startsWith: all O(L) where L = length of the word — independent of how many words are stored
- Space tradeoff: Trie uses more space than HashSet but supports prefix operations that HashSet can't
- Bitwise Trie: stores integers bit by bit (from MSB to LSB) — used for XOR maximization problems

---

### 🧩 Patterns + When to Use Them

**Pattern 1 — Basic Trie (Insert + Search + StartsWith)**

```python
class TrieNode:
    def __init__(self):
        self.children = {}  # char -> TrieNode
        self.is_end = False

class Trie:
    def __init__(self): self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True

    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children: return False
            node = node.children[char]
        return node.is_end

    def startsWith(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children: return False
            node = node.children[char]
        return True
```

Key problems: Implement Trie, Design Add and Search Words (with wildcard '.'), Replace Words

---

**Pattern 2 — Trie + Backtracking (Word Search)** Use when: search for multiple words in a grid efficiently

```python
# Word Search II — find all words from a list in a grid
# Build trie from word list, then DFS on grid
def findWords(board, words):
    trie = Trie()
    for word in words: trie.insert(word)
    result = set()
    def dfs(node, i, j, path):
        if node.is_end: result.add(path)
        if i < 0 or j < 0 or i >= len(board) or j >= len(board[0]): return
        char = board[i][j]
        if char not in node.children or char == '#': return
        board[i][j] = '#'  # mark visited
        for di, dj in [(0,1),(0,-1),(1,0),(-1,0)]:
            dfs(node.children[char], i+di, j+dj, path+char)
        board[i][j] = char  # restore
    for i in range(len(board)):
        for j in range(len(board[0])):
            dfs(trie.root, i, j, '')
    return list(result)
```

Key problems: Word Search II, Boggle Game

---

**Pattern 3 — Bitwise Trie (Max XOR)** Use when: find two numbers in array with maximum XOR

```python
# Insert integer into bitwise trie (32 bits)
# At each level, branch 0 or 1 based on the bit
# To find max XOR: at each bit, greedily go to opposite child if exists
```

Key problems: Maximum XOR of Two Numbers in an Array, Maximum XOR with an Element from Array

---

### ⚙️ Algorithms You Need

| Algorithm | What it does | Complexity |
| --- | --- | --- |
| Trie insert | Add word character by character | O(L) per word |
| Trie search | Search word or prefix | O(L) |
| Trie + DFS | Find all matching words in grid | O(W×L + 4^L) |

---

### 🏆 Pro Tips

**Standard pro tips:**

- Use a dict for children (not array of 26) unless told lowercase English only — dict handles unicode, special chars, numbers.
- Trie beats HashSet when you need prefix queries. HashSet can answer "does this exact word exist?" but not "how many words start with 'pre'?" — Trie answers both.
- When deleting from a Trie, don't delete nodes greedily — only mark `is_end = False` and clean up with a post-order check.

**🔥 Untold tips from big tech engineers:**

- **Google SDE:** "Autocomplete systems at Google are backed by a Trie at the core concept level. In the interview, if you understand that Trie enables O(prefix\_length) prefix search regardless of total words stored, it shows you understand the scalability property — which is exactly why Tries exist in search infrastructure."
- **Swiggy engineer:** "Word Search II without a Trie is O(words × 4^L). With a Trie, you prune the DFS the moment a prefix doesn't exist in the Trie. This optimization is the entire point of the problem. If you build the Trie first and explain the pruning, you've demonstrated the core of database index optimization in one problem."

## 1️⃣1️⃣ Graphs

### 📌 Must-Know Concepts

- Graph = vertices (nodes) + edges (connections). Directed vs undirected. Weighted vs unweighted.
- Adjacency list (dict/array of lists) — best for sparse graphs. Adjacency matrix — best for dense graphs or quick edge lookups.
- BFS = shortest path in unweighted graphs. DFS = connectivity, cycle detection, path enumeration.
- Connected component: a group of nodes where every node can reach every other node.
- Topological sort: linear ordering of nodes where every directed edge u→v means u comes before v. Only valid for DAGs (Directed Acyclic Graphs).
- Union-Find (Disjoint Set Union): efficiently tracks connected components and detects cycles in undirected graphs.

---

### 🧩 Patterns + When to Use Them

**Pattern 1 — BFS / DFS for Connected Components** Use when: number of islands, number of provinces, flood fill, region coloring

```python
# Number of Islands — DFS version
def numIslands(grid):
    count = 0
    def dfs(i, j):
        if i < 0 or j < 0 or i >= len(grid) or j >= len(grid[0]): return
        if grid[i][j] != '1': return
        grid[i][j] = '#'  # mark visited
        dfs(i+1,j); dfs(i-1,j); dfs(i,j+1); dfs(i,j-1)
    for i in range(len(grid)):
        for j in range(len(grid[0])):
            if grid[i][j] == '1':
                dfs(i, j)
                count += 1
    return count
```

Key problems: Number of Islands, Max Area of Island, Flood Fill, Number of Provinces, Pacific Atlantic Water Flow

---

**Pattern 2 — Topological Sort (Kahn's BFS)** Use when: task ordering, course scheduling, build dependencies

```python
from collections import deque, defaultdict

def topoSort(n, prerequisites):
    graph = defaultdict(list)
    in_degree = [0] * n
    for u, v in prerequisites:
        graph[u].append(v)
        in_degree[v] += 1
    queue = deque([i for i in range(n) if in_degree[i] == 0])
    order = []
    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    return order if len(order) == n else []  # [] means cycle exists
```

Key problems: Course Schedule I & II, Alien Dictionary, Minimum Height Trees, Parallel Courses

---

**Pattern 3 — Dijkstra's (Weighted Shortest Path)** Use when: shortest path in weighted graph with non-negative weights

```python
import heapq

def dijkstra(graph, src, n):
    dist = [float('inf')] * n
    dist[src] = 0
    heap = [(0, src)]  # (distance, node)
    while heap:
        d, u = heapq.heappop(heap)
        if d > dist[u]: continue  # outdated entry, skip
        for v, weight in graph[u]:
            if dist[u] + weight < dist[v]:
                dist[v] = dist[u] + weight
                heapq.heappush(heap, (dist[v], v))
    return dist
```

Key problems: Network Delay Time, Cheapest Flights Within K Stops (modified), Path with Minimum Effort, Find the City with Smallest Number of Neighbors

---

**Pattern 4 — Union-Find (Cycle Detection / Connectivity)**

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, x):  # with path compression
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):  # with union by rank
        px, py = self.find(x), self.find(y)
        if px == py: return False  # already connected = cycle!
        if self.rank[px] < self.rank[py]: px, py = py, px
        self.parent[py] = px
        if self.rank[px] == self.rank[py]: self.rank[px] += 1
        return True
```

Key problems: Redundant Connection, Number of Connected Components, Accounts Merge, Most Stones Removed, Kruskal's MST

---

**Pattern 5 — Cycle Detection in Directed Graph (DFS coloring)** Use when: detect cycle in directed graph, check if DAG

```python
def hasCycle(n, edges):
    graph = defaultdict(list)
    for u, v in edges: graph[u].append(v)
    # 0=unvisited, 1=in current path (gray), 2=done (black)
    color = [0] * n
    def dfs(node):
        if color[node] == 1: return True   # back edge = cycle
        if color[node] == 2: return False  # already processed
        color[node] = 1  # mark as in-progress
        for neighbor in graph[node]:
            if dfs(neighbor): return True
        color[node] = 2  # mark as done
        return False
    return any(dfs(i) for i in range(n) if color[i] == 0)
```

Key problems: Course Schedule (detect cycle), Graph Valid Tree

---

### ⚙️ Algorithms You Need

| Algorithm | Use case | Complexity |
| --- | --- | --- |
| BFS | Shortest path, unweighted | O(V+E) |
| DFS | Connectivity, cycles, paths | O(V+E) |
| Dijkstra's | Shortest path, positive weights | O((V+E) log V) |
| Bellman-Ford | Shortest path, negative weights | O(VE) |
| Floyd-Warshall | All-pairs shortest path | O(V³) |
| Kruskal's | Minimum Spanning Tree | O(E log E) |
| Prim's | Minimum Spanning Tree | O((V+E) log V) |
| Topological Sort | Task ordering, DAG | O(V+E) |
| Union-Find | Connectivity, cycle detection | O(α(n)) per op |

---

### 🏆 Pro Tips

**Standard pro tips:**

- Dijkstra fails on negative weights — use Bellman-Ford instead. But Bellman-Ford is O(VE) — slow. If you have negative weights and need faster, that's for FAANG-level prep.
- Union-Find with path compression + union by rank gives near O(1) per operation (amortized). The proof involves Ackermann's function — just know it's practically O(1).
- For grid problems (2D), treat each cell (i,j) as a graph node. Neighbors are the 4 (or 8) adjacent cells. This converts grid BFS/DFS into standard graph problems.

**🔥 Untold tips from big tech engineers:**

- **Google SDE3:** "Graphs are the hardest topic but also the richest. At Google, 80% of our core products are graph problems — Maps (shortest path), Search (PageRank = graph ranking), YouTube (recommendation = graph traversal). If you truly master BFS, DFS, Dijkstra and topological sort, you have the foundations of what we build every day."
- **Razorpay SDE2:** "In payments, transaction fraud detection is a graph problem — detect cycles in a transaction graph (money going in circles). Topological sort tells you the valid ordering. Cycle detection tells you fraud. When I see a candidate understand cycle detection in directed graphs with the 3-color DFS, I know they can reason about financial transaction integrity."
- **Flipkart engineer:** "Most candidates know BFS and DFS but mix them up. Here's the rule I give in interviews: BFS when you need shortest in terms of steps or levels. DFS when you need to explore all possibilities or need connectivity. If you remember this rule and apply it cold, you'll pick right 95% of the time."

## 1️⃣2️⃣ Dynamic Programming

### 📌 Must-Know Concepts

- DP = break a problem into overlapping subproblems + cache the results. Two techniques: Memoization (top-down) and Tabulation (bottom-up).
- DP vs Greedy: greedy makes a local choice and never revisits. DP explores all choices and picks the best. When greedy gives wrong answer, DP is usually the fix.
- DP vs Recursion: plain recursion recomputes the same subproblem many times (exponential). DP caches it (polynomial).
- State: what information do you need to uniquely define a subproblem? That becomes your dp array dimensions.
- Recurrence: how does dp\[i\] / dp\[i\]\[j\] depend on smaller states? This is the core of the solution.
- Base case: the smallest subproblem you can answer without recursion.

---

### 🧩 Patterns + When to Use Them

**Pattern 1 — 1D Linear DP (depends on previous state)** Use when: each position depends on previous positions Trigger words: climbing stairs, house robber, jump game, decode ways

```python
# House Robber — can't rob adjacent houses
def rob(nums):
    if not nums: return 0
    if len(nums) == 1: return nums[0]
    dp = [0] * len(nums)
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])
    for i in range(2, len(nums)):
        dp[i] = max(dp[i-1], dp[i-2] + nums[i])  # skip or rob
    return dp[-1]
    # space optimized: track only prev2 and prev1
```

Key problems: Climbing Stairs, House Robber I & II, Jump Game I & II, Decode Ways, Fibonacci

---

**Pattern 2 — 2D Grid DP** Use when: paths in a grid, min cost to reach destination Trigger words: unique paths, minimum path sum, robot movement

```python
# Minimum Path Sum in Grid
def minPathSum(grid):
    m, n = len(grid), len(grid[0])
    dp = [[0]*n for _ in range(m)]
    dp[0][0] = grid[0][0]
    for i in range(1, m): dp[i][0] = dp[i-1][0] + grid[i][0]   # first column
    for j in range(1, n): dp[0][j] = dp[0][j-1] + grid[0][j]   # first row
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])
    return dp[m-1][n-1]
```

Key problems: Unique Paths I & II, Minimum Path Sum, Dungeon Game, Triangle

---

**Pattern 3 — 0/1 Knapsack** Use when: include or exclude each item exactly once, maximize value within capacity Trigger words: "partition equal subset", "target sum", "count subsets with given sum"

```python
# 0/1 Knapsack: can each item be taken once?
def knapsack(weights, values, W):
    n = len(weights)
    dp = [[0]*(W+1) for _ in range(n+1)]
    for i in range(1, n+1):
        for w in range(W+1):
            dp[i][w] = dp[i-1][w]  # don't take item i
            if weights[i-1] <= w:  # take item i if it fits
                dp[i][w] = max(dp[i][w], dp[i-1][w-weights[i-1]] + values[i-1])
    return dp[n][W]

# Space optimized (1D dp, iterate W backwards!)
def knapsack_1d(weights, values, W):
    dp = [0] * (W+1)
    for i in range(len(weights)):
        for w in range(W, weights[i]-1, -1):  # MUST go backwards
            dp[w] = max(dp[w], dp[w-weights[i]] + values[i])
    return dp[W]
```

Key problems: 0/1 Knapsack, Partition Equal Subset Sum, Target Sum, Last Stone Weight II, Count of Subset Sum

---

**Pattern 4 — Unbounded Knapsack** Use when: items can be used unlimited times Trigger words: coin change, rod cutting, integer break

```python
# Coin Change — min coins to make amount (unbounded: reuse coins)
def coinChange(coins, amount):
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0  # base case: 0 coins to make 0
    for i in range(1, amount + 1):
        for coin in coins:
            if coin <= i:
                dp[i] = min(dp[i], dp[i - coin] + 1)
    return dp[amount] if dp[amount] != float('inf') else -1
```

Key problems: Coin Change I & II, Rod Cutting, Integer Break, Perfect Squares

---

**Pattern 5 — Longest Common Subsequence (LCS) Family** Use when: two strings/arrays, matching/comparing characters Trigger words: LCS, edit distance, shortest common supersequence

```python
# LCS base template
def lcs(s1, s2):
    m, n = len(s1), len(s2)
    dp = [[0]*(n+1) for _ in range(m+1)]
    for i in range(1, m+1):
        for j in range(1, n+1):
            if s1[i-1] == s2[j-1]:
                dp[i][j] = 1 + dp[i-1][j-1]
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    return dp[m][n]

# Edit Distance (Levenshtein)
def editDistance(w1, w2):
    m, n = len(w1), len(w2)
    dp = [[0]*(n+1) for _ in range(m+1)]
    for i in range(m+1): dp[i][0] = i  # delete all chars of w1
    for j in range(n+1): dp[0][j] = j  # insert all chars of w2
    for i in range(1, m+1):
        for j in range(1, n+1):
            if w1[i-1] == w2[j-1]: dp[i][j] = dp[i-1][j-1]
            else: dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
    return dp[m][n]
```

Key problems: Longest Common Subsequence, Edit Distance, Shortest Common Supersequence, Longest Palindromic Subsequence

---

**Pattern 6 — LIS (Longest Increasing Subsequence)** Use when: find length of longest strictly increasing subsequence

```python
# O(n²) DP
def lis(nums):
    dp = [1] * len(nums)
    for i in range(1, len(nums)):
        for j in range(i):
            if nums[j] < nums[i]:
                dp[i] = max(dp[i], dp[j] + 1)
    return max(dp)

# O(n log n) using patience sorting + binary search
from bisect import bisect_left
def lis_fast(nums):
    tails = []
    for num in nums:
        pos = bisect_left(tails, num)
        if pos == len(tails): tails.append(num)
        else: tails[pos] = num
    return len(tails)
```

Key problems: Longest Increasing Subsequence, Number of LIS, Russian Doll Envelopes, Minimum Number of Removals

---

### ⚙️ Algorithms You Need

| Algorithm | Pattern | Complexity |
| --- | --- | --- |
| 1D DP | Linear dependency | O(n) time, O(1) space (optimized) |
| 2D DP | Grid/two-sequence problems | O(m×n) time and space |
| 0/1 Knapsack | Include/exclude each item once | O(n×W) time |
| Unbounded Knapsack | Reuse items, iterate forward | O(n×W) time |
| LCS / Edit Distance | Two-string matching | O(m×n) time |
| LIS (fast) | Increasing subsequence | O(n log n) with binary search |

---

### 🏆 Pro Tips

**Standard pro tips:**

- When stuck on a DP problem, always start by defining what `dp[i]` means in plain English. Write it as a comment. If you can't state it clearly, you don't have the solution yet.
- Optimize space AFTER you have the 2D solution working. Going from 2D to 1D is mechanical — just check which rows the recurrence actually uses.
- In 0/1 Knapsack space-optimized version, always iterate the capacity backwards (W down to weight\[i\]). Forgetting this turns it into unbounded knapsack.

**🔥 Untold tips from big tech engineers:**

- **Amazon SDE3:** "DP is the topic where I see the biggest gap between candidates who memorized solutions and candidates who actually understand. The question I ask: 'What does dp\[i\]\[j\] represent?' If they can answer that clearly and confidently, the rest of the solution follows naturally. If they can't, no amount of memorization helps. Define the state first. Always."
- **Google SDE2:** "Edit Distance is one of the most practically useful algorithms in existence. Google uses it in spell-check, typo correction, DNA sequence alignment. When you understand that the three choices at each mismatch (insert, delete, replace) map directly to three cells in the DP table (left, up, diagonal), you understand the algorithm at its core — not just the code."
- **Cred SDE:** "Coin Change is the gateway DP at fintech companies like us. We deal with denominations and amounts every day. The pattern — build up amounts from 0 to target, using the minimum coins — is exactly how payment routing optimization works. Understanding why we iterate forwards (unbounded, can reuse) vs backwards (bounded, each coin once) shows you understand state space, which is fundamental to financial modeling."
