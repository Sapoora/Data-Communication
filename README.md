# Data-Communication
Using Lloyds for source quantization with Gaussian distribution 

Lloyd's algorithm is an iterative method that solves the quantization problem. Lloyd for finding evenly spaced sets of points in subsets of Euclidean spaces and partitions of these subsets into well-shaped and uniformly sized convex cells. Like the closely related k-means clustering algorithm, it repeatedly finds the centroid of each set in the partition and then re-partitions the input according to which of these centroids is closest. In this setting, the mean operation is an integral over a region of space, and the nearest centroid operation results in Voronoi diagrams. 
Lloyd's algorithm starts by an initial placement of some number k of point sites in the input domain. In mesh-smoothing applications, these would be the vertices of the mesh to be smoothed; in other applications they may be placed at random or by intersecting a uniform triangular mesh of the appropriate size with the input domain.

It then repeatedly executes the following relaxation step:

1) The Voronoi diagram of the k sites is computed.
2) Each cell of the Voronoi diagram is integrated, and the centroid is computed.
3) Each site is then moved to the centroid of its Voronoi cell.

We consider the following distribution for the source:

\[
f_X(x) = \frac{1}{\sqrt{2 \pi \sigma^2}} e^{-\frac{x^2}{2 \sigma^2}}
\]

We want to design a quantizer for this source. Therefore, our quantizer will consist of \( 2^b \) regions, where \( b \) is the number of bits. Each region has one representative point (\( c_1, c_2, \ldots, c_{2^b} \)), and the boundaries of the regions (\( u_1, u_2, \ldots, u_{2^b-1} \)) satisfy:

\[
u_0 = -\infty, \quad u_{2^b} = +\infty
\]

The quantizer operates in such a way that for a given value of the source, the region's representative is transmitted instead of the actual value. The objective of this exercise is to design an optimal quantizer for \( b = 2 \). The schematic diagram for \( b = 2 \) is shown below:

**Diagram Description:**
The plot illustrates the Gaussian distribution \( f_X(x) \) divided into four regions (Region 1, Region 2, Region 3, Region 4) based on the specified boundaries \( u_i \). Each region is represented by a representative point \( c_i \), and the values are quantized based on these points.

**Lloyd’s Algorithm:**

The Lloyd’s algorithm works as follows:
1. Generate a large number of samples from the source distribution \( f_X(x) \): \( X_1, X_2, \ldots, X_N \), where \( N \) is large.
2. Start with arbitrary initial boundaries.
3. For each region, find the optimal representative \( c_i(\text{new}) \) such that the average squared error in that region is minimized:
   \[
   c_i(\text{new}) = \arg \min_{c} \sum_{u_{i-1} < x_k < u_i} (x_k - c)^2
   \]
4. Update the boundaries \( u_i \) such that each boundary becomes the midpoint between two consecutive representatives:
   \[
   u_i(\text{new}) = \frac{c_i(\text{new}) + c_{i+1}(\text{new})}{2}
   \]
5. Repeat steps 3 and 4 until convergence.
