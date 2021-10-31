# Consistent Hashing

---

## Distributed hash table (DHT)
- What
  - A distributed system that provides a lookup service similar to a hash table
- Why
  - key-value pairs are stored in a DHT, and any participating node can efficiently retrieve the value associated with a given key
- Advantage
  - nodes can be added or removed with minimum work around re-distributing keys
- Property
  - Autonomy and decentralization
  - Fault tolerance
  - Scalability
- Concept (The way to measure DHT algorithm)
  - Balance
  - Monotonicity
  - Spread
  - Load
- Usage
  - P2P Network 
  - CDN (Static files -> Cache)
  - File System（File -> Disk）
  - Key-value Store (Data -> Store)
  - ... 
- Algorithm
  - Chord
  - CAN
  - Tapestry
  - Pastry
  - Kadelima
  - P-Grid
  - BitTorrent 
  - Karger
  - Rendezvous hashing
  - Ketama
  - Jump Consistent Hash


## Consistent Hashing measurement

### Definition
- Overlay network
  - Nodes in the overlay network can be thought of as being connected by virtual or logical links

- Unstructured network
  - In unstructured peer-to-peer systems, the overlay graph is highly randomized and difficult to characterize. 
  - There are no specific requirements for the placement of data objects (or pointers to them), which are spread across arbitrary peers in the network.

- Structured network
  - introduce much tighter control on overlay structuring, message routing, and object placement
  - Each peer is assigned a unique hash ID and typically maintains a routing table (different from OSI Layer 4 routint table)

- Problem
  - How to choose Hash function
    - Algorithm
      - md5
      - sha1
      - fnv1_32_hash
      - ketama_hash
      - crc32
    - Tradeoff
      - Compute speed
      - Collision (Spread)
      - CPU/Memory usage
      - Hash space size(e.g. 2^32, 2^128, ...)
        - The larger size may be more scalable, but it needs more cpu/disk size

   - How to compute hash value
     - What info will we use
     - Are these values in ring(or binary tree) a balanced distribution

   - How to decide the total virtual amount
     - Tradeoff
       - performance vs balanced
         - The more virtual nodes, the more balanced. But the performance will be poorer when find the node from hash value

   - How to decide the virtual amount in each node
     - each node has different cpu/memory/disk
     - the distance between each node is totally different

   - How to design topology
     - which type of structured network we want (Ring, Binary tree, ...)

   - What info should a node need to store
     - Routing table (Partial or Full)
     - Lookup table (Optional)
       - If we record lookup table
         - we can find routing rule quickly but need more disk space
         - for a small cluster, it may not improve performance of topology

   - What happened if some nodes crash
     - Will any info lose?
     - Should we copy more than 1 replica for each key-value record?
       - If yes, who is the coordinator between nodes
         - we need coordinators because one record may be stored to multiple nodes
         - we have to choose a node to serve a request each time
   
   - If we want to store a key-value record to many node, what will we do
     - we can store a record to many virtual nodes, but those virtual nodes may point to the same physical node
     - we may import random module when we choose virtual nodes 

   - Consistency vs Availability
     - For the CAP theory, our system can only satisfy consistency or availability

## Reference

- https://www.cs.princeton.edu/courses/archive/fall09/cos518/papers/chash.pdf
- https://www.usenix.org/legacy/event/usenix06/tech/full_papers/qiao/qiao_html/usenix06suo.html
- https://yikun.github.io/2016/06/09/%E4%B8%80%E8%87%B4%E6%80%A7%E5%93%88%E5%B8%8C%E7%AE%97%E6%B3%95%E7%9A%84%E7%90%86%E8%A7%A3%E4%B8%8E%E5%AE%9E%E8%B7%B5/
- https://arthurchiao.art/blog/amazon-dynamo-zh/
- https://iter01.com/181077.html
- https://en.wikipedia.org/wiki/Distributed_hash_table
- https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/87312/
- https://zh.codeprj.com/blog/3b56471.html
- https://cloud.tencent.com/developer/article/1677629
- https://ithelp.ithome.com.tw/articles/10226592
- https://www.iteye.com/blog/scottina-650380