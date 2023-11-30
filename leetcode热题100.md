#### [examLink](https://leetcode.cn/studyplan/top-100-liked/)

##### 多维动态规划

###### [最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/submissions/485558157/?envType=study-plan-v2&envId=top-100-liked)

```java

import java.util.Arrays;

class Solution {
    public String longestPalindrome(String s) {
        int[][] dp = new int[s.length()][s.length()];
        int maxL = 0;//定义最长的长度
        String retStr = "";//定义一个返回字符串
        //使用动态规划dp=0代表不是回文串，若是回文串，dp=长度
        if (s.isEmpty()) {
            return "";
        }
        for (int l = 0; l < s.length(); l++) {
            for (int i = 0; i + l < s.length(); i++) {
                if (l == 0) {
                    //l=0的时候dp[i][i+l] = dp[i][i]
                    dp[i][i + l] = 1;//长度为0的时候，单个字符的长度为1
                    retStr = dp[i][i + l] > maxL ? s.substring(i, i + l + 1) : retStr;
                }
                //如果长度为1(2个字符) 或者2(3个字符) 保证两端的字符相等即可
                if (l == 1 || l == 2) {
                    dp[i][i + l] = s.charAt(i) == s.charAt(i + l) ? l + 1 : 0;//满足两端字符相等，则该字符串为回文串，长度为l+1，否则为0
                    retStr = dp[i][i + l] > maxL ? s.substring(i, i + l + 1) : retStr;
                }
                //如果长度大于等于3，中间至少隔了两个字符，首先要保证两端的字符相等，然后再看中间的
                if (l > 2) {
                    dp[i][i + l] = (s.charAt(i) == s.charAt(i + l)) && (dp[i + 1][i + l - 1] > 0) ? l + 1 : 0;
                    retStr = dp[i][i + l] > maxL ? s.substring(i, i + l + 1) : retStr;
                }
            }
        }
        maxL = Arrays.stream(dp).flatMapToInt(Arrays::stream).max().orElse(0);
        System.out.println(maxL);
        return retStr;
    }
}

public class Main {
    public static void main(String[] args) {
        String str = "babad";
        Solution solution = new Solution();
        String ret = solution.longestPalindrome(str);
        System.out.println(ret);
    }
}
```

##### 链表

###### [相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/description/?envType=study-plan-v2&envId=top-100-liked)

```java
import java.util.HashMap;

class ListNode {
    int val;
    ListNode next;

    ListNode(int x) {
        val = x;
        next = null;
    }
}

class myLinkedList {
    int size;
    ListNode head;

    public myLinkedList() {
        size = 0;
        head = new ListNode(0);
    }

    public void addTail(int val) {
        ListNode add = new ListNode(val);
        ListNode cur = head;
        if (size == 0) {
            cur.next = add;
        } else {
            for (int i = 0; i < size; i++) {
                cur = cur.next;
            }
            cur.next = add;
        }
        size++;
    }

    public void jointTail(myLinkedList list) {
        ListNode cur = head;
        if (size == 0) {
            cur.next = list.head.next;
        } else {
            for (int i = 0; i < size; i++) {
                cur = cur.next;
            }
            cur.next = list.head.next;
        }
        size = size + list.size;
    }
}

class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        //双指针，分别指向两个链表，分别遍历两个链表，走到头之后，将指针指向另一个链表的头节点
        if (headA == null || headB == null) {
            return null;
        }
        ListNode curA = headA;
        ListNode curB = headB;
        while (curA != curB) {
            curA = curA == null ? headB : curA.next;
            curB = curB == null ? headA : curB.next;
        }
        return curA;
    }

    public ListNode getIntersectionNode1(ListNode headA, ListNode headB) {
        //使用HashMap，使用哈希表存储每一个节点
        //先把链表A的每一个节点存入哈希表
        //然后再把链表B的每一个节点存入哈希表，当遇到已经存在的节点，就停止存储，把该节点输出
        HashMap<ListNode, Integer> map = new HashMap<>();
        ListNode cur = headA;
        while (cur != null) {
            //只要cur不等于null，就一直放入表中
            map.put(cur, 1);
            cur = cur.next;
        }
        cur = headB;
        while (cur != null) {
            //若val不等于0，则当前的键为相交的节点
            int val = map.getOrDefault(cur, 0);
            if (val != 0) {
                return cur;
            }
            cur = cur.next;
        }
        return null;
    }
}

public class Main {
    public static void main(String[] args) {
        int[] arrA = {4, 1};
        myLinkedList listA = new myLinkedList();
        for (int i = 0; i < arrA.length; i++) {
            listA.addTail(arrA[i]);
        }

        int[] arrB = {5, 6, 1};
        myLinkedList listB = new myLinkedList();
        for (int i = 0; i < arrB.length; i++) {
            listB.addTail(arrB[i]);
        }

        int[] intersectionArr = {8, 4, 5};
        myLinkedList intersection = new myLinkedList();
        for (int i = 0; i < intersectionArr.length; i++) {
            intersection.addTail(intersectionArr[i]);
        }

        listA.jointTail(intersection);
        listB.jointTail(intersection);

        Solution solution = new Solution();
        ListNode ret = solution.getIntersectionNode1(listA.head, listB.head);
    }
}
```

###### [设计链表](https://leetcode.cn/problems/design-linked-list/description/)

