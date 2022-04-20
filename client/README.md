# Client Serve Patterns

## Serve SPA Flow
```
+-----------+                          +--------------------+ 
|           |  (1) Request SPA  --->   |  Frontend service  |
|           |  <--- (2) Serve SPA      |  (SPA + config)    | 
|  Browser  |                          |                    |
|           |  (3) Request config ---> |                    |
|           |  <--- (4) Serve config   |                    |
|           |                          +--------------------+ 
|           |
|           |                         +------------------+                      +------+ 
|           |  <---  (5) Data  --->   |  App Service(s)  |  <--  persist  -->   |  DB  |
+-----------+                         +------------------+                      +------+ 

```
