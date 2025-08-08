## Vector

How vectors are implemented?
It just creates a copy of initial vector and creates a new vector of double its size and copies it to new array
For Example - 2,4,8,16,32,64,128
Time Complexity - O(n)


### Pair [To store 2 info together]

pair<int,int>p1;
p1.first=12;
p1.second='a';
Vector<pair<int,int>>vec1;

### Sort - [[Sorting Algos]]  For Details;

- `sort(vec1.begin(),vec1.end());` ASC to DEC
- `sort(vec1.begin(),vec1.end(),greater<int>());` Reverse
- `sort(vec1.end(),vec1.begin());` <-- SIGSEGV 
								   because sort(where_to_start(), where_to_end()) and then it starts doing where_to_start++ [Undefined Behaviour for this] 

##### Custom Compare function

vector<pair<int,int>>vec1;
`sort(vec1.begin(),vec1.end(),[](pair<int,int>& a, pair<int,int>){`   
	`return a.second < b.second;`
`})`



## Queue

`Queue<int>q1;`
q1.push(1); -> [1] -> Out
q1.push(2); -> [2,1] -> Out
q1.push(3); -> [3,2,1] -> Out
q1.pop(); -> [3,2] -> Out
q1.front(); -> [2]


### Set [Unique elements in sorted order]

- Internal implementation using self balancing binary tree [often Red Black tree]
- Insertion, Deletion, Search -> O(log n) 


### Iterators  (Pointer)
begin() -> Gives pointer to first element
end() -> Gives pointer to element after last element
*iter -> Gives Pointer to current element
iter++, iter--

`set<int> s1;`
`for (int i = 0; i < n; i++) {`
	`int x;cin >> x;`
	`s1.insert(x);`
`}`
`for (auto it = s1.begin(); it != s1.end(); it++) {`
	`cout << *it << " ";`
`}`

lower_bound() -> Gives iterator to the first element >=x
upper_bound() -> Gives iterator to the first element > x
Both functions return end if desired element is not present
O(log n) Time complexity

`if(it!=s1.end()){`
	`cout<<*it<<endl;`
`}`
Because if not present it'll give segmentation fault



### Multiset 
- It can store multiple copies if same element in sorted order 
- Insertion, Deletion, Search -> O(log n) 
- Deletion of all copies of element -> O(log n + k) 
- `multiset<int>ms1;`
- ms1.insert(1);
- ms1.insert(2);
- ms1.insert(2);
- ms1.insert(1);
- ms1.count(2) -> 2 output  O(log n + k)



### Lower Bound and Upper Bound

lower_bound() -> Gives iterator to the first element >=x
upper_bound() -> Gives iterator to the first element > x
How to derive frequency of a element in a sorted order
Example -> [2,3,4,4,4,5,8,9]
upper_bound(4) gives 6th index
lower_bound(4) gives 3rd index
So, Upper_bound-Lower_bound=3


### Map and Unordered Map

Both takes key value pair
Map takes log n to fetch data
Unordered Map takes O(1) and O(n) in worst time

![[Screenshot from 2024-12-18 21-30-55.png|700]]

So the problem arises that we cannot store elements group larger then 10^7 and we use several chaining method to solve this 
- First every number is modded by a digit let's say 10 and when duplicate elements are found then they are chained using linked list in sorted fashion to access it we use this chain and to find that element it takes log n as its sorted
- Now lets say if every element went to same element so it will form a long chain resulting collision and this is the fact that Unordered map takes O(N) time for accessing element in worst case



### Priority Queue
**Priority queue** is implemented in the Standard Template Library (STL) using a **max-heap** by default, and it is internally implemented with a **vector** and the **make_heap**, **push_heap**, and **pop_heap** algorithms 

Time Complexities:
- **Insertion**: O(log n)
- **Access Top Element**: O(1)
- **Removal (pop)**: O(log n)
