# Part 1. Requirements
We want to design a distributed key-value database with the following 
properties:
- Consensus is reached using the Sync algorithm.
- **Write requests are processed by the leader node and executed in one phase**.
- **Each read request is processed by one follower node**.
- Along with the data, the follower can return one of the labels: "old version" 
  or "new version", which indicates that the data is currently changing.
- Update requests are not supported. Instead, the read/modify/write sequence 
  should be used.
- Concurrency control - MVCC.
- Information about writes is stored in a distributed log (each node keeps its 
  log), which is used for synchronization.
- Tiny log - log items contain only data identifiers, the data itself is stored 
  separately.
- Random access log - we have quick random access to log items.
- Nodes are synchronized within a finite period even when the system handles 
  user requests.
- Hash mode - each node applies a hash function to the data when writing, and
  then combines the resulting hash with the previous one. As a result, we will 
  have one hash on each node, which will help us compare their contents.
  