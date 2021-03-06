# Algorithm-anatomy

### The K Nearest Neighbor Classifier :

#### Getting the distance between two data elements

I will use a float [ ] to represent a single data element. We will use the getDistanceBetween method to get the distance between 2 float arrays

Here, X_COORDINATE , Y_COORDINATE and Z_COORDINATE have the values 0 , 1 and 2 respectively as per the value indices in their float [ ] .

private Float getDistanceBetween(@NonNull float[] dataElement1, @NonNull float[] dataElement2) {
    int x1 =  Math.round(dataElement1[X_COORDINATE_INDEX]   ) ;
    int y1 =  Math.round(dataElement1[Y_COORDINATE_INDEX]   ) ;
    int z1 =  Math.round(dataElement1[Z_COORDINATE_INDEX ] ) ;
    int x2 =   Math.round(dataElement2[X_COORDINATE_INDEX]   ) ;
    int y2 =  Math.round(dataElement2[Y_COORDINATE_INDEX]   ) ;
    int z2 =  Math.round(dataElement2[Z_COORDINATE_INDEX]   ) ;
    int term1 = (x2 - x1) * (x2 - x1);
    int term2 = (y2 - y1) * (y2 - y1);
    int term3 = (z2 - z1) * (z2 - z1);
    int sum = term1 + term2 + term3;
    String convertedSum = Integer.toString(sum);
    double convertedToDoubleSum = Double.parseDouble(convertedSum);
    double distance = Math.abs (Math.sqrt (convertedToDoubleSum ) );
    String convertedDistance = Double.toString(distance);
    return Float.parseFloat(convertedDistance);
}

Determining the value of K

The value of K determines the number of nearest neighbors to consider while classification.

    K = sqrt ( number of samples in dataset ) / 2

private int determineK () {
    size = data.size();
    String sizeString = Integer.toString( size ) ;
    sizeDouble = Double.parseDouble( sizeString );
    root = Math.sqrt( sizeDouble );
    double rawK = root / 2 ;
    int num = Math.round( ( float )rawK ) ;
    if ( num%2 != 0 ) {
        return num ;
    }
    else {
        return num - 1 ;
    }
}

We simply check whether the K is odd or even. An odd number is convenient for majority voting. if the number is even , then we return the previous number ( number -1 ).
Wrapping Up

for (int index = 0; index < trainingData.size(); index++) {
    float distance = getDistanceBetween(dataPoint, trainingData.get(index));
    distances.add(distance);
    distancesClone.add(distance);
}

Collections.sort(distances, new Comparator<Float>() {
    @Override
    public int compare(Float o1, Float o2) {
        return o1.compareTo(o2);
    }
});

int K = determineK() ;
List<Float >shortestDistances= distances.subList( 0 , K  ) ;
for ( float element : shortestDistances ) {
    Integer indexOnClone = distancesClone.indexOf(element);
    float[] nearestNeighbour = trainingData.get(indexOnClone);
    if (nearestNeighbour [ 3] == 1) {
        CLASS_1.add( nearestNeighbour ) ;
    }
    else if (nearestNeighbour [ 3] == 2) {
        CLASS_2.add( nearestNeighbour ) ;
    }
}
if ( CLASS_1.size() > CLASS_2 .size() ){
    return CLASS_1
}
else {
    return CLASS_2

}

    We loop through the elements in the training data.
    We add the calculated distance ( by getDistanceBetween )in two lists namely distances and distancesClone .
    Sort only the distances in ascending order.
    Determine K by the determineK() method.
    Get the K number of elements from the sorted list ( in ascending ).
    Loop through the elements of the sorted list.
    Get the index of each element from distancesClone using the indexOf method.
    Sort them into different lists according to their classes.
    if CLASS_1 has the greatest size ( or the greatest number of elements ) predict the output as CLASS_1 . ( i.e. by majority )
    
Nearest Neighbor is also called as Instance-based Learning or Collaborative Filtering. It is a useful data mining technique, which allow us to use our past data with known output values to predict an output value for the new incoming data.

