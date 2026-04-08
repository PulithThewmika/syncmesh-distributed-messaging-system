# Development

## Testing
`tests` currently contains a placeholder integration suite (`IntegrationTests.java`). Recommended next steps:
- Add multi-node integration tests using `curator-test` to spin up an in-memory ZooKeeper and drive multiple `ServerApplication` instances.
- Cover conflict resolution by asserting Lamport/vector precedence when two nodes race to update the same message id.

To execute the existing (placeholder) suite:
```powershell
mvn test -pl tests
```

## Roadmap Ideas
- Persist messages to an embedded DB (PostgreSQL, MongoDB, or RocksDB) so replicas survive restarts.
- Replace simple HTTP replication with gRPC streaming or Kafka-based change-log replication.
- Extend the CLI into a proper SDK with support for custom payload schemas and retries.
- Add Grafana/Loki exporters for heartbeats, replication latency, and partition-mode alerts.
- Strengthen tests with chaos scenarios (random node churn, forced ZooKeeper disconnects).
