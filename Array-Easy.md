# DSA 
```bash
# Prompt

Give me these lecture complete notes with detailed explanation, which contain complete code of all brute force, average and optimal case code in java, make sure you provide the brute force, average and optimal case code of each question not only the explaination, also provide question sample before every solution, give me complete response in .md code
```

# Lecture 1 Array Easy

```markdown
# Lecture Notes: Arrays Introduction and Core Problems

This lecture covers the basics of arrays and provides Brute Force, Better, and Optimal solutions for several fundamental array problems, focusing on time and space complexity analysis.



## 2. Problem 1: Largest Element in an Array

**Goal:** Find the largest integer in a given array of integers.

### Brute Force Approach (Sorting)

The simplest approach is to **sort the array** in ascending order and then return the last element, which will be the largest.

*   **Time Complexity (TC):** $$O(N \log N)$$ (due to sorting algorithms like Merge Sort or Quick Sort).
*   **Space Complexity (SC):** $$O(1)$$ (ignoring recursive stack space for Quick Sort).

```java
import java.util.Arrays;

class LargestElement {
    public static int findLargestBrute(int[] arr) {
        // Brute Force: Sort the array
        // TC: O(N log N)
        if (arr == null || arr.length == 0) {
            return -1; // Or throw an exception
        }
        Arrays.sort(arr);
        // The largest element is at the last index (N-1)
        return arr[arr.length - 1];
    }
}
```

### Optimal Approach (Single Pass Traversal)

The optimal solution involves traversing the array once and keeping track of the maximum element found so far.

1.  Initialize a variable, `largest`, with the first element of the array (`A[0]`).
2.  Iterate through the rest of the array (from index 1 to N-1).
3.  If the current element is greater than `largest`, update `largest` to the current element.

*   **Time Complexity (TC):** $$O(N)$$ (single pass).
*   **Space Complexity (SC):** $$O(1)$$.

```java
class LargestElement {
    public static int findLargestOptimal(int[] arr) {
        // Optimal: Single pass traversal
        // TC: O(N)
        if (arr == null || arr.length == 0) {
            return -1;
        }
        
        int largest = arr[0]; // Assume the first element is the largest
        
        // Start loop from index 1 (or 0, doesn't matter much for complexity)
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > largest) {
                largest = arr[i];
            }
        }
        return largest;
    }
}
```

---

## 3. Problem 2: Second Largest Element (Without Sorting)

**Goal:** Find the second largest element in an array without relying on sorting algorithms.

### Brute Force Approach (Sorting + Traversal)

1.  **Sort the array** (TC: $$O(N \log N)$$).
2.  Identify the **largest element** at index $$N-1$$.
3.  Traverse backward from index $$N-2$$ until an element is found that is **not equal** to the largest element. This element is the second largest.

*   **Time Complexity (TC):** $$O(N \log N) + O(N)$$ (Worst case: all elements are the same, requiring full backward traversal).
*   **Space Complexity (SC):** $$O(1)$$.

### Better Approach (Two Passes)

This approach uses two separate traversals of the array.

1.  **First Pass:** Find the **largest element** (TC: $$O(N)$$).
2.  **Second Pass:** Initialize `secondLargest` to a very small number (e.g., -1 or `Integer.MIN_VALUE`). Iterate through the array again.
    *   If the current element is greater than `secondLargest` **AND** the current element is **not equal** to `largest`, update `secondLargest`.

*   **Time Complexity (TC):** $$O(N) + O(N) = O(2N)$$.
*   **Space Complexity (SC):** $$O(1)$$.

```java
class SecondLargestElement {
    public static int findSecondLargestBetter(int[] arr) {
        // Better Approach: Two Passes
        // TC: O(2N)
        if (arr == null || arr.length < 2) {
            return -1;
        }

        // Pass 1: Find the largest element
        int largest = arr[0];
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > largest) {
                largest = arr[i];
            }
        }

        // Pass 2: Find the second largest
        int secondLargest = Integer.MIN_VALUE; // Use MIN_VALUE for general case
        
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] > secondLargest && arr[i]!= largest) {
                secondLargest = arr[i];
            }
        }

        // If secondLargest is still MIN_VALUE, it means all elements were the same.
        return (secondLargest == Integer.MIN_VALUE)? -1: secondLargest;
    }
}
```

### Optimal Approach (Single Pass)

This approach finds both the largest and second largest elements in a single traversal, optimizing the time complexity to $$O(N)$$.

1.  Initialize `largest` with `A[0]` and `secondLargest` with a very small number (e.g., -1 or `Integer.MIN_VALUE`).
2.  Traverse the array starting from the second element (index 1).
3.  **Case 1: Current element is greater than `largest`**
    *   The old `largest` becomes the new `secondLargest`.
    *   The current element becomes the new `largest`.
4.  **Case 2: Current element is NOT equal to `largest` AND is greater than `secondLargest`**
    *   The current element becomes the new `secondLargest`.

*   **Time Complexity (TC):** $$O(N)$$ (single pass).
*   **Space Complexity (SC):** $$O(1)$$.

```java
class SecondLargestElement {
    public static int findSecondLargestOptimal(int[] arr) {
        // Optimal Approach: Single Pass
        // TC: O(N)
        if (arr == null || arr.length < 2) {
            return -1;
        }

        int largest = arr[0];
        int secondLargest = Integer.MIN_VALUE; 
        
        // Start loop from index 1
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > largest) {
                // Case 1: Found a new largest
                secondLargest = largest; // Previous largest becomes second largest
                largest = arr[i];
            } else if (arr[i] < largest && arr[i] > secondLargest) {
                // Case 2: Found a new second largest
                secondLargest = arr[i];
            }
            // Note: If arr[i] == largest, we do nothing (as per the logic)
        }
        
        // If secondLargest is still MIN_VALUE, it means all elements were the same.
        return (secondLargest == Integer.MIN_VALUE)? -1: secondLargest;
    }
}
```

---

## 4. Problem 3: Check if the Array is Sorted

**Goal:** Determine if an array is sorted in a non-descending order (i.e., each element is greater than or equal to the previous element).

### Optimal Approach (Single Pass)

The most straightforward and optimal approach is to iterate through the array starting from the second element (index 1) and compare each element with its predecessor.

1.  Start the loop from index $$I=1$$ to $$N-1$$.
2.  Check if `A[I]` is less than `A[I-1]`.
3.  If this condition is ever true, the array is not sorted, so immediately return `false`.
4.  If the loop completes without returning `false`, the array is sorted, so return `true`.

*   **Time Complexity (TC):** $$O(N)$$ (single pass).
*   **Space Complexity (SC):** $$O(1)$$.

```java
class CheckSorted {
    public static boolean isSorted(int[] arr) {
        // Optimal: Single pass comparison
        // TC: O(N)
        if (arr == null || arr.length <= 1) {
            return true;
        }

        // Start from index 1 and compare with the previous element (i-1)
        for (int i = 1; i < arr.length; i++) {
            // Check for non-descending order: arr[i] >= arr[i-1]
            // If the current element is smaller than the previous one, it's not sorted.
            if (arr[i] < arr[i - 1]) {
                return false;
            }
        }
        return true;
    }
}
```

---

## 5. Problem 4: Remove Duplicates from a Sorted Array (In Place)

**Goal:** Given a sorted array, remove duplicates **in place** such that the first $$K$$ elements contain the unique elements, and return $$K$$ (the number of unique elements).

### Brute Force Approach (Using a Set)

This approach uses an auxiliary data structure (`HashSet` or `TreeSet`) to store unique elements.

1.  Iterate through the array and insert all elements into a `Set` (which automatically handles uniqueness).
2.  Iterate through the `Set` and place the unique elements back into the beginning of the original array, starting at index 0.
3.  Return the size of the set (or the final index + 1).

*   **Time Complexity (TC):** $$O(N \log N) + O(N)$$ (If using `TreeSet` for sorted insertion, or $$O(N)$$ for `HashSet` insertion + $$O(N)$$ for traversal).
*   **Space Complexity (SC):** $$O(N)$$ (to store the unique elements in the Set).

```java
import java.util.HashSet;
import java.util.Set;

