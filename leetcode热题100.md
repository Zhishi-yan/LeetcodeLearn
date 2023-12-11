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

###### [双向链表](https://blog.csdn.net/qq_54469537/article/details/127142721)

```java


```

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

###### [合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/description/?envType=study-plan-v2&envId=top-100-liked)

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        if (list1 == null) {
            return list2;
        } else if (list2 == null) {
            return list1;
        }
        if (list1.val < list2.val) {
            list1.next = mergeTwoLists(list1.next, list2);
            return list1;
        } else {
            list2.next = mergeTwoLists(list1, list2.next);
            return list2;
        }
    }
}
```


###### [LRU缓存](https://blog.csdn.net/cr7258/article/details/124841521)
> 在操作系统内部，由于内存资源十分有限，而每个进程又都希望独享一大块内存。
> 所以诞生了一种“虚拟内存”机制，将进程的一部分内容暂存在磁盘中，在需要时再进行数据交换将其放入内存。
> 这个过程就需要用到缓存算法机制进行置换。

> LRU(Least Recently Used) 最近最少使用。最久未被使用的数据应该最早被淘汰掉。
> 将数据按照访问的时间，形成一个有序序列，最久不被使用的数据，应最早被删掉。

> 哈希表 + 双向链表 实现LRU缓存 
> ![img_1.png](img_1.png)
> 1. 为什么使用双向链表，而不是单向链表
>    * 因为删除最后一个节点，使用单向链表需要从头遍历到尾部。
>    * 而使用双向链表，可以直接根据虚拟尾节点直接取到最后一个节点。
> 2. 哈希表中已经保存了key，为什么链表中还要存储key和value，只存储value不就行了？
>    * 当删除节点时，也要删除哈希表中的键值对。此时需要获取节点中的key，才能删除哈希表中的键值对。
> 3. 虚拟头节点和虚拟尾节点有什么用？
>    * 虚拟节点在链表中被广泛应用，又称为哨兵节点。通常不保存任何数据，使用虚拟节点便于处理节点的插入和删除操作。
```java 
import java.util.HashMap;

class LRUCache {
    int capacity;
    //链表节点个数
    int size;
    //创建一个双向链表需要头和尾
    node head;
    node tail;
    //创建一个哈希表
    HashMap<Integer, node> map = new HashMap<>();

    //定义节点
    class node {
        int val;
        int key;
        node next;
        node pre;

        //默认构造，无参构造
        public node() {
        }

        //有参构造
        public node(int key, int val) {
            this.val = val;
            this.key = key;
        }
    }

    public LRUCache(int capacity) {
        //LRUCache的容积
        this.capacity = capacity;
        //双向链表的节点个数
        this.size = 0;
        head = new node();
        tail = new node();
        //双向链表的头尾相连
        head.next = tail;
        tail.pre = head;
    }

    public int get(int key) {
        if (map.get(key) == null) {
            return -1;
        } else {
            moveToHead(map.get(key));
            return map.get(key).val;
        }
    }

    public void put(int key, int value) {
        if (map.get(key) == null) {
            //map中没有该键值对
            node add = new node(key, value);
            map.put(key, add);
            //往头部添加节点，如果size超过capacity，则把最后一个节点删除，把哈希表中的键值对也删除
            addHead(add);
        } else {
            //map中已有该键值对,把节点的val替换
            map.get(key).val = value;
            //把节点移动至双向链表的头部
            moveToHead(map.get(key));
        }
    }


    public void addHead(node add) {
        add.pre = head;
        add.next = head.next;
        head.next.pre = add;
        head.next = add;
        size++;
        //若双向链表的尺寸超出了缓存容积，则删除最后一个节点，以及相应的键值对
        if (size > capacity) {
            //先删除哈希表中的键值对
            map.remove(tail.pre.key);
            //再从链表中删除该节点
            tail.pre = tail.pre.pre;
            tail.pre.next.next = null;
            tail.pre.next.pre = null;
            tail.pre.next = tail;
        }
    }

    // 移动节点到头部
    public void moveToHead(node nodeTemp) {
        nodeTemp.pre.next = nodeTemp.next;
        nodeTemp.next.pre = nodeTemp.pre;
        nodeTemp.pre = head;
        nodeTemp.next = head.next;
        head.next.pre = nodeTemp;
        head.next = nodeTemp;
    }

}

