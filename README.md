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
