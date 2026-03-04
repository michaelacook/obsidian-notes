# Windows `tracert` Reference Guide

`tracert` (Trace Route) is a Windows command-line diagnostic tool that maps the path packets take to reach a destination, showing each hop, its IP/hostname, and round-trip latency.

---

## Basic Syntax

```cmd
tracert [switches] <target_host>
```

---

## Switch Reference

---

### `-d` — Skip DNS Resolution

Skips resolving IP addresses to hostnames. Makes the trace significantly **faster** since no DNS lookups are performed.

```cmd
:: Standard trace — resolves each hop's IP to a hostname (slower)
tracert google.com

:: Fast trace — displays raw IPs only, no DNS lookups
:: Use this when DNS is slow, broken, or you only care about IPs
tracert -d google.com
```

**When to use:** Troubleshooting slow traces, diagnosing DNS issues, or when you just need raw routing data quickly.

---

### `-h <max_hops>` — Set Maximum Hop Count

Controls how many hops (routers) tracert will traverse before giving up. Default is **30 hops**.

```cmd
:: Default behaviour — traces up to 30 hops
tracert google.com

:: Limit to 10 hops — useful for mapping only the local/ISP portion of a route
:: Stops early if the destination hasn't been reached by hop 10
tracert -h 10 google.com

:: Increase to 50 hops for destinations that are unusually far away
tracert -h 50 some-distant-server.net
```

**When to use:** Narrowing your trace to just your LAN/ISP hops, or extending the limit for long international routes.

---

### `-w <timeout>` — Set Timeout per Reply (milliseconds)

Sets how long tracert waits for a response from each hop before marking it as timed out (`* * *`). Default is **4000ms (4 seconds)**.

```cmd
:: Default — waits 4 seconds per hop response
tracert google.com

:: Aggressive timeout — only waits 500ms per hop
:: Great for fast local network traces where latency is low
tracert -w 500 192.168.1.1

:: Extended timeout — waits 8 seconds per hop
:: Use for high-latency links like satellite or intercontinental routes
:: Prevents false timeouts on slow but reachable hops
tracert -w 8000 server-in-australia.example.com
```

**When to use:** Reducing wait time on fast internal networks, or avoiding false `* * *` timeouts on high-latency paths.

---

### `-4` — Force IPv4

Forces tracert to use **IPv4**, even if the target has an IPv6 address (AAAA record).

```cmd
:: May use IPv6 if the host supports it and your system prefers it
tracert google.com

:: Force IPv4 — always sends ICMPv4 packets regardless of DNS result
:: Essential when comparing IPv4 vs IPv6 routing paths
tracert -4 google.com

:: Combine with -d to get a fast IPv4-only trace with no DNS lookups
tracert -4 -d google.com
```

**When to use:** Comparing IPv4 vs IPv6 paths, or when your IPv6 path has issues and you want to isolate IPv4 routing.

---

### `-6` — Force IPv6

Forces tracert to use **IPv6** (ICMPv6), even if the host resolves to an IPv4 address first.

```cmd
:: Force IPv6 — sends ICMPv6 packets only
:: The target must have an AAAA (IPv6) DNS record for this to work
tracert -6 google.com

:: Combine with -d to skip reverse DNS lookups on IPv6 addresses
:: IPv6 reverse DNS is often missing, so -d keeps output clean
tracert -6 -d google.com
```

**When to use:** Verifying IPv6 connectivity end-to-end, auditing IPv6 routing paths, or diagnosing IPv6-specific failures.

---

## Combining Switches — Practical Examples

Switches can be stacked for more targeted diagnostics.

```cmd
:: Fast, clean trace to a target — no DNS, 10 hop limit, 1s timeout
:: Great as a quick "is routing sane?" sanity check
tracert -d -h 10 -w 1000 8.8.8.8

:: Thorough trace for a flaky international connection
:: Extended hops and generous timeout to catch all slow/distant hops
tracert -h 50 -w 6000 remote-server.example.com

:: Full IPv4-only trace with no DNS, standard hop limit
:: Useful when troubleshooting dual-stack environments
tracert -4 -d -w 2000 cloudflare.com

:: Trace only to your default gateway (first hop)
:: Confirms your local router is reachable and measures its latency
tracert -h 1 -d 8.8.8.8
```

---

## Reading the Output

```
Tracing route to google.com [142.250.80.46] over a maximum of 30 hops:

  1    <1 ms    <1 ms    <1 ms  192.168.1.1        <- Your default gateway (home router)
  2     8 ms     7 ms     8 ms  10.0.0.1            <- ISP first hop
  3    12 ms    11 ms    12 ms  72.14.215.165        <- ISP backbone
  4     *        *        *     Request timed out.   <- Hop blocks ICMP (normal)
  5    18 ms    17 ms    19 ms  142.250.80.46        <- Destination reached
```

|Symbol|Meaning|
|---|---|
|`<1 ms`|Sub-millisecond latency — very fast hop (usually LAN)|
|`* * *`|Hop didn't respond — firewall blocks ICMP TTL-exceeded, **not necessarily broken**|
|`Request timed out`|Same as `* * *` — ICMP responses suppressed at that hop|
|Increasing ms|Normal — latency accumulates with distance|
|Sudden ms spike|Possible congestion, rate-limiting, or a slow link at that hop|

> **Tip:** A hop showing `* * *` mid-route is almost always a router configured to silently drop ICMP. If the **next** hop responds, routing is fine — the `* * *` hop is just quiet.

---

## Quick-Reference Table

|Switch|Purpose|Default|
|---|---|---|
|`-d`|Skip DNS hostname resolution|DNS on|
|`-h <max_hops>`|Max hops before stopping|30|
|`-w <ms>`|Timeout per hop in milliseconds|4000 ms|
|`-4`|Force IPv4|Auto|
|`-6`|Force IPv6|Auto|