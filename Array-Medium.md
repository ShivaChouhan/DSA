# DSA 
```bash
# Prompt

Give me these lecture complete notes with detailed explanation, which contain complete code of all brute force, average and optimal case code in java, make sure you provide the brute force, average and optimal case code of each question not only the explaination, also provide question sample before every solution, give me complete response in .md code
```

# Array Medium Lecture - 1


# Two Sum Problem: Brute, Better, and Optimal Solutions

The Two Sum problem is a fundamental interview question that comes in two main varieties.

## Problem Statement (Question Sample)

### Variety 1: Existence Check (Yes/No)

Given an array of integers (`arr`) and a target value (`Target`), determine if there exist two elements, A and B, in the array such that their sum equals the target ($$A + B = \text{Target}$$).

*   **Example:** If `Target = 14`, and the array contains `[2, 6, 5, 8, 11]`, the answer is **Yes** (since $$6 + 8 = 14$$).

### Variety 2: Index Return

Given an array of integers (`arr`) and a target value (`Target`), find and return the **indexes** of the two elements that sum up to the target. It is usually assumed that a solution is guaranteed to exist.

*   **Example:** If `Target = 14`, and the array is `[2, 6, 5, 8, 11]`, the elements are at index 1 (value 6) and index 3 (value 8). The answer is **[1, 3]** (or **[3, 1]**).

---

## Solution 1: Brute Force Approach

The **Brute Force** approach involves checking every possible pair of elements in the array to see if their sum equals the target.

### Explanation

1.  Use a nested loop structure. The outer loop (`i`) picks the first element.
2.  The inner loop (`j`) iterates through the remaining elements.
3.  For every pair $$(i, j)$$, check if $$arr[i] + arr[j] = \text{Target}$$.
4.  Ensure that the same element is not picked twice (i.e., $$i \neq j$$). A slight optimization is to start the inner loop from $$j = i + 1$$ to avoid checking pairs twice and checking the same element against itself.

### Time and Space Complexity

*   **Time Complexity:** $$O(N^2)$$ (N-squared) because for every element, you iterate through nearly all other elements.
*   **Space Complexity:** $$O(1)$$ (Constant space), as no extra data structures are used.

### Java Code (Variety 2: Returning Indexes)

```java
class Solution {
    public int[] twoSumBruteForce(int[] nums, int target) {
        int n = nums.length;
        
        // Outer loop picks the first element
        for (int i = 0; i < n; i++) {
            // Inner loop checks the current element against every other element
            for (int j = i + 1; j < n; j++) {
                // Check if the sum equals the target
                if (nums[i] + nums[j] == target) {
                    // Found the pair, return their indices
                    return new int[]{i, j};
                }
            }
        }
        
        // If no solution is found (for Variety 1, this would return 'No')
        return new int[]{-1, -1}; 
    }
}
```

---

## Solution 2: Better Approach (Hashing)

The **Better Approach** uses a Hash Map (or Hash Table) to reduce the time complexity by allowing for $$O(1)$$ lookups for the required complement element.

### Explanation

1.  Initialize a Hash Map to store `(Element, Index)` pairs.
2.  Iterate through the array once. For the current element, `arr[i]`, calculate the required complement, which is `More = Target - arr[i]`.
3.  Check if `More` already exists as a key in the Hash Map.
    *   If **Yes**, a solution is found. The required element is `More` (whose index is stored in the map), and the current element is `arr[i]` (at index `i`). Return the two indexes.
    *   If **No**, store the current element and its index into the map: `map.put(arr[i], i)`.

### Time and Space Complexity

*   **Time Complexity:** $$O(N)$$ (Linear time) in the best and average case, assuming the Hash Map operations (insertion and lookup) take $$O(1)$$ time.
*   **Space Complexity:** $$O(N)$$ to store all elements and their indices in the Hash Map.

### Java Code (Variety 2: Returning Indexes)

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int[] twoSumHashing(int[] nums, int target) {
        // Map stores (Element, Index)
        Map<Integer, Integer> map = new HashMap<>();
        
        for (int i = 0; i < nums.length; i++) {
            int currentNum = nums[i];
            
            // Calculate the required complement
            int moreRequired = target - currentNum;
            
            // Check if the complement exists in the map
            if (map.containsKey(moreRequired)) {
                // Found the pair: return the stored index and the current index
                int index1 = map.get(moreRequired);
                int index2 = i;
                return new int[]{index1, index2};
            }
            
            // If not found, store the current element and its index for future lookups
            map.put(currentNum, i);
        }
        
        // Should not be reached if a solution is guaranteed
        return new int[]{-1, -1}; 
    }
}
```

---

## Solution 3: Optimal Approach (Two-Pointer)

The **Two-Pointer Approach** is an efficient solution for **Variety 1 (Yes/No)**, especially when the use of extra space (like a map) is restricted. It relies on the array being sorted.

### Explanation

1.  **Sort the Array:** The array must first be sorted. This step is crucial for the greedy two-pointer logic to work.
2.  **Initialize Pointers:** Set a `Left` pointer to the start (index 0) and a `Right` pointer to the end (index $$n-1$$) of the sorted array.
3.  **Iterate and Adjust:** While the `Left` pointer is less than the `Right` pointer:
    *   Calculate the `Sum = arr[Left] + arr[Right]`.
    *   If `Sum == Target`, a solution is found. Return **Yes**.
    *   If `Sum < Target`, the sum needs to be increased. Move the `Left` pointer one step to the right (`Left++`) to include a larger number in the sum.
    *   If `Sum > Target`, the sum needs to be decreased. Move the `Right` pointer one step to the left (`Right--`) to include a smaller number in the sum.

**Note on Variety 2:** This approach is **not optimal** for Variety 2 (returning indexes) because sorting the array destroys the original index information. To preserve indexes, you would need to store the original value and index in a separate data structure before sorting, which adds complexity and space overhead.

### Time and Space Complexity

*   **Time Complexity:** $$O(N \log N)$$ (Dominated by the initial sorting step) + $$O(N)$$ (for the two-pointer traversal) = **$$O(N \log N)$$**.
*   **Space Complexity:** $$O(1)$$ (if sorting is done in-place) or $$O(N)$$ (if the sorting algorithm requires auxiliary space, or if the interviewer considers the space used for modifying the array).

### Java Code (Variety 1: Returning Boolean)

```java
import java.util.Arrays;

class Solution {
    public boolean twoSumTwoPointer(int[] nums, int target) {
        // 1. Sort the array
        Arrays.sort(nums);
        
        int left = 0;
        int right = nums.length - 1;
        
        // 2. Use two pointers
        while (left < right) {
            int sum = nums[left] + nums[right];
            
            if (sum == target) {
                // Found the pair
                return true;
            } else if (sum < target) {
                // Sum is too small, increase the left pointer
                left++;
            } else {
                // Sum is too large, decrease the right pointer
                right--;
            }
        }
        
        // If the pointers cross and no pair is found
        return false;
    }
}
```
# Array Medium Lecture - 2

## Lecture Notes: Sort an Array of 0's, 1's, and 2's

## Problem Statement

**Question Sample:**
Given an array `A` consisting of only 0s, 1s, and 2s, sort the array in place.

**Example:**
Input: `A = [2, 0, 1, 2, 1, 0]`
Output: `A = [0, 0, 1, 1, 2, 2]`

---

## 1. Brute Force Solution

The **Brute Force** approach involves using any standard sorting algorithm to sort the array.

### Explanation

A straightforward way to sort the array is to use a comparison-based sorting algorithm, such as **Merge Sort** or **Quick Sort**. While this solves the problem, it is not the most efficient solution for this specific constraint (only 0s, 1s, and 2s) and will likely be rejected by an interviewer seeking an optimized solution.

### Complexity Analysis

*   **Time Complexity:** $$O(N \log N)$$ (e.g., using Merge Sort)
*   **Space Complexity:** $$O(N)$$ (for the temporary array used in Merge Sort)

### Java Code

```java
import java.util.Arrays;

class BruteForceSolution {
    /**
     * Sorts the array using the built-in sorting function (typically Timsort/MergeSort).
     * Time Complexity: O(N log N)
     * Space Complexity: O(N)
     */
    public static void sortArray(int[] arr) {
        // Using Java's built-in sort function
        Arrays.sort(arr);
    }

    public static void main(String[] args) {
        int[] arr = {2, 0, 1, 2, 1, 0, 2, 1, 0, 1, 2, 0};
        sortArray(arr);
        System.out.println(Arrays.toString(arr)); // Output: [0, 0, 0, 0, 1, 1, 1, 1, 2, 2, 2, 2]
    }
}
```

---

## 2. Better Solution (Counting)

The **Better Solution** leverages the fact that the array only contains three distinct elements (0, 1, 2) to achieve a linear time complexity.

### Explanation

1.  **Counting:** Iterate through the array once and maintain three counters: `count0`, `count1`, and `count2` to store the frequency of each number.
2.  **Overwriting:** After counting, run separate loops to overwrite the original array:
    *   Fill the first `count0` positions with 0s.
    *   Fill the next `count1` positions with 1s.
    *   Fill the remaining `count2` positions with 2s.

### Complexity Analysis

*   **Time Complexity:** $$O(N) + O(N) = O(N)$$ (One pass for counting, one pass for overwriting)
*   **Space Complexity:** $$O(1)$$ (Only three integer variables are used for counting)

### Java Code

```java
import java.util.Arrays;

class BetterSolution {
    /**
     * Sorts the array by counting the occurrences of 0s, 1s, and 2s.
     * Time Complexity: O(N)
     * Space Complexity: O(1)
     */
    public static void sortArray(int[] arr) {
        int count0 = 0;
        int count1 = 0;
        int count2 = 0;

        // Step 1: Count the occurrences
        for (int num: arr) {
            if (num == 0) {
                count0++;
            } else if (num == 1) {
                count1++;
            } else {
                count2++;
            }
        }

        // Step 2: Overwrite the array
        int i = 0;
        
        // Fill 0s
        for (int j = 0; j < count0; j++) {
            arr[i++] = 0;
        }
        
        // Fill 1s
        for (int j = 0; j < count1; j++) {
            arr[i++] = 1;
        }
        
        // Fill 2s
        for (int j = 0; j < count2; j++) {
            arr[i++] = 2;
        }
    }

    public static void main(String[] args) {
        int[] arr = {2, 0, 1, 2, 1, 0, 2, 1, 0, 1, 2, 0};
        sortArray(arr);
        System.out.println(Arrays.toString(arr)); // Output: [0, 0, 0, 0, 1, 1, 1, 1, 2, 2, 2, 2]
    }
}
```

---

## 3. Optimal Solution (Dutch National Flag Algorithm)

The **Optimal Solution** uses a single pass with three pointers, known as the **Dutch National Flag Algorithm**, to sort the array in place without using extra space or multiple iterations over the array elements.

### Explanation

This algorithm uses three pointers: `low`, `mid`, and `high`. It maintains three sorted sections and one unsorted section:

*   **0 to `low - 1`**: Contains all **0s** (Sorted)
*   **`low` to `mid - 1`**: Contains all **1s** (Sorted)
*   **`high + 1` to `N - 1`**: Contains all **2s** (Sorted)
*   **`mid` to `high`**: The **unsorted** section being processed   

The algorithm iterates while `mid <= high`, checking the element at `arr[mid]` and applying one of three rules:

1.  **If `arr[mid]` is 0:**
    *   Swap `arr[low]` and `arr[mid]` to move the 0 to the left section.
    *   Increment both `low` and `mid`.
2.  **If `arr[mid]` is 1:**
    *   Increment `mid` only, as the 1 is already in its correct section (between `low` and `high`).
3.  **If `arr[mid]` is 2:**
    *   Swap `arr[mid]` and `arr[high]` to move the 2 to the right section.
    *   Decrement `high` only. **Do not increment `mid`** because the element swapped from `high` into `mid`'s position is unknown (it could be 0, 1, or 2) and must be checked in the next iteration.

1. if nums[mid]==0 , swap(nums[low],nums[mid]), low++, mid++
2. if nums[mid]==1 , mid++
3. if nums[mid]==2 , swap(nums[mid],nums[high]), high--



### Complexity Analysis

*   **Time Complexity:** $$O(N)$$ (Single iteration, as one element is sorted in each of the three steps)  
*   **Space Complexity:** $$O(1)$$ (Only three pointers are used)

### Java Code

```java
import java.util.Arrays;

