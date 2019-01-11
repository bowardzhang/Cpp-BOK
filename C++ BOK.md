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

## Object Orientation

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

- Access modifiers are used to implement important feature of Object Oriented Programming known as Data Hiding. Consider a real life example: What happens when a driver applies brakes? The car stops. The driver only knows that to stop the car, he needs to apply the brakes. He is unaware of how actually the car stops. That is how the engine stops working or the internal implementation on the engine side. This is what data hiding is.

- Access modifiers or Access Specifiers in a [class](https://www.geeksforgeeks.org/c-classes-and-objects/) are used to set the accessibility of the class members. That is, it sets some restrictions on the class members not to get directly accessed by the outside functions.

- There are 3 types of access modifiers available in C++:

- 1. Public
  2. Private
  3. Protected

- Note: If we do not specify any access modifiers for the members inside the class then by default the access modifier for the members will be Private.

- Let us now look at each one these access modifiers in details:

- - Public: All the class members declared under public will be      available to everyone. The data members and member functions declared      public can be accessed by other classes too. The public members of a      class can be accessed from anywhere in the program using the direct      member access operator (.) with the object of that class.

- ```c++
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

- Output:

- Radius is:5.5
  Area is:94.985

- In the above program the data member radius is public so we are allowed to access it outside the class.

- - Private: The class
    ​      members declared as private can be accessed only by the functions inside the class. They
    ​      are not allowed to be accessed directly by any object or function outside
    ​      the class. Only the member functions or the [friend functions](https://www.geeksforgeeks.org/friend-class-function-cpp/) are allowed to access the private data membe of a class.
    ​            Example:

- ```c++
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

- ​

- The output of above program will be a compile time error because we are not allowed to access the private data members of a class directly outside the class.

- Output:

-  In function 'int main()':
    11:16: error: 'double Circle::radius' is private
    ​           double radius;
    ​                  ^
    31:9: error: within this context
    ​       obj.radius = 1.5;
    ​           ^

- However we can access the private data members of a class indirectly using the public member functions of the class. Below program explains how to do this:

- ```c++
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

- Output:

- Radius is:1.5
    Area is: 7.065

- - Protected: Protected access modifier is similar to that of private access modifiers, the difference is that the class member declared as Protected are inaccessible outside the class but they can be accessed by any subclass(derived class) of that class.

- ```c++
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

- Output:

- id_protected is:81

- This article is contributed by Abhirav Kariya and [Harsh Agarwal](https://www.facebook.com/harsh.agarwal.16752) . If you like GeeksforGeeks and would like to contribute, you can also write an article using [contribute.geeksforgeeks.org](http://www.contribute.geeksforgeeks.org/) or mail your article to contribute@geeksforgeeks.org. See your article appearing on the GeeksforGeeks main page and help other Geeks.

- Please write comments if you find anything incorrect, or you want to share more information about the topic discussed above.

- From <<https://www.geeksforgeeks.org/access-modifiers-in-c/>> 

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
                 : printf("Element is present at index %d", result);
   return 0;
}
```

### Sort Algorithms

#### Bubble sort

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



#### Quick sort

#### Heap sort

