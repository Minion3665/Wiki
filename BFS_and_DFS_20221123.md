# BFS and DFS

## Breadth-first traversal

```text
┌───────────────┐                              ┌──────────┐
│To visit a node│                              │To run BFS│
└───────────────┘                              └──────────┘
        │                                           │
        ▼                                           │
        │                                           ▼
    Has it                                       Visit the starting node
    been visited?───Yes───► No-op                   │
         │                                          │
         No                                         │
         │                                          │
         ▼                                          ▼
     Add its neighbors to the list              Are there nodes in the
     of nodes to search                       ┌──list to search?     ▲
         │                                    │             │        │
         │                                    No            Yes      │
         │                                    ▼             │        │
         ▼                                    Done          │        │
     Add it to the list of visited nodes                    ▼        │
                                                         Visit the first node
```

## Depth-first traversal

```text
  ┌────────────────────┐
  │To run DFS on a node│
  └────────────────────┘
          │
          │
          │
          ▼
     Has the node been visited?──Yes────► Return
          │
          No
          │
          ▼
      Add it to the list of nodes we've visited
          │
          │
          ▼
      For every neighbor, run DFS
```