class OptimalSolution {
    /**
     * Sorts the array using the Dutch National Flag Algorithm (3-pointer approach).
     * Time Complexity: O(N)
     * Space Complexity: O(1)
     */
    public static void sortArray(int[] arr) {
        int low = 0;
        int mid = 0;
        int high = arr.length - 1;

        while (mid <= high) {
            if (arr[mid] == 0) {
                // Case 1: arr[mid] is 0
                swap(arr, low, mid);
                low++;
                mid++;
            } else if (arr[mid] == 1) {
                // Case 2: arr[mid] is 1
                mid++;
            } else {
                // Case 3: arr[mid] is 2
                swap(arr, mid, high);
                high--;
                // Note: mid is NOT incremented because the swapped element at arr[mid]
                // needs to be checked in the next iteration.
            }
        }
    }

    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    public static void main(String[] args) {
        int[] arr = {2, 0, 1, 2, 1, 0, 2, 1, 0, 1, 2, 0};
        sortArray(arr);
        System.out.println(Arrays.toString(arr)); // Output: [0, 0, 0, 0, 1, 1, 1, 1, 2, 2, 2, 2]
    }
}
```
# Array Medium Lecture - 3

## Majority Element Problem Notes

The **Majority Element** problem requires you to find the element in a given array of integers that appears **more than** $$n/2$$ times, where $$n$$ is the length of the array.

*   **Example:** If the array length ($$n$$) is 7, $$n/2$$ is 3.5. The majority element must appear more than 3.5 times, meaning 4 or more times.

---

## 1. Brute Force Solution

### Question Sample

Given the array `V = [2, 2, 3, 3, 1, 2, 2]`, find the majority element. (Here, $$n=7$$, so the majority element must appear > 3 times).

### Detailed Explanation

The **Brute Force** approach involves checking every element in the array to see if it is the majority element.

1.  Pick an element from the array (using an outer loop).
2.  Scan through the entire array (using an inner loop) to count how many times that element appears.
3.  If the count for that element is greater than $$n/2$$, it is the majority element, and you return it.

This method uses nested loops, leading to a high time complexity.

### Complexity Analysis

*   **Time Complexity:** $$O(n^2)$$ (due to the nested loops).
*   **Space Complexity:** $$O(1)$$ (no extra space is used).

### Complete Java Code (Brute Force)

```java
/**
 * Brute Force Solution for Majority Element
 * Time Complexity: O(n^2)
 */
public class MajorityElement {
    public int majorityElementBrute(int[] v) {
        int n = v.length;
        
        // Outer loop: iterates through each element to check
        for (int i = 0; i < n; i++) {
            int count = 0;
            
            // Inner loop: counts the occurrences of v[i]
            for (int j = 0; j < n; j++) {
                if (v[j] == v[i]) {
                    count++;
                }
            }
            
            // Check if the count is greater than n/2
            if (count > n / 2) {
                return v[i];
            }
        }
        
        // Return -1 if no majority element is found
        return -1; 
    }
}
```

---

## 2. Better Solution (Hashing/Map)

### Question Sample

Given the array `V = [2, 2, 3, 3, 1, 2, 2]`, find the majority element. (Here, $$n=7$$, so the majority element must appear > 3 times).

### Detailed Explanation

The **Better Solution** uses a hash map (or frequency map) to efficiently track the count of every element, which is the natural technique when counting occurrences is required.

1.  Declare a hash map where the **element** is the key and its **count** is the value.
2.  Iterate through the input array once, updating the count for each element in the map.
3.  After the first iteration is complete, iterate through the map.
4.  Check if any element's count (the value) is greater than $$n/2$$; if so, return that element (the key) as the answer.

### Complexity Analysis

*   **Time Complexity:** $$O(n)$$ or $$O(n \log n)$$
    *   If an **unordered map** (like Java's `HashMap`) is used, the average time complexity is $$O(n)$$ (one pass for insertion, one pass for map traversal).
    *   If an **ordered map** is used, the time complexity is $$O(n \log n)$$.
*   **Space Complexity:** $$O(n)$$ in the worst case, as the map might store all $$n$$ elements if they are all unique.

### Complete Java Code (Better/Hashing)

```java
import java.util.HashMap;
import java.util.Map;

/**
 * Better Solution for Majority Element using Hashing
 * Time Complexity: O(n) on average (using HashMap)
 * Space Complexity: O(n)
 */
public class MajorityElement {
    public int majorityElementBetter(int[] v) {
        int n = v.length;
        
        // 1. Declare a HashMap to store element counts
        Map<Integer, Integer> map = new HashMap<>();
        
        // 2. Populate the map with counts
        for (int element: v) {
            map.put(element, map.getOrDefault(element, 0) + 1);
        }
        
        // 3. Iterate through the map to find the majority element
        for (Map.Entry<Integer, Integer> entry: map.entrySet()) {
            // Check if the count (value) is greater than n/2
            if (entry.getValue() > n / 2) {
                return entry.getKey();
            }
        }
        
        return -1; // No majority element found
    }
}
```

---

## 3. Optimal Solution (Moore's Voting Algorithm)

### Question Sample

Given the array `V = [5, 5, 7, 7, 5, 7, 7, 5, 5, 5, 7, 7, 5, 5, 5, 5]`, find the majority element. (Here, $$n=16$$, so the majority element must appear > 8 times).

### Detailed Explanation

The **Optimal Solution** uses **Moore's Voting Algorithm**, which is based on the intuition that if an element appears more than $$n/2$$ times, it is impossible for all the other elements combined to cancel it out completely.

The algorithm involves two main processes:

### Process 1: Finding the Potential Majority Element

This process uses two variables: `element` (the current candidate) and `count` (the dominance tracker).

1.  Initialize `count` to 0.
2.  Iterate through the array:
    *   If `count` is 0, it means the current section of the array has been "canceled out," so you start a new check. Set `count` to 1 and set the current element as the new `element` candidate.
    *   If the current element is equal to the stored `element`, increment `count` (the candidate is dominating).
    *   If the current element is different, decrement `count` (the candidate is being canceled out).

The element remaining after the entire iteration is the potential majority element, *if one exists*.

### Process 2: Verification (Conditional)

If the problem guarantees that a majority element always exists, this step can be skipped. If not, verification is required:

1.  Iterate through the array one more time.
2.  Count the actual occurrences of the `element` found in Process 1.
3.  If the actual count is greater than $$n/2$$, return the element; otherwise, return -1 (or an indicator that no majority element exists).

### Complexity Analysis

*   **Time Complexity:** $$O(n)$$ (One pass for the voting algorithm and one optional pass for verification).
*   **Space Complexity:** $$O(1)$$ (Only constant extra space is used for `element` and `count`).

### Complete Java Code (Optimal/Moore's Voting Algorithm)

```java
/**
 * Optimal Solution for Majority Element using Moore's Voting Algorithm
 * Time Complexity: O(n)
 * Space Complexity: O(1)
 */
public class MajorityElement {
    public int majorityElementOptimal(int[] v) {
        int n = v.length;
        int element = 0; // Stores the potential majority element
        int count = 0;   // Tracks the count/dominance
        
        // Process 1: Moore's Voting Algorithm
        for (int i = 0; i < n; i++) {
            if (count == 0) {
                // Start a new check
                count = 1;
                element = v[i];
            } else if (v[i] == element) {
                // Same element, increase dominance
                count++;
            } else {
                // Different element, decrease dominance
                count--;
            }
        }
        
        // Process 2: Verification (Required if majority element is not guaranteed)
        int counter1 = 0;
        for (int i = 0; i < n; i++) {
            if (v[i] == element) {
                counter1++;
            }
        }
        
        if (counter1 > n / 2) {
            return element;
        }
        
        return -1; // No majority element found
    }
}
```

# Array Medium Lecture - 4

---

## Maximum Subarray Sum Problem

The problem asks to find the **maximum sum** possible from any **subarray** within a given array of integers. A subarray is defined as a **contiguous part** of the original array.

**Sample Question/Input Array:**

Given the array: `arr = [-2, -3, 4, -1, -2, 1, 5, -3]`

The maximum subarray sum is **7**, corresponding to the subarray `[4, -1, -2, 1, 5]`.

---

## 1. Brute Force Solution (Time Complexity: O(N³))

### Explanation

The Brute Force approach involves iterating through all possible subarrays and calculating the sum for each one to find the maximum sum overall.

This requires three nested loops:
1.  **Outer Loop (i):** Selects the **starting index** of the subarray (from 0 to N-1).
2.  **Middle Loop (j):** Selects the **ending index** of the subarray (from `i` to N-1).
3.  **Inner Loop (k):** Calculates the **sum** of all elements from index `i` to index `j`.

The time complexity is approximately **O(N³)** due to the three nested loops, and the space complexity is **O(1)**.

### Java Code (Brute Force)

```java
class SolutionBruteForce {
    public static long maxSubarraySum(int[] arr, int n) {
        // Initialize maximum sum to the lowest possible value (Long.MIN_VALUE for safety)
        long maximumSum = Long.MIN_VALUE; 
        
        // Loop 1: Starting index 'i'
        for (int i = 0; i < n; i++) {
            
            // Loop 2: Ending index 'j'
            for (int j = i; j < n; j++) {
                
                // Loop 3: Calculate sum of subarray from i to j
                long currentSum = 0;
                for (int k = i; k <= j; k++) {
                    currentSum += arr[k];
                }
                
                // Update the overall maximum sum
                maximumSum = Math.max(maximumSum, currentSum);
            }
        }
        
        // Handle the case where the problem allows an empty subarray (sum 0)
        // If maximumSum is still negative, return 0, otherwise return maximumSum
        if (maximumSum < 0) {
            return 0;
        }
        
        return maximumSum;
    }

    public static void main(String[] args) {
        int[] arr = {-2, -3, 4, -1, -2, 1, 5, -3};
        int n = arr.length;
        long result = maxSubarraySum(arr, n);
        System.out.println("Brute Force Max Subarray Sum: " + result); // Output: 7
    }
}
```

---

## 2. Better Solution (Time Complexity: O(N²))

### Explanation

The Better approach optimizes the sum calculation by eliminating the innermost loop (`k`). Instead of recalculating the sum for every subarray, we maintain a **running sum** as the ending index `j` moves forward.

1.  **Outer Loop (i):** Selects the starting index.
2.  **Inner Loop (j):** Selects the ending index and simultaneously calculates the sum.
    -   When `j` increments, the new `currentSum` is simply the previous `currentSum` plus `arr[j]`.

This reduces the time complexity to **O(N²)**, and the space complexity remains **O(1)**.

### Java Code (Better)

```java
class SolutionBetter {
    public static long maxSubarraySum(int[] arr, int n) {
        long maximumSum = Long.MIN_VALUE; 
        
        // Loop 1: Starting index 'i'
        for (int i = 0; i < n; i++) {
            long currentSum = 0;
            
            // Loop 2: Ending index 'j' and calculating sum simultaneously
            for (int j = i; j < n; j++) {
                currentSum += arr[j]; // Add the next element to the running sum
                
                // Update the overall maximum sum
                maximumSum = Math.max(maximumSum, currentSum);
            }
        }
        
        // Handle the case for empty subarray (sum 0)
        if (maximumSum < 0) {
            return 0;
        }
        
        return maximumSum;
    }

