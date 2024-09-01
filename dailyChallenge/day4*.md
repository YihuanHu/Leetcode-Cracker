# Day4 Linked List Part 2

## LC 24 swap-nodes-in-pairs
[LC Link](https://leetcode.com/problems/swap-nodes-in-pairs/description/)   
[Cousrse Link](https://programmercarl.com/0024.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html)

- Compare with LC 206 revere the LL
    - 206: sawp one node; 1 temp nodes; swap 3 nodes
    - 24: swap two consecutive nodes; 2 temp nodes ; swap 4 nodes


- Use iteration
```python
    def swapPairs(self, head: ListNode) -> ListNode:
        dummy_head = ListNode(next=head)
        current = dummy_head
        
        # There must be a next and next-next node for swapping; otherwise, the swapping is complete.
        while current.next and current.next.next:
            temp = current.next  # Prevent node modification
            temp1 = current.next.next.next
            
            current.next = current.next.next
            current.next.next = temp
            temp.next = temp1
            current = current.next.next
        return dummy_head.next

```

- Use recursion 
```python
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head
    
        # The two nodes to be swapped are pre and cur
        pre = head
        cur = head.next
        next = head.next.next
        
        cur.next = pre  # swap
        pre.next = self.swapPairs(next)  # Recursively swap the remaining list starting from next

```
Time: **O(n)**   
Space: **O(1)**


## LC 707 remove-nth-node-from-end-of-list
[LC Link](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)   
[Cousrse Link](https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)  
- use two pointer: fast & slow
- Logis is:
    - Fast pointer goes to n+1 (one step before the target)
    - => then slow/fast continues iteration(len - (n+1))
    - => this is exactly where the nth node from end

```python
   def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:        
        dummy = ListNode(-1)
        dummy.next = head

        p1 = p2 = dummy

        for i in range(n+1) :
            print(p1.val)
            p1 = p1.next

        while p1 != None:
            p2 = p2.next
            p1 = p1.next
        
        p2.next = p2.next.next

        return dummy.next
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
- [ ] review LC 24 video
- [ ] 
