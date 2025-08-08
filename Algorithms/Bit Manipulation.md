### Operators

- **AND (&)**: Both 1 then 1 else 0
- **OR (|)**: Any 1 then 1 else 0
- **XOR (^)**: Different 1, Same 0
- **NOT (~)**: If 1 then 0, and 0 then 1
- **Left Shift (<<)**: Multiply by 2
- **Right Shift (>>)**: Divide by 2

### MSB and LSB

- **MSB (Most Significant Bit)**: Left Most Set Bit
- **LSB (Least Significant Bit)**: Right Most Set Bit

### How Computers Store Negatives

Two methods:
1. **Sign and Magnitude**: If 32 bit then the last bit is reserved for the sign of the number (Not used as -0 doesn't mean anything).
2. **Using 2's Complement**: Last digit is always a 1 for negative numbers and 0 for positive.

### How to Check if ith Bit is Set or Not

In normal terms, taking mod of a number with 2 gives the last bit. Now repeating while (n > 0), divide (n/2) for shifting a bit.

```cpp
bool isBitSet(int n, int i) {
    return (n & (1 << i)) != 0;
}

int main() {
    int n = 5; // Example number
    int i = 1; // Check if the 1st bit is set
    if (isBitSet(n, i)) {
        cout << "Bit is set" << endl;
    } else {
        cout << "Bit is not set" << endl;
    }
    return 0;
}
```

### How to Set ith Bit

```cpp
int setBit(int n, int i) {
    return n | (1 << i);
}

int main() {
    int n = 5; // Example number
    int i = 1; // Set the 1st bit
    cout << "Result after setting bit: " << setBit(n, i) << endl;
    return 0;
}
```

### How to Unset a Bit

```cpp
int unsetBit(int n, int i) {
    return n & ~(1 << i);
}

int main() {
    int n = 5; // Example number
    int i = 0; // Unset the 0th bit
    cout << "Result after unsetting bit: " << unsetBit(n, i) << endl;
    return 0;
}
```

### How to Toggle ith Bit

```cpp
int toggleBit(int n, int i) {
    return n ^ (1 << i);
}

int main() {
    int n = 5; // Example number
    int i = 1; // Toggle the 1st bit
    cout << "Result after toggling bit: " << toggleBit(n, i) << endl;
    return 0;
}
```

### How to Unset the Rightmost Set Bit

Let's take 12 (`1100`). If we do \( 12 - 1 \) then it will change the rightmost set bit to 0 and 1 to all right of them resulting in `1011`. Now just take AND and you got the rightmost bit unset.

```cpp
​int unsetRightmostSetBit(int n) {
    return n & (n - 1);
}

int main() {
    int n = 12; // Example number
    cout << "Result after unsetting rightmost set bit: " << unsetRightmostSetBit(n) << endl;
    return 0;
}
```

## Facts (तथ्य)

#### **AND (&)**
- If we take \( a & b & c \), then the result will be **less than or equal** to **min(a, b, c)**.

##### Questions

1. **Find Subarray with Max AND**

   - [12, 14, 34, 56]
   - Max element will be the answer [56] because if we take any number and AND it, then it will be equal or less than the minimum of that number.

2. **Find Subarray with Min AND**

   - [12, 14, 34, 56]
   - All subarrays will be the answer [12, 14, 34, 56] because if we take AND of all, then it will be equal or less than the minimum element.

3. **Given N. Find N Integers such that:**

#### **OR (|)**
- If we take \( a | b | c \), then the result will be **greater than or equal** to **max(a, b, c)**.

##### Questions

4. **Find Subarray with Max OR**

   - [12, 14, 34, 56]
   - All subarrays will be the answer [12, 14, 34, 56] because if we take OR of all, then it will be equal or greater than the maximum element.

5. **Find Subarray with Min OR**

   - [12, 14, 34, 56]
   - Min element will be the answer [14] because if we take any number and OR it, then it will be equal or greater than the maximum of that number.

#### **XOR (^)**
- XOR is usually used for flipping bits.
- Prefix sum works in XOR, and we don't even have to subtract the extra part. We can just take XOR[l] ^ XOR[r] as the common part will be cancelled out.

##### Questions

1. **Given Two Numbers, Find Number of Operations to Make Them Equal if Flipping a Bit Costs 1**
   - Take XOR and count set bits in the resulting number.
   - Why this works? As we take XOR it will set bit to 1 if bits differ in X and Y which we need.

2. **Given an Array with Repeated Number Even Times Except One. Find that Number**
   - Take XOR of all numbers. Same will turn to 0 except that one.

3. **Given an Array with Repeated Elements 3 Times Except One. Find that Number**
   - Now we can't just XOR it as \( a ^ a ^ a = a \).
   - We have to think in terms of bits like making an array of 64 bits with 0 as the initial value.
   - If a number is occurring 3 times, then it will contribute 3 to that position.
   - If a number is occurring 1 time, then it will contribute 1 to that position.
   - So we will have numbers in the form of \( 3k \) or \( 3k+1 \).
   - Now just iterate over the array and check if ( arr[i] % (3k+1) == 0 ), then set that bit as 1 and output the result.

```cpp
int findUnique(vector<int>& arr) {
    vector<int> bitCount(64, 0);
    for (int num : arr) {
        for (int i = 0; i < 64; ++i) {
            if (num & (1LL << i)) {
                bitCount[i]++;
            }
        }
    }
    int result = 0;
    for (int i = 0; i < 64; ++i) {
        if (bitCount[i] % 3 == 1) {
            result |= (1LL << i);
        }
    }
    return result;
}

int main() {
    vector<int> arr = {2, 2, 3, 2};
    cout << "Unique element is: " << findUnique(arr) << endl;
    return 0;
}
```

4. **Given Array of Integers and q Queries. Answer XOR for Given Range**

   - Straightforward: Make a Prefix XOR Array and do \( prefix[l] ^ prefix[r] \).


```cpp
vector<int> buildPrefixXOR(const vector<int>& arr) {
    vector<int> prefixXOR(arr.size());
    prefixXOR[0] = arr[0];
    for (size_t i = 1; i < arr.size(); ++i) {
        prefixXOR[i] = prefixXOR[i - 1] ^ arr[i];
    }
    return prefixXOR;
}

int rangeXOR(const vector<int>& prefixXOR, int l, int r) {
    if (l == 0) {
        return prefixXOR[r];
    } else {
        return prefixXOR[r] ^ prefixXOR[l - 1];
    }
}

int main() {
    vector<int> arr = {1, 2, 3, 4, 5};
    vector<int> prefixXOR = buildPrefixXOR(arr);
    int l = 1, r = 3;
    cout << "XOR of range [" << l << ", " << r << "] is: " << rangeXOR(prefix);
}
```