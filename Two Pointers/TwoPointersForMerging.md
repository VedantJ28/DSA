### Two Pointers for Merging  

#### **Use Cases**  
The merging variation of the Two Pointers technique is commonly used in:  
1. **Merging two sorted arrays** without using extra space.  
2. **Merging two sorted linked lists** efficiently.  
3. **Merging intervals** in problems involving range processing.  
4. **Finding intersection or union** of two sorted sequences.  

---

### **1. Merging Two Sorted Arrays Without Extra Space**
#### **Problem:**  
You are given two sorted arrays `arr1` and `arr2`. Merge `arr2` into `arr1` in a sorted manner without using extra space.  

#### **Example Input:**  
```cpp
arr1 = [1, 3, 5, 7];  
arr2 = [2, 4, 6, 8];  
```
#### **Example Output:**  
```cpp
arr1 = [1, 2, 3, 4];  
arr2 = [5, 6, 7, 8];  
```

#### **Approach:**  
- Start comparing from the last element of `arr1` and `arr2`.  
- Swap elements if needed and place them in the correct position.  
- Use insertion sort logic to maintain sorted order.  

#### **Code Implementation:**  
```cpp
void mergeSortedArrays(vector<int>& arr1, vector<int>& arr2) {
    int m = arr1.size(), n = arr2.size();
    int i = m - 1, j = n - 1, k = m + n - 1;

    arr1.resize(m + n);  // Resize arr1 to accommodate arr2
    while (i >= 0 && j >= 0) {
        if (arr1[i] > arr2[j]) arr1[k--] = arr1[i--];
        else arr1[k--] = arr2[j--];
    }
    while (j >= 0) arr1[k--] = arr2[j--];  // Copy remaining elements
}
```

⏳ **Time Complexity:** `O(m + n)`  
- `m` is the size of `arr1`, and `n` is the size of `arr2`.  
- Each element is processed once.

---

### **2. Merge Two Sorted Linked Lists**
#### **Problem:**  
Given two sorted linked lists, merge them into a single sorted linked list.  

#### **Example Input:**  
```cpp
List1: 1 → 3 → 5  
List2: 2 → 4 → 6  
```
#### **Example Output:**  
```cpp
Merged List: 1 → 2 → 3 → 4 → 5 → 6  
```

#### **Approach:**  
- Use two pointers, one for each list.  
- Compare nodes and attach the smaller one to the merged list.  
- Move the pointer forward in the list from which the smaller node was taken.  
- Continue until one list is exhausted, then attach the remaining nodes.  

#### **Code Implementation:**  
```cpp
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    if (!l1) return l2;
    if (!l2) return l1;
    
    if (l1->val > l2->val) swap(l1, l2);
    ListNode* head = l1;

    while (l1 && l2) {
        ListNode* temp = nullptr;
        while (l1 && l1->val <= l2->val) {
            temp = l1;
            l1 = l1->next;
        }
        temp->next = l2;
        swap(l1, l2);
    }
    return head;
}
```

⏳ **Time Complexity:** `O(m + n)`  
- `m` and `n` are the lengths of the two linked lists.  
- Each node is processed once.

---

### **3. Merge Two Sorted Arrays In-Place (Gap Method)**
#### **Problem:**  
Given two sorted arrays `arr1` and `arr2`, merge them without using extra space.  

#### **Example Input:**  
```cpp
arr1 = [1, 5, 9];  
arr2 = [2, 3, 7];  
```
#### **Example Output:**  
```cpp
arr1 = [1, 2, 3];  
arr2 = [5, 7, 9];  
```

#### **Code Implementation:**  
```cpp
void mergeSortedArraysGap(vector<int>& arr1, vector<int>& arr2) {
    int m = arr1.size(), n = arr2.size(), gap = (m + n + 1) / 2;
    
    while (gap > 0) {
        int i = 0, j = gap;
        while (j < (m + n)) {
            if (j < m && arr1[i] > arr1[j]) swap(arr1[i], arr1[j]);
            else if (i < m && j >= m && arr1[i] > arr2[j - m]) swap(arr1[i], arr2[j - m]);
            else if (i >= m && arr2[i - m] > arr2[j - m]) swap(arr2[i - m], arr2[j - m]);
            i++, j++;
        }
        gap = (gap > 1) ? (gap + 1) / 2 : 0;
    }
}
```

