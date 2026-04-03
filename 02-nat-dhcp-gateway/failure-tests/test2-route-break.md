# Test 2 — Default Route Failure

## Objective

Validate behavior when the default route is removed from the gateway router.

---

## Break Configuration

**Device: R1**

```bash
no ip route 0.0.0.0 0.0.0.0 203.0.113.2
```

---

## Expected Behavior

* LAN communication should work
* Internet access should fail

---

## Observed Results

### Before

* `ping 8.8.8.8` → Success

### After

* `ping 8.8.8.8` → Failed (destination unreachable)
* `ping 192.168.1.1` → Success
* `ping 203.0.113.2` → Success

---

## Routing Verification

```bash
show ip route
```

* Output shows:

```text
Gateway of last resort is not set
```

---

## Root Cause

The default route was removed, so the router had no path to forward traffic destined for external networks. Even though NAT and connectivity were intact, packets could not be routed beyond the gateway.

---

## Fix

```bash
ip route 0.0.0.0 0.0.0.0 203.0.113.2
```

---

## Result After Fix

* Internet access restored
* Default route reappears in routing table

---

## Screenshots

* `screenshots/test2/before.png`
* `screenshots/test2/after.png`
* `screenshots/test2/route-missing.png`
