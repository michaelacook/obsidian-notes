# Ping Command Reference Guide (Windows)

A practical reference for the most important `ping` switches on Windows.

---

## Quick Syntax

```
ping [options] <destination>
```

---

## 1. `-n` — Limit the Number of Packets

```bat
:: Send exactly 10 ICMP echo requests, then stop (default is 4)
ping -n 10 google.com

:: Send a single ping — useful in scripts to do a quick alive check
ping -n 1 192.168.1.1
```

---

## 2. `-t` — Ping Forever

```bat
:: Ping continuously until manually stopped with Ctrl+C
:: Press Ctrl+Break to see running stats without stopping
ping -t google.com

:: Useful for monitoring a device during a reboot — you'll see the moment it drops and comes back
ping -t 192.168.1.1
```

---

## 3. `-l` — Set Packet (Payload) Size

```bat
:: Send packets with a 1000-byte payload (default is 32 bytes)
:: Useful for simulating heavier traffic or testing bandwidth behavior
ping -l 1000 google.com

:: Test the maximum standard Ethernet payload size (1472 bytes payload + 28-byte header = 1500 MTU)
ping -l 1472 google.com

:: Send a tiny 1-byte packet — useful for minimal-overhead latency checks
ping -l 1 google.com
```

---

## 4. `-i` — Set TTL (Time To Live)

```bat
:: Set TTL to 10 — the packet will be discarded after crossing 10 routers
:: Useful for detecting routing loops or estimating path depth
ping -i 10 google.com

:: A TTL of 1 means the packet dies at the first hop (your default gateway)
:: Useful for confirming your gateway is reachable
ping -i 1 192.168.1.1
```

---

## 5. `-w` — Timeout Per Reply (Milliseconds)

```bat
:: Wait up to 500ms for each reply before marking it as lost (default is 4000ms)
:: Tighten this up when scanning many hosts quickly
ping -w 500 192.168.1.1

:: Useful combo: 1 packet, 1-second timeout — fast dead-host check for scripts
ping -n 1 -w 1000 192.168.1.254
```

---

## 6. `-f` — Don't Fragment (MTU Discovery)

```bat
:: Send packets with the "Don't Fragment" bit set
:: If the packet is too large for a hop, you'll get a "Packet needs to be fragmented" error
:: Used to find the maximum MTU supported along a network path
ping -f -l 1472 google.com

:: Reduce size until pings succeed to identify the true path MTU
ping -f -l 1400 google.com
```

---

## 7. `-r` — Record Route (Up to 9 Hops)

```bat
:: Record and display the IP addresses of up to 9 hops in the IP header
:: A lightweight alternative to tracert for short paths
ping -r 9 google.com

:: Use a lower hop count for LAN hops where the path is short
ping -r 3 192.168.1.1
```

---

## 8. `-s` — Timestamp Per Hop

```bat
:: Record a timestamp at each of the first N hops (max 4)
:: Useful for rough per-hop latency inspection
ping -s 4 google.com
```

---

## 9. `-4` / `-6` — Force IPv4 or IPv6

```bat
:: Force IPv4 even if the target has an AAAA (IPv6) DNS record
ping -4 google.com

:: Force IPv6 — useful for testing dual-stack connectivity
ping -6 google.com
```

---

## 10. `-a` — Resolve IP to Hostname

```bat
:: Resolve the IP address to a hostname in the output header
:: Handy when pinging an IP and you want to confirm which host it belongs to
ping -a 8.8.8.8

:: Combine with continuous ping to monitor a host by IP but see its name
ping -a -t 8.8.8.8
```

---

## Practical Combos & Real-World Scripts

```bat
:: ── Quick alive check in a batch script ──
:: Exits with code 0 if up, 1 if down
ping -n 1 -w 1000 192.168.1.1 >nul && echo Host is UP || echo Host is DOWN

:: ── Log a continuous ping to a file with timestamps (for outage tracking) ──
ping -t 8.8.8.8 | "%SystemRoot%\System32\cmd.exe" /c "powershell -Command \"$input | ForEach-Object { \"$(Get-Date -Format 'HH:mm:ss') $_\" }\"" >> C:\ping_log.txt

:: ── Test if large packets are making it through unfragmented ──
ping -f -l 1472 -n 5 google.com

:: ── Check both IPv4 and IPv6 connectivity to the same host ──
ping -4 -n 4 google.com
ping -6 -n 4 google.com

:: ── Scan a range of LAN hosts quickly (low timeout, 1 packet each) ──
for /l %i in (1,1,254) do ping -n 1 -w 200 192.168.1.%i | find "Reply"
```

---

## Switch Cheat Sheet

|Switch|Argument|Purpose|
|---|---|---|
|`-n`|Count|Stop after N packets (default: 4)|
|`-t`|_(none)_|Ping forever until Ctrl+C|
|`-l`|Bytes|Payload size (default: 32, max: 65500)|
|`-i`|TTL|Max hops before packet is discarded|
|`-w`|Milliseconds|Per-reply timeout (default: 4000ms)|
|`-f`|_(none)_|Set "Don't Fragment" bit|
|`-r`|1–9|Record route hops in IP header|
|`-s`|1–4|Timestamp at first N hops|
|`-4`|_(none)_|Force IPv4|
|`-6`|_(none)_|Force IPv6|

| `-a` | _(none)_ | Resolve IP to hostname in output |