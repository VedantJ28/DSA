# Opposite Direction Two Pointers (Two Ends)

## Use Cases  
This variation of the Two Pointers technique is useful when:
1. **Finding pairs that meet a condition** (e.g., Two Sum in a sorted array).
2. **Checking if a sequence is a palindrome** (e.g., Valid Palindrome in a string).
3. **Finding closest pairs in a sorted array** (e.g., Smallest Difference Pair).
4. **Maximizing or minimizing sums** (e.g., Optimal pair selection problems).

## Problem 1: Two Sum in a Sorted Array

#### Problem:
Given a sorted array, find if there exists a pair that sums up to a target.

#### Approach:
1. Initialize two pointers (`left` at the beginning and `right` at the end).
2. Compute the sum at these pointers.
3. If the sum equals the target, return true.
4. If the sum is less than the target, increment `left`.
5. If the sum is greater than the target, decrement `right`.
6. Repeat until the pointers meet.

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

---

## Problem 2: Check if a String is a Palindrome

#### Problem:
Check if a given string is a palindrome (ignoring non-alphanumeric characters and case).

#### Approach:
1. Initialize two pointers (`left` at the start and `right` at the end).
2. Skip non-alphanumeric characters using helper loops.
3. Compare characters in a case-insensitive manner.
4. If the characters differ, return false.
5. Move pointers inward and repeat until they meet.

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

---

## Problem 3: Find the Closest Pair to Zero

#### Problem:
Find the pair of numbers whose sum is closest to zero in a sorted array.

#### Approach:
1. Initialize two pointers at the start (`left`) and end (`right`).
2. Calculate the sum and its absolute difference from zero.
3. Track the pair with the minimum difference.
4. If the sum is negative, increment `left`; if positive, decrement `right`.
5. Continue until the pointers cross.

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

---

## Problem 4: Container With Most Water

#### Problem:
Find two indices such that the container formed between them holds the maximum water.

#### Approach:
1. Set two pointers (`left` at the beginning and `right` at the end).
2. Calculate the area using the shorter height and the distance between pointers.
3. Track the maximum area obtained.
4. Increment the pointer with the smaller height.
5. Repeat until the pointers meet.

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

---

## Problem 5: Find Pairs with Difference K

#### Problem:
Find the number of unique pairs (i, j) such that arr[j] - arr[i] = k.

#### Approach:
1. Sort the array if not already sorted.
2. Initialize two pointers; start the second pointer right after the first.
3. Increase the second pointer if the difference is less than k.
4. Increase the first pointer if the difference is greater than k.
5. When the difference equals k, count the pair and move both pointers, skipping duplicates.

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

---

## Problem 6: Merging Two Sorted Arrays Without Extra Space

#### Problem:
Merge two sorted arrays arr1 and arr2 in-place without using extra space.

#### Approach:
1. Initialize pointers: one at the end of arr1 and one at the beginning of arr2.
2. Compare elements at these pointers.
3. Swap the elements if the element in arr1 is greater than the element in arr2.
4. Adjust pointers respectively.
5. Sort both arrays after the swapping process to finalize the merge.

#### Example Input:
```
arr1 = [1, 4, 7, 8, 10]
arr2 = [2, 3, 9]
```

#### Code Implementation:
```cpp
void mergeSortedArrays(vector<int>& arr1, vector<int>& arr2) {
    int m = arr1.size(), n = arr2.size();
    int left = m - 1, right = 0;
    
    while (left >= 0 && right < n) {
        if (arr1[left] > arr2[right]) {
            swap(arr1[left], arr2[right]);
            left--;
            right++;
        } else {
            break;
        }
    }
    sort(arr1.begin(), arr1.end());
    sort(arr2.begin(), arr2.end());
}
```

---

## Problem 7: Trapping Rain Water

#### Problem:
Given an array representing elevation heights, compute how much water can be trapped.

#### Approach:
1. Use two pointers to traverse the array from both ends.
2. Maintain two variables (leftMax and rightMax) to track the maximum height encountered from each side.
3. At each step, update the water volume by comparing the current height with the corresponding maximum.
4. Adjust the left or right pointer based on which side is lower.
5. Continue until the pointers meet.

#### Example Input:
```
heights = [0,1,0,2,1,0,1,3,2,1,2,1]
```

#### Code Implementation:
```cpp
int trap(vector<int>& height) {
    int left = 0, right = height.size() - 1, leftMax = 0, rightMax = 0, water = 0;
    while (left < right) {
        if (height[left] < height[right]) {
            height[left] >= leftMax ? leftMax = height[left] : water += leftMax - height[left];
            left++;
        } else {
            height[right] >= rightMax ? rightMax = height[right] : water += rightMax - height[right];
            right--;
        }
    }
    return water;
}
```

---

## Problem 8: Valid Mountain Array

#### Problem:
Determine if an array forms a valid mountain sequence.

#### Approach:
1. Initialize two pointers at both ends of the array.
2. Move the left pointer while elements are strictly increasing.
3. Move the right pointer while elements are strictly decreasing.
4. Check that the pointers meet and that there is an increase and subsequent decrease.
5. Return true only if conditions are met.

#### Example Input:
```
arr = [0, 3, 2, 1]
```

#### Code Implementation:
```cpp
bool validMountainArray(vector<int>& arr) {
    int left = 0, right = arr.size() - 1, n = arr.size();
    while (left + 1 < n && arr[left] < arr[left + 1]) left++;
    while (right > 0 && arr[right] < arr[right - 1]) right--;
    return left > 0 && right < n - 1 && left == right;
}
```

---

## Problem 9: Maximize Sum of k Pairs with Smallest Difference

#### Problem:
Find k pairs with the smallest difference in a sorted array.

#### Approach:
1. Initialize two pointers at the start and end of the array.
2. Pair the elements pointed by these two pointers.
3. Record the pair and move both pointers inward.
4. Repeat until k pairs are recorded or pointers cross.
5. This method prioritizes pairs from both ends to minimize the difference.

#### Example Input:
```
arr = [1, 3, 7, 8, 10]
k = 2
```

#### Code Implementation:
```cpp
vector<pair<int, int>> minDifferencePairs(vector<int>& arr, int k) {
    int left = 0, right = arr.size() - 1;
    vector<pair<int, int>> result;
    while (left < right && k > 0) {
        result.push_back({arr[left], arr[right]});
        left++;
        right--;
        k--;
    }
    return result;
}
```

---

## Problem 10: Find Maximum Index Difference (j - i)

#### Problem:
Find the maximum difference (j - i) such that arr[i] <= arr[j].

#### Approach:
1. Initialize two pointers at the extremes of the array.
2. Check if the current pair satisfies arr[i] <= arr[j].
3. If valid, calculate the difference and update the maximum difference.
4. Depending on the result, adjust the left or right pointer.
5. Continue until the pointers meet.

#### Example Input:
```
arr = [3, 5, 4, 2]
```

#### Code Implementation:
```cpp
int maxIndexDifference(vector<int>& arr) {
    int left = 0, right = arr.size() - 1, maxDiff = 0;
    while (left < right) {
        if (arr[left] <= arr[right]) {
            maxDiff = max(maxDiff, right - left);
            left++;
        } else {
            right--;
        }
    }
    return maxDiff;
}
```
