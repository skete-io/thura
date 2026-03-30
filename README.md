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
