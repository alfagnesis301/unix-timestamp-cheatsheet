# Unix Timestamp Cheatsheet

Quick reference for converting Unix/epoch timestamps in 10 languages. No dependencies, copy-paste ready.

> 🔧 Need a quick visual check? Use the free [Unix Timestamp Converter](https://chrono-time.site/unix-timestamp) — auto-detects seconds vs milliseconds, shows UTC + local time.

## The one rule that prevents 90% of bugs

Count the digits:

| Digits | Unit | Example | Typical source |
|---|---|---|---|
| 10 | **seconds** | `1782950400` | Unix, PHP, Python, MySQL, most APIs |
| 13 | **milliseconds** | `1782950400000` | JavaScript `Date.now()`, Java, MongoDB |
| 16 | microseconds | `1782950400000000` | high-precision logs |
| 19 | nanoseconds | `1782950400000000000` | Go `UnixNano()` |

Converted a 13-digit value as seconds? You get year ~58,000. A 10-digit value as ms? January 1970. Both mean: divide or multiply by 1,000.

## Get current timestamp (seconds)

```javascript
// JavaScript — Date.now() is MILLISECONDS
Math.floor(Date.now() / 1000)
```

```python
# Python
import time
int(time.time())
```

```php
// PHP
time();
```

```sql
-- MySQL
SELECT UNIX_TIMESTAMP();
-- PostgreSQL
SELECT EXTRACT(EPOCH FROM NOW())::int;
```

```java
// Java
Instant.now().getEpochSecond();
```

```go
// Go
time.Now().Unix()
```

```csharp
// C#
DateTimeOffset.UtcNow.ToUnixTimeSeconds();
```

```ruby
# Ruby
Time.now.to_i
```

```bash
# Bash / Linux
date +%s
```

```swift
// Swift
Int(Date().timeIntervalSince1970)
```

## Timestamp → date

```javascript
new Date(1782950400 * 1000).toISOString()   // multiply: JS wants ms
```

```python
from datetime import datetime, timezone
datetime.fromtimestamp(1782950400, tz=timezone.utc)   # ALWAYS pass tz=
```

```sql
-- MySQL
SELECT FROM_UNIXTIME(1782950400);
-- PostgreSQL
SELECT to_timestamp(1782950400);
```

```php
gmdate('Y-m-d H:i:s', 1782950400);   // UTC (date() uses server tz)
```

## Date → timestamp

```javascript
Math.floor(new Date('2026-07-01T00:00:00Z').getTime() / 1000)
```

```python
from datetime import datetime, timezone
int(datetime(2026, 7, 1, tzinfo=timezone.utc).timestamp())
```

```sql
SELECT UNIX_TIMESTAMP('2026-07-01 00:00:00');   -- MySQL
```

## Useful constants

| Period | Seconds |
|---|---|
| 1 hour | 3,600 |
| 1 day | 86,400 |
| 1 week | 604,800 |
| 30 days | 2,592,000 |
| 365 days | 31,536,000 |

## Reference values

| Timestamp | Date (UTC) | Note |
|---|---|---|
| `0` | 1970-01-01 00:00:00 | the Unix epoch |
| `1000000000` | 2001-09-09 | 1 billion |
| `2000000000` | 2033-05-18 | 2 billion |
| `2147483647` | 2038-01-19 03:14:07 | **32-bit overflow (Y2038)** |

## Gotchas

- Unix time is **always UTC**. Time zones only exist at display time.
- It does **not** count leap seconds — every day is exactly 86,400 s.
- Negative timestamps are valid (dates before 1970).
- Python's `fromtimestamp()` without `tz=` silently uses server-local time.
- Storing dates past Jan 2038 in 32-bit ints will overflow — audit legacy systems.

---

MIT — contributions welcome. Built as a companion to [chrono-time.site](https://chrono-time.site/). More free browser-side tools by the same maker: [TextPulses](https://textpulses.com/) (word counter, SEO title & meta checkers, readability).
