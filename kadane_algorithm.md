# Kadane algorithm

This interesting algorithm is used for a single problem, 
is to calculate maximum sum of all subarrays in a provided array, 
but could be extended in few other variances which have the pattern of 
"Calculate the global optimized solution of all subarrays"

This video visualized the algorithm to make it easier to understand.
https://www.youtube.com/watch?v=86CQq3pKSUw 

The most interesting thing from this algorithm is, when we understand it,
it became so obvious.

There're 2 main core ideas from the algorithm:
- Rephrase the problem into: maximum of all subarrays = maximum of all subarrays that end at each index
- Maximum of all subarray end at index (i), could be interpolated from the value from (i-1) by `max = max(arr[i], max + arr[i])`

