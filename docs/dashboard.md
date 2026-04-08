# Operational Dashboard

- **Cluster Nodes panel** – lists known servers, highlights the current node, and populates sender/receiver dropdowns.
- **Health & Topology** – exposes quick buttons to refresh heartbeats, replicas, leader info, and to manually trigger leader elections or partition mode.
- **Send Message** – builds JSON payloads for `/api/messages/send`. You can target a specific node or broadcast to all.
- **Messages panel** – filters history relative to the current node (SENT, RECEIVED, or both) and shows Lamport clock + origin node metadata for debugging races.

The UI lives in `server/src/main/resources/static` so feel free to customize copy, colors, or add charts without touching a React/Vite toolchain.
