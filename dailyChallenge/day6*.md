# Day6 Hash Table

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
Time: **O(n)**   
Space: **O(1)**


## LC 160 intersection-of-two-linked-lists
[LC Link](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)   
[Cousrse Link](https://labuladong.online/algo/essential-technique/linked-list-skills-summary/)  

- To make sure both pointers reach the interseaction at the same time:
- Both pointers walks through the nonoverlapping and overlapping(if exists) parts
    - assume LL1: a + c,  LL2: b + c
    - If len(LL1) < len(LL1),
    - => LL1 walks through: a + c + b
    - => LL2 walks through: b + c + a
```python
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        p1, p2 = headA, headB
    
        while p1 != p2:
            # p1 takes one step; if it reaches the end of list A, switch to list B
            if p1 is None:
                p1 = headB
            else:
                p1 = p1.next
            # p2 takes one step; if it reaches the end of list B, switch to list A
            if p2 is None:
                p2 = headA
            else:
                p2 = p2.next
        return p1

```
Time: **O(n+m)**   
Space: **O(1)**

Another solution: verbose on coding but easy to understand   
[Cousrse Link](https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)  


## Adds on
- [ ] review LC 24 video & compare against 206
- [ ] check problem [Cousrse Link](https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html)
- [ ] check LL notes
