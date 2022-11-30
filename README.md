# parallel-linear-hashing
##### Yizhen Wu, Jiaqi Dong     
##### Andrew ID: yizhenw, jiaqidon

### URL: 
https://alicewyz.github.io/parallel-linear-hashing/

MileStone Report: https://alicewyz.github.io/parallel-linear-hashing/milestone/project%20milestone%20report.pdf

### Summary: 
We are going to implement a lock-free dynamic hash table using linear hashing algorithm and analyze performance using different parallel interfaces such as OpenMP, MPI or SIMD. We will compare lock-free version of our algorithm against fine-grained and coarse-grained lock-based versions.

### Background:
Linear hashing is a dynamic data structure which implements a hash table that grows or shrinks as keys are inserted or deleted. Keys are placed into fixed-size buckets and a bucket can be redistributed when overflow occurs.

The scenario for our project will be multiple processors trying to access and update the same hash table. This can happen in large storage systems where multiple clients want to access the same data server and perform read and write concurrently. This can also occur in database systems where multiple queries access an index backed by linear hashing concurrently.

### The challenge:
  In our baseline, we will use coarse-grained and fine-grained locks to achieve parallelism. However, in a multi-processor setting, it can be expensive for many processors to acquire and release locks. To achieve better parallelism, we intend to design a lock-free linear hashing algorithm based on CAS instructions. There is currently no well-known literature on lock-free linear hashing algorithm, and as a result we will need to experiment and come up with our own solution. In our design, each processor will need to keep a local copy of these basic variables:
  1. a list of hash functions the map will use
  2. number of buckets used in the current hash table
  3. number of split cycle completed
  4. next bucket to split

The difficulty of making this data structure lock-free arises from the fact that when the hash table expands or shrinks, values in a bucket can be redistributed to other buckets. It is difficult to move multiple values within one bucket to another purely using CAS instructions. Furthermore, linear hashing data structure maintains many variables that are shared and synchronized among all processors. We need to come up with an efficient and correct algorithm that can address both problems while also maximizing concurrency.

### Resources:
We may take some inspirations from these papers on making an extensible hash table lock-free:
- https://tropars.github.io/downloads/pdf/publications/spaa2018-FKR-WF_ext_hashing.pdf
- https://www.ida.liu.se/~chrke55/courses/APP/abstracts/gunjo-SplitOrderedLists.pdf

### Goals and deliverables:
75% Goal
- Complete basic coarse-grained and fine-grained locking linear hashing.
- Develop a working and correct lock-free implmentation that can beat the speedup of coarse-grained locking implementation.
- Complete basic test cases that ensure correctness and scale tests that compare performance of lock-based vs. lock-free implementations.

100% Goal
- Improve and optimize the lock-free algorithm through experimenting with different ideas from literature.
- Achieve a speedup that can match fine-grained locking implementation. 
- Develop more sophisticated test cases focusing on multiple concurrent readers and writers.

125% Goal
- Experiment other possible methods of parallelizing the data structure through SIMD, OpenMP, or MPI.
- Compare performance of lock-free implementation against other methods.

In the final presentation we will demo the usage of such a hash table, and show the speedup achieved by using different number of processors and different parallel interfaces. We will also demonstrate the speedup under different proportion of readers and writers to test how the data structure responds to read-heavy vs. write-heavy workloads.

### Platform Choice:
We will use C++ to write the code, and will use ghc machine for parallel interfaces.

### Schedule:
- Nov 9 - 13: implementing basic linear hashing algorithm (finished)
- Nov 14 - 20: implementing a parallel linear hashing algorithm using coarse-grained lock (finished)
- Nov 21 - 27: implementing fine-grained lock (finished)
- Nov 28 - Dec 5: finish implementing lock-free linear hashing table
- Dec 6 - 12: Develop extensive test cases to ensure correctness
- Dec 13 - 18: Finalize presentation
