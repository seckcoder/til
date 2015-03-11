

One question that's often asked is how to convert a sorted list/array to
a binary search tree.

The general algorithm structure can be expressed as:

```lang:cpp

TreeNode *convert(int A[], int p, int r) {
  if (p>r) return NULL;
  
  int m = partition(r-p+1);
  TreeNode *root = new TreeNode(A[m]);
  root->left = convert(A, p,m-1);
  root->right = convert(A, m+1,r);
  return root;
}

```

The key problem for this question is how to implement the partition function.


1. If we want to convert to balanced tree

This is easy. For balanced tree, we have abs(NumNodes(left) - NumNodes(right)) <= 1
So we cut in the middle point.

```lang:cpp
int partition(int n) {
  return n/2;
}
```

2. If we want to conver to complete binary tree

This is a little difficult. We need some math.

First, definition of complete binary tree is that 0 <= Depth(left) - Depth(right) <= 1.
We can think of complete binary tree in this way:
  we fill all elements into the tree layer by layer, from left to right.
  

So the general step is:
  - fill left and right branch so that left and right is perfect bin tree.
  - put the rest elements in the last layer.

Suppose k is the largest depth for the left and right perfect bin tree given n elements.
So:
  2*(2^k-1) + 1 <= n.
  Or:
    NumNodes(left perfect) + NumNodes(right perfect) + 1(root) <= n
  So:
  rest_elems = n - 1 - 2*((2^k-1))
  We add rest_elems to the last layer. For the left branch, we add `std::min(rest_elems, 1<<k)` number
  of elements.
  
```lang:cpp
int partition(int n) {
  int k = int(log2((n-1)/2+1));
  int rest_elems = n - 2*((1<<k)-1)-1;
  return (1<<k)-1 + std::min(rest_elems, 1<<k);
}
```
