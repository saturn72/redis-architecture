# redis-architecture
Redis recomended setup by architectural requirements.

## Why Caching?
### Main reason #1 - keep fetching cost low
As the cost of fetching record(s) from database is relatively high - we want to "pay" it only once, so next time the fetched record(s) is required, the cost will be as low as possible.
Cost of fetching a record(s) can be measured in time it takes to process the transaction, system resources required to perform the action, load on our system in general and on the database in particular and more.
By caching we are able to save the fetched data in high-accessed memory layer and make it availalbe in much low cost for next requests while automatically evicting it after was not required.

2. The database system receives less queries, allowing it to serve the same dataset with a smaller number of nodes.
