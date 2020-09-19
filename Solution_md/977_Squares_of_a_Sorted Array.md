# 977 Squares of a Sorted Array

Given an array of integers A sorted in non-decreasing order, return an array of the squares of each number, also in sorted non-decreasing order.

## Intuition
No matter what language you are using, this one is a simple one, intuition would be perfectly fine, do the square of each item and sort the squares.

## Code (Python)
```python
class Solution(object):
    def sortedSquares(self, A):
        """
        :type A: List[int]
        :rtype: List[int]
        """
        result = []
        for a in A:
            result.append(a*a)
        return sorted(result)
```
or 
```python
class Solution(object):
    def sortedSquares(self, A):
        """
        :type A: List[int]
        :rtype: List[int]
        """
       return sorted(x*x for x in A)
```

## Two-pointer
We can use two pointers to read the positive and negative parts of the array - one pointer j in the positive direction, and another i in the negative direction. since A is sorted. The time complexity could be shorted from `O(NlogN)` to `O(N)`

Below is the implementation

```python
def sortedSquares(self, A):
        N = len(A)
        # i, j: negative, positive parts
        j = 0
        while j < N and A[j] < 0:
            j += 1
        i = j - 1

        ans = []
        while 0 <= i and j < N:
            if A[i]**2 < A[j]**2:
                ans.append(A[i]**2)
                i -= 1
            else:
                ans.append(A[j]**2)
                j += 1

        while i >= 0:
            ans.append(A[i]**2)
            i -= 1
        while j < N:
            ans.append(A[j]**2)
            j += 1

        return ans
```