public class KNNDemo {
	
	/**
	 * This method is to load the data set.
	 * @param fileName
	 * @return
	 * @throws IOException
	 */
	public static Instances getDataSet(String fileName) throws IOException {
		/**
		 * we can set the file i.e., loader.setFile("finename") to load the data
		 */
		int classIdx = 1;
		/** the arffloader to load the arff file */
		ArffLoader loader = new ArffLoader();
		/** load the traing data */
		loader.setSource(KNNDemo.class.getResourceAsStream("/" + fileName));
		/**
		 * we can also set the file like loader3.setFile(new
		 * File("test-confused.arff"));
		 */
		//loader.setFile(new File(fileName));
		Instances dataSet = loader.getDataSet();
		/** set the index based on the data given in the arff files */
		dataSet.setClassIndex(classIdx);
		return dataSet;
	}


	/**
	 * @param args
	 * @throws Exception 
	 */
	public static void process() throws Exception {
		
		 
		Instances data = getDataSet("data.arff");
		data.setClassIndex(data.numAttributes() - 1);
		//k - the number of nearest neighbors to use for prediction
		Classifier ibk = new IBk(1);		
		ibk.buildClassifier(data);
	
		System.out.println(ibk);
       
        Evaluation eval = new Evaluation(data);
        eval.evaluateModel(ibk, data);
		/** Print the algorithm summary */
		System.out.println("** KNN Demo  **");
		System.out.println(eval.toSummaryString());
	    System.out.println(eval.toClassDetailsString());
		System.out.println(eval.toMatrixString());
		
		

	}

}

### Closest leaf to a given node in Binary Tree :

Given a Binary Tree and a node x in it, find distance of the closest leaf to x in Binary Tree. If given node itself is a leaf, then distance is 0.

I strongly recommend you to minimize your browser and try this yourself first.
The idea is to first traverse the subtree rooted with give node and find the closest leaf in this subtree. Store this distance. Now traverse tree starting from root. If given node x is in left subtree of root, then find the closest leaf in right subtree, else find the closest left in left subtree. Below is the implementation of this idea.

class Node  
{ 
    int key; 
    Node left, right; 
   
    public Node(int key)  
    { 
        this.key = key; 
        left = right = null; 
    } 
} 
   
class Distance  
{ 
    int minDis = Integer.MAX_VALUE; 
} 
   
class BinaryTree  
{ 
    Node root; 
   
    // This function finds closest leaf to root.  This distance 
    // is stored at *minDist. 
    void findLeafDown(Node root, int lev, Distance minDist)  
    { 
           
        // base case 
        if (root == null)  
            return; 
   
        // If this is a leaf node, then check if it is closer 
        // than the closest so far 
        if (root.left == null && root.right == null)  
        { 
            if (lev < (minDist.minDis))  
                minDist.minDis = lev; 
               
            return; 
        } 
   
        // Recur for left and right subtrees 
        findLeafDown(root.left, lev + 1, minDist); 
        findLeafDown(root.right, lev + 1, minDist); 
    } 
   
    // This function finds if there is closer leaf to x through  
    // parent node. 
    int findThroughParent(Node root, Node x, Distance minDist)  
    { 
        // Base cases 
        if (root == null)  
            return -1; 
           
        if (root == x)  
            return 0; 
           
        // Search x in left subtree of root 
        int l = findThroughParent(root.left, x, minDist); 
   
        // If left subtree has x 
        if (l != -1)  
        {     
            // Find closest leaf in right subtree 
            findLeafDown(root.right, l + 2, minDist); 
            return l + 1; 
        } 
   
        // Search x in right subtree of root 
        int r = findThroughParent(root.right, x, minDist); 
   
        // If right subtree has x 
        if (r != -1)  
        { 
            // Find closest leaf in left subtree 
            findLeafDown(root.left, r + 2, minDist); 
            return r + 1; 
        } 
   
        return -1; 
    } 
   
