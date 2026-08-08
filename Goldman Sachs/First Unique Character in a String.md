

## python
```python
class Solution:
  def firstUniqChar(self, s: str) -> int:
    count = collections.Counter(s)

    for i, c in enumerate(s):
      if count[c] == 1:
        return i

    return -1
```

## C++
```cpp
class Solution {
 public:
  int firstUniqChar(string s) {
    vector<int> count(26);

    for (const char c : s)
      ++count[c - 'a'];

    for (int i = 0; i < s.length(); ++i)
      if (count[s[i] - 'a'] == 1)
        return i;

    return -1;
  }
};
```

## Java
```java
class Solution {
  public int firstUniqChar(String s) {
    int[] count = new int[26];

    for (final char c : s.toCharArray())
      ++count[c - 'a'];

    for (int i = 0; i < s.length(); ++i)
      if (count[s.charAt(i) - 'a'] == 1)
        return i;

    return -1;
  }
}
```
