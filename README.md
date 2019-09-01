# DNS infrastructure

DNS infrastructure is very simple - just a single authoritative DNS server without an recursor (since we only care only the zones on this net, without needing to access any other zones)

The DNS server used is [Knot DNS](https://www.knot-dns.cz/) running in Docker container with persistant volumes.

<https://hub.docker.com/r/cznic/knot>

## Setup

1. Import the config
   `docker run -it --rm -v $(pwd)/config:/config -v $(pwd)/storage:/storage -v $(pwd)/rundir:/rundir cznic/knot knotc conf-import /config/knot.conf`

2. Run the server
   `docker run -d --rm -p 53:53 -p 53:53/udp -v $(pwd)/config:/config -v $(pwd)/storage:/storage -v $(pwd)/rundir:/rundir cznic/knot knotd -v`

3. Set DHCP DNS server to `192.168.0.2`

4. To manage the server, run
   `docker run -it --rm -v $(pwd)/config:/config -v $(pwd)/storage:/storage -v $(pwd)/rundir:/rundir cznic/knot knotc`

Test by running `dig @127.0.0.1 test.cb TXT`. To run on local machine, you will probably have to switch the port (e.g. 5300), and then use `dig @127.0.0.1 -p5300`