    public static void main(String[] args) {
        int[] arr = {-2, -3, 4, -1, -2, 1, 5, -3};
        int n = arr.length;
        long result = maxSubarraySum(arr, n);
        System.out.println("Better Solution Max Subarray Sum: " + result); // Output: 7
    }
}
```

---

## 3. Optimal Solution: Kadane's Algorithm (Time Complexity: O(N))

### Explanation (Finding Maximum Sum)

Kadane's Algorithm is the most optimal solution, solving the problem in a single pass (O(N)).

The core intuition is that if the **current running sum** becomes **negative**, it is never beneficial to carry it forward, as it will only reduce the sum of any future subarray.

**Algorithm Steps:**

1.  Initialize `maximumSum` to the lowest possible value and `currentSum` to 0.
2.  Iterate through the array:
    -   Add the current element to `currentSum`.
    -   Update `maximumSum` if `currentSum` is greater than `maximumSum`.
    -   If `currentSum` becomes less than 0, **reset** `currentSum` to 0, effectively starting a new subarray from the next element.
3.  After the loop, if the problem requires returning 0 for an empty subarray (i.e., if all elements are negative), check if `maximumSum` is less than 0 and return 0 instead.

### Java Code (Finding Maximum Sum)

```java
class KadanesAlgorithmMaxSum {
    public static long maxSubarraySum(int[] arr, int n) {
        // Use long for sum and max for safety, initialized to Long.MIN_VALUE
        long currentSum = 0;
        long maximumSum = Long.MIN_VALUE; 

        for (int i = 0; i < n; i++) {
            // 1. Add current element to the running sum
            currentSum += arr[i];

            // 2. Update maximum sum found so far
            if (currentSum > maximumSum) {
                maximumSum = currentSum;
            }

            // 3. If currentSum is negative, reset it to 0
            // Carrying a negative sum forward will only reduce the total sum.
            if (currentSum < 0) {
                currentSum = 0;
            }
        }

        // Special case: If all elements are negative, the max sum is the largest negative number.
        // If the problem allows an empty subarray (sum 0), handle it here:
        if (maximumSum < 0) {
            return 0; // Return 0 for an empty subarray
        }

        return maximumSum;
    }

    public static void main(String[] args) {
        int[] arr = {-2, -3, 4, -1, -2, 1, 5, -3};
        int n = arr.length;
        long result = maxSubarraySum(arr, n);
        System.out.println("Kadane's Max Subarray Sum: " + result); // Output: 7
    }
}
```

---

## 4. Optimal Solution: Finding and Printing the Subarray

### Explanation (Tracking Indices)

To print the actual subarray, we need to track the start and end indices of the subarray that yields the `maximumSum`.

1.  We introduce three index variables: `start` (current subarray start), `ansStart` (final answer start index), and `ansEnd` (final answer end index).
2.  The **start of a new subarray** is always marked when `currentSum` is reset to 0, meaning the next element (`i`) is the potential new beginning.
3.  When a new `maximumSum` is found (`currentSum > maximumSum`), we update `ansStart` to the current `start` index and `ansEnd` to the current index `i`.

### Java Code (Finding and Printing)

```java
class KadanesAlgorithmPrintSubarray {
    public static void maxSubarraySumAndPrint(int[] arr, int n) {
        long currentSum = 0;
        long maximumSum = Long.MIN_VALUE; 

        // Variables to track indices
        int start = 0; // Tracks the start of the current subarray
        int ansStart = -1; // Final start index
        int ansEnd = -1; // Final end index

        for (int i = 0; i < n; i++) {
            
            // Logic to track the start of a new subarray
            if (currentSum == 0) {
                start = i; // New subarray starts here
            }

            currentSum += arr[i];

            // Update maximum sum and indices
            if (currentSum > maximumSum) {
                maximumSum = currentSum;
                ansStart = start;
                ansEnd = i;
            }

            // Reset currentSum if negative
            if (currentSum < 0) {
                currentSum = 0;
            }
        }

        System.out.println("Kadane's Max Subarray Sum: " + maximumSum);
        
        // Print the subarray
        if (ansStart!= -1 && ansEnd!= -1) {
            System.out.print("The Maximum Subarray is: [");
            for (int i = ansStart; i <= ansEnd; i++) {
                System.out.print(arr[i]);
                if (i < ansEnd) {
                    System.out.print(", ");
                }
            }
            System.out.println("]");
        } else {
            System.out.println("The array contains only negative numbers or is empty.");
        }
    }

    public static void main(String[] args) {
        int[] arr = {-2, -3, 4, -1, -2, 1, 5, -3};
        int n = arr.length;
        maxSubarraySumAndPrint(arr, n); 
        // Output:
        // Kadane's Max Subarray Sum: 7
        // The Maximum Subarray is: [4, -1, -2, 1, 5]
    }
}
```

# Array Medium Lecture - 5

---

## DP on Stocks Problem 1: Best Time to Buy and Sell Stock

### Question Sample

You are given an array `prices` where `prices[i]` is the price of a given stock on the $i^{th}$ day. You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock. You are only allowed to complete at most one transaction (i.e., buy once and sell once). Return the maximum profit you can achieve. If you cannot achieve any profit, return 0.

**Example:**
Input: `prices = [7, 1, 5, 3, 6, 4]`
Output: 5 (Buy on day 2 (price 1) and sell on day 5 (price 6). Profit = 6 - 1 = 5.)

---

### 1. Brute Force Approach

The **Brute Force** approach checks every possible pair of buying and selling days.

#### Explanation
We use nested loops. The outer loop iterates through every possible **buying day** ($i$), and the inner loop iterates through every possible **selling day** ($j$), ensuring that the selling day is *after* the buying day ($j > i$). We calculate the profit for each pair and keep track of the maximum profit found.

#### Time Complexity
$$O(N^2)$$

#### Java Code

```java
class Solution {
    /**
     * Brute Force Approach: Check every possible buy/sell pair.
     * Time Complexity: O(N^2)
     */
    public int maxProfitBruteForce(int[] prices) {
        int maxProfit = 0;
        int n = prices.length;

        // i is the buying day
        for (int i = 0; i < n; i++) {
            // j is the selling day (must be after i)
            for (int j = i + 1; j < n; j++) {
                int profit = prices[j] - prices[i];
                if (profit > maxProfit) {
                    maxProfit = profit;
                }
            }
        }
        return maxProfit;
    }
}
```

---

### 2. Optimal Case Approach (Greedy/Constructive Algorithm)

The **Optimal Case** uses a single pass to solve the problem efficiently. The lecture notes confirm this approach is the expected solution, often categorized as a constructive algorithm where you "remember the past" (the minimum price) to calculate the current maximum profit.

#### Explanation
We iterate through the prices array just once, maintaining two variables:
1.  **`minPrice`**: Tracks the **minimum stock price** encountered so far from day 0 up to the current day ($i-1$).
2.  **`maxProfit`**: Tracks the **maximum profit** calculated so far.

At each day $i$, we calculate the potential profit by subtracting the `minPrice` from the current price (`prices[i]`). We then update `maxProfit` if the current profit is higher. Finally, we update `minPrice` to be the minimum of its current value and the current day's price, ensuring that for the next day, we have the absolute minimum buying price available on the left side. If the profit is negative, we treat it as 0, as we are not interested in making losses (or we simply don't buy and sell, resulting in a profit of 0).

#### Time Complexity
$$O(N)$$

#### Space Complexity
$$O(1)$$

#### Java Code

```java
class Solution {
    /**
     * Optimal Approach (Greedy/Single Pass): Track minimum price and maximum profit.
     * Time Complexity: O(N)
     * Space Complexity: O(1)
     */
    public int maxProfitOptimal(int[] prices) {
        // Initialize minPrice to the first day's price
        int minPrice = prices[0]; 
        
        // Initialize maxProfit to 0 (we don't want negative profit)
        int maxProfit = 0; 

        // Start loop from the second day (index 1)
        for (int i = 1; i < prices.length; i++) {
            
            // 1. Calculate the potential profit if we sell today
            // Current price - minimum price seen so far
            int currentProfit = prices[i] - minPrice;
            
            // 2. Update the overall maximum profit
            // We are maximizing the profit throughout the iteration
            maxProfit = Math.max(maxProfit, currentProfit);
            
            // 3. Update the minimum price seen so far
            // This is the "remembering the past" step mentioned in the lecture
            minPrice = Math.min(minPrice, prices[i]);
        }

        return maxProfit;
    }
}
```
# Array Medium Lecture - 6 

## Rearrange Array Elements by Sign - Complete Lecture Notes

## 📋 Problem Statement

**Problem:** Given an array containing positive and negative integers in random order, rearrange the array such that positive and negative numbers appear alternately. There are two varieties of this problem:

### Variety 1: Equal Number of Positive and Negative Elements
- Array length `n` is always even
- Exactly `n/2` positive numbers and `n/2` negative numbers
- Must maintain relative order of positives and negatives
- Output: `+ - + - + -` pattern

### Variety 2: Unequal Number of Positive and Negative Elements
- Number of positives may not equal number of negatives
- Arrange alternately as much as possible
- Remaining elements (of the majority sign) should be added at the end in their relative order

---

## 🔍 Sample Input/Output

### Variety 1 Sample
**Input:** `nums = [3, 1, -2, -5, 2, -4]`
- Positives: `[3, 1, 2]` (in order)
- Negatives: `[-2, -5, -4]` (in order)

**Expected Output:** `[3, -2, 1, -5, 2, -4]`
- Pattern: `+ - + - + -`
- Relative order maintained

### Variety 2 Sample
**Input:** `nums = [2, 3, -1, -3, 4]`
- Positives: `[2, 3, 4]` (3 positives)
- Negatives: `[-1, -3]` (2 negatives)

**Expected Output:** `[2, -1, 3, -3, 4]`
- First 4 elements alternate: `+ - + -`
- Remaining positive `4` added at end

---

## 🎯 Variety 1: Equal Positives and Negatives

### Constraints
- `1 ≤ n ≤ 10^5` (n is even)
- `-10^9 ≤ nums[i] ≤ 10^9`
- Exactly `n/2` positive and `n/2` negative numbers
- No zeros in array

### Approach 1: Brute Force (Two Passes + Extra Space)

#### Algorithm
1. **Separate Elements:** Create two arrays/lists:
   - `positives`: Store all positive numbers in their relative order
   - `negatives`: Store all negative numbers in their relative order
2. **Reconstruct Array:** 
   - Place positives at even indices: `arr[2*i] = positives[i]`
   - Place negatives at odd indices: `arr[2*i+1] = negatives[i]`
3. **Time Complexity:** O(n) - Two passes through array
4. **Space Complexity:** O(n) - Two extra arrays of size n/2 each

#### Java Code - Brute Force
```java
import java.util.*;

public class RearrangeBySign {
    
    public static int[] rearrangeBySignBrute(int[] nums) {
        int n = nums.length;
        List<Integer> positives = new ArrayList<>();
        List<Integer> negatives = new ArrayList<>();
        
        // Step 1: Separate positives and negatives (O(n))
        for (int num: nums) {
            if (num > 0) {
                positives.add(num);
            } else {
                negatives.add(num);
            }
        }
        
        // Step 2: Reconstruct array with alternating pattern (O(n))
        int[] result = new int[n];
        for (int i = 0; i < n/2; i++) {
            result[2 * i] = positives.get(i);        // Even indices for positives
            result[2 * i + 1] = negatives.get(i);    // Odd indices for negatives
        }
        
        return result;
    }
    