```java
class MyLinkedList {
    int size;
    node head;

    public MyLinkedList() {
        size = 0;
        head = new node(0);
    }


    //获取index索引所对应的节点的val
    public int get(int index) {
        node currentNode;//创建一个节点，用于存储当前查询到的节点
        //一直查找到Index，因为有头节点，而头节点又不作数，因此i要遍历到index，包含index
        if (index < 0 || index >= size) {
            return -1;
        }
        //先指向头节点，才能够遍历查询
        currentNode = head;
        for (int i = 0; i <= index; i++) {
            //节点指向下一个
            currentNode = currentNode.next;
        }
        return currentNode.val;
    }

    public void addAtHead(int val) {
        node add = new node(val);
        if (size == 0) {
            head.next = add;
        } else {
            add.next = head.next;
            head.next = add;
        }
        size++;
    }

    public void addAtTail(int val) {
        node add = new node(val);
        node cur = head;
        for (int i = 0; i < size; i++) {
            cur = cur.next;
        }
        cur.next = add;
        size++;
    }

    public void addAtIndex(int index, int val) {
        if (index < 0 || index > size) {
            return;
        }
        size++;
        node temp;
        temp = head;
        node add = new node(val);
        for (int i = 0; i < index; i++) {
            temp = temp.next;
        }
        //在temp节点后插入add节点
        add.next = temp.next;
        temp.next = add;
    }

    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }
        size--;
        node temp = head;
        for (int i = 0; i < index; i++) {
            temp = temp.next;
        }
        temp.next = temp.next.next;

    }
}

class node {
    int val;
    node next;

    public node(int val) {
        this.val = val;
    }
}

public class Main {
    public static void main(String[] args) {

        MyLinkedList myLinkedList = new MyLinkedList();
        myLinkedList.addAtHead(1);
        myLinkedList.addAtTail(3);
        myLinkedList.addAtIndex(1, 2);    // 链表变为 1->2->3
        myLinkedList.get(1);              // 返回 2
        myLinkedList.deleteAtIndex(1);    // 现在，链表变为 1->3
        myLinkedList.get(1);              // 返回 3
    }
}
```

###### [反转链表](https://leetcode.cn/problems/UHnkqh/)

```java
public class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode left = null;
        ListNode middle = head;
        ListNode right = head;
        while (right != null) {
            right = middle.next;
            middle.next = left;
            left = middle;
            middle = right;
        }
        return left;
    }
}

```

###### [回文链表](https://leetcode.cn/problems/palindrome-linked-list/?envType=study-plan-v2&envId=top-100-liked)

```java
import java.util.ArrayDeque;
import java.util.Deque;

class myLinkedList {
    int size;
    ListNode head;

    public myLinkedList() {
        this.size = 0;
        head = new ListNode(0);
    }

    public void addTail(int val) {
        ListNode add = new ListNode(val);
        ListNode cur = head;
        if (size == 0) {
            cur.next = add;
        } else {
            for (int i = 0; i < size; i++) {
                cur = cur.next;
            }
            cur.next = add;
        }
        size++;
    }
}

class ListNode {
    int val;
    ListNode next;

    ListNode() {
    }

    ListNode(int val) {
        this.val = val;
    }

    ListNode(int val, ListNode next) {
        this.val = val;
        this.next = next;
    }
}

class Solution {
    public boolean isPalindrome(ListNode head) {
        //把链表中的元素保存到双端队列
        Deque<Integer> deque = new ArrayDeque<>();
        ListNode cur = head;
        while (cur != null) {
            deque.add(cur.val);
            cur = cur.next;
        }
        if (deque.size() == 1) {
            return true;
        }
        //遍历双端队列的头和尾部，如果头和尾相等，则把头和尾弹出，否则就直接返回false
        while (deque.size() > 1) {

            if (deque.peek() == deque.peekLast()) {
                deque.pop();
                deque.removeLast();
            } else {
                return false;
            }
        }


        return true;
    }
}

public class Main {
    public static void main(String[] args) {
        //创建链表
//        int[] arr = {1, 2, 2, 1};
        int[] arr = {0};
        Solution solution = new Solution();
        myLinkedList mylist = new myLinkedList();
        for (int i = 0; i < arr.length; i++) {
            mylist.addTail(arr[i]);
        }
        solution.isPalindrome(mylist.head.next);
    }
}
```

###### [环形链表](https://leetcode.cn/problems/linked-list-cycle/?envType=study-plan-v2&envId=top-100-liked)

```java
public class Solution {
    //使用map做这题
    public boolean hasCycle(ListNode head) {
        HashMap<ListNode, Integer> map = new HashMap<>();
        ListNode cur = head;
        while (cur != null) {
            int val = map.getOrDefault(cur, 0);
            if (val == 0) {
                map.put(cur, 1);
            } else {
                return true;
            }
            cur = cur.next;
        }
        return false;
    }
}
```

###### [环形链表2](https://leetcode.cn/problems/linked-list-cycle-ii/?envType=study-plan-v2&envId=top-100-liked)

```java
public class Solution {
    //使用map做这题
    public ListNode detectCycle(ListNode head) {
        HashMap<ListNode, Integer> map = new HashMap<>();
        ListNode cur = head;
        while (cur != null) {
            int val = map.getOrDefault(cur, 0);
            if (val == 0) {
                map.put(cur, 1);
            } else {
                return cur;
            }
            cur = cur.next;
        }
        return null;
    }
}
```

