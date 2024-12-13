## 移除链表元素

### 题目

地址：https://leetcode.cn/problems/remove-linked-list-elements/description/

给你一个链表的头节点 `head` 和一个整数 `val` ，请你删除链表中所有满足 `Node.val == val` 的节点，并返回 **新的头节点** 。

思路是使用虚拟节点的方式比较简单一些，直接定义一个虚拟节点指向头结点，在链表中移除元素就是将指针域向后移动两部就可

### 代码

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        //使用虚拟头结点方式
        ListNode dumpNode = new ListNode();
        dumpNode.next = head;
        
        //定义当前节点为虚拟节点
        ListNode cur = dumpNode;
        while(cur.next != null){
            if(cur.next.val == val){
                //移除元素
                cur.next = cur.next.next;
            }else{
                cur = cur.next;
            }
        }
        //指向虚拟节点的下一个节点即头节点
        return dumpNode.next;
    }
}
~~~



## 设计链表

### 题目

地址：https://leetcode.cn/problems/design-linked-list/description/

在链表类中实现这些功能：

- get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
- addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
- addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
- addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
- deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。

![707示例](https://code-thinking-1253855093.file.myqcloud.com/pics/20200814200558953.png)

### 代码

~~~java
class MyLinkedList {
    //链表数量
    int size;
    //虚拟头节点
    ListNode head; 

    //初始化链表
    public MyLinkedList() {
        size = 0;
        head = new ListNode(0);
    }
    
    // 获取链表中下标为 index 的节点的值。如果下标无效，则返回 -1 
    public int get(int index) {
        if(index <0 || index >= size){
            return -1;
        }
        ListNode curNode = head;
        for(int i=0; i <= index; i++){
            curNode = curNode.next;
        }
        return curNode.val;
    }
    
    // 将一个值为 val 的节点插入到链表中第一个元素之前。在插入完成后，新节点会成为链表的第一个节点。
    public void addAtHead(int val) {
        addAtIndex(0,val);
    }
    
    //将一个值为 val 的节点追加到链表中作为链表的最后一个元素。
    public void addAtTail(int val) {
        addAtIndex(size,val);
    }
    
    //将一个值为 val 的节点插入到链表中下标为 index 的节点之前。如果 index 等于链表的长度，那么该节点会被追加到链表的 末尾。如果 index 比长度更大，该节点将 不会插入 到链表中。
    public void addAtIndex(int index, int val) {
        if(index > size){
            return;
        }
        index = Math.max(0,index);
        size++;

        ListNode preNode = head;
        for(int i=0;i < index ;i++){
            preNode = preNode.next;
        }
        ListNode toAdd = new ListNode(val);
        toAdd.next = preNode.next;
        preNode.next = toAdd;
    }
    
    // 如果下标有效，则删除链表中下标为 index 的节点。
    public void deleteAtIndex(int index) {
        if(index < 0 || index >= size){
            return;
        }
        size--;
        ListNode curNode = head;
        for(int i=0 ; i < index ; i++){
            curNode = curNode.next; 
        }
        curNode.next = curNode.next.next;
    }
}

class ListNode{
    int val;
    ListNode next;

    public ListNode(int val){
        this.val = val;
    }

    public ListNode(){}
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
~~~

## 反转链表

### 题目

地址：https://leetcode.cn/problems/reverse-linked-list/description/

题意：反转一个单链表。

示例: 输入: 1->2->3->4->5->NULL 输出: 5->4->3->2->1->NULL

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

### 代码

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        //双指针方法
        ListNode preNode = null;
        ListNode curNode = head;
        ListNode tempNode = null;

        while(curNode != null){
          //保存下一个节点
          tempNode = curNode.next;
          curNode.next = preNode;
          preNode = curNode;
          curNode = tempNode;
        }
        return preNode;
    }
}
~~~

双指针方法是最容易理解的，当然参考代码随想录，还有其他的两种方法就是递归方法和从后向前递归方法

递归：

~~~java
class Solution {
    public ListNode reverseList(ListNode head) {
        return reverse(null, head);
    }

    private ListNode reverse(ListNode prev, ListNode cur) {
        if (cur == null) {
            return prev;
        }
        ListNode temp = null;
        temp = cur.next;// 先保存下一个节点
        cur.next = prev;// 反转
        // 更新prev、cur位置
        // prev = cur;
        // cur = temp;
        return reverse(cur, temp);
    }
}
~~~

从后向前递归

~~~java
class Solution {
    ListNode reverseList(ListNode head) {
        // 边缘条件判断
        if(head == null) return null;
        if (head.next == null) return head;
        
        // 递归调用，翻转第二个节点开始往后的链表
        ListNode last = reverseList(head.next);
        // 翻转头节点与第二个节点的指向
        head.next.next = head;
        // 此时的 head 节点为尾节点，next 需要指向 NULL
        head.next = null;
        return last;
    } 
}
~~~

## 两两交换链表中的节点

### 题目

地址：

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