    // Returns minimum distance of a leaf from given node x 
    int minimumDistance(Node root, Node x)  
    { 
        // Initialize result (minimum distance from a leaf) 
        Distance d = new Distance(); 
   
        // Find closest leaf down to x 
        findLeafDown(x, 0, d); 
   
        // See if there is a closer leaf through parent 
        findThroughParent(root, x, d); 
   
        return d.minDis; 
    } 
   
    // Driver program 
    public static void main(String[] args)  
    { 
        BinaryTree tree = new BinaryTree(); 
           
        // Let us create Binary Tree shown in above example 
        tree.root = new Node(1); 
        tree.root.left = new Node(12); 
        tree.root.right = new Node(13); 
   
        tree.root.right.left = new Node(14); 
        tree.root.right.right = new Node(15); 
   
        tree.root.right.left.left = new Node(21); 
        tree.root.right.left.right = new Node(22); 
        tree.root.right.right.left = new Node(23); 
        tree.root.right.right.right = new Node(24); 
   
        tree.root.right.left.left.left = new Node(1); 
        tree.root.right.left.left.right = new Node(2); 
        tree.root.right.left.right.left = new Node(3); 
        tree.root.right.left.right.right = new Node(4); 
        tree.root.right.right.left.left = new Node(5); 
        tree.root.right.right.left.right = new Node(6); 
        tree.root.right.right.right.left = new Node(7); 
        tree.root.right.right.right.right = new Node(8); 
   
        Node x = tree.root.right; 
   
        System.out.println("The closest leaf to node with value "
                + x.key + " is at a distance of "
                + tree.minimumDistance(tree.root, x)); 
    } 
} 

### Graph Data Structure And Algorithms :

A Graph is a non-linear data structure consisting of nodes and edges. The nodes are sometimes also referred to as vertices and the edges are lines or arcs that connect any two nodes in the graph. 

#### Find the minimum cost to reach destination using a train

There are N stations on route of a train. The train goes from station 0 to N-1. The ticket cost for all pair of stations (i, j) is given where j is greater than i. Find the minimum cost to reach the destination.

Consider the following example:

Input: 
cost[N][N] = { {0, 15, 80, 90},
              {INF, 0, 40, 50},
              {INF, INF, 0, 70},
              {INF, INF, INF, 0}
             };
There are 4 stations and cost[i][j] indicates cost to reach j 
from i. The entries where j < i are meaningless.

Output:
The minimum cost is 65
The minimum cost can be obtained by first going to station 1 
from 0. Then from station 1 to station 3.

The minimum cost to reach N-1 from 0 can be recursively written as following:

minCost(0, N-1) = MIN { cost[0][n-1],  
                        cost[0][1] + minCost(1, N-1),  
                        minCost(0, 2) + minCost(2, N-1), 
                        ........, 
                        minCost(0, N-2) + cost[N-2][n-1] } 
			
class shortest_path 
{ 
  
    static int INF = Integer.MAX_VALUE,N = 4; 
    // A recursive function to find the shortest path from 
    // source 's' to destination 'd'. 
    static int minCostRec(int cost[][], int s, int d) 
    { 
        // If source is same as destination 
        // or destination is next to source 
        if (s == d || s+1 == d) 
          return cost[s][d]; 
       
        // Initialize min cost as direct ticket from 
        // source 's' to destination 'd'. 
        int min = cost[s][d]; 
       
        // Try every intermediate vertex to find minimum 
        for (int i = s+1; i<d; i++) 
        { 
            int c = minCostRec(cost, s, i) + 
                    minCostRec(cost, i, d); 
            if (c < min) 
               min = c; 
        } 
        return min; 
    } 
       
    // This function returns the smallest possible cost to 
    // reach station N-1 from station 0. This function mainly 
    // uses minCostRec(). 
    static int minCost(int cost[][]) 
    { 
        return minCostRec(cost, 0, N-1); 
    } 
  
