# Getting Started

## Prerequisites
- Java 17+
- Maven 3.9+
- Apache ZooKeeper 3.9.x (standalone instance is enough; default connect string `localhost:2181`)
- Modern browser for the dashboard (no bundler/build step required)

## Quick Start
1. **Run ZooKeeper**
   ```powershell
   # Windows example (adjust path to your ZooKeeper install)
   .\bin\zkServer.cmd
   ```
2. **Build every module**
   ```powershell
   .\mvnw.cmd -DskipTests install
   ```
3. **Start your first server node**
   ```powershell
   cd server
   java -jar target\server-0.0.1-SNAPSHOT.jar --server.port=8081 --node.id=server-8081
   ```
4. **Open the dashboard** – navigate to `http://localhost:8081/` and you should see node metadata, leader info, and the send-message form.

That's enough to observe a single-node deployment. Continue with the next section to simulate a cluster.

## Running Multiple Nodes
Run each node in its own terminal so logs remain readable:
```powershell
# Node 1
java -jar target\server-0.0.1-SNAPSHOT.jar --server.port=8081 --node.id=server-8081

# Node 2
java -jar target\server-0.0.1-SNAPSHOT.jar --server.port=8082 --node.id=server-8082

# Node 3 (optional)
java -jar target\server-0.0.1-SNAPSHOT.jar --server.port=8083 --node.id=server-8083
```
Every node automatically:
- Registers itself in ZooKeeper (so the dashboard immediately shows it).
- Rehydrates the replica list and replays recent messages to newcomers.
- Receives broadcasts (`receiver=BROADCAST`) and unicast messages where it is the target.

Use the dashboard on any node to send inter-node messages, broadcast announcements, or inspect health. Because the static assets are served locally, `http://localhost:PORT/` always shows the perspective of that node (helpful for testing replica filtering logic).

## CLI Smoke Test
The client module is a minimalist way to verify discovery + send logic without the browser.
```powershell
cd client
java -jar target\client-0.0.1-SNAPSHOT.jar --spring.main.web-application-type=none
```
The `CliRunner` will:
1. Connect to ZooKeeper using `Config.ZK_CONNECT`.
2. Grab the first available server from `/dms-system/servers`.
3. POST a sample message (`"hello from client"`) via `MessageSender`.

Tail server logs to confirm the inbound message, Lamport bump, and replication fan-out.
