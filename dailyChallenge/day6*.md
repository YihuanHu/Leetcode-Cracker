# Day6 Hash Table
# Hash
- Sacrifice space to get O(1) query time => frequetly used when check the existance of a set
- Hash function: Maps input data to a fixed-size integer value (hash code) that represents the data, used to index into a hash table
- Hash Collision: When two different inputs produce the same hash code
  - Linear probing: Resolving collisions by sequentially searching for the next available slot in the hash table
  - Chaining: Resolving collisions by maintaining a list of all elements that hash to the same index
- Common data structures in Python:
  - `dict`: A mutable mapping of unique keys to values, allowing O(1) average-time complexity for lookups, inserts, and deletions.
    - `for key, value in my_dict.items():`
    - `for key in my_dict:`
    - `my_dict.pop('b')`
    - `my_dict.get('d', 0)`
  - `set`: An unordered collection of unique elements that supports O(1) average-time complexity for membership tests, insertions, and deletions.
    - `my_set.add(4)`
    - `my_set.remove(2)`



## LC 242 valid-anagram
[LC Link](https://leetcode.com/problems/valid-anagram/)   
[Cousrse Link](https://programmercarl.com/0242.%E6%9C%89%E6%95%88%E7%9A%84%E5%AD%97%E6%AF%8D%E5%BC%82%E4%BD%8D%E8%AF%8D.html)


```python
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        dicS, dicT = {} , {}

        for i in range(len(s)):
            dicS[s[i]] = 1 + dicS.get(s[i],0)
            dicT[t[i]] = 1 + dicT.get(t[i],0)

        return dicS == dicT

```
Time: **O(n)**   
Space: **O(1)**


## LC 707 intersection-of-two-arrays
[LC Link](https://leetcode.com/problems/intersection-of-two-arrays/description/)   
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
