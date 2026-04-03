# Test 4 — WAN Link Failure

## Objective

Validate behavior when the WAN interface is physically down.

---

## Break Configuration

**Device: R1**

```bash
interface s0/0/0
shutdown
```

---

## Expected Behavior

* LAN communication should work
* WAN and Internet should fail

---

## Observed Results

### Before

* `ping 8.8.8.8` → Success

### After

* `ping 8.8.8.8` → Failed
* `ping 192.168.1.1` → Success
* `ping 203.0.113.2` → Failed

---

## Interface Verification

```bash
show ip interface brief
```

* Output:

```text
Serial0/0/0 → administratively down/down
```

---

## Root Cause

The WAN interface was manually shut down, breaking physical connectivity between the gateway router and the ISP router. Without this link, no traffic could leave the local network.

---

## Fix

```bash
interface s0/0/0
no shutdown
```

---

## Result After Fix

* Interface restored (up/up)
* Internet access restored

---

## Screenshots

* `screenshots/test4/before.png`
* `screenshots/test4/interface-down.png`
* `screenshots/test4/after.png`