    public static void main(String args[]) 
    { 
        int cost[][] = { {0, 15, 80, 90}, 
                      {INF, 0, 40, 50}, 
                      {INF, INF, 0, 70}, 
                      {INF, INF, INF, 0} 
                    }; 
        System.out.println("The Minimum cost to reach station "+ N+ 
                                               " is "+minCost(cost)); 
    } 
  
}

### Bubble sort :

Bubble Sort is the simplest sorting algorithm that works by repeatedly swapping the adjacent elements if they are in wrong order.

Example:
First Pass:
( 5 1 4 2 8 ) –> ( 1 5 4 2 8 ), Here, algorithm compares the first two elements, and swaps since 5 > 1.
( 1 5 4 2 8 ) –>  ( 1 4 5 2 8 ), Swap since 5 > 4
( 1 4 5 2 8 ) –>  ( 1 4 2 5 8 ), Swap since 5 > 2
( 1 4 2 5 8 ) –> ( 1 4 2 5 8 ), Now, since these elements are already in order (8 > 5), algorithm does not swap them.

Second Pass:
( 1 4 2 5 8 ) –> ( 1 4 2 5 8 )
( 1 4 2 5 8 ) –> ( 1 2 4 5 8 ), Swap since 4 > 2
( 1 2 4 5 8 ) –> ( 1 2 4 5 8 )
( 1 2 4 5 8 ) –>  ( 1 2 4 5 8 )
Now, the array is already sorted, but our algorithm does not know if it is completed. The algorithm needs one whole pass without any swap to know it is sorted.

Third Pass:
( 1 2 4 5 8 ) –> ( 1 2 4 5 8 )
( 1 2 4 5 8 ) –> ( 1 2 4 5 8 )
( 1 2 4 5 8 ) –> ( 1 2 4 5 8 )
( 1 2 4 5 8 ) –> ( 1 2 4 5 8 )


// Java program for implementation of Bubble Sort 
class BubbleSort 
{ 
	void bubbleSort(int arr[]) 
	{ 
		int n = arr.length; 
		for (int i = 0; i < n-1; i++) 
			for (int j = 0; j < n-i-1; j++) 
				if (arr[j] > arr[j+1]) 
				{ 
					// swap arr[j+1] and arr[i] 
					int temp = arr[j]; 
					arr[j] = arr[j+1]; 
					arr[j+1] = temp; 
				} 
	} 

	/* Prints the array */
	void printArray(int arr[]) 
	{ 
		int n = arr.length; 
		for (int i=0; i<n; ++i) 
			System.out.print(arr[i] + " "); 
		System.out.println(); 
	} 

	// Driver method to test above 
	public static void main(String args[]) 
	{ 
		BubbleSort ob = new BubbleSort(); 
		int arr[] = {64, 34, 25, 12, 22, 11, 90}; 
		ob.bubbleSort(arr); 
		System.out.println("Sorted array"); 
		ob.printArray(arr); 
	} 
} 

Output: 
`Sorted array:
11 12 22 25 34 64 90`

### Dijkstra’s shortest path algorithm :

iven a graph and a source vertex in the graph, find shortest paths from source to all vertices in the given graph.

Dijkstra’s algorithm is very similar to Prim’s algorithm for minimum spanning tree. Like Prim’s MST, we generate a SPT (shortest path tree) with given source as root. We maintain two sets, one set contains vertices included in shortest path tree, other set includes vertices not yet included in shortest path tree. At every step of the algorithm, we find a vertex which is in the other set (set of not yet included) and has a minimum distance from the source.

Below are the detailed steps used in Dijkstra’s algorithm to find the shortest path from a single source vertex to all other vertices in the given graph.
Algorithm
1) Create a set sptSet (shortest path tree set) that keeps track of vertices included in shortest path tree, i.e., whose minimum distance from source is calculated and finalized. Initially, this set is empty.
2) Assign a distance value to all vertices in the input graph. Initialize all distance values as INFINITE. Assign distance value as 0 for the source vertex so that it is picked first.
3) While sptSet doesn’t include all vertices
….a) Pick a vertex u which is not there in sptSet and has minimum distance value.
….b) Include u to sptSet.
….c) Update distance value of all adjacent vertices of u. To update the distance values, iterate through all adjacent vertices. For every adjacent vertex v, if sum of distance value of u (from source) and weight of edge u-v, is less than the distance value of v, then update the distance value of v.

