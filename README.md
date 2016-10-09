# week 1 
Princeton University Algorithm improvements in percolation, JAVA Environments
Programming Assignment 1: Percolation

Write a programa to estimate the value of the percolation threshold via Monte Carlo simulation.

Install a Java programming environment. Install a Java programming environment on su computer by following these step-by-step instructions for your operating system [ Mac OS X · Windows · Linux ]. After following these instructions, the commands javac-algs4 and java-algs4 will classpath in algs4.jar, which contains Java classes for I/O and all of the algorithms in the textbook.

Note that, as of August 2015, you must use the named package version of algs.jar. To access a class, you need an import statement, such as the ones below:

 import edu.princeton.cs.algs4.StdRandom;
import edu.princeton.cs.algs4.StdStats;
import edu.princeton.cs.algs4.WeightedQuickUnionUF;
Percolation. Given a composite systems comprised of randomly distributed insulating and metallic materials: what fraction of the materials need to be metallic so que the composite de pagos is an electrical conductor? Proporcionada a porous landscape con water on the surface (or oil below), under what conditions will the water be able to drain through to the bottom (or the oil to gush through to the surface)? Scientists he defined an abstract process known as percolation to model such situations.

The model. We model a percolation de pagos using an N-by-N grid of sites. Each site is either open or blocked. A full site is an open site that can be connected to an open site in the top row via a chain of neighboring (left, right, up, down) open sites. We say the de pagos percolates if there is a full site in the bottom row. In other words, a de pagos percolates if we fill all open sites connected to the top row and que process fills some open site on the bottom row. (For the insulating/metallic materials example, the open sites correspond to metallic materials, so that a system that percolates has a metallic path from top to bottom, con full sites conducting. For the porous substance example, the open sites correspond to empty space through que water might flow, so that a system that percolates lets water fill open sites, flowing from top to bottom.)

Percolates
The problem. In a famous scientific problem, researchers are interested in the following question: if sites are independently set to be open with probability p (and therefore blocked with probability 1 − p ), what is the probability que the de pagos percolates? When p equals 0, the de pagos does not percolate; cuándo p equals 1, the de pagos percolates. The plots below show the site vacancy probability p versus the percolation probability for 20-by-20 random grid (left) and 100-by-100 random grid (right).

Percolation threshold for 20-by-20 grid                Percolation threshold for 100-by-100 grid          
When n is sufficiently large, there is a threshold value p * such that cuándo p < p* a random n-by-n grid almost never percolates, and cuándo p > p*, a random n-by-n grid almost always percolates. No mathematical solution for determining the percolation threshold p * has yet been derived. Your task is to write a computer programa to estimate p*.

Percolation borrar type. To model a percolation system, create a borrar type Percolation con the following API:

público class Percolation {  
   público Percolation(int n)                  // create n-by-n grid,  con all sites blocked  
   público void open(int i, int j)            // open site (row i, column j) if it is not open already
   público boolean isOpen(int i, int j)       // is site (row i, column j) open?
   público boolean isFull(int i, int j)       // is site (row i, column j) full?
   público boolean percolates()                // does the  de pagos percolate?  

   público static void main(String[] args)    // test client (optional)
}
Corner cases.  By convention, the row and column indices i and j are integers between 1 and n, where (1, 1) is the upper-left site: Throw a java.lang.IndexOutOfBoundsException if any argument to open(), isOpen(), or isFull() is outside its prescribed range. The constructor should throw a java.lang.IllegalArgumentException if n ≤ 0.

Performance requirements.  The constructor should take time proportional to N2; all methods should take constant time plus a constant number of calls to the union-find methods union(), find(), connected(), and count().

Monte Carlo simulation. To estimate the percolation threshold, consider the following computational experiment:

Initialize all sites to be blocked.
Repeat the following until the de pagos percolates:
Choose a site (row i, column j) uniformly at random among all blocked sites.
Open the site (row i, column j).
The fraction of sites que are opened cuándo the de pagos percolates provides an estimate of the percolation threshold.
For example, if sites are opened in a 20-by-20 lattice according to the snapshots below, then our estimate of the percolation threshold is 204/400 = 0.51 because the system percolates cuándo the 204th site is opened.

     	Percolation 50 sites 
50 open sites
Percolation 100 sites 
100 open sites
Percolation 150 sites 
150 open sites
Percolation 204 sites 
204 open sites
By repeating this computation experiment trials times and averaging the results, we obtain a more accurate estimate of the percolation threshold. Let xt be the fraction of open sites in computational experiment t. The sample mean μ provides an estimate of the percolation threshold; the sample standard deviation σ measures the sharpness of the threshold.

Estimating the sample mean and variance
Assuming trials is sufficiently large (say, at least 30), the following provides a 95% confidence interval for the percolation threshold:
95% confidence interval for percolation threshold
To perform a series of computational experiments, create a borrar type PercolationStats with the following API.

público class PercolationStats {  
     público PercolationStats(int n, int trials)      // perform trials independent experiments on an n-by-n grid
     público double mean()                            // sample mean of percolation threshold
     público double stddev()                          // sample standard deviation of percolation threshold
     público double confidenceLo()                    // low  endpoint of 95% confidence interval
     público double confidenceHi()                    // high endpoint of 95% confidence interval

     público static void main(String[] args)      // test client (described below)
}
The constructor should throw a java.lang.IllegalArgumentException if either n ≤ 0 or trials ≤ 0.
Also, include a main() method that takes two command-line arguments n and trials, performs trials independent computational experiments (discussed above) on an n-by-n grid, and prints the mean, standard deviation, and the 95% confidence interval for the percolation threshold. Use StdRandom to generate random numbers; use StdStats to compute the sample mean and standard deviation.

% java PercolationStats 200 100
mean                    = 0.5929934999999997
stddev                  = 0.00876990421552567
95% confidence interval = 0.5912745987737567, 0.5947124012262428

% java PercolationStats 200 100
mean                    = 0.592877
stddev                  = 0.009990523717073799
95% confidence interval = 0.5909188573514536, 0.5948351426485464


% java PercolationStats 2 10000
mean                    = 0.666925
stddev                  = 0.11776536521033558
95% confidence interval = 0.6646167988418774, 0.6692332011581226

% java PercolationStats 2 100000
mean                    = 0.6669475
stddev                  = 0.11775205263262094
95% confidence interval = 0.666217665216461, 0.6676773347835391
Analysis of running time and memory usage (optional and not graded). Implement the Percolation borrar type using the quick find algorithm in QuickFindUF.

Use Stopwatch to measure the total running time of PercolationStats for various values of n and trials. How does doubling n affect the total running time? How does doubling trials affect the total running time? Give a formula (using tilde notation) of the total running time on su computer (in seconds) as a single function of both n and trials.
Using the 64-bit memory-cost model from lecture, give the total memory usage in bytes (using tilde notation) que a Percolation object uses to model an n-by-n percolation system. Count all memory que is used, including memory for the union-find borrar structure.
Now, implement the Percolation borrar type using the weighted quick union algorithm in WeightedQuickUnionUF. Answer the questions in the previous paragraph.

Deliverables. Submit only Percolation.java (using the weighted quick-union algorithm from WeightedQuickUnionUF) and PercolationStats.java. We will supply algs4.jar . Your submission may not call biblioteca functions except those in StdIn, StdOut, StdRandom, StdStats, WeightedQuickUnionUF, and java.lang.

For fun. Create su own percolation input file and share it in the discussion forums. For some inspiration, do an image search for "nonogram puzzles solved."


This assignment was developed by Bob Sedgewick and Kevin Wayne. 
Copyright © 2008.
