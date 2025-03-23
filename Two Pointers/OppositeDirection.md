# Opposite Direction Two Pointers (Two Ends)

## Use Cases  
This variation of the Two Pointers technique is useful when:
1. **Finding pairs that meet a condition** (e.g., Two Sum in a sorted array).
2. **Checking if a sequence is a palindrome** (e.g., Valid Palindrome in a string).
3. **Finding closest pairs in a sorted array** (e.g., Smallest Difference Pair).
4. **Maximizing or minimizing sums** (e.g., Optimal pair selection problems).

## Approach  
1. **Initialize two pointers**: One at the beginning (`left`) and one at the end (`right`) of the array or string.  
2. **Move the pointers toward each other** based on conditions:  
   - If the sum/product/condition is too **small**, move the `left` pointer forward.  
   - If the sum/product/condition is too **large**, move the `right` pointer backward.  
3. **Continue until they meet**: Once `left` meets `right`, stop processing.  
4. **Return the result** based on the problem’s requirement.  

## Examples

### 1. Two Sum in a Sorted Array
#### Problem:
Given a sorted array, find if there exists a pair that sums up to a target.

#### Example Input:
```
arr = [1, 2, 3, 5, 7, 9, 11]  
target = 10
```

#### Code Implementation:
```cpp
bool twoSumSorted(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    while (left < right) {
        int sum = arr[left] + arr[right];
        if (sum == target) return true;
        else if (sum < target) left++;
        else right--;
    }
    return false;
}
```

### 2. Check if a String is a Palindrome
#### Problem:
Check if a given string is a palindrome (ignoring non-alphanumeric characters and case).

#### Example Input:
```
s = "A man, a plan, a canal: Panama"
```

#### Code Implementation:
```cpp
bool isPalindrome(string s) {
    int left = 0, right = s.length() - 1;
    while (left < right) {
        while (left < right && !isalnum(s[left])) left++;
        while (left < right && !isalnum(s[right])) right--;
        if (tolower(s[left]) != tolower(s[right])) return false;
        left++;
        right--;
    }
    return true;
}
```

### 3. Find the Closest Pair to Zero
#### Problem:
Find the pair of numbers whose sum is closest to zero in a sorted array.

#### Example Input:
```
arr = [-10, -3, -1, 4, 8, 12]
```

#### Code Implementation:
```cpp
pair<int, int> closestPairToZero(vector<int>& nums) {
    int left = 0, right = nums.size() - 1;
    int minDiff = INT_MAX;
    pair<int, int> result;
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (abs(sum) < minDiff) {
            minDiff = abs(sum);
            result = {nums[left], nums[right]};
        }
        if (sum < 0) left++;
        else right--;
    }
    return result;
}
```

### 4. Container With Most Water
#### Problem:
Find two indices `i` and `j` such that the container formed between them holds the maximum water.

#### Example Input:
```
heights = [1, 8, 6, 2, 5, 4, 8, 3, 7]
```

#### Code Implementation:
```cpp
int maxArea(vector<int>& height) {
    int left = 0, right = height.size() - 1;
    int maxWater = 0;
    while (left < right) {
        int water = min(height[left], height[right]) * (right - left);
        maxWater = max(maxWater, water);
        if (height[left] < height[right]) left++;
        else right--;
    }
    return maxWater;
}
```

### 5. Find Pairs with Difference K
#### Problem:
Find the number of unique pairs `(i, j)` such that `arr[j] - arr[i] = k`.

#### Example Input:
```
arr = [1, 3, 5, 7, 9]
k = 2
```

#### Code Implementation:
```cpp
int findPairs(vector<int>& nums, int k) {
    sort(nums.begin(), nums.end());
    int left = 0, right = 1, count = 0;
    while (right < nums.size()) {
        if (left == right || nums[right] - nums[left] < k) right++;
        else if (nums[right] - nums[left] > k) left++;
        else {
            count++;
            left++;
            right++;
            while (right < nums.size() && nums[right] == nums[right - 1]) right++;
        }
    }
    return count;
}
```

## Summary
| **Use Case** | **Example Problem** | **Complexity** |
|-------------|-------------------|---------------|
| **Find a pair with a sum** | Two Sum (Sorted Array) | \(O(N)\) |
| **Check Palindrome** | Valid Palindrome | \(O(N)\) |
| **Find closest pair to zero** | Smallest Sum Pair | \(O(N)\) |
| **Max Water Storage** | Container With Most Water | \(O(N)\) |
| **Find pairs with difference K** | Unique Pairs Difference | \(O(N \log N)\) |

This technique efficiently solves problems where we process data **from both ends towards the center** while optimizing **space and time complexity**. 🚀

