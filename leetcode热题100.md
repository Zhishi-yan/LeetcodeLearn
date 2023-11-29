
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
        ListNode right=head;
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


















