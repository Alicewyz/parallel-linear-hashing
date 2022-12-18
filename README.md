# parallel-linear-hashing
##### Yizhen Wu, Jiaqi Dong     
##### Andrew ID: yizhenw, jiaqidon

### URL: 
https://alicewyz.github.io/parallel-linear-hashing/

MileStone Report: https://alicewyz.github.io/parallel-linear-hashing/milestone/project%20milestone%20report.pdf

Final Report: https://alicewyz.github.io/parallel-linear-hashing/milestone/Final%20Project%20Report.pdf

### Summary: 
We implemented a concurrent hash table using linear hashing algorithm and compared performance of coarse-locked, fine-locked, and locked-free versions of the algorithm. We performed experiments on an 8-core Intel Core i7 machine and demonstrated that the lock-free version can achieve 8x speedup compared to lock-based versions in most workloads.


### Background:
The scenario for our project will be multiple threads trying to access and update the same hash table. This can happen in parallel applications where different threads try to perform read and write to a hash table concurrently. This can also occur in database systems where multiple queries access an index backed by linear hashing concurrently.

Linear hashing is a dynamic data structure which implements a hash table that grows or shrinks as keys are inserted or deleted. Keys are placed into fixed-size buckets and a bucket can be redistributed when overflow occurs. In splitting-round i, the data structure makes use of two hash functions, hi and hi+1, where hk(x) = x % 2i. It also makes use of a split-pointer, p, called next split pointer, that points to the next bucket to be split if overflow occurs. Any keys that hash to buckets before the split pointer use the first hash function to locate the hash bucket, otherwise it uses the second hash function. Compared to extendible hashing, which doubles the number of the buckets in the table whenever there is a split, linear hashing only increases the number of the buckets by one and splits the bucket at split pointer. However, compared to extendible hashing which only preserves one global variable among different threads, linear hashing would need to maintain four different global variables: splitting round, number of elements, number of buckets, and split pointer.

The hash table supports three key operations: insert (where a key-value pair is inserted into the hashtable), get (where the value corresponding to a given key is retrieved), and remove (where a key-value pair is removed from the hash table). 

In our workload, multiple threads are concurrently inserting, removing, or retrieving key-value pairs from the hash table. Each of these operations could resize table directory size and increase or decrease the number of buckets. Therefore, this algorithm is not data parallel. We focus our efforts primarily on efficient synchronization of concurrent accesses to the data structure. In particular, multiple threads making updates of a hash table can benefit significantly from parallelizing accesses and updates from different threads. Our goal is to maximize concurrency of threads adding and reading data from the same hash table without being blocked by other threads. We also need to ensure hash table correctness such that the hash table ends up in a consistent state after a series of modifications.


### The challenge:
  In our baseline, we use coarse-grained and fine-grained locks to achieve parallelism. However, in a multi-processor setting, it can be expensive for many processors to acquire and release locks. To achieve better parallelism, we intend to design a lock-free linear hashing algorithm based on CAS instructions. There is currently no well-known literature on lock-free linear hashing algorithm, and as a result we will need to experiment and come up with our own solution. In our design, each processor will need to keep a local copy of these basic variables:
  1. a list of hash functions the map will use
  2. number of buckets used in the current hash table
  3. number of split cycle completed
  4. next bucket to split

We use a uint64 integer, and divide it into four parts: first 16 bits for number of buckets, second 16 bits for number of elements, third 16 bits for exponents which decide the version of hash function currently used by the hash table, and last 16 bits for the next bucket to split. The bits can be altered according to different use cases. For example, if the default bucket size is very big, then we can distribute more bits to the number of elements, and less bits to the number of buckets and the next bucket to split.

### Resources:
We may take some inspirations from these papers on making an extensible hash table lock-free:
- https://tropars.github.io/downloads/pdf/publications/spaa2018-FKR-WF_ext_hashing.pdf
- https://www.ida.liu.se/~chrke55/courses/APP/abstracts/gunjo-SplitOrderedLists.pdf
- https://timharris.uk/papers/2001-disc.pdf

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
- Nov 28 - Dec 5: finish implementing lock-free linear hashing table (finished)
- Dec 6 - 12: Develop extensive test cases to ensure correctness (finished)
- Dec 13 - 18: Finalize presentation (finished)
