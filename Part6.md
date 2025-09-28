
# Post #6 — Divide & Conquer Recursion (Java Modular Classes)

## Problems we’ll cover

1. **Binary Search** — `BinarySearch.java` + `BinarySearchTest.java` (LC 704)
2. **Merge Sort** — `MergeSort.java` + `MergeSortTest.java` (classic problem)
3. **Quick Select (K-th Largest/Smallest)** — `QuickSelect.java` + `QuickSelectTest.java` (LC 215)
4. **Maximum Subarray (Divide & Conquer)** — `MaxSubarrayDivideConquer.java` + `MaxSubarrayDivideConquerTest.java` (LC 53)
5. **Pow(x, n)** — `PowXN.java` + `PowXNTest.java` (LC 50)

---

## 1️⃣ Binary Search (LC 704)

```java
// BinarySearch.java
public class BinarySearch {
    public int search(int[] nums, int target) {
        return binarySearch(nums, target, 0, nums.length - 1);
    }

    private int binarySearch(int[] nums, int target, int left, int right) {
        if (left > right) return -1;
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) return mid;
        else if (nums[mid] < target) return binarySearch(nums, target, mid + 1, right);
        else return binarySearch(nums, target, left, mid - 1);
    }
}
```

```java
// BinarySearchTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class BinarySearchTest {
    @Test
    public void testSearch() {
        BinarySearch solver = new BinarySearch();
        int[] nums = {-1,0,3,5,9,12};
        assertEquals(4, solver.search(nums, 9));
        assertEquals(-1, solver.search(nums, 2));
    }
}
```

---

## 2️⃣ Merge Sort

```java
// MergeSort.java
import java.util.Arrays;

public class MergeSort {
    public void mergeSort(int[] arr) {
        if (arr == null || arr.length < 2) return;
        mergeSortHelper(arr, 0, arr.length - 1);
    }

    private void mergeSortHelper(int[] arr, int left, int right) {
        if (left >= right) return;
        int mid = left + (right - left) / 2;
        mergeSortHelper(arr, left, mid);
        mergeSortHelper(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }

    private void merge(int[] arr, int left, int mid, int right) {
        int[] tmp = new int[right - left + 1];
        int i = left, j = mid + 1, k = 0;
        while (i <= mid && j <= right) {
            tmp[k++] = arr[i] <= arr[j] ? arr[i++] : arr[j++];
        }
        while (i <= mid) tmp[k++] = arr[i++];
        while (j <= right) tmp[k++] = arr[j++];
        System.arraycopy(tmp, 0, arr, left, tmp.length);
    }
}
```

```java
// MergeSortTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class MergeSortTest {
    @Test
    public void testMergeSort() {
        int[] arr = {5,2,9,1,5,6};
        MergeSort solver = new MergeSort();
        solver.mergeSort(arr);
        assertArrayEquals(new int[]{1,2,5,5,6,9}, arr);
    }
}
```

---

## 3️⃣ Quick Select (K-th Largest) (LC 215)

```java
// QuickSelect.java
import java.util.Random;

public class QuickSelect {
    private final Random rand = new Random();

    public int findKthLargest(int[] nums, int k) {
        return quickSelect(nums, 0, nums.length - 1, nums.length - k);
    }

    private int quickSelect(int[] nums, int left, int right, int k) {
        int pivotIndex = left + rand.nextInt(right - left + 1);
        int pivot = nums[pivotIndex];
        int i = left, j = right;
        swap(nums, pivotIndex, right);
        for (int l = left; l < right; l++) {
            if (nums[l] < pivot) swap(nums, i++, l);
        }
        swap(nums, i, right);

        if (i == k) return nums[i];
        else if (i < k) return quickSelect(nums, i + 1, right, k);
        else return quickSelect(nums, left, i - 1, k);
    }

    private void swap(int[] nums, int i, int j) {
        int t = nums[i]; nums[i] = nums[j]; nums[j] = t;
    }
}
```

```java
// QuickSelectTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class QuickSelectTest {
    @Test
    public void testFindKthLargest() {
        QuickSelect solver = new QuickSelect();
        int[] nums = {3,2,1,5,6,4};
        assertEquals(5, solver.findKthLargest(nums, 2));
    }
}
```

---

## 4️⃣ Maximum Subarray (Divide & Conquer) (LC 53)

```java
// MaxSubarrayDivideConquer.java
public class MaxSubarrayDivideConquer {
    public int maxSubArray(int[] nums) {
        return helper(nums, 0, nums.length - 1);
    }

    private int helper(int[] nums, int left, int right) {
        if (left == right) return nums[left];
        int mid = left + (right - left) / 2;
        int leftMax = helper(nums, left, mid);
        int rightMax = helper(nums, mid + 1, right);
        int crossMax = crossSum(nums, left, mid, right);
        return Math.max(Math.max(leftMax, rightMax), crossMax);
    }

    private int crossSum(int[] nums, int left, int mid, int right) {
        int leftSum = Integer.MIN_VALUE, sum = 0;
        for (int i = mid; i >= left; i--) {
            sum += nums[i];
            leftSum = Math.max(leftSum, sum);
        }
        int rightSum = Integer.MIN_VALUE;
        sum = 0;
        for (int i = mid + 1; i <= right; i++) {
            sum += nums[i];
            rightSum = Math.max(rightSum, sum);
        }
        return leftSum + rightSum;
    }
}
```

```java
// MaxSubarrayDivideConquerTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class MaxSubarrayDivideConquerTest {
    @Test
    public void testMaxSubArray() {
        MaxSubarrayDivideConquer solver = new MaxSubarrayDivideConquer();
        int[] nums = {-2,1,-3,4,-1,2,1,-5,4};
        assertEquals(6, solver.maxSubArray(nums));
    }
}
```

---

## 5️⃣ Pow(x, n) (LC 50)

```java
// PowXN.java
public class PowXN {
    public double myPow(double x, int n) {
        if (n == 0) return 1;
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        double half = myPow(x, (int)(N / 2));
        return N % 2 == 0 ? half * half : half * half * x;
    }
}
```

```java
// PowXNTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class PowXNTest {
    @Test
    public void testMyPow() {
        PowXN solver = new PowXN();
        assertEquals(1024.0, solver.myPow(2.0, 10));
        assertEquals(0.25, solver.myPow(2.0, -2));
    }
}
```

---

## ✅ Engineering / Practice Notes

* **Divide & Conquer pattern**: split problem → solve recursively → combine results.
* **Binary search, quick select**: rely on index manipulation.
* **Merge sort, max subarray**: combine results carefully; pay attention to cross-boundary cases.
* **Pow(x, n)**: recursive exponentiation reduces time to O(log n).
* Time & space complexity analysis is critical in interviews.

---

Next post (Post #7) will cover **Dynamic Programming via Recursion / Memoization**, including Fibonacci, climbing stairs, unique paths, and more — modular + testable + LeetCode references.


