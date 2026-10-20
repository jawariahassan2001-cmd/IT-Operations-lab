# DNS Resolution Failure Incident

## Overview

I simulated a DNS failure on my Ubuntu Server 24.04.5 LTS virtual machine to practise network troubleshooting.

The server could reach IP addresses, but it could not resolve domain names. I used connectivity tests and DNS queries to identify the cause, restore name resolution, and verify recovery.

**Environment:** Ubuntu Server 24.04.5 LTS, running in VirtualBox.

**Incident type:** Simulated lab incident.

## 1. Symptoms

- `ping google.com` failed with `Temporary failure in name resolution`.
- `curl -I https://example.com` failed with `Could not resolve host`.
- The server could still reach the router and a public IP address.

## 2. Investigation and Evidence

### Step 1: Check local network connectivity

I tested the router:

`ping 192.168.1.1`

The router responded successfully.

I then tested a public IP address:

`ping 8.8.8.8`

This also succeeded.

**Finding:** The local network and IP connectivity were working. The evidence pointed towards a name-resolution problem rather than a complete network outage.

### Step 2: Test domain-name resolution

I ran:

`ping google.com`

The command failed because the hostname could not be resolved.

I also ran:

`dig google.com`

The DNS query timed out through the local resolver at `127.0.0.53`.

**Finding:** DNS requests were not receiving a usable response through the configured resolver.

### Step 3: Test an alternative DNS server

I ran:

`dig @8.8.8.8 google.com +short`

The query returned an IP address.

**Finding:** An alternative DNS server could resolve the domain, providing further evidence that the configured DNS path was the problem.

### Step 4: Check the configured DNS server

I ran:

`resolvectl status`

The output showed that the interface `enp0s3` was configured to use `192.0.2.1`.

This was the incorrect DNS address I had deliberately configured during the exercise.

## 3. Root Cause

The DNS server configured on the Ubuntu network interface was incorrect.

I had set the interface's DNS server to `192.0.2.1`, which did not provide a response to the DNS queries.

IP connectivity remained available, but applications could not resolve domain names.

## 4. Corrective Actions

I reverted the temporary DNS configuration:

`sudo resolvectl revert enp0s3`

Then I flushed the resolver cache:

`sudo resolvectl flush-caches`

## 5. Verification

After reverting the configuration, I checked the DNS status again.

The configured DNS servers returned to the router's addresses, including `192.168.1.1`.

I then repeated the name-resolution tests. `ping` and `dig` successfully resolved the domain again.

The issue was resolved at approximately 20:31:46 UTC on 7 October 2026.

## 6. Prevention and Monitoring

- Verify DNS settings when investigating name-resolution failures.
- Test both IP connectivity and hostname resolution.
- Consider monitoring DNS resolution separately from basic network connectivity.
- Document the expected DNS configuration for the host.
- Consider a secondary DNS resolver where the network design supports it.

## 7. Lessons Learned

- If a public IP is reachable but a hostname cannot be resolved, investigate DNS.
- `resolvectl status` helps identify the DNS servers configured on a Linux system.
- `dig @server hostname` tests a specific DNS server.
- A successful ping to an IP address does not prove that DNS is working.
- After making a fix, repeat the original failing test to verify recovery.

## Scope

This was a deliberately simulated incident in my personal IT Operations learning lab. It was not a production incident.
