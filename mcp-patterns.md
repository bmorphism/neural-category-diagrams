# MCP Data Flow Patterns

```
                                 DATA FLOW PATTERNS

+----------------+    search     +----------------+    process    +----------------+
|   DISCOVERY    |------------->|  ACQUISITION   |------------->|  PROCESSING    |
|                |              |                |              |                |
| [exa]          |              | [filesystem]   |              | [duckdb]       |
| [screenpipe]   |              | [github]       |              | [juvix]        |
+----------------+              +----------------+              +----------------+
        |                              |                              |
        |         +------------------+ |                              |
        |         |    PERSISTENCE   | |          transform           |
        |         |                  | |                              |
        |         | [apple-notes]    | |                              |
        |         | [coinflip]       | |                              |
        |         | [mcp-mcp]        | |                              |
        |         +------------------+ |                              |
        |                  ^          |                              |
        |                  |          |                              |
        v                  |          v                              v
+----------------+         |   +----------------+    analyze    +----------------+
|   EXTERNAL     |         |   |  INTEGRATION   |<------------|   ANALYSIS     |
|   SERVICES     |         |   |                |              |                |
| [google-maps]  |         |   | [github]       |              | [duckdb]       |
| [youtube]      |         |   | [filesystem]   |              | [screenpipe]   |
+----------------+         |   +----------------+              +----------------+
        |                  |          |                              ^
        |                  |          |                              |
        |                  |          v                              |
        |                  |   +----------------+    extract     +----------------+
        |                  |   |   SYNTHESIS    |------------->|    STORAGE     |
        |                  |   |                |              |                |
        |                  +---|  [combined     |              | [filesystem]   |
        |                      |   operations]   |              | [github]       |
        |                      +----------------+              +----------------+
        |                                                              |
        |                       feedback loop                          |
        +--------------------------------------------------------------+


                               OPERATION PATTERNS

1. Search -> Process -> Store
   [exa] -> [duckdb] -> [filesystem]
   ----------------------------------------
   search -> read-query -> write_file

2. External -> Transform -> Integrate
   [google-maps] -> [duckdb] -> [github]
   ----------------------------------------
   maps_search -> write-query -> push_files

3. Discover -> Analyze -> Synthesize
   [screenpipe] -> [duckdb] -> [apple-notes]
   ----------------------------------------
   search-content -> read-query -> create_note

4. Acquire -> Process -> Persist
   [filesystem] -> [juvix] -> [github]
   ----------------------------------------
   read_file -> run_command -> create_or_update_file


                               COMPOSITION RULES

+-------------------+----------------+------------------+
|     OPERATION     |    REQUIRES   |    PROVIDES     |
+-------------------+----------------+------------------+
| DISCOVERY         | Query terms   | Raw data        |
| ACQUISITION       | Source path   | File content    |
| PROCESSING        | Input data    | Modified data   |
| ANALYSIS          | Dataset       | Results         |
| INTEGRATION       | Multiple data | Combined data   |
| SYNTHESIS         | Components    | Final product   |
| PERSISTENCE       | Data to save  | Stored data     |
+-------------------+----------------+------------------+
```