## Agent

* It is just a single daemon running in Kubernetes.

* But it can be made HA by forming a raft between 3 servers.

  * Rqlite is already doing this. If SQLite is chosen, then RQlite is a given.


## Central Server

* The server itself is stateless.

* The websocket connections are distributed via Nats. It uses Nats as a broker.

* Nats is the only dependency and it is already HA.