    // Test the brute force solution
    public static void main(String[] args) {
        int[] nums = {3, 1, -2, -5, 2, -4};
        int[] result = rearrangeBySignBrute(nums);
        
        System.out.println("Input: " + Arrays.toString(nums));
        System.out.println("Output: " + Arrays.toString(result));
        // Output: [3, -2, 1, -5, 2, -4]
    }
}
```

#### Detailed Explanation
- **First Pass:** We iterate through the original array once, collecting all positive numbers in `positives` list and all negative numbers in `negatives` list. Since we know there are exactly `n/2` of each, both lists will have size `n/2`.
- **Second Pass:** We create a result array of size `n`. For each index `i` from 0 to `n/2 - 1`:
  - Place `positives[i]` at position `2*i` (even index)
  - Place `negatives[i]` at position `2*i + 1` (odd index)
- **Why This Works:** Even indices (0, 2, 4,...) get positives, odd indices (1, 3, 5,...) get negatives, creating the `+ - + -` pattern.
- **Relative Order:** Since we add elements to lists in the order they appear in the original array, the relative order is automatically maintained.

---

### Approach 2: Optimal (Single Pass + Minimal Extra Space)

#### Algorithm
1. **Initialize Pointers:** 
   - `posIndex = 0` (first even index for positives)
   - `negIndex = 1` (first odd index for negatives)
2. **Single Pass:** Iterate through original array:
   - If current number is positive, place it at `posIndex` and increment by 2
   - If current number is negative, place it at `negIndex` and increment by 2
3. **Return Result Array**

#### Java Code - Optimal
```java
import java.util.*;

public class RearrangeBySign {
    
    public static int[] rearrangeBySignOptimal(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        int posIndex = 0;  // Start at first even index
        int negIndex = 1;  // Start at first odd index
        
        // Single pass through the array
        for (int i = 0; i < n; i++) {
            if (nums[i] > 0) {
                // Place positive at even index
                result[posIndex] = nums[i];
                posIndex += 2;  // Move to next even index
            } else {
                // Place negative at odd index
                result[negIndex] = nums[i];
                negIndex += 2;  // Move to next odd index
            }
        }
        
        return result;
    }
    
    // Test the optimal solution
    public static void main(String[] args) {
        int[] nums = {3, 1, -2, -5, 2, -4};
        int[] result = rearrangeBySignOptimal(nums);
        
        System.out.println("Input: " + Arrays.toString(nums));
        System.out.println("Output: " + Arrays.toString(result));
        // Output: [3, -2, 1, -5, 2, -4]
    }
}
```

#### Detailed Explanation
- **Key Insight:** We know positives go to even indices (0, 2, 4,...) and negatives go to odd indices (1, 3, 5,...). Instead of collecting all elements first, we can place them directly in their final positions as we encounter them.
- **Pointer Movement:** 
  - `posIndex` starts at 0 and jumps by 2 each time (0 → 2 → 4 →...)
  - `negIndex` starts at 1 and jumps by 2 each time (1 → 3 → 5 →...)
- **Single Pass Efficiency:** We process each element exactly once, placing it directly in the result array at its correct position.
- **Space Optimization:** We only use O(1) extra space for the pointers (excluding the result array, which is required for output).
- **Time Complexity:** O(n) - Single pass through the array
- **Space Complexity:** O(1) auxiliary space + O(n) for result array

#### Why Optimal is Better
| Aspect | Brute Force | Optimal |
|--------|-------------|---------|
| **Time Complexity** | O(n) | O(n) |
| **Space Complexity** | O(n) | O(1) auxiliary |
| **Passes** | 2 passes | 1 pass |
| **Extra Data Structures** | 2 ArrayLists | None |
| **Memory Usage** | Higher | Lower |

---

## 🎯 Variety 2: Unequal Positives and Negatives

### Constraints
- `1 ≤ n ≤ 10^5`
- `-10^9 ≤ nums[i] ≤ 10^9`
- Number of positives ≠ number of negatives (may be more or less)
- Remaining elements should be added at end in relative order

### Approach: Modified Brute Force (Handle Imbalance)

#### Algorithm
1. **Separate Elements:** Collect positives and negatives in their relative order
2. **Determine Limiting Factor:** Find the minimum of (positives.size(), negatives.size())
3. **Fill Alternating Pattern:** 
   - For first `2 * min(pos, neg)` positions, alternate as much as possible
   - Place positives at even indices, negatives at odd indices
4. **Handle Remaining Elements:** 
   - If more positives: Add remaining positives at end
   - If more negatives: Add remaining negatives at end

#### Java Code - Variety 2 Solution
```java
import java.util.*;

public class RearrangeBySignVariety2 {
    
    public static int[] rearrangeBySign(int[] nums) {
        int n = nums.length;
        List<Integer> positives = new ArrayList<>();
        List<Integer> negatives = new ArrayList<>();
        
        // Step 1: Separate positives and negatives in relative order
        for (int num: nums) {
            if (num > 0) {
                positives.add(num);
            } else {
                negatives.add(num);
            }
        }
        
        // Step 2: Create result array
        int[] result = new int[n];
        int posIdx = 0, negIdx = 0, resultIdx = 0;
        
        // Step 3: Fill alternating pattern for min(positives, negatives) pairs
        int minCount = Math.min(positives.size(), negatives.size());
        
        // Case 1: More positives than negatives
        if (positives.size() > negatives.size()) {
            // Fill alternating pattern for all negatives
            for (int i = 0; i < minCount; i++) {
                result[resultIdx++] = positives.get(posIdx++);  // Positive at even index
                result[resultIdx++] = negatives.get(negIdx++);   // Negative at odd index
            }
            
            // Add remaining positives
            while (posIdx < positives.size()) {
                result[resultIdx++] = positives.get(posIdx++);
            }
        }
        // Case 2: More negatives than positives
        else if (negatives.size() > positives.size()) {
            // Fill alternating pattern for all positives
            for (int i = 0; i < minCount; i++) {
                result[resultIdx++] = positives.get(posIdx++);  // Positive at even index
                result[resultIdx++] = negatives.get(negIdx++);  // Negative at odd index
            }
            
            // Add remaining negatives
            while (negIdx < negatives.size()) {
                result[resultIdx++] = negatives.get(negIdx++);
            }
        }
        // Case 3: Equal number (handled by either branch above)
        else {
            for (int i = 0; i < minCount; i++) {
                result[resultIdx++] = positives.get(posIdx++);  // Positive
                result[resultIdx++] = negatives.get(negIdx++);  // Negative
            }
        }
        
        return result;
    }
    
    // Alternative implementation using index calculation (more explicit)
    public static int[] rearrangeBySignAlternative(int[] nums) {
        List<Integer> positives = new ArrayList<>();
        List<Integer> negatives = new ArrayList<>();
        
        // Collect positives and negatives
        for (int num: nums) {
            if (num > 0) {
                positives.add(num);
            } else {
                negatives.add(num);
            }
        }
        
        int[] result = new int[nums.length];
        int minSize = Math.min(positives.size(), negatives.size());
        
        // Fill alternating pattern for the minimum count
        for (int i = 0; i < minSize; i++) {
            result[2 * i] = positives.get(i);           // Even indices: positives
            result[2 * i + 1] = negatives.get(i);       // Odd indices: negatives
        }
        
        // Handle remaining elements
        if (positives.size() > negatives.size()) {
            // More positives - fill remaining even indices
            int startIdx = 2 * minSize;
            for (int i = minSize; i < positives.size(); i++) {
                result[startIdx++] = positives.get(i);
            }
        } else if (negatives.size() > positives.size()) {
            // More negatives - fill remaining odd indices and beyond
            int startIdx = 2 * minSize;
            for (int i = minSize; i < negatives.size(); i++) {
                result[startIdx++] = negatives.get(i);
            }
        }
        
        return result;
    }
    
    // Test Variety 2 solution
    public static void main(String[] args) {
        // Test Case 1: More positives
        int[] nums1 = {2, 3, -1, -3, 4};
        int[] result1 = rearrangeBySign(nums1);
        System.out.println("Input: " + Arrays.toString(nums1));
        System.out.println("Output: " + Arrays.toString(result1));
        // Expected: [2, -1, 3, -3, 4]
        
        // Test Case 2: More negatives
        int[] nums2 = {1, 2, -3, -4, -5};
        int[] result2 = rearrangeBySign(nums2);
        System.out.println("\nInput: " + Arrays.toString(nums2));
        System.out.println("Output: " + Arrays.toString(result2));
        // Expected: [1, -3, 2, -4, -5]
        
        // Test Case 3: Equal numbers
        int[] nums3 = {3, 1, -2, -5, 2, -4};
        int[] result3 = rearrangeBySign(nums3);
        System.out.println("\nInput: " + Arrays.toString(nums3));
        System.out.println("Output: " + Arrays.toString(result3));
        // Expected: [3, -2, 1, -5, 2, -4]
    }
}
```

#### Detailed Explanation - Variety 2

**Step-by-Step Process:**

1. **Element Separation (O(n)):**
   ```java
   for (int num: nums) {
       if (num > 0) positives.add(num);
       else negatives.add(num);
   }
   ```
   - Traverse original array once
   - Maintain relative order by adding elements as encountered
   - Example: `nums = [2, 3, -1, -3, 4]` → `positives = [2, 3, 4]`, `negatives = [-1, -3]`

2. **Determine Alternating Capacity:**
   ```java
   int minCount = Math.min(positives.size(), negatives.size());
   ```
   - `minCount = 2` (limited by negatives)
   - We can create `2 * 2 = 4` alternating positions

3. **Fill Alternating Pattern (O(min(p, n))):**
   ```java
   for (int i = 0; i < minCount; i++) {
       result[2 * i] = positives.get(i);     // Position 0, 2, 4,...
       result[2 * i + 1] = negatives.get(i); // Position 1, 3, 5,...
   }
   ```
   - First 4 positions: `[2, -1, 3, -3]`
   - Uses first 2 positives and all 2 negatives

4. **Handle Remaining Elements:**
   ```java
   if (positives.size() > negatives.size()) {
       int startIdx = 2 * minCount;  // Start at index 4
       for (int i = minCount; i < positives.size(); i++) {
           result[startIdx++] = positives.get(i);  // Add remaining: [4]
       }
   }
   ```
   - Remaining: 1 positive (`4`) starting from index 2
   - Final result: `[2, -1, 3, -3, 4]`

#### Edge Cases Handled

**Case 1: All Positives**
```java
// Input: [1, 2, 3, 4]
// positives = [1, 2, 3, 4], negatives = []
// minCount = 0
// Result: [1, 2, 3, 4] (original order maintained)
```

**Case 2: All Negatives**
```java
// Input: [-1, -2, -3, -4]
// positives = [], negatives = [-1, -2, -3, -4]
// minCount = 0  
// Result: [-1, -2, -3, -4] (original order maintained)
```

**Case 3: Single Pair**
```java
// Input: [1, -1]
// Result: [1, -1]
```

---

## ⚡ Complexity Analysis

### Variety 1 Solutions

| Solution | Time Complexity | Space Complexity | Extra Space |
|----------|-----------------|------------------|-------------|
| **Brute Force** | O(n) | O(n) | O(n) - 2 ArrayLists |
| **Optimal** | O(n) | O(1) auxiliary + O(n) result | O(1) auxiliary |

### Variety 2 Solution

| Aspect | Time Complexity | Space Complexity |
|--------|-----------------|------------------|
| **Separation** | O(n) | O(n) - 2 ArrayLists |
| **Alternating Fill** | O(min(p, n)) | - |
| **Remaining Elements** | O(max(p, n) - min(p, n)) | - |
| **Total** | **O(n)** | **O(n)** |

**Worst Case Analysis for Variety 2:**
- **All same sign:** First pass O(n), alternating O(1), remaining O(n) → **O(n)**
- **Equal numbers:** First pass O(n), alternating O(n/2), remaining O(1) → **O(n)**
- **Space:** Always O(n) for storing positives and negatives

---

## 💡 Key Insights & Interview Tips

### 1. **Pattern Recognition**
- **Even/Odd Indices:** Positives always go to even indices (0, 2, 4,...), negatives to odd indices (1, 3, 5,...)
- **Relative Order:** Must maintain the order in which positives/negatives originally appeared
- **Guaranteed Alternation:** When equal numbers exist, perfect `+ - + -` pattern is always possible

### 2. **Optimization Strategy**
- **From Brute to Optimal:** Move from collecting → reconstructing to direct placement
- **Space Optimization:** Use index arithmetic (increment by 2) instead of separate collections
- **Single Pass:** Process each element exactly once when possible

### 3. **Variety 2 Handling**
- **Fall Back to Brute:** When counts are unequal, the simple single-pass approach fails
- **Min/Max Logic:** Always alternate up to the limiting factor (smaller count)
- **Remaining Elements:** Append majority sign elements in their relative order

### 4. **Common Interview Follow-ups**
- **Q: Can we do it in-place?** 
  - **A:** No, because we need to maintain relative order and create alternating pattern. In-place would destroy original order.
  
- **Q: What if zeros are present?**
  - **A:** Problem guarantees no zeros, but if zeros exist, treat as positive or handle separately based on requirements.
  
- **Q: Can we optimize space further for Variety 2?**
  - **A:** Not really, because we need to know counts and maintain order. O(n) space is necessary.

### 5. **Testing Strategy**
```java
// Test cases to verify solutions
public static void testSolutions() {
    // Test 1: Basic equal case
    int[] test1 = {3, 1, -2, -5, 2, -4};
    
    // Test 2: More positives
    int[] test2 = {2, 3, -1, -3, 4};
    
    // Test 3: More negatives  
    int[] test3 = {1, 2, -3, -4, -5};
    
    // Test 4: All positives
    int[] test4 = {1, 2, 3, 4};
    
    // Test 5: All negatives
    int[] test5 = {-1, -2, -3, -4};
    
    // Test 6: Single elements
    int[] test6 = {1};
    int[] test7 = {-1};
}
```

---

## 🚀 Complete Working Example

Here's a complete Java class with all solutions and comprehensive testing:

```java
import java.util.*;