public class Main {
    public static void main(String[] args) {
        LRUCache cache = new LRUCache(2);
        cache.put(1, 1);
        cache.put(2, 2);
        cache.get(1);
        cache.put(3, 3);
        cache.get(2);
        cache.put(4, 4);
        cache.get(1);
        cache.get(3);
        cache.get(4);
    }
}
```
###### [LFU](https://leetcode.cn/problems/lfu-cache/solutions/2457716/tu-jie-yi-zhang-tu-miao-dong-lfupythonja-f56h/)
```java
import java.util.TreeMap;

class LFUCache {
    //使用两个哈希表，一个存储所有的节点(键-节点)，另一个按照使用频率维护多个链表(fre-list),每一个频率维护一个链表。
    //与LRU不同的时，只要对节点操作，fre就会加1，就需要把节点从原来的链表移动至另外一个链表，因此不需要moveToHead这个函数
    //第一次put某个节点时，在fre=1的链表头部添加节点。
    //get或第二次put时，在fre++的链表头部添加节点。
    //超出capacity时，删除fre最低的链表的尾部节点，并删除keyTable中的键值对
    int capacity;
    int curSize;
    TreeMap<Integer, Node> keyTable = new TreeMap<>();
    TreeMap<Integer, MylinkedList> freTable = new TreeMap<>();

    class Node {
        int key;
        int val;
        int fre;
        Node next = null;
        Node pre = null;

        public Node() {
            key = 0;
            val = 0;
            fre = 0;
        }

        public Node(int key, int val, int fre) {
            this.key = key;
            this.val = val;
            this.fre = fre;
        }
    }

    class MylinkedList {
        //创建头节点尾节点
        Node head = new Node();
        Node tail = new Node();
        int size = 0;

        public MylinkedList() {

            head.next = tail;
            tail.pre = head;
            this.size = 0;
        }

        public void addHead(Node add) {
            //链表头部添加元素，size+1
            add.pre = this.head;
            add.next = this.head.next;
            this.head.next.pre = add;
            this.head.next = add;
            this.size++;
        }

        public void removeNode(Node node) {
            if (this.size == 0) {
                return;
            } else {
                node.pre.next = node.next;
                node.next.pre = node.pre;
                size--;
            }
        }

        public void removeTail() {
            //链表尾部删除元素，size-1
            if (this.size == 0) {
                return;
            } else {
                //获取键值
                int key = this.tail.pre.key;
                //先从链表中删除节点
                this.tail.pre.pre.next = this.tail;
                this.tail.pre = this.tail.pre.pre;
                //把节点的pre 和 next指针 置空
                keyTable.get(key).pre = null;
                keyTable.get(key).next = null;
                //从哈希表中删除键值对
                keyTable.remove(key);
                size--;
            }
        }
    }

    public LFUCache(int capacity) {
        this.capacity = capacity;
    }

    public int get(int key) {
        Node node = keyTable.get(key);
        //如果在keyTable中查询不到，则返回-1
        if (node == null) {
            return -1;
        } else {
            //从原来的链表中删掉该节点
            freTable.get(node.fre).removeNode(node);
            //如果在keyTable中能查询到key，则fre+1
            node.fre++;
            putList(node);
            return node.val;
        }
    }

    public void put(int key, int value) {
        //首先查询，keyTable中是否有当前key，没有就添加到keyTable和freTable中的链表中，添加后判断总size是否超出capacity
        Node node = keyTable.get(key);
        if (node == null) {
            //keyTable中不存在key
            int fre = 1;
            node = new Node(key, value, fre);
            keyTable.put(key, node);

            curSize++;
            //若当前的尺寸超出了capacity,则移除频率最低链表的尾部节点
            if (curSize > capacity) {
                for (int keyFre : freTable.keySet()) {
                    if (freTable.get(keyFre).size > 0) {
                        freTable.get(keyFre).removeTail();
                        curSize--;
                        putList(node);
                        return;
                    }
                }
            }
            putList(node);

        } else {
            //若，keyTable中已经有当前key,则修改key对应的value，从原来的链表中移除，然后将fre+1,再添加到新链表中
            node.val = value;
            freTable.get(node.fre).removeNode(node);
            node.fre++;
            putList(node);
        }

    }

    public void putList(Node node) {
        //查询是否已经创建了当前频率的链表
        MylinkedList newList = freTable.get(node.fre);
        if (newList == null) {
            //未创建
            newList = new MylinkedList();//新建一个链表
            newList.addHead(node);//链表中添加节点
            freTable.put(node.fre, newList);//链表插入到freTable中
        } else {
            //已创建
            newList.addHead(node);
            freTable.put(node.fre, newList);//链表插入到freTable中
        }
    }

}


