## Input Receiver

### Protocol

* TCP? Yes. The communication will most likely happened on localhost anyway.

* UDP? Nah. If we use UDP then we have to worry about datagram size.

* HTTP? Then the sysadmin needs to use curl.

  * It is not too much work to have a POST+JSON endpoint. We can support this feature.

## SQL Send and Receive

### Protocol

Secure Websocket(wss://). Why? We can easily secure the agent's connection on boot.

1. Client sends HTTP to Central Server.

2. Central Server generate a queryID(if not exists) and process the SQL a bit.

3. Central Server broadcast scatter requests to all relevant agents via the websocket connection.

4. Most of the filtering happens on the agent.

5. Central Server gathers the results and filter it further.

6. Return results to the client as HTTP response.

## Frontend facing

HTTP is preferable because it is stateless and proxy friendly.

We may have to craft a special endpoint to play well with Grafana.

