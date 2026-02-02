It sounds like your Cisco router is **double-applying the daylight saving time (DST) offset**, which is a common issue when configuring the **`clock timezone`** and **`clock summer-time`** commands together.

### The Problem:
- When you set **`clock timezone EST -4`**, you're telling the router that the base timezone is **UTC -4**.
- Then, when you configure **`clock summer-time`**, it adds **another hour** on top of this, making it **UTC -3 instead of UTC -4**.

### Correct Configuration for Ontario:
In Ontario, the standard time is **Eastern Standard Time (UTC -5)**, and daylight saving time moves it to **Eastern Daylight Time (UTC -4)**.

#### ✅ **Correct Cisco IOS Configuration**
```
clock timezone EST -5
clock summer-time EDT recurring 2 Sun Mar 02:00 1 Sun Nov 02:00
```
### Explanation:
- `clock timezone EST -5` → Sets standard time to **UTC -5** (Eastern Standard Time).
- `clock summer-time EDT recurring 2 Sun Mar 02:00 1 Sun Nov 02:00`  
  - **Second Sunday in March at 2:00 AM**, the clock moves **forward** by 1 hour (UTC -4).
  - **First Sunday in November at 2:00 AM**, the clock moves **back** by 1 hour (UTC -5).

### Why Your Setup Was Wrong:
- If you set `clock timezone EST -4`, it assumes that **standard time** (winter) is UTC -4, which is incorrect.
- When `clock summer-time` adds another hour, you end up at **UTC -3**, which is **one hour ahead** of the correct time.

This should fix your issue! Let me know if you need further clarification. 🚀⏰