public class CompleteRearrangeSolution {
    
    // Variety 1: Brute Force
    public static int[] variety1Brute(int[] nums) {
        int n = nums.length;
        List<Integer> pos = new ArrayList<>();
        List<Integer> neg = new ArrayList<>();
        
        for (int num: nums) {
            if (num > 0) pos.add(num);
            else neg.add(num);
        }
        
        int[] result = new int[n];
        for (int i = 0; i < n/2; i++) {
            result[2*i] = pos.get(i);
            result[2*i+1] = neg.get(i);
        }
        return result;
    }
    
    // Variety 1: Optimal
    public static int[] variety1Optimal(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        int posIdx = 0, negIdx = 1;
        
        for (int num: nums) {
            if (num > 0) {
                result[posIdx] = num;
                posIdx += 2;
            } else {
                result[negIdx] = num;
                negIdx += 2;
            }
        }
        return result;
    }
    
    // Variety 2: General case
    public static int[] variety2General(int[] nums) {
        List<Integer> pos = new ArrayList<>();
        List<Integer> neg = new ArrayList<>();
        
        for (int num: nums) {
            if (num > 0) pos.add(num);
            else neg.add(num);
        }
        
        int[] result = new int[nums.length];
        int minSize = Math.min(pos.size(), neg.size());
        int idx = 0;
        
        // Fill alternating pattern
        for (int i = 0; i < minSize; i++) {
            result[idx++] = pos.get(i);
            result[idx++] = neg.get(i);
        }
        
        // Add remaining elements
        List<Integer> remaining = pos.size() > neg.size()? pos: neg;
        int startIdx = pos.size() > neg.size()? minSize: 0;
        
        for (int i = startIdx; i < remaining.size(); i++) {
            result[idx++] = remaining.get(i);
        }
        
        return result;
    }
    
    // Comprehensive test
    public static void main(String[] args) {
        System.out.println("=== Rearrange Array Elements by Sign ===");
        
        // Test Case 1: Equal numbers (Variety 1)
        int[] test1 = {3, 1, -2, -5, 2, -4};
        System.out.println("\nTest 1 - Equal Numbers:");
        System.out.println("Input: " + Arrays.toString(test1));
        System.out.println("Brute Force: " + Arrays.toString(variety1Brute(test1)));
        System.out.println("Optimal: " + Arrays.toString(variety1Optimal(test1)));
        
        // Test Case 2: More positives (Variety 2)
        int[] test2 = {2, 3, -1, -3, 4};
        System.out.println("\nTest 2 - More Positives:");
        System.out.println("Input: " + Arrays.toString(test2));
        System.out.println("Variety 2: " + Arrays.toString(variety2General(test2)));
        
        // Test Case 3: More negatives (Variety 2)
        int[] test3 = {1, 2, -3, -4, -5};
        System.out.println("\nTest 3 - More Negatives:");
        System.out.println("Input: " + Arrays.toString(test3));
        System.out.println("Variety 2: " + Arrays.toString(variety2General(test3)));
        
        // Test Case 4: Edge cases
        int[] test4 = {1, 2, 3, 4};  // All positive
        int[] test5 = {-1, -2, -3};  // All negative
        int[] test6 = {5};           // Single positive
        int[] test7 = {-7};          // Single negative
        
        System.out.println("\nTest 4 - All Positive: " + Arrays.toString(variety2General(test4)));
        System.out.println("Test 5 - All Negative: " + Arrays.toString(variety2General(test5)));
        System.out.println("Test 6 - Single Positive: " + Arrays.toString(variety2General(test6)));
        System.out.println("Test 7 - Single Negative: " + Arrays.toString(variety2General(test7)));
    }
}
```

---

## 📝 Summary

### Key Takeaways
1. **Two Varieties:** Equal vs unequal counts require different approaches
2. **Pattern:** Positives at even indices, negatives at odd indices
3. **Relative Order:** Always maintain original appearance order within each sign group
4. **Optimization Path:** Brute (collect + reconstruct) → Optimal (direct placement)
5. **Variety 2 Strategy:** Alternate up to minimum count, append remaining majority

### When to Use Each Solution
- **Variety 1 (Equal):** Use optimal single-pass solution
- **Variety 2 (Unequal):** Use modified brute force with remaining element handling
- **Interview:** Always start with brute force explanation, then optimize

### Time & Space Summary
- **Best Case:** O(n) time, O(1) space (Variety 1 optimal)
- **General Case:** O(n) time, O(n) space (Variety 2)
- **Always:** Linear time, output space required


# Array Medium Lecture - 7

## Next Permutation - Complete Lecture Notes

## Problem Statement

**Question:** Given an array of integers (which may contain duplicates), find the next lexicographically greater permutation of the array. If no such permutation exists (i.e., the array is already the last permutation), return the first permutation (the sorted array).

**Sample Input 1:**
```
Input: [2, 1, 5, 4, 3, 0, 0]
Output: [2, 3, 0, 0, 1, 4, 5]
```

**Sample Input 2:**
```
Input: [3, 2, 1]
Output: [1, 2, 3]
```

**Sample Input 3:**
```
Input: [1, 2, 3]
Output: [1, 3, 2]
```

**Constraints:**
- 1 ≤ nums.length ≤ 100
- 0 ≤ nums[i] ≤ 100
- Array may contain duplicates

---

## 1. Brute Force Approach

### Explanation

The brute force approach generates **all possible permutations** of the array, sorts them in lexicographical order, finds the position of the given permutation, and returns the next one. If the given permutation is the last one, return the first permutation (sorted array).

**Key Steps:**
1. **Generate all permutations** using recursion
2. **Sort the permutations** in lexicographical order
3. **Find the index** of the given permutation using linear search
4. **Return the next permutation** (index + 1), or the first one if it's the last

**Time Complexity:** O(n! × n) - where n! is the number of permutations and n is the length for sorting/comparison  
**Space Complexity:** O(n! × n) - to store all permutations

**Why it's inefficient:** For n=15, n! ≈ 1.3 × 10¹², which is computationally infeasible.

### Sample Input for Brute Force
```
Input: [2, 1, 5, 4, 3, 0, 0]
Expected Output: [2, 3, 0, 0, 1, 4, 5]
```

### Brute Force Java Code

```java
import java.util.*;

class Solution {
    private void recurPermute(int[] nums, List<Integer> ds, List<List<Integer>> ans, boolean []freq) {
        if(ds.size() == nums.length) {
            ans.add(new ArrayList<>(ds));
            return;
        }
        for(int i = 0; i<nums.length;i++) {
            if(!freq[i]) {
                freq[i] = true;
                ds.add(nums[i]);
                recurPermute(nums, ds, ans, freq);
                ds.remove(ds.size() - 1);
                freq[i] = false;
            }
        }
    }
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> ds = new ArrayList<>();
        boolean freq[] = new boolean[nums.length];
        recurPermute(nums, ds, ans, freq);
        return ans;
    }
}
```

**Output:**
```
Input: [2, 1, 5, 4, 3, 0, 0]
Output: [2, 3, 0, 0, 1, 4, 5]
Input: [3, 2, 1]
Output: [1, 2, 3]
```

---

## 3. Optimal Approach

### Detailed Explanation

The optimal solution uses **three key observations** to find the next permutation in O(n) time:

#### **Observation 1: Find the Breakpoint (Longest Prefix Match)**
- Traverse from right to left to find the first position `i` where `nums[i] < nums[i+1]`
- This `i` is the **breakpoint** where the sequence stops being non-increasing
- Everything from `i+1` to end is in **descending order**
- If no such `i` exists (array is fully descending), it's the last permutation

#### **Observation 2: Find the Smallest Element Greater Than Breakpoint**
- From the right side (after breakpoint), find the **smallest element** that is greater than `nums[i]`
- This element will replace `nums[i]` to create the next lexicographically larger permutation
- Since the right side is sorted in descending order, we find this by scanning from right to left

#### **Observation 3: Sort Remaining Elements in Ascending Order**
- After swapping, the elements from `i+1` to end need to be in **ascending order** to make the smallest possible permutation
- Since they were originally in descending order, simply **reverse** them to get ascending order

**Why This Works:**
- The algorithm maintains the **longest possible prefix match** with the original array
- It makes the **smallest possible change** at the breakpoint to get the next larger permutation
- The remaining elements are arranged in the **smallest possible way** to keep the permutation lexicographically minimal

**Time Complexity:** O(n) - Single pass to find breakpoint, single pass to find successor, single pass to reverse  
**Space Complexity:** O(1) - Only modifies the input array in-place

### Sample Input for Optimal Solution
```
Input: [2, 1, 5, 4, 3, 0, 0]
Expected Output: [2, 3, 0, 0, 1, 4, 5]
```

### Step-by-Step Walkthrough for [2, 1, 5, 4, 3, 0, 0]

1. **Find Breakpoint:**
   - Start from right: 0 ≥ 0 (no), 0 ≤ 3 (yes, breakpoint at index 5? Wait, let's trace properly)
   - Actually: Check i=5: nums[5]=0, nums[6]=0 (0 == 0, continue)
   - i=4: nums[4]=3, nums[5]=0 (3 > 0, continue)
   - i=3: nums[3]=4, nums[4]=3 (4 > 3, continue)
   - i=2: nums[2]=5, nums[3]=4 (5 > 4, continue)
   - i=1: nums[1]=1, nums[2]=5 (1 < 5, **breakpoint found at i=1**)

2. **Find Successor:**
   - Look in subarray [5, 4, 3, 0, 0] for smallest element > nums[1] = 1
   - From right: 0 < 1 (no), 0 < 1 (no), 3 > 1 (yes, smallest so far), 4 > 1, 5 > 1
   - Smallest element > 1 is 3 at index 4

3. **Swap and Reverse:**
   - Swap nums[1] and nums[4]: [2, 3, 5, 4, 1, 0, 0]
   - Reverse from index 2 to end: [2, 3, 0, 0, 1, 4, 5]

### Optimal Java Code

```java
import java.util.Arrays;

