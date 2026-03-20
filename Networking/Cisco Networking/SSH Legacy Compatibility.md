# SSH Legacy Compatibility

## What Happened

When you SSH into a device, the client and server negotiate a set of cryptographic algorithms before any login occurs. Think of it like two routers negotiating which routing protocol to use — if there's no match, the session can't form.

Modern OpenSSH clients (shipped with Windows Server 2022) have dropped support for old, weak algorithms. IOSv defaults to those old algorithms. This caused two separate failures:

1. **The RSA key was too short** — the client refused to even try
2. **The algorithms didn't overlap** — even if key length was fine, the handshake would have failed

---

## Terminology

**RSA Key / Modulus Size** RSA is a public-key algorithm used to prove the switch's identity to the client. The "modulus" is the size of the math problem underpinning the key — measured in bits. Bigger = harder to break. 768-bit keys are considered cryptographically broken. 2048-bit is the current safe minimum. The fix was `crypto key generate rsa modulus 2048`.

**Key Exchange (KEX) Algorithm** After the identity check, the client and server need to agree on a _session key_ — a temporary symmetric key used to encrypt the actual traffic. KEX is the algorithm used to safely negotiate that key over an untrusted network without ever transmitting it directly (Diffie-Hellman is the classic method). If the client and server have no KEX algorithms in common, the session fails.

**Host Key Algorithm** The format used to _sign_ the server's identity during the handshake. `ssh-rsa` is the legacy format. Modern OpenSSH prefers `rsa-sha2-256` or `rsa-sha2-512` (same RSA key, stronger signature hash). If the client won't accept the format the server is offering, connection fails.

**MAC Algorithm (Message Authentication Code)** A checksum applied to each SSH packet to detect tampering in transit. Think of it like a per-packet integrity check. `hmac-sha1` is legacy; `hmac-sha2-256` is preferred.

---

## The Two Fixes

### 1. Regenerate the RSA Key (on the switch)

```
crypto key zeroize rsa
crypto key generate rsa modulus 2048
```

This replaces the broken 768-bit key with a 2048-bit key that modern clients will accept.

---

### 2. SSH Client Config File (on Windows)

Location: `C:\Users\<you>\.ssh\config`

```
Host 192.168.99.2
    KexAlgorithms +diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
    HostKeyAlgorithms +ssh-rsa
    PubkeyAcceptedKeyTypes +ssh-rsa
```

**What each line does:**

- `Host` — applies these settings only to this IP, so you're not weakening SSH for everything else
- `KexAlgorithms +...` — adds legacy DH key exchange methods back to the client's offer list
- `HostKeyAlgorithms +ssh-rsa` — tells the client to accept the switch's legacy `ssh-rsa` signature format
- `PubkeyAcceptedKeyTypes +ssh-rsa` — allows `ssh-rsa` keys during the authentication phase (not just handshake)
- The `+` prefix _appends_ to the default list rather than replacing it — modern algorithms remain available for other hosts

---

## Syslog Messages

These are the common Cisco IOS SSH syslog messages you'll encounter with this class of problem:

|Message|What it means|
|---|---|
|`%SSH-4-SSH2_UNEXPECTED_MSG`|Client and server reached a point in the handshake where the message type didn't match expectations — usually a KEX or host key algorithm mismatch. The negotiation got far enough to start but broke down mid-handshake.|
|`%SSH-3-DH_RANGE_FAIL`|The client requested a Diffie-Hellman key size outside the range the switch supports. Seen when modern clients request >4096-bit DH and the switch can't accommodate it.|
|`%SSH-5-SSH2_SESSION` with `Failed`|The session failed after a partial negotiation. Often paired with `SSH2_UNEXPECTED_MSG`. Shows the cipher and MAC that were tentatively selected before failure.|
|`%SSH-5-SSH2_CLOSE`|The session was closed — appears after any failure or normal disconnect. By itself it's informational, but the cipher/MAC fields can help confirm what was (or wasn't) negotiated.|

**On the client side (Windows PowerShell `ssh -vvv`)**, the equivalent messages look like:

- `Unable to negotiate ... no matching key exchange method found` — KEX mismatch
- `no matching host key type found. Their offer: ssh-rsa` — host key algorithm rejected
- `Invalid key length` — RSA modulus too small (what you hit first)

The IOS syslog message you received (`SSH2_UNEXPECTED_MSG`) is the switch's way of reporting that the client sent a message type it didn't expect — which happens when the two sides think they've agreed on something but actually haven't, causing the handshake state machine to go out of sync.

---

## Why the Switch Defaults Are So Old

IOS was designed long before these algorithms were deprecated. Older IOS versions simply don't support the modern alternatives. The right long-term fix is to explicitly configure stronger algorithms on the switch (`ip ssh server algorithm kex/mac/hostkey`) where the IOS version supports it — but for IOSv in a lab, the client config workaround is practical.