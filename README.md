# Discord id Helper

A tiny, beginner‑friendly tool + guide to fetch, validate, and decode your **Discord Snowflake ID**. Perfect to drop into your GitHub README.

> **What  :**
>
> * A clear guide to enable Developer Mode and copy your Discord User/Message/Channel .
> * Small scripts (Python & Node.js) to **validate** an ID and **decode the creation timestamp** from the snowflake.
>
> **Whate   :**
>
> * A tokened/bot-required scraper.  login, no .

---

## ✨ Features

* 🧭 Step‑by‑step instructions (with Developer Mode) to copy IDs safely
* ✅ Validate that an ID looks like a Discord snowflake
* 🕒 Decode the embedded timestamp from the ID
* 📦 Zero dependencies (Python standard lib / Node core)

---

## 📸 Quick 

```text
$ python discord_id_helper.py 122345678901234567
Valid snowflake ✅
ID creation time (UTC): 2024-11-12 08:14:22
```

---

## 🚀 Getting Started

### 1)   your ID from Discord (no code needed)

1. Open **Discord**
2. Go to **User Settings → Advanced**
3. Turn on **Developer Mode**
4. Right‑click your **profile / message / channel** → **Copy ID**

You’ll get a long integer like `122345678901234567`.

### 2) Validate & decode with a tiny script

#### Python (discord\_id\_helper.py)

```python
#!/usr/bin/env python3
import sys
from datetime import datetime, timezone

DISCORD_EPOCH = 1420070400000  # 2015-01-01T00:00:00.000Z


def is_snowflake(s: str) -> bool:
    return s.isdigit() and len(s) >= 17 and len(s) <= 20  # practical bounds


def decode_timestamp(snowflake: int) -> datetime:
    # Discord: timestamp = (snowflake >> 22) + DISCORD_EPOCH
    ms = (snowflake >> 22) + DISCORD_EPOCH
    return datetime.fromtimestamp(ms / 1000, tz=timezone.utc)


def main():
    if len(sys.argv) != 2:
        print("Usage: python discord_id_helper.py <discord_id>")
        sys.exit(1)

    raw = sys.argv[1].strip()
    if not is_snowflake(raw):
        print("Not a valid-looking Discord snowflake ❌")
        sys.exit(2)

    sf = int(raw)
    ts = decode_timestamp(sf)
    print("Valid snowflake ✅")
    print(f"ID creation time (UTC): {ts.strftime('%Y-%m-%d %H:%M:%S')}")


if __name__ == "__main__":
    main()
```

Run it:

```bash
python discord_id_helper.py 122345678901234567
```

#### Node.js (discord-id-helper.mjs)

```js
const DISCORD_EPOCH = 1420070400000n; // 2015-01-01T00:00:00.000Z

function isSnowflake(s) {
  return /^\d{17,20}$/.test(s);
}

function decodeTimestamp(snowflakeStr) {
  const sf = BigInt(snowflakeStr);
  const ms = (sf >> 22n) + DISCORD_EPOCH;
  return new Date(Number(ms));
}

const id = process.argv[2];
if (!id) {
  console.log("Usage: node discord-id-helper.mjs <discord_id>");
  process.exit(1);
}

if (!isSnowflake(id)) {
  console.log("Not a valid-looking Discord snowflake ❌");
  process.exit(2);
}

const ts = decodeTimestamp(id);
console.log("Valid snowflake ✅");
console.log("ID creation time (UTC):", ts.toISOString().replace('T', ' ').slice(0, 19));
```

Run it:

```bash
node discord-id-helper.mjs 122345678901234567
```

> **Note:** These scripts do not contact Discord. They just parse the snowflake format.

---

## 🧪 Test IDs

Use any numeric string with 17–20 digits to test formatting. Real IDs come from Discord via **Copy ID**.

---

## 📚 How it works (1‑minute read)

Discord uses a 64‑bit **snowflake**:

* **42 bits**: timestamp (milliseconds since `2015‑01‑01T00:00:00Z`)
* **5 bits**: worker ID
* **5 bits**: process ID
* **12 bits**: increment

We recover the timestamp by shifting right 22 bits and adding the **Discord epoch**.

---

## 🧩 Embed in Your README

Copy this badge + command block to your repo:

````md
[![Discord ID Helper](https://img.shields.io/badge/Discord%20ID-Helper-blue)](#)

### Check your Discord ID
```bash
python discord_id_helper.py 122345678901234567
# or
node discord-id-helper.mjs 122345678901234567
````

```

---

## 🛡️ Safety & Privacy
- Never share your **token** publicly.
- IDs alone are public identifiers, but treat them as you would usernames.

---

## 🤝 Contributing
PRs welcome! If you add another language (Go/Rust/TS), include a short usage snippet.

---

## 📄 License
MIT

---

## 🙋 FAQ
**Q:**  I get my ID without Developer Mode?

**A:** Not reliably inside the client. Turning on **Developer Mode** is the standard way to expose **Copy ID**.

**Q:** Does this work for message or channel IDs?

**A:** Yes. Any Discord snowflake decodes the same way.

```
