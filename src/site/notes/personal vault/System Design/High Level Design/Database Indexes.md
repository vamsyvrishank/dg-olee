---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/database-indexes/"}
---


### The Core Concept (Mental Model)

At its simplest, an index is a **data structure** that improves the speed of data retrieval operations on a database table at the cost of additional writes and storage space.

- **Without an Index:** The database performs a **Full Table Scan**. It has to load every single page from disk to memory to check if the row matches your query. Complexity: O(N).
    
- **With an Index:** The database navigates a sorted structure to find a pointer to the physical data location. Complexity: Typically O(logN).

