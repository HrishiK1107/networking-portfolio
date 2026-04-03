# Test 3 — ACL Blocking NAT

## Objective

Demonstrate how an ACL can silently break NAT translation.

---

## Precondition

**Device: R2**

```bash
no ip route 192.168.1.0 255.255.255.0 203.0.113.1
```

(This ensures NAT is required for return traffic)

---

## Break Configuration

**Device: R1**

```bash
no access-list 1
access-list 1 deny 192.168.1.0 0.0.0.255
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

* `ping 8.8.8.8` → Failed
* `ping 192.168.1.1` → Success
* `ping 203.0.113.2` → Failed

---

## NAT Verification

```bash
show ip nat translations
```

* Output: **Empty**

---

## Root Cause

The ACL used for NAT denied traffic from the LAN network. Although NAT configuration was present, no traffic matched the ACL, preventing translation. As a result, packets were sent with private IP addresses and dropped by the upstream router.

---

## Fix

**R1**

```bash
no access-list 1
access-list 1 permit 192.168.1.0 0.0.0.255
```

**R2**

```bash
ip route 192.168.1.0 255.255.255.0 203.0.113.1
```

---

## Result After Fix

* NAT translations restored
* Internet access working

---

## Screenshots

* `screenshots/test3/before.png`
* `screenshots/test3/after.png`
* `screenshots/test3/nat-empty.png`