`
import java.util.*; 
import java.lang.*; 
import java.io.*; 
  
class ShortestPath 
{ 
    // A utility function to find the vertex with minimum distance value, 
    // from the set of vertices not yet included in shortest path tree 
    static final int V=9; 
    int minDistance(int dist[], Boolean sptSet[]) 
    { 
        // Initialize min value 
        int min = Integer.MAX_VALUE, min_index=-1; 
  
        for (int v = 0; v < V; v++) 
            if (sptSet[v] == false && dist[v] <= min) 
            { 
                min = dist[v]; 
                min_index = v; 
            } 
  
        return min_index; 
    } 
  
    // A utility function to print the constructed distance array 
    void printSolution(int dist[], int n) 
    { 
        System.out.println("Vertex   Distance from Source"); 
        for (int i = 0; i < V; i++) 
            System.out.println(i+" tt "+dist[i]); 
    } 
  
    // Funtion that implements Dijkstra's single source shortest path 
    // algorithm for a graph represented using adjacency matrix 
    // representation 
    void dijkstra(int graph[][], int src) 
    { 
        int dist[] = new int[V]; // The output array. dist[i] will hold 
                                 // the shortest distance from src to i 
  
        // sptSet[i] will true if vertex i is included in shortest 
        // path tree or shortest distance from src to i is finalized 
        Boolean sptSet[] = new Boolean[V]; 
  
        // Initialize all distances as INFINITE and stpSet[] as false 
        for (int i = 0; i < V; i++) 
        { 
            dist[i] = Integer.MAX_VALUE; 
            sptSet[i] = false; 
        } 
  
        // Distance of source vertex from itself is always 0 
        dist[src] = 0; 
  
        // Find shortest path for all vertices 
        for (int count = 0; count < V-1; count++) 
        { 
            // Pick the minimum distance vertex from the set of vertices 
            // not yet processed. u is always equal to src in first 
            // iteration. 
            int u = minDistance(dist, sptSet); 
  
            // Mark the picked vertex as processed 
            sptSet[u] = true; 
  
            // Update dist value of the adjacent vertices of the 
            // picked vertex. 
            for (int v = 0; v < V; v++) 
  
                // Update dist[v] only if is not in sptSet, there is an 
                // edge from u to v, and total weight of path from src to 
                // v through u is smaller than current value of dist[v] 
                if (!sptSet[v] && graph[u][v]!=0 && 
                        dist[u] != Integer.MAX_VALUE && 
                        dist[u]+graph[u][v] < dist[v]) 
                    dist[v] = dist[u] + graph[u][v]; 
        } 
  
        // print the constructed distance array 
        printSolution(dist, V); 
    } 
  
    // Driver method 
    public static void main (String[] args) 
    { 
        /* Let us create the example graph discussed above */
       int graph[][] = new int[][]{{0, 4, 0, 0, 0, 0, 0, 8, 0}, 
                                  {4, 0, 8, 0, 0, 0, 0, 11, 0}, 
                                  {0, 8, 0, 7, 0, 4, 0, 0, 2}, 
                                  {0, 0, 7, 0, 9, 14, 0, 0, 0}, 
                                  {0, 0, 0, 9, 0, 10, 0, 0, 0}, 
                                  {0, 0, 4, 14, 10, 0, 2, 0, 0}, 
                                  {0, 0, 0, 0, 0, 2, 0, 1, 6}, 
                                  {8, 11, 0, 0, 0, 0, 1, 0, 7}, 
                                  {0, 0, 2, 0, 0, 0, 6, 7, 0} 
                                 }; 
        ShortestPath t = new ShortestPath(); 
        t.dijkstra(graph, 0); 
    } 
} 
`