class RemoveDuplicates {
    public static int removeDuplicatesBrute(int[] arr) {
        // Brute Force: Using a Set
        // TC: O(N) or O(N log N), SC: O(N)
        if (arr == null || arr.length == 0) {
            return 0;
        }

        Set<Integer> uniqueElements = new HashSet<>();
        
        // Pass 1: Insert all elements into the set
        for (int num: arr) {
            uniqueElements.add(num);
        }

        // Pass 2: Put unique elements back into the array
        int i = 0;
        for (int num: uniqueElements) {
            arr[i++] = num;
        }

        // Return the count of unique elements
        return uniqueElements.size();
    }
}
```

### Optimal Approach (Two Pointers - In Place)

Since the array is sorted, we can use a two-pointer approach to modify the array in place without extra space.

1.  Initialize a pointer `i` (for the unique element position) to 0.
2.  Initialize a pointer `j` (for traversal) to 1.
3.  Traverse the array with `j` from 1 to N-1.
4.  If `arr[j]` is **not equal** to `arr[i]`, it means we found a new unique element:
    *   Increment `i` (move the unique position forward).
    *   Set `arr[i] = arr[j]` (place the new unique element at the next unique position).
5.  If `arr[j]` is equal to `arr[i]`, it's a duplicate, so we just increment `j` and continue.
6.  The number of unique elements is `i + 1`.

*   **Time Complexity (TC):** $$O(N)$$ (single pass).
*   **Space Complexity (SC):** $$O(1)$$ (in-place modification).

```java
class RemoveDuplicates {
    public static int removeDuplicatesOptimal(int[] arr) {
        // Optimal Approach: Two Pointers (In-Place)
        // TC: O(N), SC: O(1)
        if (arr == null || arr.length == 0) {
            return 0;
        }

        // i is the pointer for the last unique element found
        int i = 0; 
        
        // j is the pointer for traversal
        for (int j = 1; j < arr.length; j++) {
            // If the element at j is different from the element at i, 
            // it's a new unique element.
            if (arr[j]!= arr[i]) {
                i++; // Move the unique pointer forward
                arr[i] = arr[j]; // Place the new unique element
            }
            // If arr[j] == arr[i], it's a duplicate, so we just increment j
            // and wait for a non-duplicate element.
        }

        // The number of unique elements is i + 1 (since i is 0-indexed)
        return i + 1;
    }
}
```

# Lecture 2 Array Easy


```markdown

