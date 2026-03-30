cat > README.md << 'EOF'
# thura
```
________________________________________________________________________________
|                                                                              |
|  _____ _   _ _   _ ____      _                                               |
| |_   _| | | | | | |  _ \    / \                                              |
|   | | | |_| | | | | |_) |  / _ \    [ The Gate ]                            |
|   | | |  _  | |_| |  _ <  / ___ \                                           |
|   |_| |_| |_|\___/|_| \_\/_/   \_\                                          |
|                                                                              |
|        .                        |                                            |
|       / \                       |  $ thura add https://...                   |
|      /   \                      |  > Fetching... [OK]                        |
|     /_____\                     |  > Stored to queue [DONE]                  |
|     |     |                     |                                            |
|     |     |                     |  $ thura queue                             |
|_____|_____|_____________________|  > Opening triage... [OK]                  |
|                                                                              |
________________________________________________________________________________
```

Your personal gate. Nothing enters your attention without passing through you first.

Part of the [skete-io](https://github.com/skete-io) ecosystem:

- **skete** — distillation pipeline, articles → epub
- **cella** — sync server, epub → Supernote  
- **grapheus** — platform tamer, YouTube/Teams → transcripts
- **thura** — the gate, knowledge interface, memory

## philosophy

The desert is not emptiness. It is chosen contents.

Thura is named after the Greek word for gate or door. Everything that enters
your knowledge graph passes through a conscious act — adding, anchoring,
deciding. No algorithm decides for you.

## commands
```
thura add <url>        capture a URL to your queue
thura queue            open triage TUI — face the pile
thura anchor <url>     pull something deep, keep it forever
thura recall <query>   search your archive
thura research <query> expand from your graph outward (planned)
```

## data

Everything lives in `~/.skete/thura.db` — a single DuckDB file.
No cloud. No account. No sync unless you choose it.

## stack

- Go + Cobra CLI
- Bubbletea TUI
- DuckDB for storage + FTS
- go-readability for extraction
- nomic-embed-text via Ollama for embeddings (planned)
EOF
```

Now the OpenCode prompt. This is the most important part — be specific about what you want in v1, ruthlessly exclude what you don't:

---

**OpenCode prompt:**
```
You are helping me build `thura` — a Go CLI tool that is the intake and 
triage layer for my personal knowledge system. It is part of the skete-io 
ecosystem (skete, cella, grapheus, thura).

The project is already initialized at ~/ws/skete-io/thura with:
- go.mod (github.com/skete-io/thura)
- README.md (read it for philosophy and context)
- directory structure already created

## SCOPE FOR THIS SESSION — v0.1 ONLY

Build exactly these two commands, nothing else:

### 1. `thura add <url>`
- fetch the URL using net/http with a proper user agent
- extract title and clean text using go-readability  
- store in DuckDB at ~/.skete/thura.db
- print confirmation with title

Schema for queue table:
```sql
CREATE TABLE IF NOT EXISTS queue (
    id          VARCHAR PRIMARY KEY DEFAULT gen_random_uuid(),
    url         VARCHAR NOT NULL UNIQUE,
    domain      VARCHAR NOT NULL,
    title       VARCHAR,
    text        TEXT,
    added_at    TIMESTAMP DEFAULT now(),
    status      VARCHAR DEFAULT 'pending'  -- pending, read, dropped, anchored
);
```

### 2. `thura queue` — bubbletea TUI
- list all items with status='pending', newest first
- show: index, domain, title, time added (relative: "2h ago", "3 days ago")
- vim keys: j/k navigate, d drop (sets status='dropped'), q quit
- enter: preview mode — render extracted text with glamour in a viewport
- in preview: b back to list, a anchor (sets status='anchored'), q quit
- status bar at bottom showing count

## TECHNICAL CONSTRAINTS

- DuckDB file path: ~/.skete/thura.db (create ~/.skete/ if not exists)
- config via ~/.skete/config.toml using spf13/viper (just db path for now)
- errors: always wrap with context, never panic in normal operation
- Go style: idiomatic, no unnecessary abstractions for v0.1
- single binary output: `thura`

## WHAT NOT TO BUILD

Do not build: embeddings, recall, research, anchor command (just the status 
change in queue), search, any LLM integration, any network calls beyond 
fetching the URL. This is v0.1. Resist the urge to add things.

## FILE STRUCTURE

Follow the structure in the README directory layout exactly.
internal/db/schema.go contains SQL, internal/db/db.go handles connection.
cmd/ contains one file per command.

## CODE STYLE

- no global state except the db connection passed via cobra PersistentPreRun
- lipgloss for all styling — use var styles defined at package level
- bubbletea model follows the standard Model/Update/View pattern strictly
- glamour renderer initialized once, dark style

## STARTING POINT

Begin with internal/db/schema.go and internal/db/db.go, then main.go and 
cmd/root.go, then cmd/add.go, then cmd/queue.go with TUI last.

After each file ask me if it looks right before continuing.