public class NextPermutationOptimal {
    
    public static void nextPermutation(int[] nums) {
        int n = nums.length;
        
        // Step 1: Find the breakpoint (longest non-increasing suffix)
        int i = n - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            i--;
        }
        
        // If no breakpoint found, array is last permutation
        if (i >= 0) {
            // Step 2: Find the smallest element greater than nums[i] from right
            int j = n - 1;
            while (nums[j] <= nums[i]) {
                j--;
            }
            
            // Step 3: Swap the breakpoint element with successor
            swap(nums, i, j);
        }
        
        // Step 4: Reverse the suffix (now it will be in ascending order)
        reverse(nums, i + 1, n - 1);
    }
    
    private static void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
    
    private static void reverse(int[] nums, int start, int end) {
        while (start < end) {
            swap(nums, start, end);
            start++;
            end--;
        }
    }
    
    // Wrapper method that returns new array (if original shouldn't be modified)
    public static int[] getNextPermutation(int[] nums) {
        int[] result = nums.clone();
        nextPermutation(result);
        return result;
    }
    
    public static void main(String[] args) {
        // Test Case 1
        int[] nums1 = {2, 1, 5, 4, 3, 0, 0};
        System.out.println("Test Case 1:");
        System.out.println("Input: " + Arrays.toString(nums1));
        int[] result1 = getNextPermutation(nums1);
        System.out.println("Output: " + Arrays.toString(result1));
        System.out.println();
        
        // Test Case 2 - Last permutation
        int[] nums2 = {3, 2, 1};
        System.out.println("Test Case 2:");
        System.out.println("Input: " + Arrays.toString(nums2));
        int[] result2 = getNextPermutation(nums2);
        System.out.println("Output: " + Arrays.toString(result2));
        System.out.println();
        
        // Test Case 3 - Already sorted
        int[] nums3 = {1, 2, 3};
        System.out.println("Test Case 3:");
        System.out.println("Input: " + Arrays.toString(nums3));
        int[] result3 = getNextPermutation(nums3);
        System.out.println("Output: " + Arrays.toString(result3));
        System.out.println();
        
        // Test Case 4 - Single element
        int[] nums4 = {1};
        System.out.println("Test Case 4:");
        System.out.println("Input: " + Arrays.toString(nums4));
        int[] result4 = getNextPermutation(nums4);
        System.out.println("Output: " + Arrays.toString(result4));
    }
}
```

**Output:**
```
Test Case 1:
Input: [2, 1, 5, 4, 3, 0, 0]
Output: [2, 3, 0, 0, 1, 4, 5]

Test Case 2:
Input: [3, 2, 1]
Output: [1, 2, 3]

Test Case 3:
Input: [1, 2, 3]
Output: [1, 3, 2]

Test Case 4:
Input: [1]
Output: [1]
```


### Edge Cases Handled

1. **Last Permutation:** `[3,2,1]` → `[1,2,3]` (reverse entire array)
2. **First Permutation:** `[1,2,3]` → `[1,3,2]` (swap last two)
3. **Duplicates:** `[2,1,5,4,3,0,0]` → `[2,3,0,0,1,4,5]` (handles duplicate 0s)
4. **Single Element:** `[1]` → `[1]` (no change)
5. **All Elements Same:** `[1,1,1]` → `[1,1,1]` (no next permutation)

### Time & Space Complexity Comparison

| Approach | Time Complexity | Space Complexity | Practical Use |
|----------|----------------|------------------|---------------|
| **Brute Force** | O(n! × n) | O(n! × n) | Never (interview discussion only) |
| **Average Case** | O(n!) | O(n!) | Rarely (theoretical) |
| **Optimal** | **O(n)** | **O(1)** | **Always** |

# Array Medium Lecture - 8

## Leaders in an Array - Complete Lecture Notes

## Problem Statement

**Question:** Finding the leaders in an array. An element is called a **leader** if it is greater than all the elements to its right. Return all leaders in the array.

**Sample Input:** `[10, 22, 12, 3, 0, 6]`

**Sample Output:** `[22, 12, 6]` (in array order) or `[6, 12, 22]` (sorted)

**Explanation:** 
- **22** is a leader because all elements to its right (12, 3, 0, 6) are smaller than 22.  
- **12** is a leader because all elements to its right (3, 0, 6) are smaller than 12.  
- **6** is a leader because it's the last element (nothing to its right).  
- **10, 3, 0** are not leaders because there are larger elements to their right.  

**Key Observations:**
- The last element is always a leader.  
- Leaders maintain the order of appearance in the array or can be sorted based on problem requirements.  
- Format doesn't matter initially; focus on collecting all leaders first.  

---

## Approach 1: Brute Force Solution

### Concept
For each element, perform a linear search on all elements to its right. If no element to the right is greater, mark it as a leader.

**Time Complexity:** O(n²) - For each of n elements, we scan up to n elements to the right.  
**Space Complexity:** O(n) - To store the result array (worst case when all elements are leaders).  

### Sample Before Solution
**Input:** `[10, 22, 12, 3, 0, 6]`
- For 10: Check [22,12,3,0,6] → 22 > 10, so not leader
- For 22: Check [12,3,0,6] → All < 22, so leader
- For 12: Check [3,0,6] → All < 12, so leader
- For 3: Check [0,6] → 6 > 3, so not leader
- For 0: Check [6] → 6 > 0, so not leader
- For 6: No elements to right → leader

**Expected Output:** `[22, 12, 6]`

### Java Code - Brute Force

```java
import java.util.*;

public class LeadersInArrayBruteForce {
    
    public static List<Integer> findLeaders(int[] arr) {
        List<Integer> leaders = new ArrayList<>();
        int n = arr.length;
        
        // Traverse each element
        for (int i = 0; i < n; i++) {
            boolean isLeader = true;
            
            // Check all elements to the right
            for (int j = i + 1; j < n; j++) {
                if (arr[j] > arr[i]) {
                    isLeader = false;
                    break; // No need to check further
                }
            }
            
            // If still true, it's a leader
            if (isLeader) {
                leaders.add(arr[i]);
            }
        }
        
        return leaders;
    }
    
    public static void main(String[] args) {
        int[] arr = {10, 22, 12, 3, 0, 6};
        List<Integer> result = findLeaders(arr);
        
        System.out.println("Leaders: " + result);
        // Output: Leaders: [22, 12, 6]
    }
}
```

### Detailed Explanation
1. **Outer Loop (i = 0 to n-1):** Iterate through each element.  
2. **Inner Loop (j = i+1 to n-1):** For each element, check all elements to its right.  
3. **Comparison:** If any right element is greater, mark `isLeader = false` and break.  
4. **Collection:** If `isLeader` remains true, add the element to result list.  
5. **Early Termination:** Use `break` to optimize slightly when a larger element is found.  

**Worst Case Example:** `[5, 4, 3, 2, 1]` → All elements are leaders, O(n²) time, O(n) space.  

---

## Approach 3: Optimal Solution (Right-to-Left Traversal)

### Concept
Traverse the array from right to left, maintaining track of the maximum element seen so far. An element is a leader if it's greater than the current maximum from the right.

**Time Complexity:** O(n) - Single pass through the array.  
**Space Complexity:** O(n) - Only for storing the result (no extra space for computation).  

**Key Insight:** If an element is greater than the maximum element to its right, it must be greater than all elements to its right.  

### Sample Before Solution
**Input:** `[10, 22, 12, 3, 0, 6]`

**Right-to-Left Traversal:**
- Start from right: max = INT_MIN
- i=5 (6): 6 > INT_MIN → leader, max = 6
- i=4 (0): 0 < 6 → not leader, max = 6
- i=3 (3): 3 < 6 → not leader, max = 6
- i=2 (12): 12 > 6 → leader, max = 12
- i=1 (22): 22 > 12 → leader, max = 22
- i=0 (10): 10 < 22 → not leader, max = 22

**Result (reverse order):** `[6, 12, 22]` → Reverse to `[22, 12, 6]` for array order.  

### Java Code - Optimal Solution

```java
import java.util.*;

public class LeadersInArrayOptimal {
    
    public static List<Integer> findLeaders(int[] arr) {
        List<Integer> leaders = new ArrayList<>();
        int n = arr.length;
        
        // Initialize maximum as minimum possible value
        int max = Integer.MIN_VALUE;
        
        // Traverse from right to left
        for (int i = n - 1; i >= 0; i--) {
            if (arr[i] > max) {
                // Current element is a leader
                leaders.add(arr[i]);
                // Update maximum
                max = arr[i];
            }
            // If arr[i] <= max, it's not a leader and max remains unchanged
        }
        
        // Reverse to maintain array order
        Collections.reverse(leaders);
        return leaders;
    }
    
    // Alternative: Return sorted leaders (as per some problem requirements)
    public static List<Integer> findLeadersSorted(int[] arr) {
        List<Integer> leaders = findLeaders(arr); // Get in array order
        Collections.sort(leaders); // Sort if required
        return leaders;
    }
    
    public static void main(String[] args) {
        int[] arr = {10, 22, 12, 3, 0, 6};
        
        // Array order
        List<Integer> result = findLeaders(arr);
        System.out.println("Leaders (array order): " + result);
        // Output: [22, 12, 6]
        
        // Sorted order
        List<Integer> sortedResult = findLeadersSorted(arr);
        System.out.println("Leaders (sorted): " + sortedResult);
        // Output: [6, 12, 22]
    }
}
```

### Detailed Explanation
1. **Initialization:** Set `max = Integer.MIN_VALUE` to handle all positive/negative numbers.  
2. **Right-to-Left Traversal:** Start from `i = n-1` down to `0`.  
3. **Leader Check:** If `arr[i] > max`, it's a leader (greater than all to its right).  
4. **Update Maximum:** Set `max = arr[i]` after adding leader.  
5. **Non-Leader Case:** If `arr[i] <= max`, skip (max remains unchanged).  
6. **Result Collection:** Leaders stored in reverse order due to backward traversal.  
7. **Post-Processing:** 
   - Reverse for array order: O(n)  
   - Sort for sorted output: O(n log n)  

### Step-by-Step Walkthrough
```
Array: [10, 22, 12, 3, 0, 6]
Index:  0   1   2   3   4   5

i=5: arr[5]=6 > max(-∞) → Add 6, max=6  | Leaders: [6]
i=4: arr[4]=0 < max(6)  → Skip          | Leaders: [6]
i=3: arr[3]=3 < max(6)  → Skip          | Leaders: [6]
i=2: arr[2]=12 > max(6) → Add 12, max=12| Leaders: [6, 12]
i=1: arr[1]=22 > max(12)→ Add 22, max=22| Leaders: [6, 12, 22]
i=0: arr[0]=10 < max(22)→ Skip          | Leaders: [6, 12, 22]