# Array Manipulation Problems: Complete Notes

## I. Left Rotate an Array by One Place

### Optimal Approach (In-Place)

**Explanation:** The optimal solution avoids using a second array. It stores the first element in a **temporary variable** (`temp`) and then shifts every subsequent element one position to the left. Finally, the stored element is placed at the last index of the array.

**Complexity Analysis:**
*   **Time Complexity (Worst/Average/Best):** $$O(N)$$ (A single loop runs $N-1$ times to shift elements)
*   **Space Complexity (Extra Space):** $$O(1)$$ (Only a single temporary variable is used)

**Java Code Structure (Optimal):**
```java
public static void leftRotateByOne(int[] arr, int n) {
    if (n <= 1) return;

    // 1. Store the first element
    int temp = arr[0]; //   

    // 2. Shift elements to the left (from index 1 to n-1)
    for (int i = 1; i < n; i++) {
        arr[i - 1] = arr[i]; //   
    }

    // 3. Place the temporary element at the end
    arr[n - 1] = temp; //   
}
```

---

## II. Left Rotate an Array by D Places

### Approach 1: Brute Force (Temporary Array)

**Explanation:** This approach involves three steps: 1) Store the first $D$ elements in a temporary array, 2) Shift the remaining $N-D$ elements to the front, and 3) Copy the $D$ elements from the temporary array back to the end of the main array.

**Complexity Analysis:**
*   **Time Complexity:** $$O(N + D)$$ (Sum of three linear passes: $O(D) + O(N-D) + O(D)$)
*   **Space Complexity (Extra Space):** $$O(D)$$ (For the temporary array to store the first $D$ elements)

### Approach 2: Optimal (Reversal Method)

**Explanation:** This method is an in-place solution that uses three reversals to achieve the rotation, making it highly efficient in terms of space. First, normalize $D$ by calculating $D \pmod N$.

**Reversal Steps:**
1.  Reverse the first $D$ elements (indices $0$ to $D-1$).
2.  Reverse the remaining $N-D$ elements (indices $D$ to $N-1$).
3.  Reverse the entire array (indices $0$ to $N-1$).

**Complexity Analysis:**
*   **Time Complexity (Worst/Average/Best):** $$O(N)$$ (Total time is $O(D) + O(N-D) + O(N) = O(2N)$)
*   **Space Complexity (Extra Space):** $$O(1)$$ (In-place operation)

**Java Code Structure (Optimal - Reversal):**
```java
// Helper function to reverse a subarray
private static void reverse(int[] arr, int start, int end) {
    while (start < end) {
        int temp = arr[start];
        arr[start] = arr[end];
        arr[end] = temp;
        start++;
        end--;
    }
}

public static void leftRotateByD(int[] arr, int n, int d) {
    // 1. Normalize D
    d = d % n; 

    // 2. Reverse first D elements (0 to d-1)
    reverse(arr, 0, d - 1); 

    // 3. Reverse remaining elements (d to n-1)
    reverse(arr, d, n - 1); 

    // 4. Reverse the entire array (0 to n-1)
    reverse(arr, 0, n - 1); 
}
```

---

## III. Move Zeros to the End

### Approach 1: Brute Force (Temporary Array)

**Explanation:** This involves collecting all non-zero elements into a temporary list, copying them back to the front of the original array, and then filling the remaining indices with zeros.

**Complexity Analysis:**
*   **Time Complexity:** $$O(N)$$ (Three linear passes: collecting, copying, filling)
*   **Space Complexity (Extra Space):** $$O(N)$$ (Worst case: temporary array size is proportional to $N$)

### Approach 2: Optimal (Two-Pointer Swap)

**Explanation:** This in-place solution uses two pointers: $J$ tracks the index of the **first zero** encountered, and $I$ iterates through the rest of the array. When $I$ finds a non-zero element, it is swapped with the zero at $J$. Both $I$ and $J$ advance after a swap, ensuring $J$ always points to the next available zero slot for swapping.

**Complexity Analysis:**
*   **Time Complexity (Worst/Average/Best):** $$O(N)$$ (A single pass through the array)
*   **Space Complexity (Extra Space):** $$O(1)$$ (In-place modification)

