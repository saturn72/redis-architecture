# redis-architecture
Redis recomended setup by architectural requirements.

# Why Caching?
As the cost of fetching record(s) from database is relatively high - we want to "pay" it only once, so next time the fetched record(s) is required, the cost will be as low as possible.
Cost of fetching a record(s) can be measured in time it takes to process the transaction, system resources required to perform the action, load on our system in general and on the database in particular and more.
By caching we are able to save the fetched data in high-accessed memory layer and make it availalbe in much low cost for next requests while automatically evicting it after was not required.

### Main reason #1 
Keep fetching cost low as data is availble in small latency
### Main reason #2
Reduce system stress - the system does not fetches already fetched data from the database, freeing it to serve other requests

## Caching options
### Client Caching
When? performance, no need to share data between app instances (single instance OR the cost of fetching the data is low)
Pros - Simple, trivial and easy to implement
Cons - inconsis data is returned
  no sync between instances
### Distributed caching
When? need to share data between app instances
Pros - Single source of true
Cons - May cause race condition on same resource
## Distributed & client Caching 
When? performance,  need to share data between app instances
Pros - same caching snapshot for all clients
Cons - server overhead for managing the connections
