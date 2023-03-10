# Good read on monotonic stack

https://leetcode.com/problems/sum-of-subarray-minimums/solutions/178876/stack-solution-with-very-detailed-explanation-step-by-step


# Questions

## [2104. Sum of Subarray Ranges](https://leetcode.com/problems/sum-of-all-odd-length-subarrays/solutions/3043498/java-all-three-approach-explained/)

```java
	class Solution {
	  public int sumOddLengthSubarrays(int[] arr) {
	    int n = arr.length;
	    int res = 0;
	    for (int i = 0; i < n; i++) {
	      res += ((int) Math.ceil((i + 1) * (n - i) / 2.0)) * arr[i];
	    }
	    return res;
	  }
	}
```

## [Sum of Subarray Minimums](https://leetcode.com/problems/sum-of-subarray-minimums/solutions/3050209/java-monotonic-stack-solution-explained-with-intuition/)

```java
	class Solution {
	  public int sumSubarrayMins(int[] arr) {
	    int n = arr.length;
	
	    Stack < Integer > ms = new Stack < > ();
	    int[] nsl = new int[n];
	    for (int i = 0; i < n; i++) {
	      while (!ms.isEmpty() && arr[i] < arr[ms.peek()]) // non-strict
	        ms.pop();
	      nsl[i] = ms.isEmpty() ? i : ms.peek();
	      ms.push(i);
	    }
	    ms.clear();
	    int[] nsr = new int[n];
	    for (int i = n - 1; i >= 0; i--) {
	      while (!ms.isEmpty() && arr[i] <= arr[ms.peek()]) // strict
	        ms.pop();
	      nsr[i] = ms.isEmpty() ? i : ms.peek();
	      ms.push(i);
	    }
	
	    long sum = 0;
	    int MOD = (int) 1e9 + 7;
	
	    for (int i = 0; i < n; i++) {
	      int leftDis = i == nsl[i] ? i + 1 : i - nsl[i]; // i == nsl[i] means that no previous elements were smaller than current element
	      int rightDis = i == nsr[i] ? n - i : nsr[i] - i; // i == nsr[i] means that no next elements were smaller than current element
	      sum = (sum + ((1 l * leftDis * rightDis) % MOD * arr[i]) % MOD) % MOD;
	    }
	
	    return (int) sum;
	  }
	}
```

## [2104. Sum of Subarray Ranges](https://leetcode.com/problems/sum-of-subarray-ranges/solutions/1624268/reformulate-problem-o-n/)

### My Solution

```java
	class Solution {
	  public long subArrayRanges(int[] nums) {
	    return sumSubarrayMax(nums) - sumSubarrayMins(nums);
	  }
	
	  public long sumSubarrayMins(int[] arr) {
	    int n = arr.length;
	
	    Stack < Integer > ms = new Stack < > ();
	    int[] nsl = new int[n];
	    int[] nsr = new int[n];
	    for (int i = 0; i < n; i++) {
	      while (!ms.isEmpty() && arr[i] < arr[ms.peek()]) ms.pop();
	      nsl[i] = ms.isEmpty() ? i : ms.peek();
	      ms.push(i);
	    }
	    ms.clear();
	    for (int i = n - 1; i >= 0; i--) {
	      while (!ms.isEmpty() && arr[i] <= arr[ms.peek()]) ms.pop();
	      nsr[i] = ms.isEmpty() ? i : ms.peek();
	      ms.push(i);
	    }
	
	    long sum = 0;
	
	    for (int i = 0; i < n; i++) {
	      int leftDis = i == nsl[i] ? i + 1 : i - nsl[i];
	      int rightDis = i == nsr[i] ? n - i : nsr[i] - i;
	      sum += (long) leftDis * rightDis * arr[i];
	    }
	
	    return sum;
	  }
	
	  public long sumSubarrayMax(int[] arr) {
	    int n = arr.length;
	
	    Stack < Integer > ms = new Stack < > ();
	    int[] nsl = new int[n];
	    int[] nsr = new int[n];
	    for (int i = 0; i < n; i++) {
	      while (!ms.isEmpty() && arr[i] > arr[ms.peek()]) ms.pop();
	      nsl[i] = ms.isEmpty() ? i : ms.peek();
	      ms.push(i);
	    }
	    ms.clear();
	    for (int i = n - 1; i >= 0; i--) {
	      while (!ms.isEmpty() && arr[i] >= arr[ms.peek()]) ms.pop();
	      nsr[i] = ms.isEmpty() ? i : ms.peek();
	      ms.push(i);
	    }
	
	    long sum = 0;
	
	    for (int i = 0; i < n; i++) {
	      int leftDis = i == nsl[i] ? i + 1 : i - nsl[i];
	      int rightDis = i == nsr[i] ? n - i : nsr[i] - i;
	      sum += (long) leftDis * rightDis * arr[i];
	    }
	
	    return sum;
	  }
	}
```

### Highly Optimzed approach

```java
	class Solution {
	  public long subArrayRanges(int[] nums) {
	    return sumSubArrayComp(nums, (a, b) -> (a < b)) - sumSubArrayComp(nums, (a, b) -> (a > b));
	  }
	  private long sumSubArrayComp(int[] nums, BiPredicate < Integer, Integer > comp) {
	    Deque < Integer > stack = new ArrayDeque < > ();
	    long res = 0 L;
	    for (int i = 0; i <= nums.length; i++) {
	      while (!stack.isEmpty() && (i == nums.length || comp.test(nums[stack.peek()], nums[i]))) {
	        int j = stack.pop(), k = stack.isEmpty() ? -1 : stack.peek();
	        res += (long) nums[j] * (i - j) * (j - k);
	      }
	      stack.push(i);
	    }
	    return res;
	  }
	}
```