**Java Code Structure (Optimal - Two-Pointer):**
```java
public static void moveZerosToEnd(int[] arr, int n) {
    // 1. Find the index of the first zero (J)
    int j = -1;
    for (int i = 0; i < n; i++) {
        if (arr[i] == 0) {
            j = i; //   
            break;
        }
    }

    // If no zeros are found, the array is already sorted
    if (j == -1) return; 

    // 2. Iterate and swap (I starts after J)
    for (int i = j + 1; i < n; i++) {
        if (arr[i]!= 0) { // If a non-zero element is found
            // Swap arr[i] (non-zero) with arr[j] (zero)
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;

            j++; // Move J to the next zero position   
        }
        // If arr[i] is 0, only I moves ahead
    }
}
```

---

## IV. Linear Search

**Explanation:** Linear search is the simplest search algorithm. It iterates through the array from the start, comparing each element to the target value until a match is found or the end is reached. It returns the index of the **first occurrence**.

**Complexity Analysis:**
*   **Best Case Time:** $$O(1)$$ (Element found at index 0)
*   **Worst Case Time:** $$O(N)$$ (Element found at the last index or not present)
*   **Average Case Time:** $$O(N)$$
*   **Space Complexity (Extra Space):** $$O(1)$$

**Java Code Structure:**
```java
public static int linearSearch(int[] arr, int n, int target) {
    for (int i = 0; i < n; i++) {
        if (arr[i] == target) { 
            return i; // Return the first occurrence index
        }
    }
    return -1; // Return -1 if not found
}
```

---
------------------------------------------------------------------------------------------------------------------------------------------------------
## V. Union of Two Sorted Arrays

### Approach 1: Brute Force (Using a Set)

**Explanation:** A `Set` data structure (like `TreeSet` in Java) is used to store elements from both arrays. The set automatically handles uniqueness and maintains the sorted order. Elements are then copied from the set to the final result list.

**Complexity Analysis:**
*   **Time Complexity:** $$O(N1 \log N1 + N2 \log N2)$$ (Dominated by the logarithmic time complexity of set insertions)
*   **Space Complexity (Extra Space):** $$O(N1 + N2)$$ (For the set and the final result list in the worst case)

### Approach 2: Optimal (Two-Pointer Method)

**Explanation:** This approach uses two pointers, $I$ for $A1$ and $J$ for $A2$, to merge the two sorted arrays while skipping duplicates. The smaller element is always chosen and added to the union list, provided it is not the same as the last element added. If elements are equal, one is added, and both pointers advance.

**Complexity Analysis:**
*   **Time Complexity (Worst/Average/Best):** $$O(N1 + N2)$$ (A single pass through both arrays)
*   **Space Complexity (Extra Space):** $$O(1)$$ (Excluding the space for the returned list)

**Java Code Structure (Optimal):**
```java
import java.util.ArrayList;
import java.util.List;

public static List<Integer> findUnion(int[] a, int[] b, int n1, int n2) {
    int i = 0; // Pointer for array a 
    int j = 0; // Pointer for array b 
    List<Integer> unionList = new ArrayList<>();

    while (i < n1 && j < n2) {
        if (a[i] <= b[j]) {
            // Check for duplicates or if the list is empty
            if (unionList.isEmpty() || unionList.get(unionList.size() - 1)!= a[i]) {
                unionList.add(a[i]); //   
            }
            i++; 
        } else { // a[i] > b[j]
            // Check for duplicates
            if (unionList.isEmpty() || unionList.get(unionList.size() - 1)!= b[j]) {
                unionList.add(b[j]); //   
            }
            j++; 
        }
    }

    // Handle remaining elements in a
    while (i < n1) {
        if (unionList.get(unionList.size() - 1)!= a[i]) {
            unionList.add(a[i]); //   
        }
        i++;
    }

    // Handle remaining elements in b
    while (j < n2) {
        if (unionList.get(unionList.size() - 1)!= b[j]) {
            unionList.add(b[j]); //   
        }
        j++;
    }

    return unionList;
}
```

---

## VI. Intersection of Two Sorted Arrays

### Approach 1: Brute Force (Using a Visited Array)

**Explanation:** For every element in $A1$, iterate through $A2$ to find a match. A separate `visited` array for $A2$ ensures that each element in $A2$ is matched only once, which is crucial for handling duplicates correctly in the intersection result.

**Complexity Analysis:**
*   **Time Complexity:** $$O(N1 \times N2)$$ (Worst case: nested loops run fully)
*   **Space Complexity (Extra Space):** $$O(N2)$$ (For the `visited` array)

### Approach 2: Optimal (Two-Pointer Method)

**Explanation:** This optimal solution uses two pointers, $I$ for $A1$ and $J$ for $A2$. Since both arrays are sorted, comparison dictates movement:
*   If $A1[I] < A2[J]$, increment $I$.
*   If $A2[J] < A1[I]$, increment $J$.
*   If $A1[I] = A2[J]$, a match is found. Add the element to the intersection list, and increment **both** $I$ and $J$ to look for the next pair, correctly handling duplicate occurrences in both arrays.

