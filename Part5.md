
# Post #5 — Recursion on Linked Lists (Java Modular Classes)

## Problems we’ll cover

1. **Reverse a Linked List** — `ReverseLinkedList.java` + `ReverseLinkedListTest.java` (LC 206)
2. **Remove N-th Node from End** — `RemoveNthFromEnd.java` + `RemoveNthFromEndTest.java` (LC 19)
3. **Merge Two Sorted Lists** — `MergeTwoLists.java` + `MergeTwoListsTest.java` (LC 21)
4. **Palindrome Linked List Check** — `PalindromeLinkedList.java` + `PalindromeLinkedListTest.java` (LC 234)
5. **Linked List Cycle Detection (DFS style)** — `LinkedListCycle.java` + `LinkedListCycleTest.java` (LC 141)

We’ll define a **common ListNode class**:

```java
// ListNode.java
public class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
}
```

---

## 1️⃣ Reverse a Linked List (LC 206)

```java
// ReverseLinkedList.java
public class ReverseLinkedList {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode newHead = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }
}
```

```java
// ReverseLinkedListTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class ReverseLinkedListTest {
    @Test
    public void testReverseList() {
        ListNode head = new ListNode(1);
        head.next = new ListNode(2);
        head.next.next = new ListNode(3);
        ReverseLinkedList solver = new ReverseLinkedList();
        ListNode reversed = solver.reverseList(head);
        assertEquals(3, reversed.val);
        assertEquals(2, reversed.next.val);
    }
}
```

---

## 2️⃣ Remove N-th Node from End (LC 19)

```java
// RemoveNthFromEnd.java
public class RemoveNthFromEnd {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        remove(dummy, n);
        return dummy.next;
    }

    private int remove(ListNode node, int n) {
        if (node == null) return 0;
        int pos = remove(node.next, n) + 1;
        if (pos == n + 1) node.next = node.next.next;
        return pos;
    }
}
```

```java
// RemoveNthFromEndTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class RemoveNthFromEndTest {
    @Test
    public void testRemoveNthFromEnd() {
        ListNode head = new ListNode(1);
        head.next = new ListNode(2);
        head.next.next = new ListNode(3);
        RemoveNthFromEnd solver = new RemoveNthFromEnd();
        ListNode result = solver.removeNthFromEnd(head, 2);
        assertEquals(1, result.val);
        assertEquals(3, result.next.val);
    }
}
```

---

## 3️⃣ Merge Two Sorted Lists (LC 21)

```java
// MergeTwoLists.java
public class MergeTwoLists {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) return l2;
        if (l2 == null) return l1;
        if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
```

```java
// MergeTwoListsTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class MergeTwoListsTest {
    @Test
    public void testMergeTwoLists() {
        ListNode l1 = new ListNode(1);
        l1.next = new ListNode(2);
        ListNode l2 = new ListNode(1);
        l2.next = new ListNode(3);
        MergeTwoLists solver = new MergeTwoLists();
        ListNode result = solver.mergeTwoLists(l1, l2);
        assertEquals(1, result.val);
        assertEquals(1, result.next.val);
        assertEquals(2, result.next.next.val);
        assertEquals(3, result.next.next.next.val);
    }
}
```

---

## 4️⃣ Palindrome Linked List Check (LC 234)

```java
// PalindromeLinkedList.java
public class PalindromeLinkedList {
    private ListNode front;

    public boolean isPalindrome(ListNode head) {
        front = head;
        return dfs(head);
    }

    private boolean dfs(ListNode node) {
        if (node == null) return true;
        boolean res = dfs(node.next) && (node.val == front.val);
        front = front.next;
        return res;
    }
}
```

```java
// PalindromeLinkedListTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class PalindromeLinkedListTest {
    @Test
    public void testIsPalindrome() {
        ListNode head = new ListNode(1);
        head.next = new ListNode(2);
        head.next.next = new ListNode(1);
        PalindromeLinkedList solver = new PalindromeLinkedList();
        assertTrue(solver.isPalindrome(head));
    }
}
```

---

## 5️⃣ Linked List Cycle Detection (LC 141)

```java
// LinkedListCycle.java
import java.util.*;

public class LinkedListCycle {
    public boolean hasCycle(ListNode head) {
        Set<ListNode> seen = new HashSet<>();
        return dfs(head, seen);
    }

    private boolean dfs(ListNode node, Set<ListNode> seen) {
        if (node == null) return false;
        if (!seen.add(node)) return true;
        return dfs(node.next, seen);
    }
}
```

```java
// LinkedListCycleTest.java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class LinkedListCycleTest {
    @Test
    public void testHasCycle() {
        ListNode head = new ListNode(3);
        ListNode node2 = new ListNode(2);
        ListNode node0 = new ListNode(0);
        ListNode nodeNeg4 = new ListNode(-4);
        head.next = node2; node2.next = node0; node0.next = nodeNeg4; nodeNeg4.next = node2; // cycle

        LinkedListCycle solver = new LinkedListCycle();
        assertTrue(solver.hasCycle(head));
    }
}
```

---

## ✅ Engineering / Practice Notes

* Recursive linked list problems often rely on **DFS down to null**, then backtrack during unwinding.
* Be careful with **mutable references** (e.g., `front` in palindrome check).
* Recursive depth is O(n), so **stack overflow** can occur for very long lists — iterative versions exist.
* Using **HashSet** for cycle detection is simple but O(n) extra space; Floyd’s Tortoise & Hare is O(1).

---

Next post (Post #6) will cover **Divide & Conquer Recursion**: binary search, merge sort, quick select — modular classes + JUnit tests + complexity analysis.


