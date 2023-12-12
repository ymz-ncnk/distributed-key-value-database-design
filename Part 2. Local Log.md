# Local Log
In a distributed system, we can store the log on each node in a local key-value
database (the sequential number of the log element will be the key). This allows 
us to achieve:
1. Tiny log - when each log item contains only identifiers, such as keys of 
   added or deleted values. The values themselves are stored separately.
2. Random log access - a good property that will allow us to compact logs 
   quite easily. Moreover, we can do compaction in the same transaction that 
   writes the data.
3. And most importantly, the distributed key-value database with the ability to 
   write data in only one phase.