**Complexity Analysis:**
*   **Time Complexity (Worst/Average/Best):** $$O(N1 + N2)$$ (A single pass through both arrays)
*   **Space Complexity (Extra Space):** $$O(1)$$ (Excluding the space for the returned list)

**Java Code Structure (Optimal):**
```java
import java.util.ArrayList;
import java.util.List;

public static List<Integer> findIntersection(int[] a, int[] b, int n1, int n2) {
    int i = 0; // Pointer for array a 
    int j = 0; // Pointer for array b 
    List<Integer> intersectionList = new ArrayList<>();

    while (i < n1 && j < n2) {
        if (a[i] < b[j]) {
            i++; // A[i] is smaller, move I   
        } else if (b[j] < a[i]) {
            j++; // B[j] is smaller, move J   
        } else { // a[i] == b[j] - Match found
            intersectionList.add(a[i]); // Add the common element 
            i++; // Move both pointers   
            j++;
        }
    }

    return intersectionList;
}
```


# Lecture 3 Array Easy


## 1. Finding Missing Number in an Array

### Question Sample

You are given an integer $N$ and an array containing $N-1$ distinct numbers. These numbers are between 1 and $N$. The task is to find the single missing number in the array.

**Example:**
$$N = 5$$
$$Array = [1, 2, 4, 5]$$
**Missing Number:** 3

### Brute Force Solution

The **Brute Force** approach involves checking every number from 1 to $N$ to see if it exists in the given array.

*   **Concept:** Iterate from $I=1$ to $N$. For each $I$, perform a linear search across the entire input array. If $I$ is not found, it is the missing number.
*   **Time Complexity:** $O(N^2)$ (Due to the nested loops, although often better in practice because the inner loop breaks early).
*   **Space Complexity:** $O(1)$.

```java
// Brute Force Solution
public class MissingNumber {
    public static int findMissingBruteForce(int[] arr, int N) {
        // Iterate through all expected numbers from 1 to N
        for (int i = 1; i <= N; i++) {
            boolean found = false;
            // Linear search in the array
            for (int j = 0; j < arr.length; j++) {
                if (arr[j] == i) {
                    found = true;
                    break; // Number found, move to the next expected number
                }
            }
            if (!found) {
                return i; // This is the missing number
            }
        }
        return -1; 
    }
}
```

### Better Solution (Hashing)

The **Better Solution** uses a hash array (or frequency array) to keep track of the numbers present in the input array.

*   **Concept:**
    1.  Declare a hash array of size $N+1$ (to cover indices 1 through $N$) and initialize it to zero.
    2.  Iterate through the input array and mark the presence of each element in the hash array (e.g., set `hash[arr[i]] = 1`).
    3.  Iterate through the hash array from index 1 to $N$. The index $I$ where `hash[I]` is still 0 is the missing number.
*   **Time Complexity:** $O(N)$ (One loop for marking, one loop for checking, resulting in $O(2N)$).
*   **Space Complexity:** $O(N)$ (For the hash array).

```java
// Better Solution (Hashing)
public class MissingNumber {
    public static int findMissingHashing(int[] arr, int N) {
        int[] hash = new int[N + 1]; // Size N+1 for indices 1 to N

        // 1. Mark presence of elements in the hash array
        for (int i = 0; i < arr.length; i++) {
            hash[arr[i]] = 1; 
        }

        // 2. Find the missing number by checking for 0
        for (int i = 1; i <= N; i++) {
            if (hash[i] == 0) {
                return i;
            }
        }
        return -1;
    }
}
```

### Optimal Solution 1 (Summation)

The first **Optimal Solution** uses the mathematical property of the sum of natural numbers.

*   **Concept:**
    1.  Calculate the **expected sum** of numbers from 1 to $N$ using the formula: $$Sum = \frac{N(N+1)}{2}$$.
    2.  Calculate the **actual sum** ($S2$) of all elements present in the given array.
    3.  The missing number is the difference: $Missing = Sum - S2$.
*   **Time Complexity:** $O(N)$ (Single loop to calculate the actual sum).
*   **Space Complexity:** $O(1)$.
*   **Note:** For large values of $N$ (e.g., $10^5$), the expected sum can overflow a standard 32-bit integer, so a larger data type like `long` should be used for the sums.

```java
// Optimal Solution 1 (Summation)
public class MissingNumber {
    public static int findMissingSum(int[] arr, int N) {
        // Use long to prevent potential integer overflow for large N
        long expectedSum = (long)N * (N + 1) / 2;
        long actualSum = 0;

        // Calculate the actual sum of array elements
        for (int num: arr) {
            actualSum += num;
        }

        // The difference is the missing number
        return (int)(expectedSum - actualSum);
    }
}
```

### Optimal Solution 2 (XOR)

The second **Optimal Solution** uses the properties of the XOR (Exclusive OR) bitwise operator, which is generally preferred as it avoids potential overflow issues with large numbers.

