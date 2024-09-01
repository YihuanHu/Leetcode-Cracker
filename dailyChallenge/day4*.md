# Day4 Linked List Part 2

## LC 24 swap-nodes-in-pairs
[LC Link](https://leetcode.com/problems/swap-nodes-in-pairs/description/)   
[Cousrse Link](https://programmercarl.com/0024.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html)

- Create an extra **dummy head** ListNode to handle the head edge case
- Manipulate of LL needs to think one step ahead
    - That's to say, if you want to update curr.val. You need to stand at curr.prev and delete the link by curr.prev.next
    - That's why check **curr.next is not null** is important to avoid null pointer

```python
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        # dummy head in case to delete head
        dummy = ListNode(next = head) 

        curr= dummy
        while curr.next:
            if curr.next.val == val:
                curr.next = curr.next.next
            else: 
                curr = curr.next
            
        return dummy.next
```
Time: **O(n)**   
Space: **O(1)**


## LC 707 design-linked-list
[LC Link](https://leetcode.com/problems/design-linked-list/description/)   
[Cousrse Link](https://programmercarl.com/0059.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5II.html#%E6%80%9D%E8%B7%AF)  
- get familiar with LL manipulation

```python
  class MyLinkedList:

    def __init__(self):
        self.dummy = ListNode()
        self.size = 0
        

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        curr = self.dummy.next 
        for i in range(index):
            curr = curr.next
        return curr.val

    def addAtHead(self, val: int) -> None:
        self.dummy.next = ListNode(val, self.dummy.next)
        self.size += 1

    def addAtTail(self, val: int) -> None:
        curr = self.dummy
        while curr.next:
            curr= curr.next
        curr.next = ListNode(val)
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return 
        curr = self.dummy
        for i in range(index):
            curr = curr.next
        curr.next = ListNode(val, next = curr.next)
        self.size += 1
        

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return 
        curr = self.dummy
        for i in range(index):
            curr = curr.next
        curr.next = curr.next.next
        self.size -= 1
```


## LC 206 reverse-linked-list
[LC Link](https://leetcode.com/problems/reverse-linked-list/description/)   
[Cousrse Link](https://programmercarl.com/0206.%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.html)  



- Use iteration:  create a temp node to store the next and then its a 4 circle relationship between temp, next, pre, curr
```python
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        pre = None
        curr = head

        while curr:
            temp = curr.next
            curr.next = pre
            pre = curr
            curr = temp

        return pre
```
Time: **O(n)**   
Space: **O(1)**

- Use recursion : good example of reviewing recursion
```python
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        ########## recursion
        # Base case: if the list is empty or has only one element
        # when head = null, it is the end
        if not head or not head.next:
            return head

        # Recursive case: reverse the rest of the list
        last = self.reverseList(head.next)

        # Adjust the pointers
        head.next.next = head
        head.next = None

        # Return the new head of the reversed list
        return last
```

Time: **O(n)**   
Space: **O(n)**


## Adds on
- [ ] review 206 for recursive solution
