# Problem Formulation
We formulate the problem as an "approximate nearest neighbour" and provide a solution in high dimensional data. In our case, data lies in three dimensional space with longtitude, latitude, and age of the person as the 3 dimensions. Given, the data is stored in a data structure we should be able to query a person for his/her top 10 nearest neighbour.

Our solution will comprised of KDTree a datastructure that stores persons and will allows query with time complixty of the order of the depth of the tree which is usually lograithmic in the number of elements in our datastore.

# Background

## KD Tree
The KDTree is a binary tree where nodes corresponds to point in a k-dimensional space. Leaves usually store the actual data (or a colleciton of them) and non-leaves are implicitly generating a hyperplane that divides the space into two parts. Points to the left/right of this hyperplane are represented by the left/right subtree.