*   **Core XOR Property:** $X \oplus X = 0$ and $X \oplus 0 = X$.
*   **Concept:**
    1.  Initialize two XOR variables: $XOR1$ (for expected numbers 1 to $N$) and $XOR2$ (for array elements).
    2.  XOR all numbers from 1 to $N$ into $XOR1$.
    3.  XOR all numbers in the array into $XOR2$.
    4.  The final result is $XOR1 \oplus XOR2$. Since all present numbers appear twice (once in the expected set, once in the array), they cancel out to 0, leaving only the missing number.
*   **Time Complexity:** $O(N)$ (Achieved by combining the XOR operations into a single loop).
*   **Space Complexity:** $O(1)$.

```java
// Optimal Solution 2 (XOR)
public class MissingNumber {
    public static int findMissingXOR(int[] arr, int N) {
        int xor1 = 0; // XOR of expected numbers (1 to N)
        int xor2 = 0; // XOR of array elements (N-1 elements)

        // Combine XOR operations into a single loop for O(N)
        for (int i = 0; i < N - 1; i++) {
            xor2 = xor2 ^ arr[i]; // XOR of array elements
            xor1 = xor1 ^ (i + 1); // XOR of expected numbers 1 to N-1
        }
        
        // XOR with N, since the loop only went up to N-1
        xor1 = xor1 ^ N;

        // The result is the missing number
        return xor1 ^ xor2;
    }
}
```

---

## 2. Maximum Consecutive Ones

### Question Sample

Given a binary array (containing only 0s and 1s), return the maximum number of consecutive 1s in the array.

**Example:**
$$Array = [1, 1, 0, 1, 1, 1, 0, 1, 1]$$
**Maximum Consecutive Ones:** 3

### Optimal Solution (Single Pass)

This problem is best solved using a single pass, as the optimal solution is very straightforward.

*   **Concept:** Iterate through the array, maintaining two variables: a `counter` for the current sequence of ones and a `maximum` to store the largest count found so far.
    *   If the element is **1**, increment the `counter` and update `maximum` if the `counter` is greater.
    *   If the element is **0**, reset the `counter` to 0, as the consecutive sequence is broken.
*   **Time Complexity:** $O(N)$ (Single iteration).
*   **Space Complexity:** $O(1)$.

```java
// Optimal Solution (Single Pass)
public class MaxConsecutiveOnes {
    public static int findMaxConsecutiveOnes(int[] nums) {
        int maximum = 0; // Stores the overall maximum count
        int counter = 0; // Stores the current consecutive count

        for (int num: nums) {
            if (num == 1) {
                counter++;
                // Update maximum if the current counter is higher
                maximum = Math.max(maximum, counter); 
            } else {
                // Reset counter when a 0 is encountered
                counter = 0;
            }
        }
        return maximum;
    }
}
```

-------------------------------

## 3. Find Element That Appears Once

### Question Sample

Given an array where every number appears twice, except for one number that appears only once, find that single number.

**Example:**
$$Array = [4, 1, 2, 1, 3, 4, 3]$$
**Single Number:** 2

### Brute Force Solution

The **Brute Force** approach involves checking the frequency of every element in the array.

*   **Concept:** Pick every element in the array. For that element, iterate through the entire array again to count its occurrences. If the count is exactly 1, that element is the answer.
*   **Time Complexity:** $O(N^2)$ (Two nested loops).
*   **Space Complexity:** $O(1)$.

```java
// Brute Force Solution
public class SingleNumber {
    public static int findSingleBruteForce(int[] arr) {
        int N = arr.length;
        for (int i = 0; i < N; i++) {
            int number = arr[i];
            int count = 0;
            // Inner loop to count occurrences
            for (int j = 0; j < N; j++) {
                if (arr[j] == number) {
                    count++;
                }
            }
            if (count == 1) {
                return number;
            }
        }
        return -1; 
    }
}
```

### Better Solution (Hashing/Map)

The **Better Solution** uses a hash map to store the frequency of each element, which is necessary when the numbers are large or negative, making a simple hash array impractical.

*   **Concept:**
    1.  Use a `HashMap` to store elements as keys and their counts as values.
    2.  Iterate through the array, incrementing the count for each number in the map.
    3.  Iterate through the map's entries and return the key whose value (count) is 1.
*   **Time Complexity:** $O(N)$ average case (using an unordered map/HashMap) or $O(N \log M)$ worst case (using an ordered map), where $M$ is the size of the map (approx. $N/2$).
*   **Space Complexity:** $O(N/2)$ (Since roughly half the elements are stored in the map).

```java
// Better Solution (Hashing/Map)
import java.util.HashMap;
import java.util.Map;

public class SingleNumber {
    public static int findSingleHashing(int[] arr) {
        Map<Integer, Integer> freqMap = new HashMap<>();

        // 1. Store frequencies
        for (int num: arr) {
            freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
        }

        // 2. Find the element with frequency 1
        for (Map.Entry<Integer, Integer> entry: freqMap.entrySet()) {
            if (entry.getValue() == 1) {
                return entry.getKey();
            }
        }
        return -1;
    }
}
```

