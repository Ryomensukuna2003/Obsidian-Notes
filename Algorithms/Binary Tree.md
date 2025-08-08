Full Binary Tree - Every children has either 2 or 0 children
Complete Binary Tree - All levels should be completely filled and last level should be left aligned
Prefect Binary Tree - Last level should be completely filled and each parent should have 2 children
Balanced Binary Tree - Height can be at max log(n)
Degenerate Binary Tree - Every Parent has a single children

```cpp
struct Node{
	int data;
	struct Node *left;
	struct Node *right;
	Node (int val){
		data=val;
		left=right=NULL;
	}
}
```

