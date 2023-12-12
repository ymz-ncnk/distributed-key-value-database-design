# Consistency Model
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
