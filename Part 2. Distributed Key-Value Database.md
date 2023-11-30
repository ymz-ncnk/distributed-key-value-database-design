# Part 2. Distributed Key-Value Database
More details can be found in the 
[Sync consensus algorithm](https://github.com/ymz-ncnk/sync-consensus-algorithm), 
here are only a few additions.

This is not the final version of the document, work on it is still ongoing.

## Local Log
In a distributied system, we can store the log on each node in a local key-value
database (sequential number of the log element will be the key). This allows us 
to achieve:
1. Tiny log - when each log item contains only identifiers, such as keys of 
   added or deleted values. The values themselves are stored separately.
2. Random log access - is a good property that will allow us to compact logs 
   quite easily. Moreover, we can do compaction in the same transaction that 
   writes the data.
3. And most importantly, the distributed key-value database with the ability to 
   write data in only one phase.

## Consistency Model
[Sync algorithm](https://github.com/ymz-ncnk/sync-consensus-algorithm) states 
that some transactions can be canceled when a new epoch is entered. For us, this 
means that the data read by the client from some node may disappear in the 
future. Actually, it won't be so bad if it will be warned about this.

For example, if data is currently being written to the distributed system, the 
client may receive it from one of the nodes marked as "old version" or as "new 
version". Both of these lables indicate that the writing is in progress and
that it can fail.

Thanks to this approach a read request can be handled by any node from the 
cluster:
1. First, the client sends a request to the leader node, which knows the current 
   state of all data.
2. Than the leader adds metadata to this request and redirects it to one of its 
   followers, who, in turn, will return the necessary data to the client with 
   the appropriate label.

The obvious disadvantage of this algorithm is that the client essentially has to 
make two requests - first to the leader and then to the follower, which is 
time-consuming. However, if we consider not one, but many requests, then several 
nodes will cope with their processing much faster than one. It should also be 
noted that the leader can execute its requests very quickly. In fact, it acts as
a load balancer, while the main burden of request processing falls on the 
followers.
