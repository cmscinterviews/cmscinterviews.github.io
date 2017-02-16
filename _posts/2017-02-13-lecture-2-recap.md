---
layout:     post
title:      Lecture 2 Recap
date:       2017-2-12 12:00:00
summary:    All the material and concepts covered in the second lecture.
categories: Strings, Arrays etc
---


Hi again!

Hopefully you gained some insight into how to approach some questions involving
strings and arrays. The lecture slides can be found [here](https://docs.google.com/presentation/d/1zMbSrBBLBcH5PGgMZKHceZcydBi1U10Q4Az2jRtPkMg/edit?usp=sharing).
As a follow up I would like to discuss some of the problems that are supplied here
so you can get a better understanding of thoughtful approaches.


## Reverse an Array

We want to write a function that reverses an array. Since Java is my strongest
language, I will use that to implement the function. To start my method looks
something like this:

```
public int[] reversedArray(int[] a) {

}
```

For simplicity we are only handling an integer array. It's ok to make assumptions
about the question that don't make the question trivial. What I mean by that is
assumptions are ok as the assumption doesn't remove the technical challenge of
the question. A bad assumption here would be that I already have some method
called reverse that reverses and array for me. This is kind of a bad example, but
the point is to not make assumptions that solve the problem for you. The assumption
that the array is an int array still leave the bulk of the work to us. Additionally,
it is ok to make assumptions that you have helper functions to use in your implementation
of a function, just make sure that you eventually implement them!

This question is pretty straight forward, usually it is important to discuss test
cases before diving into the question but this problem is simple enough that we
can skip them to start. My first intuition is to create a new array of the same
size and then store a reversed version of the input array in it. I will start by
initializing a new array.

```
public int[] reversedArray(int[] a) {
  /** Create a new array to store the reversed array in */
  int[] b = int[a.length];
}
```

After creating the new array with the correct size, now we want to iterate over
the input array and put all of its elements into the new array. My first intuition
was to iterate over all of the elements in reverse order (because we are
reversing the list) but as I thought about that solution, I realized in order to
figure out where to place the new element you have to subtract the current index
from the length of the array anyway so I might as well do a forward iteration.

```
public int[] reversedArray(int[] a) {
  /** Create a new array to store the reversed array in */
  int[] b = int[a.length];
  /** Iterate over the array and place each element **/
  /** in an appropriate place in the new array **/
  for (int i = 0; i < a.length; i++) {

  }
}
```

How do we know what the new location of each element is? Well if we look at a
test case then we can figure out that each element ends up in (a.length - 1 - i)
as their position. So lets place them in that position.

```
public int[] reversedArray(int[] a) {
  /** Create a new array to store the reversed array in */
  int[] b = int[a.length];
  /** Iterate over the array and place each element **/
  /** in an appropriate place in the new array **/
  for (int i = 0; i < a.length; i++) {
    b[a.length - 1 - i] = a[i];
  }

  return b;
}
```
Cool! We reversed an array. Now a follow up question would be, what is the time
complexity of this function? I see that we iterate over the array once so it's
linear time and more specifically O(n) and ϴ(n) (this means the number of steps
we are taking is of the same order of n, not just at most n).

So can we do better? The answer is usually yes. What if we wanted to reverse the
array in place? What would that function look like? Well for starters we no
longer need a new array to store the elements in, so lets start with the for
loop. Note that this is now a void function because we are altering the contents
of the parameter.

```
public void reversedArray(int[] a) {

}
```

Our approach now is to reverse this array in place, so we need to swap every
position with its counterpart. We already know what the position to swap with
from the first array reversal. Each element at i should be swapped with the
element at (a.length - 1 - i). When we consider this, we also notice that a full
iteration of the array isn't sufficient because it will swap all the elements
twice. So let's only iterate over half the array and swap the first half of the
elements with the second half.

```
public void reversedArray(int[] a) {
  /** Iterate over the first half of the elements **/
  for (int i = 0; i < a.length / 2; i++) {
    /** Swap the element with its counterpart **/
    int b = a[i];
    a[i] = a[a.length - i - 1];
    a[a.length - i - 1] - b;
  }
}
```

What's the new complexity of this new function? Since we are only iterating to
half of the array then we now have O(n) and θ(n/2). So we halved the steps!
Additionally, since we are doing our operations in place, the space complexity
has been reduced from linear space to constant space. So my final question is
why would we want to use the first function over the second?


## Check if an array could be a valid encoding of a binary search tree
I thought this was actually a really good question and it was a shame we didn't
get to finish writing it during the lecture. This question can be broken down
into two parts, checking that there is a valid binary tree in the array, and
validating that the tree is a binary search tree. For simplicity we are going
to assume the array is an int array. Additionally, we will assume that the
root of the tree is at position 0 in the array (if it weren't what extra steps
would we have to take?).  

The first problem to tackle is how do we tell if the tree in the array is a
valid binary tree? So the way to solve that problem by itself, is we do a
traversal of the tree from the root, find the number of elements in the tree and
then check the number of elements in the array (through an iteration) and see
if they are equal. A nonexistent node in this tree is just a null value in
the array. So if we had an array that looked like this:

```
[1,2,3,null,null,4,5]
```

Then the tree would look like this:

```
   1
  / \
 2   3
    / \
   4   5
```

The null values in the middle are OK because theyre representing the nonexistent
children of the 2 node. What would be bad is something like this:

```
[1,2,null,null,null,4,5]
```

Now the 4 and 5 no longer are reachable from the root so we have to make sure
there are no stray nodes in the array. If we did a traversal of the tree from
the root node, we would only find 2 nodes (1 and 2), and after we did an iteration
we would find 4 values in the array, this is an invalid tree.

So before we tackle the first problem lets consider the second, we have to make
sure that the tree is actually a valid binary search tree. A naive (and incorrect)
solution would be to simply check that your right child is larger than you and
your left child is smaller and recursively call this check on your children. Why
doesn't that work? This check means the following is a valid binary search tree:

```
   3
  / \
 2   4
    / \
   1   5
```
This is clearly not a valid binary search tree because 1, while smaller than its
parent, is not larger than its grandparent and therefore shouldn't be there. So
when we are traversing a binary search tree, what is actually happening? What
information do we gain when we move to the left or right child of a particular
node? So what actually happens, is when we go to the left or right child of a node,
we tighten the bounds of valid values in the subtree.
Take this tree for example:
```
   4
  / \
 2   7
    / \
   5   8
```
When we are at the root of the tree, the range of elements in that tree is all
the numbers, or (-inf,inf). So when we go to the right child, we are changing
the lower bound of the range of that subtree. Suppose the root of the tree is 5,
we now know that the right subtree's range is (5,inf) and the left subtree's range
is (-inf,5). And when we are traversing the subtrees, we expect the numbers to
be in that range. If the number at the current node is not in the range of the
subtree it roots, then the binary search tree is invalid.

So given these two ideas, how can we come up with an efficient solution? Well
to me it looks like to check that it's a valid tree we have to do a traversal
of the tree in the array, and to check that it's a valid binary search tree, we
have to do a traversal as well. We can probably do both at the same time. I can
write a isValidBST() function that also returns the number of nodes traversed so
when I call it on the root of the tree, it will return the number of nodes in the
tree. I can return -1 as an error value (meaning that it's not a valid BST). After
I get the number of nodes traversed, I can look through the array and check how
many elements were in the array, and if the number matches, then it's valid!
Sounds like a good plan to me so let's start implementing the solution. We
will do the hard function first isValidBST(). This will be a helper function
for the larger, isArrayValidBST() function.

So what should the method signature be? We know it is going to return an integer
(the number of nodes seen), but we also need to keep track of the range of the
current tree and it's root location.

```
public boolean isArrayValidBST(int[] a) {

}

public int isValidBST(int[] a, int currentNode, int leftBound, int rightBound) {

}
```

The first thing we should check for is our base case, if the currentNode is null,
then we have an empty tree and return 0. Additionally, if the currentNode is not null,
but outside of the bounds, return an error value. Finally, how do we represent negative
and positive infinity as part of a range? I think using null, instead of Integer.MAX_VALUE
or some other actual value, is best.

```
public boolean isArrayValidBST(int[] a) {

}

public int isValidBST(int[] a, int currentNode, int leftBound, int rightBound) {
  /** If we are on an empty tree return that we have encountered 0 nodes **/
  if (a[currentNode] == null) {
    return 0;
  }
  /** If the currentNode is outside the range of the current tree */
  if (leftBound != null) {
    if (a[i] < leftBound) {
      return -1;
    }
  }

  if (rightBound != null) {
    if (a[i] > rightBound) {
      return -1;
    }
  }
}
```

Cool, so now we have handled all the base cases of this binary search tree. So
now lets move on to looking at the children. Since the base cases handle all the
error cases, then it is safe for us to just blindly call our function on our child.
First we have to calculate where our children are located. I would prefer to do
this in helper functions, because then the code itself is clearer. I will add two
functions getRightChild(int p) and getLeftChild(int p). That will simply return
the left and right child calculation which is 2p + 1 and 2p + 2 respectively. Note
that this calculation had to be adjusted to accomodate for the 0 indexing of Java.
In most explanations arrays are 1 indexed so the formula is slightly different.

```
public boolean isArrayValidBST(int[] a) {

}

public int isValidBST(int[] a, int currentNode, int leftBound, int rightBound) {
  /** If we are on an empty tree return that we have encountered 0 nodes **/
  if (a[currentNode] == null) {
    return 0;
  }

  /** If the currentNode is outside the range of the current tree */
  if (leftBound != null) {
    if (a[i] < leftBound) {
      return -1;
    }
  }

  if (rightBound != null) {
    if (a[i] > rightBound) {
      return -1;
    }
  }
}

public int getLeftChild(int p) {
  return 2 * p + 1;
}

public int getRightChild(int p) {
  return 2 * p + 2;
}

```

Now that I can get my left and right child, I can add recursive calls and checks
to the function. So first I want to do a recursive call on my right child while
updating the range. If the recursive call returns -1 that means the tree is
invalid and returning -1 immediately is ok. Otherwise proceed to the left child
and update the range and the same logic follows. If both children are valid, then
return the summation of their sizes plus one for the current node.

```
public boolean isArrayValidBST(int[] a) {

}

public int isValidBST(int[] a, int currentNode, int leftBound, int rightBound) {
  /** If we are on an empty tree return that we have encountered 0 nodes **/
  if (a[currentNode] == null) {
    return 0;
  }

  /** If the currentNode is outside the range of the current tree */
  if (leftBound != null) {
    if (a[i] < leftBound) {
      return -1;
    }
  }

  if (rightBound != null) {
    if (a[i] > rightBound) {
      return -1;
    }
  }

  /** Recursively check the left and right children **/
  /** Since we are going left, the right bound is now updated **/
  /** the left bound remains the same **/
  int leftTree = isValidBST(a, getLeftChild(currentNode), leftBound, a[currentNode]);

  /** If the leftTree returns -1 then the tree is invalid **/
  if (leftTree == -1) {
    return -1;
  }

  int rightTree = isValidBST(a, getRightChild(currentNode), a[currentNode], rightBound);

  if (rightTree == -1) {
    return -1;
  }

  /** If neither are error values, return the size of this tree */
  return leftTree + rightTree + 1;

}

public int getLeftChild(int p) {
  return 2 * p + 1;
}

public int getRightChild(int p) {
  return 2 * p + 2;
}

```

Ok now that we have a working check for isValidBST, we need to make sure that there
arent any stray nodes in the array. That means we just need to count the number
of elements in the array. So lets write another helper function to do that.

```
public int numElements(int[] a) {
  int size = 0;

  for (int i = 0; i < a.length; i++) {
    if (a[i] != null) {
      size++;
    }
  }

  return size;
}
```

So now that we have a function that can tell us the number of elements in the array.
Lets combine the two and get a working solution.

```
public boolean isArrayValidBST(int[] a) {

  /** First check that the array contains a validBST **/
  int nodes = isValidBST(a,0,null,null);

  /** If we got an error value return false **/
  if (nodes == -1) {
    return false;
  }

  /** Check the number of elements in the array **/
  /** Return that the two counts are equal **/
  return numElements(a) == nodes;

}

public int isValidBST(int[] a, int currentNode, int leftBound, int rightBound) {
  /** If we are on an empty tree return that we have encountered 0 nodes **/
  if (a[currentNode] == null) {
    return 0;
  }

  /** If the currentNode is outside the range of the current tree */
  if (leftBound != null) {
    if (a[i] < leftBound) {
      return -1;
    }
  }

  if (rightBound != null) {
    if (a[i] > rightBound) {
      return -1;
    }
  }

  /** Recursively check the left and right children **/
  /** Since we are going left, the right bound is now updated **/
  /** the left bound remains the same **/
  int leftTree = isValidBST(a, getLeftChild(currentNode), leftBound, a[currentNode]);

  /** If the leftTree returns -1 then the tree is invalid **/
  if (leftTree == -1) {
    return -1;
  }

  int rightTree = isValidBST(a, getRightChild(currentNode), a[currentNode], rightBound);

  if (rightTree == -1) {
    return -1;
  }

  /** If neither are error values, return the size of this tree */
  return leftTree + rightTree + 1;

}

public int getLeftChild(int p) {
  return 2 * p + 1;
}

public int getRightChild(int p) {
  return 2 * p + 2;
}

/** This function returns the number of non-null elements in an array **/
public int numElements(int[] a) {
  int size = 0;

  for (int i = 0; i < a.length; i++) {
    if (a[i] != null) {
      size++;
    }
  }

  return size;
}

```

That's about it! This should be the correct solution. I haven't tested it so if
you can think of edge cases let me know!
