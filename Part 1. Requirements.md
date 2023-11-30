# Part 1. Requirements
We want to design a distributed key-value database with the following 
properties:
- Consensus is reached using the Sync algorithm.
- Write requests are processed by the leader node.
- Each read request is processed by one follower node.
- Along with the data, the follower can return one of the labels: "old version" 
  or "new version", which indicate that the data is currently changing.
- Update requests are not supported. Instead read/modify/write sequence should 
  be used.
- Concurrency control - MVCC.
- Information about writes is stored in a distributed log (each node keeps its 
  own log), which is used for synchronization.
- Tiny log - log items contain only data identifiers, the data itself is stored 
  separately.
- Random access log - we have quick random access to log items.
- Nodes are synchronized within a finite period of time even when the system
  handles user requests.
