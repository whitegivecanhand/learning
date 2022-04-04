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

删除第

