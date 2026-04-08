# Admin & Diagnostics APIs

| Endpoint | Purpose |
| --- | --- |
| `GET /admin/nodes` | List `NodeInfo` objects discovered via ZooKeeper. |
| `GET /admin/messages` | Dump in-memory message store for observability. |
| `GET /admin/heartbeats` | Inspect heartbeat timestamps per node. |
| `GET /admin/leader` | Show the node id recognized as leader. |
| `GET /admin/replicas` | Return the HTTP replica list maintained by `ReplicationService`. |
| `GET /admin/refresh-replicas` | Force-refresh replica list from ZooKeeper. |
| `GET /admin/partition/enable\|disable` | Toggle partition mode (skips quorum enforcement). |
| `GET /admin/trigger-election` | Manually prompt a leader re-evaluation. |
| `GET /admin/test/unicast?target=<nodeId>` | Fire a diagnostic message directly at a node. |

All endpoints return JSON or plain text and are safe to call via the provided UI buttons or your own tooling (curl/Postman).