⏳ **Time Complexity:** `O((m + n) * log(m + n))`  
- The gap reduces logarithmically, and each iteration processes `m + n` elements.

---

### **4. Merge K Sorted Arrays Using Two Pointers**
#### **Problem:**  
Given `k` sorted arrays, merge them into a single sorted array efficiently.  

#### **Example Input:**  
```cpp
arrays = [[1, 4, 7], [2, 5, 8], [3, 6, 9]]  
```
#### **Example Output:**  
```cpp
[1, 2, 3, 4, 5, 6, 7, 8, 9]  
```

#### **Code Implementation:**  
```cpp
vector<int> mergeTwoSortedArrays(vector<int>& arr1, vector<int>& arr2) {
    vector<int> merged;
    int i = 0, j = 0;
    while (i < arr1.size() && j < arr2.size()) {
        if (arr1[i] < arr2[j]) merged.push_back(arr1[i++]);
        else merged.push_back(arr2[j++]);
    }
    while (i < arr1.size()) merged.push_back(arr1[i++]);
    while (j < arr2.size()) merged.push_back(arr2[j++]);
    return merged;
}

vector<int> mergeKSortedArrays(vector<vector<int>>& arrays) {
    if (arrays.empty()) return {};
    vector<int> mergedArray = arrays[0];

    for (int i = 1; i < arrays.size(); i++) {
        mergedArray = mergeTwoSortedArrays(mergedArray, arrays[i]);
    }
    return mergedArray;
}
```

⏳ **Time Complexity:**  
- **Pairwise merging:** `O(N * K)`  
- **Heap optimization:** `O(N log K)`  
  - `N` is the total number of elements, and `K` is the number of arrays.

---

### **5. Merge Intervals**
#### **Problem:**  
Given an array of overlapping intervals, merge them to remove redundancies.  

#### **Example Input:**  
```cpp
intervals = [[1, 3], [2, 6], [8, 10], [15, 18]]  
```
#### **Example Output:**  
```cpp
[[1, 6], [8, 10], [15, 18]]  
```

#### **Code Implementation:**  
```cpp
vector<vector<int>> mergeIntervals(vector<vector<int>>& intervals) {
    if (intervals.empty()) return {};

    sort(intervals.begin(), intervals.end());
    vector<vector<int>> merged;
    
    for (auto& interval : intervals) {
        if (merged.empty() || merged.back()[1] < interval[0]) {
            merged.push_back(interval);
        } else {
            merged.back()[1] = max(merged.back()[1], interval[1]);
        }
    }
    return merged;
}
```

⏳ **Time Complexity:** `O(N log N)`  
- Sorting the intervals dominates the time complexity.

---

### **6. Intersection of Two Sorted Arrays**
#### **Problem:**  
Find the intersection of two sorted arrays, i.e., elements that appear in both.  

#### **Example Input:**  
```cpp
arr1 = [1, 2, 4, 5, 6];  
arr2 = [2, 3, 5, 7];  
```
#### **Example Output:**  
```cpp
[2, 5]  
```

#### **Code Implementation:**  
```cpp
vector<int> intersectionSortedArrays(vector<int>& arr1, vector<int>& arr2) {
    vector<int> result;
    int i = 0, j = 0;
    
    while (i < arr1.size() && j < arr2.size()) {
        if (arr1[i] == arr2[j]) {
            result.push_back(arr1[i]);
            i++; j++;
        } else if (arr1[i] < arr2[j]) {
            i++;
        } else {
            j++;
        }
    }
    return result;
}
```

⏳ **Time Complexity:** `O(N + M)`  
- `N` and `M` are the sizes of the two arrays.  
- Each element is processed once.