### Optimal Solution (XOR)

The **Optimal Solution** leverages the XOR property to solve the problem in a single pass without using any extra space.

*   **Concept:** XOR all the elements in the array together. Since every number appears twice, all the pairs will cancel each other out ($X \oplus X = 0$). The single non-repeating number will be XORed with 0, leaving the number itself as the final result.
*   **Time Complexity:** $O(N)$ (Single iteration).
*   **Space Complexity:** $O(1)$.

```java
// Optimal Solution (XOR)
public class SingleNumber {
    public static int findSingleXOR(int[] arr) {
        int resultXOR = 0;

        // XOR all elements in the array
        for (int num: arr) {
            resultXOR = resultXOR ^ num;
        }

        // The final result is the single number
        return resultXOR;
    }
}
```

------------------------------

# Lecture 4 Array Easy
---

## Problem Statement

Given an array `A` of size $N$ and an integer $K$, find the length of the **longest contiguous subarray** whose elements sum up to $K$.

A **subarray** is defined as a **contiguous part of the array**.

### Example

Input: `A = [1, 2, 3, 1, 1, 1, 1]`, $K = 3$

Output: 3 (The subarray `[1, 1, 1]` has a sum of 3 and a length of 3, which is the longest possible length in this example).

---

## 1. Brute Force Solution

The Brute Force approach involves generating **all possible subarrays** and calculating the sum for each one to check if it equals $K$.

### Concept

This solution uses three nested loops:
1.  **Outer Loop (i)**: Selects the starting index of the subarray.
2.  **Middle Loop (j)**: Selects the ending index of the subarray.
3.  **Inner Loop (k)**: Iterates from $i$ to $j$ to calculate the sum of the subarray $A[i...j]$.

### Complexity Analysis

*   **Time Complexity**: $O(N^3)$ (due to three nested loops)
*   **Space Complexity**: $O(1)$

### Java Code (Brute Force - O(N³))

```java
class Solution {
    /**
     * Finds the length of the longest subarray with sum K using the O(N^3) Brute Force approach.
     * @param A The input array.
     * @param K The target sum.
     * @return The length of the longest subarray.
     */
    public static int longestSubarrayBruteForceN3(int[] A, int K) {
        int n = A.length;
        int maxLength = 0;

        // Outer loop for starting index 'i'
        for (int i = 0; i < n; i++) {
            // Middle loop for ending index 'j'
            for (int j = i; j < n; j++) {
                long currentSum = 0;
                
                // Inner loop for calculating the sum from i to j
                for (int k = i; k <= j; k++) {
                    currentSum += A[k];
                }

                // Check if the current subarray sum equals K
                if (currentSum == K) {
                    // Length is j - i + 1
                    maxLength = Math.max(maxLength, j - i + 1);
                }
            }
        }
        return maxLength;
    }
}
```

---

## 2. Better Brute Force Solution

This is an optimization of the Brute Force approach by eliminating the third loop used for summation.

### Concept

Instead of recalculating the sum for every subarray $A[i...j]$ from scratch, we calculate the sum **on the fly** as the end pointer $j$ moves.
1.  **Outer Loop (i)**: Selects the starting index.
2.  **Inner Loop (j)**: Selects the ending index, starting from $i$.
3.  The `currentSum` is initialized to 0 for each new starting index $i$, and then $A[j]$ is added to it iteratively as $j$ increases.

### Complexity Analysis

*   **Time Complexity**: $O(N^2)$
*   **Space Complexity**: $O(1)$

### Java Code (Better Brute Force - O(N²))

```java
class Solution {
    /**
     * Finds the length of the longest subarray with sum K using the O(N^2) approach.
     * @param A The input array.
     * @param K The target sum.
     * @return The length of the longest subarray.
     */
    public static int longestSubarrayBetterBruteForceN2(int[] A, int K) {
        int n = A.length;
        int maxLength = 0;

        // Outer loop for starting index 'i'
        for (int i = 0; i < n; i++) {
            long currentSum = 0;
            
            // Inner loop for ending index 'j' and calculating sum on the fly
            for (int j = i; j < n; j++) {
                currentSum += A[j]; // Summation is done incrementally
                
                // Check if the current subarray sum equals K
                if (currentSum == K) {
                    // Length is j - i + 1
                    maxLength = Math.max(maxLength, j - i + 1);
                }
            }
        }
        return maxLength;
    }
}
```

---

## 3. Better Solution (Hashing/Prefix Sum)

This solution uses the concept of **prefix sums** and a hash map to reduce the time complexity significantly. This approach is the **optimal solution** if the array contains **negative numbers** as well as positives and zeros.

### Concept

