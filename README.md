# learning
第二周
设计链表 707
class Node:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class MyLinkedList:
    def __init__(self):
        # 初始设置一个空节点
        self.dummp_head = Node()
        self.count = 0

    def get(self, index: int) -> int:
        # 存在空节点的情况下，假设链表为：None->2->1->3，此时count = 3，index为0，1，2的情况下才返回值
        if index < self.count:
            n = self.dummp_head
            for i in range(index):
                n = n.next
            return n.next.val
        return -1

    def addAtHead(self, val: int) -> None:
        # 先把空节点的值设为val，然后创建空节点n，连接上即可
        self.dummp_head.val = val
        n = Node()
        n.next = self.dummp_head
        self.dummp_head = n
        self.count += 1

    def addAtTail(self, val: int) -> None:
        # 这里一致循环到末尾，赋值即可
        n = self.dummp_head
        while n:
            if n.next == None:
                n.next = Node(val)
                break
            n = n.next

        self.count += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index <= 0:
            self.addAtHead(val)
        elif index == self.count:
            self.addAtTail(val)
        # 大于count的话不进行操作
        elif index > self.count:
            pass
        else:
            # 这里推荐画图理解，循环到插入点，先用tem保存n下面的链表，然后把n的next改为val节点，n.next.next等于tem即可
            n = self.dummp_head
            for i in range(index):
                n = n.next

            tem = n.next
            n.next = Node(val)
            n.next.next = tem
                   
            self.count += 1

    def deleteAtIndex(self, index: int) -> None:
        # 删除应该是最好理解的了，不做多解释
        if index < self.count:
            n = self.dummp_head
            for i in range(index):
                n = n.next
            n.next = n.next.next
      
            self.count -= 1
   
  反转链表 206
  1.双指针
  双指针反转的重点在于需要保存反转节点反转之前的指针，不然反转之后就脱节
  class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* cur = head;
        ListNode* pre = nullptr;
        ListNode* temp;
        while(cur){
            temp = cur -> next;
            cur -> next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
    }
};
2.递归法
递归主要是通过函数返回来实现pre = cur ,cur = tmep这一步
class Solution {
public:
    ListNode* reverse(ListNode* cur,ListNode* pre){
        if (cur == nullptr)return pre;
        ListNode* temp = cur -> next;
        cur -> next = pre;
        return reverse(temp, cur);这里返回temp和cur，代替最初的cur和pre
    }
    ListNode* reverseList(ListNode* head) {
        return reverse(head, nullptr);
    }
};

删除倒数第N个节点 19
1.遍历法
遍历法首先计算链表长度，然后长度减去n在减1就是删除节点的前一个节点，然后就是常规删除
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummynode = new ListNode(0);
        dummynode -> next = head;
        int s = 0;
        ListNode* temp = dummynode;
        while(temp){
            temp = temp ->next;
            s++;
        }
        int index1 = s - n - 1;
        ListNode* cur = dummynode;
        for(int i = 0;i < index1;i++){
            cur = cur -> next;
        }
        cur -> next = cur -> next -> next;
        return dummynode -> next;
    }   
};
2.双指针法
双指针法的重点在于快指针比指针快n+1的点，当快指针到空的时候，慢指针正好在删除节点的前一个节点，直接可以删除
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummynode = new ListNode(0);
        dummynode -> next = head;
        ListNode* slow = dummynode;
        ListNode* fast = dummynode;
        for(int i = 0;i <=n;i++){这里的<=就是n+1
            fast = fast->next;
        }
        while(fast){
            slow = slow->next;
            fast = fast->next;
        }
        slow->next = slow->next->next;
        return dummynode->next;
    }
        
};


环形链表 142
环形链表的重点在于快慢指针的追及，快指针一次两个，慢指针一次一个，快指针首先进入环，由于快指针速度是慢指针两倍，因此慢指针在进入环后一定会在一圈之内被快指针追上，如果快指针一次三个，可能会跳过慢指针，设进环前长度x，环中相遇点距环入口y和z，环长z+y,快指针路程为x+y+n(z+y),慢指针路程为x+y，由速度得公式：x+y+n(z+y) = 2(x + y),化简为x = nz + (n-1)y,n大于等于1，是1的时候则x=z，在头节点和相遇节点开始同时出发就能在入口碰头，如果N大于1，则相遇节点里的指针会多转几圈，还是会在入口相遇。


class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while(fast != nullptr && fast -> next != nullptr){
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast){
                ListNode* index1 = slow;
                ListNode* index2 = head;
                while(index1 != index2){
                    index1 = index1->next;
                    index2 = index2->next;
                }return index1;

            }
        }return nullptr;
        
    }
};

哈希表
有效的字母异位词