### QuickSort :

Like Merge Sort, QuickSort is a Divide and Conquer algorithm. It picks an element as pivot and partitions the given array around the picked pivot. There are many different versions of quickSort that pick pivot in different ways.

    Always pick first element as pivot.
    Always pick last element as pivot (implemented below)
    Pick a random element as pivot.
    Pick median as pivot.

The key process in quickSort is partition(). Target of partitions is, given an array and an element x of array as pivot, put x at its correct position in sorted array and put all smaller elements (smaller than x) before x, and put all greater elements (greater than x) after x. All this should be done in linear time.

Pseudo Code for recursive QuickSort function :


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

Partition Algorithm
`/* low  --> Starting index,  high  --> Ending index */
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
`

Pseudo code for partition()

`/* This function takes last element as pivot, places
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
}`


### Merge Sort :

Merge Sort is a Divide and Conquer algorithm. It divides input array in two halves, calls itself for the two halves and then merges the two sorted halves. The merge() function is used for merging two halves. The merge(arr, l, m, r) is key process that assumes that arr[l..m] and arr[m+1..r] are sorted and merges the two sorted sub-arrays into one. See following C implementation for details.

`MergeSort(arr[], l,  r)
If r > l
     1. Find the middle point to divide the array into two halves:  
             middle m = (l+r)/2
     2. Call mergeSort for first half:   
             Call mergeSort(arr, l, m)
     3. Call mergeSort for second half:
             Call mergeSort(arr, m+1, r)
     4. Merge the two halves sorted in step 2 and 3:
             Call merge(arr, l, m, r)`
	    
class MergeSort 
{ 
    // Merges two subarrays of arr[]. 
    // First subarray is arr[l..m] 
    // Second subarray is arr[m+1..r] 
    void merge(int arr[], int l, int m, int r) 
    { 
        // Find sizes of two subarrays to be merged 
        int n1 = m - l + 1; 
        int n2 = r - m; 
  
        /* Create temp arrays */
        int L[] = new int [n1]; 
        int R[] = new int [n2]; 
  
        /*Copy data to temp arrays*/
        for (int i=0; i<n1; ++i) 
            L[i] = arr[l + i]; 
        for (int j=0; j<n2; ++j) 
            R[j] = arr[m + 1+ j]; 
  
  
        /* Merge the temp arrays */
  
        // Initial indexes of first and second subarrays 
        int i = 0, j = 0; 
  
        // Initial index of merged subarry array 
        int k = l; 
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
  
        /* Copy remaining elements of L[] if any */
        while (i < n1) 
        { 
            arr[k] = L[i]; 
            i++; 
            k++; 
        } 
  
        /* Copy remaining elements of R[] if any */
        while (j < n2) 
        { 
            arr[k] = R[j]; 
            j++; 
            k++; 
        } 
    } 
  
    // Main function that sorts arr[l..r] using 
    // merge() 
    void sort(int arr[], int l, int r) 
    { 
        if (l < r) 
        { 
            // Find the middle point 
            int m = (l+r)/2; 
  
            // Sort first and second halves 
            sort(arr, l, m); 
            sort(arr , m+1, r); 
  
            // Merge the sorted halves 
            merge(arr, l, m, r); 
        } 
    } 
  
    /* A utility function to print array of size n */
    static void printArray(int arr[]) 
    { 
        int n = arr.length; 
        for (int i=0; i<n; ++i) 
            System.out.print(arr[i] + " "); 
        System.out.println(); 
    } 
  
    // Driver method 
    public static void main(String args[]) 
    { 
        int arr[] = {12, 11, 13, 5, 6, 7}; 
  
        System.out.println("Given Array"); 
        printArray(arr); 
  
        MergeSort ob = new MergeSort(); 
        ob.sort(arr, 0, arr.length-1); 
  
        System.out.println("\nSorted array"); 
        printArray(arr); 
    } 
}


