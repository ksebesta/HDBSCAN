# HDBSCAN
HDBSCAN stands for Hierarchical Density-Based Spatial Clustering of Applications with Noise. This approach uses a technique that extracts flat clusterings that are considered stable clusters. According to the HDBSCAN documentation at https://hdbscan.readthedocs.io/en/latest/how_hdbscan_works.html "We can break it out into a series of steps: 
1) Transform the space according to the density/sparsity. 
2) Build the minimum spanning tree of the distance weighted graph. 
3) Construct a cluster hierarchy of connected components. 
4) Condense the cluster hierarchy based on minimum cluster size. 
5) Extract the stable clusters from the condensed tree."<br>

Lets explain these steps in greater detail for a better understanding. Transforming the space essentially is the way to "find the islands of higher density amongst the sea of sparser noise". This makes the algorthim robust to noise which is extremely useful and important. In order to accomplish this there are two distance metrics that are being used, **core distance** and **mutual reachablity**. Core distance is the distance from the center of the cluster to the boundary of the cluster. This can changed based on how many k neighbors are in the cluster. Essentially, the core distance is the distances from a point (core) to the k nearest neighbor. Mutual reachability is the longest distance between two point looking at the three following options:
- the core distance of the first point (core(a)
- the core distance of the second point (core(b))
- the distance between point a and point b

The mutual reachability gives the information needed to build the minimum spanning tree. Along with the mutual reachability values the practioner needs to choose a threshold value. Create a weighted graph using the mutual reachability and connect all points using the densities from the mutual reachability as edges. If an edge is above the chosen threshold then drop the edge, in this manner the spanning tree is created. It is important to note that the threshold value should start high and then adjust it lower to find the optimal value.<br>

Next, building the cluster heirarchy uses the information from the minimum spanning tree. The cluster heirarchy is essentially creating connected components from the minimum spanning tree. This can be done by sorting the edges of the tree by increasing order of distance and then iterating through to create/merge clusters together for each edge.<br>

From there the HDBSCAN algorithm condenses the cluster tree. This is accomplished by using the prameter **minimum cluster size** as the "fall out point". This means that at each split in the hierarchy if the new cluster that was created by the split has few points than the minimum cluster size than it is considered noise. If both clusters from the split have the minimum cluster size or larger number of points than they are both considered true clusters.<br>

It is important to note that HDBSCAN has 15 parameter to tune which makes this algorithm not only robust to noise but also applicable to many differing data.
