---
{"dg-publish":true,"permalink":"/kdb/q-notes/"}
---


kdb+ is

- a high-performance cross-platform historical time series columnar database
- an in-memory compute engine
- a realtime streaming processor
- an expressive query and programming language called q


> [!NOTE] Columnar Database
> Column-store(OLAP) - vertical storage, optimized for analytics not transactions.

Row store
```
[id][symbol][price][volume][timestamp]
[id][symbol][price][volume][timestamp]
```
Column Store
```
Column: symbol     → [AAPL][AAPL][MSFT][GOOG]...
Column: price      → [189.2][189.3][412.1][132.5]...
Column: volume     → [100][200][150][300]...
Column: timestamp  → [...]

```


![Pasted image 20260317163143.png](/img/user/KDB/attachments/Pasted%20image%2020260317163143.png)
Ex:
**For tick pipeline project** — the TP → RDB → HDB flow in section 2 is exactly the architecture you're building. The tickerplant logs everything, the RDB holds today in RAM (your Redis + Kafka layer is the rough equivalent), and the HDB is your historical KDB+ store. Knowing this vocabulary cold will matter in interviews.

