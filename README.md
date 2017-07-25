# Problem Formulation
We formulate the problem as an "approximate nearest neighbour" and provide a solution in high dimensional data. In our case, data lies in three dimensional space with longitude, latitude, and age of the person as the 3 dimensions. Given, the data is stored in a data structure we should be able to query a person for his/her top 10 nearest neighbor.

Our solution will comprised of KDTree a data structure that stores persons and will allows query with time complexity of the order of the depth of the tree which is usually logarithmic in the number of elements in our datastore.

# Data Generation


# Proposed Solution
In this section we presents our solution

## KD Tree
The KDTree is a binary tree where nodes corresponds to point in a k-dimensional space. Leaves usually store the actual data (or a collection of them) and non-leaves are implicitly generating a hyperplane that divides the space into two parts. Points to the left/right of this hyperplane are represented by the left/right subtree. 

## Distance Measure
The first issues one should deals with the measure of "closeness"? Depending on the application one might consider different metrics.
For example, one might see 2 years of age a big difference for persons which make the people very far from each other even if they are located in the same city. However, for a different application 2 years of different might be considered very close while locating even on a village will be a large distance. We capture the distance between the three dimensional persons p = [p.lat, p.long, p.age] and q = [q.lat, q.long, q.age] by an inverse kernel matrix K where the distance can be computed via
$$ dist(p, q) = (p-q)' K^-1 (p-q) $$
where, $K$ is any positive semidefinite matrix.

In our case, we choose $K$ to be diagonal, i.e., different dimensions don't interact with each other for computing the distance. In other words, the difference that age is imposing is indecent of the location and vice versa.
Therefore, we simply get $K = diag([c_{lat}, c_{long}, c_{age}])$ where the elements are the normalizer for the corresponding dimension.
In this case the distance simply reduces to:
$$ dist(p, q) =  \sqrt{ (p.lat-q.lat)^2/c_{lat} + (p.long-q.long)^2/c_{long} + (p.age-q.age)^2/c_{age} } $$

## Tree Construction
The tree is simply constructed in a recursive fashion. The constructor of the KDTree first fins the best dimension to with which to split the points. We try to select the dimension whose range is the biggest first. Generally, more complex dimension selection can be employed depending on the application. Then, we find the median and divide the points into two groups by being on the left or right of the median along the selected dimension. The left and the right child nodes are created and the construction proceeds recursively until a node contains 20 or less points. In this case the recursion will stop.
The complexity of the algorithm is $O(n \log(n))$ where $n$ is the number of elements.

## Tree Search
The search is conducted using the tree structure. We compare the query with the root of the current subtree along the selected axis. If it's smaller we go to the left subtree and if the query value is larger we recurse the search procedure on the right subtree. This will continue until we reach out to a leaf. A leaf will have more than 10 and less than 20 nodes (because of the cutoff value of 20 we used in the previous section). Then, we simply sort this set based on their distance and output the top 10 closest ones.
This procedure takes $O (log n)$ time.

## Discussion
The algorithm we proposed based on KDTree requires $O(n \log(n))$ as a preprocess and $O(log(n))$ for the query time. Just compare to the naive algorithm which takes $O(1)$ for preprocessing and $O(n)$ for query time.
Alternatively, one may be able to use Hash. More specifically, Locality Sensitive Hashing is an algorithm which can be efficient as well. In our case, the KDtree algorithm was efficient enough.