public class Main {
    public static void main(String[] args) {
        LFUCache cache = new LFUCache(3);
        cache.put(1, 1);
        cache.put(2, 2);
        cache.put(3, 3);
        cache.put(4, 4);
        cache.get(4);
        cache.get(3);
        cache.get(2);
        cache.get(1);
        cache.put(5, 5);
        cache.get(1);
        cache.get(2);
        cache.get(3);
        cache.get(4);
        cache.get(5);

        return;
    }
}
```

##### 二叉树

###### [二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/?envType=study-plan-v2&envId=top-100-liked)

```java
//使用BFS 广度优先搜索

class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode() {
    }

    TreeNode(int val) {
        this.val = val;
    }

    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

public class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        Deque<TreeNode> deque = new ArrayDeque<>();
        List<List<Integer>> res = new ArrayList<>();
        if (root != null) {
            deque.add(root);
        }

        while (!deque.isEmpty()) {
            int n = deque.size();
            List<Integer> list = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                //队列非空,把队列中的元素暂时存储到node中，并从队列中将该节点删除
                TreeNode node = deque.peek();
                deque.pop();
                //node节点的值暂存到list中
                list.add(node.val);

                //node如果有左右节点，则将左右节点入队
                if (node.left != null) {
                    deque.add(node.left);
                }

                if (node.right != null) {
                    deque.add(node.right);
                }
            }
            res.add(list);
        }
        return res;
    }
}
```

###### [二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/?envType=study-plan-v2&envId=top-100-liked)

###### [二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/?envType=study-plan-v2&envId=top-100-liked)

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        inorder(root, res);
        return res;
    }

    private void inorder(TreeNode root, List<Integer> res) {
        //若根节点为null则返回上层
        if (root == null) {
            return;
        } else {
            inorder(root.left, res);//先深度搜索左节点，一直到左节点为null
            res.add(root.val);//先把左节点的值添加到res中，一直添加到根节点
            inorder(root.right, res);//再查找右节点(从右节点的左节点查起)
        }

    }

}
```

##### 矩阵

###### [矩阵置零](https://leetcode.cn/problems/set-matrix-zeroes/solutions/669901/ju-zhen-zhi-ling-by-leetcode-solution-9ll7/?envType=study-plan-v2&envId=top-100-liked)

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int rows = matrix.length;//行
        int cols = matrix[0].length;//列
        boolean[] rowBool = new boolean[rows];
        boolean[] colBool = new boolean[cols];
        //遍历每一行每一列，若matrix中元素为0，则记录下当前的行列
        for (int i = 0; i < rows; i++) {
            for (int i1 = 0; i1 < cols; i1++) {
                if (matrix[i][i1] == 0) {
                    rowBool[i] = colBool[i1] = true;
                } else {
                    //无指令
                }
            }
        }

        for (int i = 0; i < rows; i++) {
            for (int i1 = 0; i1 < cols; i1++) {
                if (rowBool[i] || colBool[i1]) {
                    //所在行或者列为0 则置零
                    matrix[i][i1] = 0;
                }
            }
        }

    }
}
```

###### [螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/description/?envType=study-plan-v2&envId=top-100-liked)

```java
//螺旋矩阵

import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<>();
        int up = 0;
        int down = matrix.length - 1;
        int left = 0;
        int right = matrix[0].length - 1;
        while (true) {
            //上边界，从左向右
            for (int i = left; i <= right; i++) {
                res.add(matrix[up][i]);
            }
            if (++up > down) {
                //上边界向下
                break;
            }
            //右边界，从上向下
            for (int i = up; i <= down; i++) {
                res.add(matrix[i][right]);
            }
            if (--right < left) {
                //右边界向左
                break;
            }
            //下边界，从右向左
            for (int i = right; i >= left; i--) {
                res.add(matrix[down][i]);
            }
            //下边界向上
            if (--down < up) {
                break;
            }
            //左边界，从下向上
            for (int i = down; i >= up; i--) {
                res.add(matrix[i][left]);
            }
            if (++left > right) {
                break;
            }
        }
        return res;
    }
}

public class Main {
    public static void main(String[] args) {
        int[][] matrix = {{1, 2, 3, 4}, {5, 6, 7, 8}, {9, 10, 11, 12}, {13, 14, 15, 16}};
        Solution solution = new Solution();
        solution.spiralOrder(matrix);
    }
}
```