The core idea is based on **reverse mathematics**:
1.  Maintain a running `prefixSum` as you iterate through the array.
2.  If the current `prefixSum` is $X$, and we are looking for a subarray sum $K$, then the sum of the required subarray must be $K$.
3.  This means the sum of the elements *before* the required subarray must be $X - K$ (i.e., `remaining = prefixSum - K`).
4.  We use a hash map to store previously encountered prefix sums and their corresponding indices: `(prefixSum, index)`.
5.  If we find `remaining` in the hash map, a subarray with sum $K$ exists. The length is calculated as `current_index - map.get(remaining)`.

### Edge Case Handling (Positives and Zeros)

To ensure we find the **longest** subarray, we must store the index of the **first occurrence** of any given prefix sum. If a sum is encountered again, we **do not re-update** its index in the map, because the earlier index will result in a longer subarray length when subtracted from the current index $i$.

### Complexity Analysis

*   **Time Complexity**: $O(N)$ on average (using `HashMap` in Java, which is an unordered map) or $O(N \log N)$ (if using an ordered map like `TreeMap`).
*   **Space Complexity**: $O(N)$ to store prefix sums in the hash map.

### Java Code (Better/Optimal for Negatives - O(N))

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    /**
     * Finds the length of the longest subarray with sum K using the Hashing/Prefix Sum approach.
     * This is the optimal solution for arrays with positives, negatives, and zeros.
     * @param A The input array.
     * @param K The target sum.
     * @return The length of the longest subarray.
     */
    public static int longestSubarrayPrefixSum(int[] A, int K) {
        int n = A.length;
        // Use Long for sum to prevent overflow, as suggested in the lecture
        long sum = 0; 
        int maxLength = 0;
        
        // Map to store (prefixSum, index)
        Map<Long, Integer> preSumMap = new HashMap<>();
        
        // Initialize map with (0, -1) to handle the case where the subarray starts from index 0
        // The lecture implicitly handles this by checking if sum == K first, but (0, -1) is standard practice.
        // The lecture's logic:
        // preSumMap.put(0L, -1); 

        for (int i = 0; i < n; i++) {
            sum += A[i];

            // 1. Check if the prefix sum itself equals K (subarray starts from index 0)
            if (sum == K) {
                // Length is i + 1 (0-based index)
                maxLength = Math.max(maxLength, i + 1);
            }

            // 2. Check for the remaining sum (prefixSum - K)
            long remaining = sum - K;

            if (preSumMap.containsKey(remaining)) {
                // If found, calculate the length
                int len = i - preSumMap.get(remaining);
                maxLength = Math.max(maxLength, len);
            }

            // 3. Update the map: ONLY store the first occurrence of a prefix sum
            // This ensures we find the LONGEST subarray (by taking the leftmost index)
            if (!preSumMap.containsKey(sum)) {
                preSumMap.put(sum, i);
            }
        }
        return maxLength;
    }
}
```

---

## 4. Optimal Solution (Two Pointers/Sliding Window)

This approach is the **most optimal** solution, but it is **only applicable** when the array contains **positives and zeros**.

### Concept

This method uses a **Sliding Window** defined by two pointers, `left` and `right`.
1.  **Expansion**: The `right` pointer always moves forward, expanding the window and adding the new element to the `sum`.
2.  **Contraction (Shrinking)**: If the `sum` becomes **greater than $K$**, the window must shrink. The `left` pointer moves forward, and the element $A[left]$ is subtracted from the `sum`. This continues until `sum <= K`.
3.  **Check**: If at any point `sum == K`, the current length (`right - left + 1`) is compared with the `maxLength` and updated if necessary.

### Complexity Analysis

*   **Time Complexity**: $O(2N)$ or $O(N)$. Both the `left` and `right` pointers traverse the array at most once, making it a linear time complexity solution.
*   **Space Complexity**: $O(1)$ as no extra data structures are used.

### Java Code (Optimal for Positives/Zeros - O(N))

```java
class Solution {
    /**
     * Finds the length of the longest subarray with sum K using the Two-Pointer/Sliding Window approach.
     * This is the optimal solution for arrays with only positives and zeros.
     * @param A The input array.
     * @param K The target sum.
     * @return The length of the longest subarray.
     */
    public static int longestSubarrayTwoPointers(int[] A, int K) {
        int n = A.length;
        int left = 0;
        long sum = 0; // Use long for sum
        int maxLength = 0;

        // The right pointer 'right' iterates through the array, expanding the window
        for (int right = 0; right < n; right++) {
            // 1. Expand the window: Add the current element to the sum
            sum += A[right];

            // 2. Shrink the window: If sum exceeds K, move the left pointer
            // This loop ensures the sum is always <= K before checking for equality
            while (sum > K && left <= right) {
                // Subtract the element at the left pointer
                sum -= A[left];
                // Move the left pointer forward
                left++;
            }

            // 3. Check for equality: If the sum equals K, update the max length
            if (sum == K) {
                // Current length is right - left + 1
                maxLength = Math.max(maxLength, right - left + 1);
            }
        }

        return maxLength;
    }
}
```