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