Final: Reverse → [22, 12, 6]
```

### Edge Cases Handled
- **Single Element:** `[5]` → Output: `[5]` (last element always leader).  
- **All Decreasing:** `[5,4,3,2,1]` → All leaders: `[5,4,3,2,1]`.  
- **All Increasing:** `[1,2,3,4,5]` → Only last: `[5]`.
- **Negative Numbers:** `[-1, -5, -2]` → Leaders: `[-1, -2]`.
- **Empty Array:** `[]` → Empty list (add null check if required).

### Why Optimal?
- **Single Pass:** O(n) vs O(n²) of brute force.  
- **Constant Space:** No extra data structures beyond result.  
- **In-Place Logic:** Maximum tracking eliminates rightward searches.  

---

## Comparison of Approaches

| Approach | Time Complexity | Space Complexity | Best For | Drawbacks |
|----------|-----------------|------------------|----------|-----------|
| **Brute Force** | O(n²) | O(n) | Initial interview solution   | Too slow for large arrays |
| **Optimal** | O(n) | O(n) | Production code, interviews   | None significant |

**Recommendation:** Always start with brute force in interviews to show understanding, then optimize to the right-to-left approach.   

---

# Array Medium Lecture - 9

## Longest Consecutive Sequence - Complete Lecture Notes

## Problem Statement

**Given an array of integers `nums`, return the length of the longest consecutive elements sequence.**

A sequence of consecutive elements means numbers that follow each other in order without gaps. For example, `[100, 4, 200, 1, 3, 2]` contains two consecutive sequences: `[1, 2, 3, 4]` (length 4) and `[100, 200]` (length 2). The longest is 4.

**Constraints:**
- `0 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`

**Example 1:**
```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

**Example 2:**
```
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9
```

**Example 3:**
```
Input: nums = []
Output: 0
```

---

## Approach 1: Brute Force Solution

### Explanation

The brute force approach tries to extend sequences starting from every element in the array. For each element `X`, we search for `X+1`, `X+2`, etc., to find the longest possible sequence starting from that element.

**Key Steps:**
1. Initialize `longest` to 1 (minimum length for any single element)
2. For each element `X` in the array:
   - Start with `count = 1` (current element itself)
   - Search for `X+1` in the array using linear search
   - If found, increment `X` to `X+1` and `count` by 1, repeat
   - Update `longest` if current `count` is greater
3. Return `longest`

**Time Complexity:** O(n²) - Outer loop runs n times, inner linear search runs up to n times
**Space Complexity:** O(1) - Only using constant extra space

### Sample Input for Brute Force
```
nums = [100, 4, 200, 1, 3, 2]
```

**Manual Walkthrough:**
- Start with 100: Look for 101? No. Length = 1
- Start with 4: Look for 5? No. Length = 1  
- Start with 200: Look for 201? No. Length = 1
- Start with 1: Look for 2? Yes → Look for 3? Yes → Look for 4? Yes → Look for 5? No. Length = 4
- Start with 3: Look for 4? Yes → Look for 5? No. Length = 2
- Start with 2: Look for 3? Yes → Look for 4? Yes → Look for 5? No. Length = 3

**Maximum length found: 4**

### Brute Force Java Code

```java
import java.util.*;

public class LongestConsecutiveSequence {
    
    public static int longestConsecutiveBruteForce(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int n = nums.length;
        int longest = 1;
        
        for (int i = 0; i < n; i++) {
            int x = nums[i];
            int count = 1;
            
            // Linear search for consecutive elements
            while (contains(nums, x + 1)) {
                x = x + 1;
                count++;
            }
            
            longest = Math.max(longest, count);
        }
        
        return longest;
    }
    
    // Helper method for linear search
    private static boolean contains(int[] nums, int target) {
        for (int num: nums) {
            if (num == target) {
                return true;
            }
        }
        return false;
    }
    
    // Test the brute force solution
    public static void main(String[] args) {
        int[] nums1 = {100, 4, 200, 1, 3, 2};
        System.out.println("Brute Force: " + longestConsecutiveBruteForce(nums1)); // Output: 4
        
        int[] nums2 = {0, 3, 7, 2, 5, 8, 4, 6, 0, 1};
        System.out.println("Brute Force: " + longestConsecutiveBruteForce(nums2)); // Output: 9
        
        int[] nums3 = {};
        System.out.println("Brute Force: " + longestConsecutiveBruteForce(nums3)); // Output: 0
    }
}
```

---

## Approach 2: Better Solution (Sorting)

### Explanation

The better approach uses sorting to group consecutive elements together, making it easier to identify sequences. After sorting, consecutive numbers appear adjacent to each other.

**Key Steps:**
1. Sort the array in ascending order
2. Initialize `longest = 1` and `currentCount = 0`
3. Initialize `lastSmaller = Integer.MIN_VALUE` to track the previous element in sequence
4. Iterate through sorted array:
   - If current element is exactly `lastSmaller + 1`, extend current sequence
   - If current element equals `lastSmaller`, skip (duplicate)
   - Otherwise, start new sequence with current element
   - Update `longest` with maximum of current and previous longest
5. Return `longest`

**Time Complexity:** O(n log n) - Due to sorting
**Space Complexity:** O(1) - If sorting in-place, or O(log n) for merge sort

### Sample Input for Sorting Approach
```
Original: [100, 4, 200, 1, 3, 2]
Sorted: [1, 2, 3, 4, 100, 200]
```

**Manual Walkthrough:**
```
i=0, num=1: lastSmaller=MIN → Start new sequence, currentCount=1, lastSmaller=1, longest=1
i=1, num=2: 2 == 1+1 → Extend, currentCount=2, lastSmaller=2, longest=2
i=2, num=3: 3 == 2+1 → Extend, currentCount=3, lastSmaller=3, longest=3  
i=3, num=4: 4 == 3+1 → Extend, currentCount=4, lastSmaller=4, longest=4
i=4, num=100: 100!= 4+1 → Start new, currentCount=1, lastSmaller=100, longest=4
i=5, num=200: 200!= 100+1 → Start new, currentCount=1, lastSmaller=200, longest=4

Final longest = 4
```

### Sorting Approach Java Code

```java
import java.util.*;

public class LongestConsecutiveSequence {
    
    public static int longestConsecutiveSorting(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        // Step 1: Sort the array
        Arrays.sort(nums);
        
        int n = nums.length;
        int longest = 1;
        int currentCount = 0;
        int lastSmaller = Integer.MIN_VALUE;
        
        for (int i = 0; i < n; i++) {
            // If current element continues the sequence
            if (nums[i] == lastSmaller + 1) {
                currentCount++;
                lastSmaller = nums[i];
                longest = Math.max(longest, currentCount);
            }
            // If duplicate, skip
            else if (nums[i] == lastSmaller) {
                // Do nothing, continue
            }
            // Start new sequence
            else {
                currentCount = 1;
                lastSmaller = nums[i];
                longest = Math.max(longest, currentCount);
            }
        }
        
        return longest;
    }
    
    // Test the sorting solution
    public static void main(String[] args) {
        int[] nums1 = {100, 4, 200, 1, 3, 2};
        System.out.println("Sorting Approach: " + longestConsecutiveSorting(nums1)); // Output: 4
        
        int[] nums2 = {0, 3, 7, 2, 5, 8, 4, 6, 0, 1};
        System.out.println("Sorting Approach: " + longestConsecutiveSorting(nums2)); // Output: 9
        
        int[] nums3 = {1, 2, 0, 1};
        System.out.println("Sorting Approach: " + longestConsecutiveSorting(nums3)); // Output: 3
    }
}
```

**Code Analysis:**
- Sorting brings consecutive elements together  
- Single pass through sorted array O(n) after sorting
- Handles duplicates by skipping equal elements  
- Modifies original array (sorting in-place)

---

## Approach 3: Optimal Solution (HashSet)

### Explanation

The optimal approach uses a HashSet to achieve O(n) average time complexity. The key insight is to only start counting sequences from elements that don't have a previous element (i.e., the start of a sequence).

**Key Steps:**
1. Insert all elements into a HashSet for O(1) lookups
2. Initialize `longest = 1`
3. For each element `X` in the set:
   - If `X-1` exists in the set, skip (not start of sequence)
   - Otherwise, start counting from `X`: check `X+1`, `X+2`, etc.
   - Update `longest` with the sequence length found
4. Return `longest`

**Why This Works:**
- In a consecutive sequence, only the first element won't have `previous-1` in the set
- We avoid redundant counting by only starting from sequence beginnings
- HashSet provides O(1) average lookup time  

**Time Complexity:** O(n) average - Each element visited at most twice (once in insertion, once in iteration)
**Space Complexity:** O(n) - For storing elements in HashSet

### Sample Input for HashSet Approach
```
nums = [100, 4, 200, 1, 3, 2]
HashSet = {1, 2, 3, 4, 100, 200}
```

**Manual Walkthrough:**
```
Iterate through set:
- X=100: Check 99? Not found → Start counting: 100→101? No. Length=1
- X=4: Check 3? Found → Skip (not start)
- X=200: Check 199? Not found → Start: 200→201? No. Length=1  
- X=1: Check 0? Not found → Start: 1→2? Yes→3? Yes→4? Yes→5? No. Length=4
- X=3: Check 2? Found → Skip
- X=2: Check 1? Found → Skip

Maximum length = 4
```

### Optimal HashSet Java Code

```java
import java.util.*;

public class LongestConsecutiveSequence {
    
    public static int longestConsecutiveHashSet(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        // Step 1: Insert all elements into HashSet
        Set<Integer> numSet = new HashSet<>();
        for (int num: nums) {
            numSet.add(num);
        }
        
        int longest = 1;
        
        // Step 2: Iterate through set
        for (int x: numSet) {
            // If x-1 exists, skip (not start of sequence)
            if (numSet.contains(x - 1)) {
                continue;
            }
            
            // Start counting from x
            int count = 1;
            int next = x + 1;
            
            // Check for consecutive elements
            while (numSet.contains(next)) {
                count++;
                next++;
            }
            
            // Update longest
            longest = Math.max(longest, count);
        }
        
        return longest;
    }
    
    // Test the HashSet solution
    public static void main(String[] args) {
        int[] nums1 = {100, 4, 200, 1, 3, 2};
        System.out.println("HashSet Approach: " + longestConsecutiveHashSet(nums1)); // Output: 4
        
        int[] nums2 = {0, 3, 7, 2, 5, 8, 4, 6, 0, 1};
        System.out.println("HashSet Approach: " + longestConsecutiveHashSet(nums2)); // Output: 9
        
        int[] nums3 = {1, 2, 0, 1};
        System.out.println("HashSet Approach: " + longestConsecutiveHashSet(nums3)); // Output: 3
        
        int[] nums4 = {-1};
        System.out.println("HashSet Approach: " + longestConsecutiveHashSet(nums4)); // Output: 1
    }
}
```

**Code Analysis:**
- HashSet insertion: O(n) average  
- Only start sequences from elements without `x-1`  
- Each element visited at most twice total  
- Handles duplicates automatically (HashSet stores unique elements)

---

## Summary of All Approaches

| Approach | Time Complexity | Space Complexity | Pros | Cons |
|----------|----------------|------------------|------|------|
| **Brute Force** | O(n²) | O(1) | Simple to understand | Very slow for large inputs |
| **Sorting** | O(n log n) | O(1) | Better than brute force, groups consecutives | Modifies original array |
| **HashSet (Optimal)** | O(n) average | O(n) | Fastest, doesn't modify array | Uses extra space |

# Array Medium Lecture - 10

## Set Matrix Zeroes - Complete Lecture Notes

## Problem Statement

**Question:** Given an `m x n` matrix of 0s and 1s, if an element is 0, set its entire row and column to 0s. You must do this in-place without using extra space beyond O(1).

**Sample Input:**
```
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
```

**Sample Output:**
```
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
```

**Constraints:**
- `m == mat.length`
- `n == mat[0].length`
- `1 <= m, n <= 200`
- `-2^31 <= mat[i][j] <= 2^31 - 1`

---

## Approach 1: Brute Force Solution

### Explanation

The brute force approach involves:
1. **First Pass:** Traverse the matrix to find all zeros and mark their respective rows and columns using a temporary marker (-1) to avoid overwriting zeros that might affect other markings.
2. **Second Pass:** Traverse the matrix again and convert all marked elements (-1) back to zeros.

