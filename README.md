# SyncMesh — Distributed Messaging Sandbox

SyncMesh is a distributed messaging system that blends Spring Boot, ZooKeeper coordination, Lamport clocks, and vector clocks to demonstrate how modern data platforms keep messages consistent across multiple nodes. It ships with a React-free (vanilla JS) operations dashboard, an opt-in CLI client, and a set of admin endpoints you can use to simulate partitions, observe leader election, and replay replication logs.

## Documentation

Detailed documentation has been separated into the `docs/` directory for easier reading:

- **[Getting Started & Quick Start](docs/getting-started.md)** – Prerequisites, multi-node setups, and CLI testing.
- **[System Architecture](docs/architecture.md)** – Overview of the control plane and data plane.
- **[API Reference](docs/api-reference.md)** – Admin and diagnostic endpoints to manage nodes.
- **[Operational Dashboard](docs/dashboard.md)** – Details on real-time visualization and troubleshooting.
- **[Development & Roadmap](docs/development.md)** – Information on running tests and future project ideas.

---

## Key Features
- **ZooKeeper-backed coordination** – Each server registers as an ephemeral znode under `/dms-system/servers`, emits heartbeat znodes, and participates in a simple leader election using sequential znodes.
- **Hybrid clocks** – Messages capture both Lamport logical clocks and a per-node vector clock map so replicas can reason about causality when conflicts arise.
- **Dynamic replication** – `ReplicationService` keeps an in-memory list of peer HTTP endpoints, pushes unicast or broadcast copies, and replays a short log to newcomers for quick catch-up.
- **Partition simulation** – Toggle partition mode via REST to see how the cluster behaves when quorum cannot be met (warnings instead of hard failures).
- **Operations dashboard** – A lightweight static UI (no build step) shows membership, heartbeats, leader state, replica targets, and message flow with SENT/RECEIVED highlighting.
- **CLI discovery client** – A Spring Boot CommandLineRunner samples ZooKeeper, discovers a live server, and fires a test message as a sanity check.
- **Polyglot-friendly contract** – Shared `common` module exposes `Message`, `NodeInfo`, and `Config` so new clients/services can align on payload format without duplicating code.

---

## Repository Layout
```text
SyncMesh/
├── common/   # Shared domain objects and config constants
├── server/   # Spring Boot node that stores, replicates, and visualizes messages
├── client/   # CLI/automation client that discovers a node via ZooKeeper
├── tests/    # Placeholder module to host integration tests
├── docs/     # Extensive project documentation
├── pom.xml   # Maven parent + dependency management
└── README.md
```

---

Happy hacking! If you build on top of SyncMesh, feel free to add your own module, mention the project in your portfolio, or extend the README with your learnings.

