A major application of binary tries is solving problems related to XOR operations, such as finding the maximum XOR subarray or the maximum XOR pair
Now, For node structure we need 2 child as 0 and 1
Also any Trie has a Root node so we add it manually and increment trie dynamically 

```cpp
struct node {
    node *child[2];
    int count = 0; // Extra variables can be defined here
    node() {
        child[0] = NULL;
        child[1] = NULL;
    }
};

struct trie {
    node *root;
    trie() {
        root = new node;
    }
};

```

#### Insert a Element

```cpp
void insert(int n) {
    node *cur = root;  // Start from root node
    for (int i = LN - 1; i >= 0; i--) { // LN is length of bits, e.g., 32 or 64
        cur->count++;
        int x = (n & (1LL << i)) ? 1 : 0; // Check if ith bit of n is set
        if (cur->child[x] == NULL) { // Create node if not present
            cur->child[x] = new node;
        }
        cur = cur->child[x]; // Move to the next bit
    }
    cur->count++;
}

```

Time Complexity - **O(LN)** like 32 or 64


#### Delete a Element

```cpp
void deleteNode(int n) {
    node *cur = root;  // Start from root node
    for (int i = LN - 1; i >= 0; i--) { // LN is length of bits, e.g., 32 or 64
        if (!cur) return; // Ensure the node exists before proceeding
        cur->count--;
        int x = (n & (1LL << i)) ? 1 : 0; // Check if ith bit of n is set
        cur = cur->child[x]; // Move to the next bit
    }
    if (cur) cur->count--;
}

```

Time Complexity - **O(LN)** like 32 or 64



#### Question ---------------------------------------------------------

Given a array A containing N numbers, find pair of indices i,j such that i!=, and the value of A[i]^A[j] is maximum 
Constraints N<=10^5 
			A[i]<=10^6

So basic intuition is to use a data structure in which we can query element in log n time and incrementally build the solution e.g., a1, a2, a3 ,a4, ......... ax, ax+1, ax+2,..... an;

So here if we calculate upto a4 and when its time for ax, we will find best pair in out trie and store it in any array hence building solution incrementally

Hence, Question boils down to given a set of elements in Trie Data structure, for given query X find Y such that x^y is maximum;

Now if we have a number and choices of 0 and 1 so what we will choose for each bit for max XOR?
Opposite bit right?? YES
So we will start from MSB and check for bits of query. if its 1 then we will chose 0 and vice versa

```cpp
int queryMax(int n) {
    node *cur = root;
    int ans = 0; // Store max XOR result
    for (int i = LN - 1; i >= 0; i--) { // Iterate from MSB to LSB
        int x = (n & (1 << i)) ? 1 : 0; // Check the i-th bit of n
        if (cur->child[1 ^ x] != NULL) { // Choose the opposite bit if available
            ans |= (1 << i); // Set the corresponding bit in the answer
            cur = cur->child[1 ^ x]; // Move to the next node
        } else {
            cur = cur->child[x]; // Otherwise, follow the existing path
        }
    }
    return ans;
}

```


#### Question ---------------------------------------------------------

Create a Data Structure, that supports following operations efficiently
	Insert X
	Delete X
	Given Y and k, Find number of x such that (X^Y)<=k

