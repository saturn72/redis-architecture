# signalR acale-architecture
`signalR` options for scale

New to `signalR`? please read the official [documentation](https://docs.microsoft.com/en-us/aspnet/core/signalr)

#`SignalR` Scaling
`signalR` scaling problem is discussed in the `asp.net core` [Hosting and Scaling](https://docs.microsoft.com/en-us/aspnet/core/signalr/scale) page. This page intention is to provide non-biased multiple options to solve scaling problem for `signalR`

## `signalR` scaling problem, in short


#### Reason #1 
Keep fetching cost low as data is availble in small latency
#### Reason #2
Reduce system stress - the system does not fetches already fetched data from the database, freeing it to serve other requests

## Caching options
### Client Caching
- When? 
  - High post-fetch performance 
  - No requirement to share/aync data between app's instances (single app instance matches this criteria)
  - Low cost for fetching data
  - Data is rarely changes and have insignificant effect or no effect on app's behavior
  - Updating and/or deleting the data insignificant effect or no effect on app's behavior

- Pros - Simple, trivial and easy to implement
- Cons
  - Inconsist data is returned
  - No sync between instances

#### Client Caching Flow
```
                               +-----------------------+                               +------+
(1) Request a record --->      |      Application      |   <--- (2) Fetch from DB ---> |  DB  |
                               |                       |                               +------+
                               |-----------------------|
                               | (3) Insert into cache |
    <--- (4) Response          |                       |
                               |                       |
(5) Request same record --->   |                       |
<--- (6) Response from cache   |                       |
                               +-----------------------+

```
### Distributed caching
- When
  - Share/aync data between app's instances is required (multi-instances app)
- Pros
  - Single source of true
- Cons
  - May cause race condition on same resource

#### Distributed Caching Flow
```

                              +----------------------+                              +------+    +---------------------+
(1) Request a record --->     |      Application     |   <-- (2) Fetch from DB -->  |  DB  |    |  Distributed Cache  |
                              |      Instance 1      |                              +------+    |                     |
                              |                      |   ---  (3) Insert into cache   --->      |                     |
  <--- (4) Response           |                      |                                          |                     |
                              +----------------------+                                          |                     |
                                                                                                |                     |
                              +----------------------+                                          |                     |
(5) Request same record --->  |      Application     |  < --- (6) Fetch from dist. cache --->   |                     |
                              |      Instance 2      |                                          |                     |
<--- (7) Response             |                      |                                          +---------------------+
                              +----------------------+
```

### Use both distributed & client Caching 
- When
  - performance,  need to share data between app instances
- Pros
  - same caching snapshot for all clients
- Cons
  - Server side overhead in managing clients cache state

#### Use both distributed & client Caching Flow
```

                              +-----------------------+                              +----+     +---------------------+
(1) Request a record --->     |      Application      |   <-- (2) Fetch from DB -->  | DB |     |  Distributed Cache  |
                              |      Instance 1       |                              +----+     |                     |
                              |-----------------------|   ---  (3) Insert into cache  --->      |                     |
  <--- (6) Response           | (5) Insert into cache |   <--  (4) Notify app cache  ---        |                     |
                              +-----------------------+                                         |                     |
                                                                                                |                     |
                              +-----------------------+                                         |                     |
(7) Request same record --->  |      Application      |   <--  (4) Notify app cache  ---        |                     |
                              |      Instance 2       |                                         |                     |
<--- (8) Response from cache  |-----------------------|                                         +---------------------+
                              | (5) Insert into cache |
                              +-----------------------+
```
