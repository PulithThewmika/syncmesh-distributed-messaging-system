# System Architecture

**Control plane**  
`ZooKeeperConnector` bootstraps every node by:
- Ensuring the `/dms-system` root, `/servers`, `/heartbeats`, and `/election` znodes exist.
- Publishing its own `NodeInfo` under `/servers/<nodeId>` as an ephemeral node (so crashed nodes automatically disappear).
- Updating a heartbeat znode every 2s (consumed by `/admin/heartbeats` and the UI).
- Participating in leader election via `/election/node-XXXX` sequential znodes. The node owning the smallest sequence becomes leader.

**Data plane**  
```
REST request → MessageController → MessageService
   ↳ validates receiver membership via ZooKeeper
   ↳ bumps Lamport clock + vector clock
   ↳ persists message in an in-memory repository with last-writer-wins semantics
   ↳ calls ReplicationService.replicate(...) or .broadcast(...)
```
Replication uses simple HTTP fan-out with quorum tracking for unicast writes and best-effort fan-out for broadcasts. Replication targets are refreshed via ZooKeeper watches plus a 10s polling safety net.

**Observability**  
The dashboard (`server/src/main/resources/static/`) consumes `/admin/**` endpoints to visualize cluster health, let you trigger elections, toggle partition mode, and inspect message flow from the perspective of the node hosting the UI.
