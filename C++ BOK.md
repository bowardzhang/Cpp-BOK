# C++ BOK
Bo Zhang

## Size of Data Types

size of different data types.

```c++
#include <iostream>
#include <string>

using namespace std;

int main()
{
    cout<<"Hello World"<<endl;
    cout<<sizeof(int)<<endl;        // 4
    cout<<sizeof(double)<<endl;     // 8
    cout<<sizeof(char*)<<endl;      // 8
    cout<<sizeof(string)<<endl;     // 8

    char *a = "123";
    char *b = "123";

    if (a==b)
        cout<<"same pointer"<<&a<<&b<<endl;

    return 0;
}
```
## Function
### Virtual Function
see https://www.geeksforgeeks.org/virtual-function-cpp/

A virtual function a member function which is declared within base class and is re-defined (Overriden) by derived class.When you refer to a derived class object using a pointer or a reference to the base class, you can call a virtual function for that object and execute the derived class’s version of the function.

-   Virtual functions ensure that the correct function is called for an object, regardless of the type of reference (or pointer) used for function call.
-   They are mainly used to achieve [Runtime polymorphism](https://www.geeksforgeeks.org/polymorphism-in-c/)
-   Functions are declared with a  **virtual** keyword in base class.
-   The resolving of function call is done at Run-time.

**Rules for Virtual Functions**

1.  They Must be declared in public section of class.
2.  Virtual functions cannot be static and also cannot be a friend function of another class.
3.  Virtual functions should be accessed using pointer or reference of base class type to achieve run time polymorphism.
4.  The prototype of virtual functions should be same in base as well as derived class.
5.  They are always defined in base class and overridden in derived class. It is not mandatory for derived class to override (or re-define the virtual function), in that case base class version of function is used.
6.  A class may have  [virtual destructor](https://www.geeksforgeeks.org/virtual-destructor/)  but it cannot have a virtual constructor.

**Compile-time(early binding) VS run-time(late binding) behavior of Virtual Functions**

Consider the following simple program showing run-time behavior of virtual functions.

```c++
// CPP program to illustrate concept of Virtual Functions
#include<iostream>
using namespace std;
 
class base
{
public:
    virtual void print ()
    { cout<< "print base class" <<endl; }
 
    void show ()
    { cout<< "show base class" <<endl; }
};
 
class derived:public base
{
public:
    void print ()
    { cout<< "print derived class" <<endl; }
 
    void show ()
    { cout<< "show derived class" <<endl; }
};
 
int main()
{
    base *bptr;
    derived d;
    bptr = &d;
     
    //virtual function, binded at runtime
    bptr->print(); 
     
    // Non-virtual function, binded at compile time
    bptr->show(); 
}

```
Output:

	print derived class
	show base class

## Class

### Class vs Struct

In C++,a structure is same as class except the following differences:

1. Members of a class are `private` by default and members of struct are `public` by default.

   For example program 1 fails in compilation and program 2 works fine.

```c++
//Program 1
#include<stdio.h>

class Test {
	int x;   // x is private
};

int main()
{
  Test t;
  t.x = 20; //compiler error because x is private
  getchar();
  return 0;
}
```

```c++
//Program 2
#include<stdio.h>

struct Test {
	int x;// x is public
};

int main()
{
  Test t;
  t.x = 20; // worksfine because x is public
  getchar();
  return 0;
}
```

1. When deriving a struct from a class/struct, default access-specifier for a baseclass/struct is `public`. And when deriving a class, default access specifier is `private`.

   For example program 3 fails in compilation and program 4 works fine.

```c++
//Program 3
#include<stdio.h>
class Base {
public:
	int x;
};

class Derived : Base { }; // is equilalent to class Derived : privateBase {}

int main()
{
  Derived d;
  d.x = 20; //compiler error becuase inheritance is private
  getchar();
  return 0;
}
```



```c++
//Program 4
#include<stdio.h>

class Base {
public:
	int x;
};

struct Derived : Base { }; // is equilalent to struct Derived : publicBase {}

int main()
{
  Derived d;
  d.x = 20; // works fine becuase inheritance is public
  getchar();
  return 0;
}
```

 From<<https://www.geeksforgeeks.org/g-fact-76/>>

### Class Member Access

Access modifiers are used to implement important feature of Object Oriented Programming known as Data Hiding. Consider a real life example: What happens when a driver applies brakes? The car stops. The driver only knows that to stop the car, he needs to apply the brakes. He is unaware of how actually the car stops. That is how the engine stops working or the internal implementation on the engine side. This is what data hiding is.

Access modifiers or Access Specifiers in a [class](https://www.geeksforgeeks.org/c-classes-and-objects/) are used to set the accessibility of the class members. That is, it sets some restrictions on the class members not to get directly accessed by the outside functions.

There are 3 types of access modifiers available in C++:

1. Public
 2. Private
 3. Protected

- Note: If we do not specify any access modifiers for the members inside the class then by default the access modifier for the members will be Private.

Let us now look at each one these access modifiers in details:

- **Public**: All the class members declared under public will be      available to everyone. The data members and member functions declared      public can be accessed by other classes too. The public members of a      class can be accessed from anywhere in the program using the direct      member access operator (.) with the object of that class.

```c++
  // C++ program to demonstrate public access modifier
  #include<iostream>
  using namespace std;

  // class definition
  class Circle
  {
  	public: 
  		double radius;
  		double  compute_area()
  		{
  			return 3.14*radius*radius;
  		}
  };

  int main()
  {
  	Circle obj;

  	// accessing public datamember outside class
  	obj.radius = 5.5;
  	cout << "Radius is:" << obj.radius << "\n";
  	cout << "Area is:" << obj.compute_area();
  	return 0;
  }
  ```

Output:

Radius is:5.5
  Area is:94.985

In the above program the data member radius is public so we are allowed to access it outside the class.

- **Private**: The class members declared as private can be accessed only by the functions inside the class. They are not allowed to be accessed directly by any object or function outside the class. Only the member functions or the [friend functions](https://www.geeksforgeeks.org/friend-class-function-cpp/) are allowed to access the private data membe of a class.
    ​            Example:

```c++
  // C++ program to demonstrate private access modifier
  #include<iostream>

  using namespace std;
  class Circle
  {  
  	private:   // private data member
  		double radius;

  	public:    // public member function   
  		double  compute_area()
  		{   // member function can access private data member radius
  			return 3.14*radius*radius;
  		}
  };

  int main()
  {   
  	// creating object of the class
  	Circle obj;

  	// trying to access private data member directly outside the class
  	obj.radius = 1.5;
  	cout << "Area is:" << obj.compute_area();
  	return 0;

  }
  ```


The output of above program will be a compile time error because we are not allowed to access the private data members of a class directly outside the class.

Output:

In function 'int main()':
    11:16: error: 'double Circle::radius' is private
    ​           double radius;
    ​                  ^
    31:9: error: within this context
    ​       obj.radius = 1.5;
    ​           ^

However we can access the private data members of a class indirectly using the public member functions of the class. Below program explains how to do this:

```c++
  // C++ program to demonstrate private access modifier
  #include<iostream>
  using namespace std;

  class Circle
  {   
  	private: // private data member  
  		double radius;

  	public:    // public member function    
  		double  compute_area(double r)
          {   // member function can access private data member radius
  			radius = r;
  			double area = 3.14*radius*radius;
  			cout << "Radius is:" << radius << endl;
  			cout << "Area is: " << area;
  		}
  };

  int main()
  {   
  	// creating object of the class
  	Circle obj;

  	// trying to access private data member directly outside the class
  	obj.compute_area(1.5);
  	return 0;
  }
  ```

Output:

Radius is:1.5
    Area is: 7.065

- **Protected**: Protected access modifier is similar to that of private access modifiers, the difference is that the class member declared as Protected are inaccessible outside the class but they can be accessed by any subclass(derived class) of that class.

```c++
  // C++ program to demonstrate protected access modifier
  #include <bits/stdc++.h>
  using namespace std;

  // base class
  class Parent
  {   
  	protected:    // protected data members
  		int id_protected;
  };

  // sub class or derived class
  class Child : public Parent
  {
  	public:
  		void setId(int id)
  		{
  			// Child class is able to access the inherited protected data members of base class
  			id_protected = id;
  		}

  		void displayId()
  		{
  			cout << "id_protected is:" << id_protected << endl;
  		}
  };

  int main() {
  	Child obj1;

  	// member function of derived class can access the protected data members of base class
  	obj1.setId(81);
  	obj1.displayId();
  	return 0;
  }
  ```

Output:

id_protected is:81

This article is contributed by Abhirav Kariya and [Harsh Agarwal](https://www.facebook.com/harsh.agarwal.16752) . If you like GeeksforGeeks and would like to contribute, you can also write an article using [contribute.geeksforgeeks.org](http://www.contribute.geeksforgeeks.org/) or mail your article to contribute@geeksforgeeks.org. See your article appearing on the GeeksforGeeks main page and help other Geeks.

Please write comments if you find anything incorrect, or you want to share more information about the topic discussed above.

From <<https://www.geeksforgeeks.org/access-modifiers-in-c/>> 

## Algorithms

### Search Algorithms 

#### Depth First Search

From <https://www.geeksforgeeks.org/depth-first-traversal-for-a-graph/>

```c++
// C++ program to print DFS traversal from a given vertex in a  given graph
#include<iostream>
#include<list>
using namespace std;

// Graph class represents a directed graph using adjacency list representation
class Graph
{
    int V;    // No. of vertices
 
    // Pointer to an array containing
    // adjacency lists
    list<int> *adj;
 
    // A recursive function used by DFS
    void DFSUtil(int v, bool visited[]);
public:
    Graph(int V);   // Constructor
 
    // function to add an edge to graph
    void addEdge(int v, int w);
 
    // DFS traversal of the vertices
    // reachable from v
    void DFS(int v);
};
 
Graph::Graph(int V)
{
    this->V = V;
    adj = new list<int>[V];
}
 
void Graph::addEdge(int v, int w)
{
    adj[v].push_back(w); // Add w to v’s list.
}
 
void Graph::DFSUtil(int v, bool visited[])
{
    // Mark the current node as visited and print it
    visited[v] = true;
    cout << v << " ";
 
    // Recur for all the vertices adjacent to this vertex
    list<int>::iterator i;
    for (i = adj[v].begin(); i != adj[v].end(); ++i)
        if (!visited[*i])
            DFSUtil(*i, visited);
}
 
// DFS traversal of the vertices reachable from v.
// It uses recursive DFSUtil()
void Graph::DFS(int v)
{
    // Mark all the vertices as not visited
    bool *visited = new bool[V];
    for (int i = 0; i < V; i++)
        visited[i] = false;
 
    // Call the recursive helper function
    // to print DFS traversal
    DFSUtil(v, visited);
}
 
int main()
{
    // Create a graph given in the above diagram
    Graph g(4);
    g.addEdge(0, 1);
    g.addEdge(0, 2);
    g.addEdge(1, 2);
    g.addEdge(2, 0);
    g.addEdge(2, 3);
    g.addEdge(3, 3);
 
    cout << "Following is Depth First Traversal"
            " (starting from vertex 2) \n";
    g.DFS(2);
 
    return 0;
}
```

#### Breath First Search

From<<https://www.geeksforgeeks.org/breadth-first-traversal-for-a-graph/>>

```c++
// Program to print BFS traversal from a given source vertex.
// BFS(int s) traverses vertices reachable from s.
#include<iostream>
#include <list>

using namespace std;
 
// This class represents a directed graph using adjacency list representation
class Graph
{
    int V;    // No. of vertices
 
    // Pointer to an array containing adjacency
    // lists
    list<int> *adj;   
public:
    Graph(int V);  // Constructor
 
    // function to add an edge to graph
    void addEdge(int v, int w); 
 
    // prints BFS traversal from a given source s
    void BFS(int s);  
};
 
Graph::Graph(int V)
{
    this->V = V;
    adj = new list<int>[V];
}
 
void Graph::addEdge(int v, int w)
{
    adj[v].push_back(w); // Add w to v’s list.
}
 
void Graph::BFS(int s)
{
    // Mark all the vertices as not visited
    bool *visited = new bool[V];
    for(int i = 0; i < V; i++)
        visited[i] = false;
 
    // Create a queue for BFS
    list<int> queue;
 
    // Mark the current node as visited and enqueue it
    visited[s] = true;
    queue.push_back(s);
 
    // 'i' will be used to get all adjacent vertices of a vertex
    list<int>::iterator i;
 
    while(!queue.empty())
    {
        // Dequeue a vertex from queue and print it
        s = queue.front();
        cout << s << " ";
        queue.pop_front();
 
        // Get all adjacent vertices of the dequeued
        // vertex s. If a adjacent has not been visited, 
        // then mark it visited and enqueue it
        for (i = adj[s].begin(); i != adj[s].end(); ++i)
        {
            if (!visited[*i])
            {
                visited[*i] = true;
                queue.push_back(*i);
            }
        }
    }
}
 
// Driver program to test methods of graph class
int main()
{
    // Create a graph given in the above diagram
    Graph g(4);
    g.addEdge(0, 1);
    g.addEdge(0, 2);
    g.addEdge(1, 2);
    g.addEdge(2, 0);
    g.addEdge(2, 3);
    g.addEdge(3, 3);
 
    cout << "Following is Breadth First Traversal "
         << "(starting from vertex 2) \n";
    g.BFS(2);
 
    return 0;
}
```

#### Binary Search

From <<https://www.geeksforgeeks.org/binary-search/>>

```c++
// C program to implement recursive Binary Search
#include <stdio.h>
 
// A recursive binary search function. It returns location of x in given array arr[l..r] is present, otherwise -1
int binarySearch(int arr[], int l, int r, int x)
{
   if (r >= l)
   {
        int mid = l + (r - l)/2;
 
        // If the element is present at the middle 
        // itself
        if (arr[mid] == x)  
            return mid;
 
        // If element is smaller than mid, then 
        // it can only be present in left subarray
        if (arr[mid] > x) 
            return binarySearch(arr, l, mid-1, x);
 
        // Else the element can only be present
        // in right subarray
        return binarySearch(arr, mid+1, r, x);
   }
 
   // We reach here when element is not 
   // present in array
   return -1;
}
 
int main(void)
{
   int arr[] = {2, 3, 4, 10, 40};
   int n = sizeof(arr)/ sizeof(arr[0]);
   int x = 10;
   int result = binarySearch(arr, 0, n-1, x);
   (result == -1)? printf("Element is not present in array")
                 : printf("Element is present at index %d", result);
   return 0;
}
```

### Sort Algorithms
#### Complexity of Sort Algorithms
from https://www.geeksforgeeks.org/time-complexities-of-all-sorting-algorithms/

Following is time complexity of different sort algorithms

**Algorithm**   | **Best** |  **Average** |  **Worst** 
-------- | :---------: | :---------: | :---------:
Selection Sort | $Ω(n^2)$ | $θ(n^2)$ | $O(n^2)$
Bubble Sort | $Ω(n)$ | $θ(n^2)$ | $O(n^2)$
Insertion Sort | $Ω(n)$ | $θ(n^2)$ | $O(n^2)$
Heap Sort | $Ω(n\log(n))$ | $θ(n\log(n))$| $O(n\log(n))$
Quick Sort | $Ω(n\log(n))$ | $θ(n\log(n))$ | $O(n^2)$
Merge Sort | $Ω(n\log(n))$ | $θ(n\log(n))$ | $O(n\log(n))$
Bucket Sort | $Ω(n+k)$ | $θ(n+k)$ | $O(n^2)$
Radix Sort | $Ω(nk)$ | $θ(nk)$ | $O(nk)$


#### Bubble Sort
From <<https://www.geeksforgeeks.org/bubble-sort/>>

Bubble Sort is the simplest sorting algorithm that works by repeatedly swapping the adjacent elements if they are in wrong order.

Example:

First Pass:
( 5 1 4 2 8 ) –> ( 1 5 4 2 8 ), Here, algorithm comparesthe first two elements, and swaps since 5 > 1.
( 1 5 4 2 8 ) –>  ( 1 4 5 2 8 ), Swap since 5 > 4
( 1 4 5 2 8 ) –>  ( 1 4 2 5 8 ), Swap since 5 > 2
( 1 4 2 5 8 ) –>( 1 4 2 5 8 ), Now, since these elements arealready in order (8 > 5), algorithm does not swap them.

Second Pass:
( 1 4 2 5 8 ) –> ( 1 4 2 5 8 )
( 1 4 2 5 8 ) –> ( 1 2 4 5 8 ), Swap since 4 > 2
( 1 2 4 5 8 ) –> ( 1 2 4 5 8 )
( 1 2 4 5 8 ) –> ( 1 2 4 5 8 )

Now, the array is already sorted, but our algorithm does not know ifit is completed. The algorithm needs one whole pass without any swap to know it is sorted.

Third Pass:
( 1 2 4 5 8 ) –> ( 1 2 4 5 8 )
( 1 2 4 5 8 ) –> ( 1 2 4 5 8 )
( 1 2 4 5 8 ) –> ( 1 2 4 5 8 )
( 1 2 4 5 8 ) –> ( 1 2 4 5 8 )

Following is the implementations of Bubble Sort.
```c++
// C program for implementation of Bubble sort
#include <stdio.h>
 
void swap(int *xp, int *yp)
{
    int temp = *xp;
    *xp = *yp;
    *yp = temp;
}
 
// A function to implement bubble sort
void bubbleSort(int arr[], int n)
{
   int i, j;
   for (i = 0; i < n-1; i++)      
 
       // Last i elements are already in place   
       for (j = 0; j < n-i-1; j++) 
           if (arr[j] > arr[j+1])
              swap(&arr[j], &arr[j+1]);
}
 
/* Function to print an array */
void printArray(int arr[], int size)
{
    int i;
    for (i=0; i < size; i++)
        printf("%d ", arr[i]);
    printf("n");
}
 
// Driver program to test above functions
int main()
{
    int arr[] = {64, 34, 25, 12, 22, 11, 90};
    int n = sizeof(arr)/sizeof(arr[0]);
    bubbleSort(arr, n);
    printf("Sorted array: \n");
    printArray(arr, n);
    return 0;
}
```

#### Insert Sort
see https://www.geeksforgeeks.org/insertion-sort/

Insertion sort is a simple sorting algorithm that works the way we sort playing cards in our hands.

**Algorithm**  
// Sort an arr\[\] of size n  
insertionSort(arr, n)  
Loop from i = 1 to n-1.  
……a) Pick element arr\[i\] and insert it into sorted sequence arr\[0…i-1\]

**Another Example:**  
**12**, 11, 13, 5, 6

Let us loop for i = 1 (second element of the array) to 5 (Size of input array)

i = 1. Since 11 is smaller than 12, move 12 and insert 11 before 12  
**11, 12**, 13, 5, 6

i = 2. 13 will remain at its position as all elements in A\[0..I-1\] are smaller than 13  
**11, 12, 13**, 5, 6

i = 3. 5 will move to the beginning and all other elements from 11 to 13 will move one position ahead of their current position.  
**5, 11, 12, 13**, 6

i = 4. 6 will move to position after 5, and elements from 11 to 13 will move one position ahead of their current position.  
**5, 6, 11, 12, 13**

```c++
// C program for insertion sort
#include <stdio.h>
#include <math.h>
 
/* Function to sort an array using insertion sort*/
void insertionSort(int arr[], int n)
{
   int i, key, j;
   for (i = 1; i < n; i++)
   {
       key = arr[i];
       j = i-1;
 
       /* Move elements of arr[0..i-1], that are
          greater than key, to one position ahead
          of their current position */
       while (j >= 0 && arr[j] > key)
       {
           arr[j+1] = arr[j];
           j = j-1;
       }
       arr[j+1] = key;
   }
}
 
// A utility function to print an array of size n
void printArray(int arr[], int n)
{
   int i;
   for (i=0; i < n; i++)
       printf("%d ", arr[i]);
   printf("\n");
}
 
/* Driver program to test insertion sort */
int main()
{
    int arr[] = {12, 11, 13, 5, 6};
    int n = sizeof(arr)/sizeof(arr[0]);
 
    insertionSort(arr, n);
    printArray(arr, n);
 
    return 0;
}
```

#### Merge Sort
Like [QuickSort](http://quiz.geeksforgeeks.org/quick-sort/), Merge Sort is a [Divide and Conquer](https://www.geeksforgeeks.org/divide-and-conquer-set-1-find-closest-pair-of-points/) algorithm. It divides input array in two halves, calls itself for the two halves and then merges the two sorted halves.  **The merge() function** is used for merging two halves. The merge(arr, l, m, r) is key process that assumes that arr\[l..m\] and arr\[m+1..r\] are sorted and merges the two sorted sub-arrays into one. See following C implementation for details.

```
MergeSort(arr[], l,  r)
If r > l
     1. Find the middle point to divide the array into two halves:  
             middle m = (l+r)/2
     2. Call mergeSort for first half:   
             Call mergeSort(arr, l, m)
     3. Call mergeSort for second half:
             Call mergeSort(arr, m+1, r)
     4. Merge the two halves sorted in step 2 and 3:
             Call merge(arr, l, m, r)
```

```c++
/* C program for Merge Sort */
#include<stdlib.h>
#include<stdio.h>
 
// Merges two subarrays of arr[].
// First subarray is arr[l..m]
// Second subarray is arr[m+1..r]
void merge(int arr[], int l, int m, int r)
{
    int i, j, k;
    int n1 = m - l + 1;
    int n2 =  r - m;
 
    /* create temp arrays */
    int L[n1], R[n2];
 
    /* Copy data to temp arrays L[] and R[] */
    for (i = 0; i < n1; i++)
        L[i] = arr[l + i];
    for (j = 0; j < n2; j++)
        R[j] = arr[m + 1+ j];
 
    /* Merge the temp arrays back into arr[l..r]*/
    i = 0; // Initial index of first subarray
    j = 0; // Initial index of second subarray
    k = l; // Initial index of merged subarray
    while (i < n1 && j < n2)
    {
        if (L[i] <= R[j])
        {
            arr[k] = L[i];
            i++;
        }
        else
        {
            arr[k] = R[j];
            j++;
        }
        k++;
    }
 
    /* Copy the remaining elements of L[], if there are any */
    while (i < n1)
    {
        arr[k] = L[i];
        i++;
        k++;
    }
 
    /* Copy the remaining elements of R[], if there are any */
    while (j < n2)
    {
        arr[k] = R[j];
        j++;
        k++;
    }
}
 
/* l is for left index and r is right index of the
   sub-array of arr to be sorted */
void mergeSort(int arr[], int l, int r)
{
    if (l < r)
    {
        // Same as (l+r)/2, but avoids overflow for large l and h
        int m = l+(r-l)/2;
 
        // Sort first and second halves
        mergeSort(arr, l, m);
        mergeSort(arr, m+1, r);
 
        merge(arr, l, m, r);
    }
}
 
/* UTILITY FUNCTIONS */
/* Function to print an array */
void printArray(int A[], int size)
{
    int i;
    for (i=0; i < size; i++)
        printf("%d ", A[i]);
    printf("\n");
}
 
/* Driver program to test above functions */
int main()
{
    int arr[] = {12, 11, 13, 5, 6, 7};
    int arr_size = sizeof(arr)/sizeof(arr[0]);
 
    printf("Given array is \n");
    printArray(arr, arr_size);
 
    mergeSort(arr, 0, arr_size - 1);
 
    printf("\nSorted array is \n");
    printArray(arr, arr_size);
    return 0;
}
```

#### Heap Sort
Heap sort is a comparison based sorting technique based on Binary Heap data structure. It is similar to selection sort where we first find the maximum element and place the maximum element at the end. We repeat the same process for remaining element.

**What is  [Binary Heap](http://geeksquiz.com/binary-heap/)?**  
Let us first define a Complete Binary Tree. A complete binary tree is a binary tree in which every level, except possibly the last, is completely filled, and all nodes are as far left as possible (Source  [Wikipedia](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees))

A  [Binary Heap](http://geeksquiz.com/binary-heap/)  is a Complete Binary Tree where items are stored in a special order such that value in a parent node is greater(or smaller) than the values in its two children nodes. The former is called as max heap and the latter is called min heap. The heap can be represented by binary tree or array.

**Why array based representation for Binary Heap?**  
Since a Binary Heap is a Complete Binary Tree, it can be easily represented as array and array based representation is space efficient. If the parent node is stored at index I, the left child can be calculated by 2 * I + 1 and right child by 2 * I + 2 (assuming the indexing starts at 0).

**Heap Sort Algorithm for sorting in increasing order:**  
**1.**  Build a max heap from the input data.  
**2.**  At this point, the largest item is stored at the root of the heap. Replace it with the last item of the heap followed by reducing the size of heap by 1. Finally, heapify the root of tree.  
**3.**  Repeat above steps while size of heap is greater than 1.

**How to build the heap?**  
Heapify procedure can be applied to a node only if its children nodes are heapified. So the heapification must be performed in the bottom up order.

Lets understand with the help of an example:
```
Input data: 4, 10, 3, 5, 1
              4(0)
             /   \
         10(1)   3(2)
         /   \
     5(3)    1(4)

The numbers in bracket represent the indices in the array 
representation of data.

Applying heapify procedure to index 1:
              4(0)
             /   \
         10(1)    3(2)
         /   \
     5(3)    1(4)

Applying heapify procedure to index 0:
            10(0)
            /  \
         5(1)  3(2)
        /   \
     4(3)    1(4)
The heapify procedure calls itself recursively to build heap
 in top down manner.
```

```c++
// C++ program for implementation of Heap Sort
#include <iostream>
using namespace std;
 
// To heapify a subtree rooted with node i which is an index in arr[]. n is size of heap
void heapify(int arr[], int n, int i)
{
    int largest = i;  // Initialize largest as root
    int l = 2*i + 1;  // left = 2*i + 1
    int r = 2*i + 2;  // right = 2*i + 2
 
    // If left child is larger than root
    if (l < n && arr[l] > arr[largest])
        largest = l;
 
    // If right child is larger than largest so far
    if (r < n && arr[r] > arr[largest])
        largest = r;
 
    // If largest is not root
    if (largest != i)
    {
        swap(arr[i], arr[largest]);
 
        // Recursively heapify the affected sub-tree
        heapify(arr, n, largest);
    }
}
 
// main function to do heap sort
void heapSort(int arr[], int n)
{
    // Build heap (rearrange array)
    // starting from the first non-leaf node
    for (int i = n / 2 - 1; i >= 0; i--) 
        heapify(arr, n, i);
 
    // One by one extract an element from heap
    for (int i=n-1; i>=0; i--)
    {
        // Move the largest element at root to end
        swap(arr[0], arr[i]);
 
        // shrink heap by 1 and heapify to get the larget
        heapify(arr, i, 0);
    }
}
 
/* A utility function to print array of size n */
void printArray(int arr[], int n)
{
    for (int i=0; i<n; ++i)
        cout << arr[i] << " ";
    cout << "\n";
}
 
// Driver program
int main()
{
    int arr[] = {12, 11, 13, 5, 6, 7};
    int n = sizeof(arr)/sizeof(arr[0]);
 
    heapSort(arr, n);
 
    cout << "Sorted array is \n";
    printArray(arr, n);
}
```

#### Quick Sort
Like  [Merge Sort](http://quiz.geeksforgeeks.org/merge-sort/), QuickSort is a Divide and Conquer algorithm. It picks an element as pivot and partitions the given array around the picked pivot.  There are many different versions of quickSort that pick pivot in different ways.

1.  Always pick first element as pivot.
2.  Always pick last element as pivot (implemented below)
3.  Pick a random element as pivot.
4.  Pick median as pivot.

The key process in quickSort is partition(). Target of partitions is, given an array and an element x of array as pivot, put x at its correct position in sorted array and put all smaller elements (smaller than x) before x, and put all greater elements (greater than x) after x. All this should be done in linear time.
**Pseudo Code for recursive QuickSort function :**
```c++
/* low  --> Starting index,  high  --> Ending index */
quickSort(arr[], low, high)
{
    if (low < high)
    {
        /* pi is partitioning index, arr[pi] is now
           at right place */
        pi = partition(arr, low, high);

        quickSort(arr, low, pi - 1);  // Before pi
        quickSort(arr, pi + 1, high); // After pi
    }
}
```

**Partition Algorithm**  
There can be many ways to do partition, following pseudo code adopts the method given in CLRS book. The logic is simple, we start from the leftmost element and keep track of index of smaller (or equal to) elements as i. While traversing, if we find a smaller element, we swap current element with arr\[i\]. Otherwise we ignore current element.
**Pseudo code for partition()**
```c++
/* This function takes last element as pivot, places
   the pivot element at its correct position in sorted
    array, and places all smaller (smaller than pivot)
   to left of pivot and all greater elements to right
   of pivot */
partition (arr[], low, high)
{
    // pivot (Element to be placed at right position)
    pivot = arr[high];  
 
    i = (low - 1)  // Index of smaller element

    for (j = low; j <= high- 1; j++)
    {
        // If current element is smaller than or
        // equal to pivot
        if (arr[j] <= pivot)
        {
            i++;    // increment index of smaller element
            swap arr[i] and arr[j]
        }
    }
    swap arr[i + 1] and arr[high])
    return (i + 1)
}
```
**Implementation in C++:**
```c++
/* C implementation QuickSort */
#include<stdio.h>
 
// A utility function to swap two elements
void swap(int* a, int* b)
{
    int t = *a;
    *a = *b;
    *b = t;
}
 
/* This function takes last element as pivot, places
   the pivot element at its correct position in sorted
    array, and places all smaller (smaller than pivot)
   to left of pivot and all greater elements to right
   of pivot */
int partition (int arr[], int low, int high)
{
    int pivot = arr[high];    // pivot starts from the right most
    int i = (low - 1);  // Store index of element smaller than pivot
 
    for (int j = low; j <= high- 1; j++)
    {
        // If current element is smaller than or equal to pivot
        if (arr[j] <= pivot)
        {
            i++;    // increment index of smaller element
            swap(&arr[i], &arr[j]); // put the small elements from the left side of array
        }
    }
    swap(&arr[i + 1], &arr[high]); // put the pivot at the correct position (i+1)
    return (i + 1); // return the pivot position
}
 
/* The main function that implements QuickSort
 arr[] --> Array to be sorted,
  low  --> Starting index,
  high  --> Ending index */
void quickSort(int arr[], int low, int high)
{
    if (low < high)
    {
        /* pi is partitioning index, arr[p] is now
           at right place */
        int pi = partition(arr, low, high);
 
        // Separately sort elements before
        // partition and after partition
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
 
/* Function to print an array */
void printArray(int arr[], int size)
{
    int i;
    for (i=0; i < size; i++)
        printf("%d ", arr[i]);
    printf("n");
}
 
// Driver program to test above functions
int main()
{
    int arr[] = {10, 7, 8, 9, 1, 5};
    int n = sizeof(arr)/sizeof(arr[0]);
    quickSort(arr, 0, n-1);
    printf("Sorted array: n");
    printArray(arr, n);
    return 0;
}
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MjMyNzM1MThdfQ==
-->