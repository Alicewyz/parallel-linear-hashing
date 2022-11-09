# parallel-linear-hashing
##### Yizhen Wu, Jiaqi Dong     Andrew ID: yizhenw, jiaqidon

### URL: 
https://github.com/Alicewyz/parallel-linear-hashing.git

### Summary: 
We are going to implement a parallel lock-free linear hashing table and analyze performance using different parallel interfaces such as OpenMP, MPI or SIMD.

### Background:
The scenario for our project will be multiple processors trying to access and update the same hash table. This can happen in large storage systems where multiple clients want to access the same data server and perform read and write concurrently.

### The challenge:
There is currently no well-known published parallel implementation on lock-free linear hashing table, so we will need to experiment and analyze a lot to come to a runnable solution. We will refer to existing linear hashing table implementations, and start our analysis on that. Our baseline will use locks to achieve the parallelism. 
To parallelize a hash table based on linear hashing, each processor will need to keep a local copy of these basic variables:
  1. a list of hash functions the map will use
  2. number of buckets used in the current hash table
  3. number of split cycle completed
  4. next bucket to split
  
The more difficult part will be implementing a lock-free version of linear hashing table. One constrain for realizing lock-free linear hashing is that linear hashing has a lot of variables that needs to be shared and synchronized among all processors, and without locks this will be harder to achieve.

### Resources:
We may borrow some ideas from these papers on making a hash table lock-free:
https://tropars.github.io/downloads/pdf/publications/spaa2018-FKR-WF_ext_hashing.pdf
https://www.ida.liu.se/~chrke55/courses/APP/abstracts/gunjo-SplitOrderedLists.pdf
And for implementing a linear hashing map, we will refer to some codes on github such as:
https://github.com/pavlosdais/Abstract-Data-Types/tree/main/modules/HashTable/LinearHashing#readme

### Goals and deliverables:
Our first goal will be achieveing a usable lock-free linear hashing table, which can be accessed by multiple processors performing read and write to the table without waiting. We would also like to compare the performance of using a lock-free vs lock-used hash table, and how different parallel interfaces can achieve an increase of performance.
In the final presentation we will demo the usage of such a hash table, and show the speedup achieved by using different number of processors and different parallel interfaces.

### Platform Choice:
We will use C++ to write the code, and will use ghc machine for parallel interfaces.

### Schedule:
Nov 9 - 13: implementing a linear hashing table
Nov 14 - 20: implementing a parallel linear hashing table using coarse-grained lock & fine-grained lock using openmpi
Nov 21 - 27: finish implementing lock-used parallel linear hashing table and start on lock-free design
Nov 28 - Dec 5: implementing lock-free linear hashing table
Dec 6 - 11: trying lock-free linear hashing table in simple scenarios
Dec 12 - 18: Finalize presentation