![img](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

思路：

使用虚拟头结点

![24.两两交换链表中的节点1](https://code-thinking.cdn.bcebos.com/pics/24.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B91.png)

### 代码

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode swapPairs(ListNode head) {
        //使用虚拟头结点来操作
        ListNode dumpNode = new ListNode(-1);
        dumpNode.next = head;
        ListNode curNode = dumpNode;
        //定义三个临时节点
        ListNode firstNode;//两个节点中的第一个节点
        ListNode secondeNode;//两个节点中的第二个节点
        ListNode temp;////两个节点后面的节点

        while(curNode.next!=null && curNode.next.next != null){
            temp = curNode.next.next.next;
            firstNode = curNode.next;
            secondeNode  =curNode.next.next;
            //进行交换
            curNode.next = secondeNode;
            secondeNode.next = firstNode;
            firstNode.next = temp;
            //下一轮循环
            curNode = firstNode;
        }
        return dumpNode.next;
    }
}
~~~

## 删除链表倒数第N个节点

### 题目

地址：https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

### 代码

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        //使用快慢指针方法
        ListNode dumpNode = new ListNode(0);
        dumpNode.next = head;

        //定义快慢指针
        ListNode fastIndex = dumpNode;
        ListNode slowIndex = dumpNode;

        //快慢指针只要相差n即可
        for(int i=0;i<=n;i++){
            fastIndex = fastIndex.next;
        }
        while(fastIndex != null){
            fastIndex = fastIndex.next;
            slowIndex = slowIndex.next;
        }

        if(slowIndex.next != null){
            slowIndex.next = slowIndex.next.next;
        }

        return dumpNode.next;
    }
}
~~~

## 链表相交

### 题目

地址：https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/

给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。

![img](https://code-thinking-1253855093.file.myqcloud.com/pics/20211219221657.png)

简单来说，就是求两个链表交点节点的**指针**。 这里要注意，交点不是数值相等，而是指针相等。

### 代码

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode p1 = headA;
        ListNode p2 = headB;
        while(p1 != p2){
            if(p1 == null){
                p1 = headB;
            }else{
                p1 = p1.next;
            }
            if(p2 == null){
                p2 = headA;
            }else{
                p2 = p2.next;
            }
        }
        return p1;
    }
}
~~~

## 环形链表II

### 题目

地址：https://leetcode.cn/problems/linked-list-cycle-ii/description/

题意： 给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

**说明**：不允许修改给定的链表。

思路：

判断链表是否有环，可以使用快慢指针法，分别定义 fast 和 slow 指针，从头结点出发，fast指针每次移动两个节点，slow指针每次移动一个节点，如果 fast 和 slow指针在途中相遇 ，说明这个链表有环。

判断链表是否有环

可以使用快慢指针法，分别定义 fast 和 slow 指针，从头结点出发，fast指针每次移动两个节点，slow指针每次移动一个节点，如果 fast 和 slow指针在途中相遇 ，说明这个链表有环。

**此时已经可以判断链表是否有环了，那么接下来要找这个环的入口了。**

假设从头结点到环形入口节点 的节点数为x。 环形入口节点到 fast指针与slow指针相遇节点 节点数为y。 从相遇节点 再到环形入口节点节点数为 z。 如图所示：

![img](https://code-thinking-1253855093.file.myqcloud.com/pics/20220925103433.png)

那么相遇时： slow指针走过的节点数为: `x + y`， fast指针走过的节点数：`x + y + n (y + z)`，n为fast指针在环内走了n圈才遇到slow指针， （y+z）为 一圈内节点的个数A。

因为fast指针是一步走两个节点，slow指针一步走一个节点， 所以 fast指针走过的节点数 = slow指针走过的节点数 * 2：

```
(x + y) * 2 = x + y + n (y + z)
```

两边消掉一个（x+y）: `x + y = n (y + z)`

因为要找环形的入口，那么要求的是x，因为x表示 头结点到 环形入口节点的的距离。

所以要求x ，将x单独放在左面：`x = n (y + z) - y` ,

再从n(y+z)中提出一个 （y+z）来，整理公式之后为如下公式：`x = (n - 1) (y + z) + z` 注意这里n一定是大于等于1的，因为 fast指针至少要多走一圈才能相遇slow指针。

这个公式说明什么呢？

先拿n为1的情况来举例，意味着fast指针在环形里转了一圈之后，就遇到了 slow指针了。

当 n为1的时候，公式就化解为 `x = z`，

这就意味着，**从头结点出发一个指针，从相遇节点 也出发一个指针，这两个指针每次只走一个节点， 那么当这两个指针相遇的时候就是 环形入口的节点**。

也就是在相遇节点处，定义一个指针index1，在头结点处定一个指针index2。

让index1和index2同时移动，每次移动一个节点， 那么他们相遇的地方就是 环形入口的节点。

动画如下：

![142.环形链表II（求入口）](https://code-thinking.cdn.bcebos.com/gifs/142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II%EF%BC%88%E6%B1%82%E5%85%A5%E5%8F%A3%EF%BC%89.gif)

那么 n如果大于1是什么情况呢，就是fast指针在环形转n圈之后才遇到 slow指针。

其实这种情况和n为1的时候 效果是一样的，一样可以通过这个方法找到 环形的入口节点，只不过，index1 指针在环里 多转了(n-1)圈，然后再遇到index2，相遇点依然是环形的入口节点。

### 代码

~~~java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        //使用快慢指针来实现
        ListNode slowNode = head;
        ListNode fastNode = head;
        while(fastNode != null && fastNode.next != null){
            slowNode = slowNode.next;
            fastNode = fastNode.next.next;
            if(fastNode == slowNode){//有环
                ListNode indexNode1 = head;
                ListNode indexNode2 = slowNode;
                while(indexNode1 != indexNode2){
                    indexNode1 = indexNode1.next;
                    indexNode2 = indexNode2.next;
                }
                return indexNode1;
            }
        }
        return null;
    }
}
~~~

