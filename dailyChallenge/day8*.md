# Day8 String 

## LC 541 reverse-string-ii
[LC Link](https://leetcode.com/problems/reverse-string-ii/description/)   
[Cousrse Link](https://programmercarl.com/0344.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.html)
- Use range(start, end, step) to determine the starting position for reversing
- For a string s = 'abc', if you use s[0:999] ===> 'abc'. If the slice exceeds the maximum length of the string, it will automatically return up to the last character. This characteristic helps avoid some edge case handling.
- Replace using slicing for the entire segment instead of replacing characters one by one.

```python
  def reverseStr(self, s: str, k: int) -> str:
      def reverse_substring(text):
          left, right = 0, len(text) - 1
          while left < right:
              text[left], text[right] = text[right], text[left]
              left += 1
              right -= 1
          return text
      
      res = list(s)
  
      for cur in range(0, len(s), 2 * k):
          res[cur: cur + k] = reverse_substring(res[cur: cur + k])
      
      return ''.join(res)
```
Time: **O(n)**   
Space: **O(1)**

- Pythonic way
```python
    def reverseStr(self, s: str, k: int) -> str:
        # Two pointers. Another is inside the loop.
        p = 0
        while p < len(s):
            p2 = p + k
            # Written in this could be more pythonic.
            s = s[:p] + s[p: p2][::-1] + s[p2:] 
            p = p + 2 * k
        return s
```
Time: **O(n)**   
Space: **O(1)**


## Adds on
