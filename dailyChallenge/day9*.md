# Day9 Hash Table Part 2

## LC 151 reverse-words-in-a-string
[LC Link](https://leetcode.com/problems/reverse-words-in-a-string/description/)   
[Cousrse Link](https://programmercarl.com/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html)
- Split can covert a string to list seperated by space
- String is **IMMUTABLE** in Python
- Several solutions:
    - split in words and reverse
    - reverse the whole string and split and then rever word by word
  
```python
    def reverseWords(self, s):
        words = s.split() #type(words) --- list
        words = words[::-1] # reverse list
        return ' '.join(words) #convert list to string
```
```python
    def reverseWords(self, s: str) -> str:
        # Reverse the entire string
        s = s[::-1]
        # Split the string into words and reverse each word
        s = ' '.join(word[::-1] for word in s.split())
        return s
```
Time: **O(n)**   
Space: **O(n)**


## LC 28 find-the-index-of-the-first-occurrence-in-a-string (did not get it :(
[LC Link](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)   
[Cousrse Link with Iteration](https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)  
[Cousrse Link with DP](https://mp.weixin.qq.com/s/r9pbkMyFyMAvmkf4QnL-1g)  
- Why use KMP algo?
- What is prefix/next table?
- How to find out next table?
- How to use this table ?

```python
    def getNext(self, next: List[int], s: str) -> None:
        j = 0
        next[0] = 0
        for i in range(1, len(s)):
            while j > 0 and s[i] != s[j]:
                j = next[j - 1]
            if s[i] == s[j]:
                j += 1
            next[i] = j
    
    def strStr(self, haystack: str, needle: str) -> int:
        if len(needle) == 0:
            return 0
        next = [0] * len(needle)
        self.getNext(next, needle)
        j = 0
        for i in range(len(haystack)):
            while j > 0 and haystack[i] != needle[j]:
                j = next[j - 1]
            if haystack[i] == needle[j]:
                j += 1
            if j == len(needle):
                return i - len(needle) + 1
        return -1
```
Time: **O(m+n)**   
Space: **O(n)**  - prefix/next table



## Adds on
- [ ] KMP practice: [Link](https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html)

- String Summary :[Link](https://programmercarl.com/%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%80%BB%E7%BB%93.html#%E4%BB%80%E4%B9%88%E6%98%AF%E5%AD%97%E7%AC%A6%E4%B8%B2)
- 2 pointers Summary: [Link](https://programmercarl.com/%E5%8F%8C%E6%8C%87%E9%92%88%E6%80%BB%E7%BB%93.html#%E6%95%B0%E7%BB%84%E7%AF%87)