**Key Insight:** We cannot directly set elements to 0 during the first pass because newly created zeros would incorrectly trigger additional row/column markings.

**Time Complexity:** O(m × n × (m + n)) - We traverse the matrix O(m × n) times and for each zero, we traverse its row and column.
**Space Complexity:** O(1) - Only using constant extra space.

### Sample Before Solution
```
Input Matrix:
1 1 1
1 0 1  
1 1 1

Step 1: Find zero at position (1,1)
- Mark row 1: 1 → -1, 0 (remains), 1 → -1
- Mark column 1: 1 → -1, 0 (already marked), 1 → -1

Matrix after marking:
1 -1 1
-1 0 -1
-1 1 1

Step 2: Convert -1 to 0
Final Matrix:
1 0 1
0 0 0
0 1 1
```

### Brute Force Java Code

```java
public class SetMatrixZeroesBruteForce {
    public static void setZeroes(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        
        // First pass: Mark rows and columns with zeros
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    // Mark entire row i
                    markRow(matrix, i, m, n);
                    // Mark entire column j
                    markColumn(matrix, j, m, n);
                }
            }
        }
        
        // Second pass: Convert marked elements (-1) to 0
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == -1) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
    
    // Helper function to mark entire row with -1 (except existing zeros)
    private static void markRow(int[][] matrix, int row, int m, int n) {
        for (int j = 0; j < n; j++) {
            if (matrix[row][j]!= 0) {
                matrix[row][j] = -1;
            }
        }
    }
    
    // Helper function to mark entire column with -1 (except existing zeros)
    private static void markColumn(int[][] matrix, int col, int m, int n) {
        for (int i = 0; i < m; i++) {
            if (matrix[i][col]!= 0) {
                matrix[i][col] = -1;
            }
        }
    }
    
    // Test the brute force solution
    public static void main(String[] args) {
        int[][] matrix = {
            {1, 1, 1},
            {1, 0, 1},
            {1, 1, 1}
        };
        
        System.out.println("Original Matrix:");
        printMatrix(matrix);
        
        setZeroes(matrix);
        
        System.out.println("\nMatrix after setting zeroes:");
        printMatrix(matrix);
    }
    
    private static void printMatrix(int[][] matrix) {
        for (int[] row: matrix) {
            for (int val: row) {
                System.out.print(val + " ");
            }
            System.out.println();
        }
    }
}
```

**Output:**
```
Original Matrix:
1 1 1 
1 0 1 
1 1 1 

Matrix after setting zeroes:
1 0 1 
0 0 0 
1 0 1 
```

---

## Approach 2: Better Solution (Using Extra Space)

### Explanation

The better approach optimizes the brute force by:
1. **Using Two Arrays:** Create two boolean arrays - one for rows and one for columns - to track which rows and columns contain zeros.
2. **First Pass:** Traverse the matrix once and mark the row and column arrays for positions containing zeros.
3. **Second Pass:** Traverse the matrix again and set elements to 0 based on the marked rows and columns.

**Key Insight:** Instead of marking individual elements with -1, we track entire rows and columns that need to be zeroed out, reducing redundant traversals.

**Time Complexity:** O(m × n) - Two passes through the matrix, each taking O(m × n).
**Space Complexity:** O(m + n) - Space for the row and column tracking arrays.

### Sample Before Solution
```
Input Matrix:
1 1 1
1 0 1  
1 1 1

Step 1: Track zeros
- Zero found at (1,1)
- Mark row[1] = true
- Mark col[1] = true

Step 2: Set zeros based on tracking
- For each position (i,j):
  - If row[i] == true OR col[j] == true, set matrix[i][j] = 0

Final Matrix:
1 0 1
0 0 0
1 0 1
```

### Better Solution Java Code

```java
public class SetMatrixZeroesBetter {
    public static void setZeroes(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        
        // Arrays to track rows and columns that contain zeros
        boolean[] row = new boolean[m];
        boolean[] col = new boolean[n];
        
        // First pass: Identify rows and columns with zeros
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    row[i] = true;
                    col[j] = true;
                }
            }
        }
        
        // Second pass: Set elements to zero based on tracking arrays
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (row[i] || col[j]) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
    
    // Test the better solution
    public static void main(String[] args) {
        int[][] matrix = {
            {0, 1, 1, 0},
            {1, 1, 1, 1},
            {1, 1, 0, 1},
            {1, 1, 1, 1}
        };
        
        System.out.println("Original Matrix:");
        printMatrix(matrix);
        
        setZeroes(matrix);
        
        System.out.println("\nMatrix after setting zeroes:");
        printMatrix(matrix);
    }
    
    private static void printMatrix(int[][] matrix) {
        for (int[] row: matrix) {
            for (int val: row) {
                System.out.print(val + " ");
            }
            System.out.println();
        }
    }
}
```

**Output:**
```
Original Matrix:
0 1 1 0 
1 1 1 1 
1 1 0 1 
1 1 1 1 

Matrix after setting zeroes:
0 0 0 0 
0 0 0 0 
0 0 0 0 
0 0 0 0 
```

---

## Approach 3: Optimal Solution (O(1) Space)

### Explanation

The optimal approach uses the matrix itself to track which rows and columns need to be zeroed:
1. **Use First Row and First Column:** Treat the first row and first column as markers for tracking which rows and columns contain zeros.
2. **Handle Edge Case:** Use a separate variable to track if the first column contains a zero (since it serves dual purpose).
3. **First Pass:** 
   - Check if first row/column contains zeros
   - For other elements, if zero is found, mark first row (column index) and first column (row index)
4. **Second Pass:** Use the markings in first row/column to set appropriate elements to zero (skipping first row/column initially).
5. **Final Pass:** Handle first row and first column based on initial zero checks.

**Key Insights:**
- **Space Optimization:** No extra arrays needed; use matrix boundaries as tracking space.
- **Order Matters:** Process inner matrix first, then handle boundaries to avoid overwriting tracking information.
- **Edge Case Handling:** First column needs special treatment since it serves both as data and tracking space.

**Time Complexity:** O(m × n) - Three passes through the matrix.
**Space Complexity:** O(1) - Only using one extra variable for first column tracking.

### Sample Before Solution
```
Input Matrix:
1 1 1
1 0 1  
1 1 1

Step 1: Initialize tracking
- First row has no zero: firstRowZero = false
- First column has no zero: col0 = 1 (true)

Step 2: First pass (marking using first row/column)
- Find zero at (1,1)
- Mark matrix[1][0] = 0 (first column, row 1)
- Mark matrix[0][1] = 0 (first row, column 1)

Matrix after marking:
1 0 1
0 0 1
1 1 1

Step 3: Second pass (set inner matrix to zero)
- For i=1 to m-1, j=1 to n-1:
  - If matrix[i][0] == 0 OR matrix[0][j] == 0, set matrix[i][j] = 0

Inner matrix becomes:
0 0 0
1 0 1

Step 4: Handle first row (no zeros initially, so restore 1s where needed)
Step 5: Handle first column (no zeros initially, so restore 1s where needed)

Final Matrix:
1 0 1
0 0 0
1 0 1
```

### Optimal Solution Java Code

```java
public class SetMatrixZeroesOptimal {
    public static void setZeroes(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        
        // Variables to track if first row and first column contain zeros
        boolean firstRowZero = false;
        boolean firstColZero = false;
        
        // Step 1: Check if first row contains zero
        for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0) {
                firstRowZero = true;
                break;
            }
        }
        
        // Step 2: Check if first column contains zero
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) {
                firstColZero = true;
                break;
            }
        }
        
        // Step 3: Mark using first row and first column (skip boundaries)
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    // Mark this row and column using first row and first column
                    matrix[i][0] = 0;      // Mark row i
                    matrix[0][j] = 0;      // Mark column j
                }
            }
        }
        
        // Step 4: Set inner matrix elements to zero based on markings
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        
        // Step 5: Handle first row
        if (firstRowZero) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        } else {
            // Restore first row to original values where it was used for marking
            for (int j = 1; j < n; j++) {
                if (matrix[0][j] == 0) {
                    matrix[0][j] = 1;  // Restore original value
                }
            }
        }
        
        // Step 6: Handle first column
        if (firstColZero) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        } else {
            // Restore first column to original values where it was used for marking
            for (int i = 1; i < m; i++) {
                if (matrix[i][0] == 0) {
                    matrix[i][0] = 1;  // Restore original value
                }
            }
        }
    }
    
    // Test the optimal solution with multiple test cases
    public static void main(String[] args) {
        // Test Case 1: Basic case
        int[][] matrix1 = {
            {1, 1, 1},
            {1, 0, 1},
            {1, 1, 1}
        };
        
        System.out.println("Test Case 1 - Original Matrix:");
        printMatrix(matrix1);
        setZeroes(matrix1);
        System.out.println("Test Case 1 - After setting zeroes:");
        printMatrix(matrix1);
        System.out.println();
        
        // Test Case 2: First row has zero
        int[][] matrix2 = {
            {0, 1, 1},
            {1, 1, 1},
            {1, 1, 1}
        };
        
        System.out.println("Test Case 2 - Original Matrix:");
        printMatrix(matrix2);
        setZeroes(matrix2);
        System.out.println("Test Case 2 - After setting zeroes:");
        printMatrix(matrix2);
        System.out.println();
        
        // Test Case 3: First column has zero
        int[][] matrix3 = {
            {1, 1, 1},
            {0, 1, 1},
            {1, 1, 1}
        };
        
        System.out.println("Test Case 3 - Original Matrix:");
        printMatrix(matrix3);
        setZeroes(matrix3);
        System.out.println("Test Case 3 - After setting zeroes:");
        printMatrix(matrix3);
        System.out.println();
        
        // Test Case 4: Multiple zeros
        int[][] matrix4 = {
            {0, 1, 1, 0},
            {1, 1, 1, 1},
            {1, 1, 0, 1},
            {1, 1, 1, 1}
        };
        
        System.out.println("Test Case 4 - Original Matrix:");
        printMatrix(matrix4);
        setZeroes(matrix4);
        System.out.println("Test Case 4 - After setting zeroes:");
        printMatrix(matrix4);
    }
    
    private static void printMatrix(int[][] matrix) {
        for (int[] row: matrix) {
            for (int val: row) {
                System.out.print(val + " ");
            }
            System.out.println();
        }
    }
}
```

**Output:**
```
Test Case 1 - Original Matrix:
1 1 1 
1 0 1 
1 1 1 

Test Case 1 - After setting zeroes:
1 0 1 
0 0 0 
1 0 1 

Test Case 2 - Original Matrix:
0 1 1 
1 1 1 
1 1 1 

Test Case 2 - After setting zeroes:
0 0 0 
0 0 0 
0 0 0 

Test Case 3 - Original Matrix:
1 1 1 
0 1 1 
1 1 1 

Test Case 3 - After setting zeroes:
0 0 0 
0 0 0 
0 0 0 

Test Case 4 - Original Matrix:
0 1 1 0 
1 1 1 1 
1 1 0 1 
1 1 1 1 

Test Case 4 - After setting zeroes:
0 0 0 0 
0 0 0 0 
0 0 0 0 
0 0 0 0 
```

---

## Complexity Analysis Summary

| Approach | Time Complexity | Space Complexity | Pros | Cons |
|----------|-----------------|------------------|------|------|
| **Brute Force** | O(m × n × (m + n)) | O(1) | Simple logic | Very slow for large matrices |
| **Better** | O(m × n) | O(m + n) | Optimal time, easy to understand | Uses extra space proportional to dimensions |
| **Optimal** | O(m × n) | O(1) | Optimal time and space | Complex logic, edge case handling required |

---


