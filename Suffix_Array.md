# Demonstration of Suffix Array and Substring Search

This document demonstrates how to construct a **Suffix Array (SFA)** and use it to efficiently search for substrings in a given string. It also explains key concepts such as lexico-graphical sorting and binary search for substring prefix matching.

---

## **What is a Suffix Array?**
A Suffix Array is a data structure that contains all suffixes of a string sorted in lexicographical order. It enables efficient substring search and other string operations.

### **Key Concepts**
- **Lexicographical Order**: A sorting order similar to how words are arranged in a dictionary.
- **Suffix**: A substring that starts from any position in the string to the end of the string.

---

## **Use Case**
The Suffix Array is commonly used for:
- Substring search.
- Pattern matching in DNA sequences or text processing.

### **Example Use Case**
Suppose we want to check if substring `"GC"` exists in the string `"AGCU"`. Instead of examining all possible substrings, we can leverage a Suffix Array.

---

## **Python Implementation**

```python
# RNA Sequence
sequence = "AGCU"
substring_to_find = "GC"

# Step 1: Generate all suffixes
suffixes = [(sequence[i:], i) for i in range(len(sequence))]

# Step 2: Sort suffixes lexicographically
sorted_suffixes = sorted(suffixes, key=lambda x: x[0])

# Extract sorted suffixes and their indices
sorted_suffixes_only = [suff[0] for suff in sorted_suffixes]
suffix_array = [suff[1] for suff in sorted_suffixes]

# Step 3: Binary search for the substring
def binary_search_prefix(suffixes, substring):
    """Performs binary search to check if substring exists as a prefix."""
    low, high = 0, len(suffixes) - 1
    while low <= high:
        mid = (low + high) // 2
        if suffixes[mid].startswith(substring):
            return True  # Found a prefix match
        elif suffixes[mid] < substring:
            low = mid + 1
        else:
            high = mid - 1
    return False  # Not found

# Step 4: Search for the substring in sorted suffixes
is_found = binary_search_prefix(sorted_suffixes_only, substring_to_find)

# Output
print("Sorted Suffixes:", sorted_suffixes_only)
print("Suffix Array:", suffix_array)
print(f"Does substring '{substring_to_find}' exist in '{sequence}'? {is_found}")
```

---

## **Explanation of the Code**

### **Step 1: Generate Suffixes**
- Extract all suffixes of the string `sequence`.
  ```python
  suffixes = [(sequence[i:], i) for i in range(len(sequence))]
  ```
- For the string `"AGCU"`, suffixes are:
  ```
  [("AGCU", 0), ("GCU", 1), ("CU", 2), ("U", 3)]
  ```

### **Step 2: Sort Suffixes Lexicographically**
- Sort the suffixes based on their string values.
  ```python
  sorted_suffixes = sorted(suffixes, key=lambda x: x[0])
  ```
- Sorted suffixes are:
  ```
  [("AGCU", 0), ("CU", 2), ("GCU", 1), ("U", 3)]
  ```
- Extract only the sorted suffix strings and their starting indices:
  ```python
  sorted_suffixes_only = [suff[0] for suff in sorted_suffixes]
  suffix_array = [suff[1] for suff in sorted_suffixes]
  ```
  Results:
  ```
  sorted_suffixes_only = ["AGCU", "CU", "GCU", "U"]
  suffix_array = [0, 2, 1, 3]
  ```

### **Step 3: Binary Search for Substring**
The function `binary_search_prefix` performs a binary search to efficiently check if a substring exists as a prefix:
```python
def binary_search_prefix(suffixes, substring):
    low, high = 0, len(suffixes) - 1
    while low <= high:
        mid = (low + high) // 2
        if suffixes[mid].startswith(substring):
            return True  # Found a prefix match
        elif suffixes[mid] < substring:
            low = mid + 1
        else:
            high = mid - 1
    return False  # Not found
```

**Logic:**
- Compare the `substring` with the middle element.
- If it matches the prefix, return `True`.
- If the `substring` is lexicographically smaller, search the left half of the array.
- Otherwise, search the right half.

### **Example Execution**
- Input:
  ```python
  suffixes = ["AGCU", "CU", "GCU", "U"]
  substring = "GC"
  ```
- Steps:
  1. Check middle suffix: `"CU"`. (Move right: `low = mid + 1`)
  2. Check next middle suffix: `"GCU"`. (`"GCU".startswith("GC")` is `True`.)
- Output:
  ```
  Does substring 'GC' exist in 'AGCU'? True
  ```

---

## **Output Example**
When the code is executed, it produces:
```
Sorted Suffixes: ['AGCU', 'CU', 'GCU', 'U']
Suffix Array: [0, 2, 1, 3]
Does substring 'GC' exist in 'AGCU'? True
```

---

## **Key Advantages of Using Suffix Array**
1. **Efficiency**: Searching for substrings is \(O(\log n)\) due to binary search.
2. **Preprocessing**: Constructing the Suffix Array is \(O(n \log n)\).
3. **Versatility**: Useful in text processing, bioinformatics (e.g., DNA sequence analysis), and data compression.

---

For any questions or further extensions, feel free to reach out!
