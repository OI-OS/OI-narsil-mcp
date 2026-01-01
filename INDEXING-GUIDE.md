# OI-narsil-mcp Indexing Guide

## Overview

OI-narsil-mcp requires indexing your repository before it can provide code intelligence features. This guide explains how to index your codebase directly (bypassing OI/BT4) to avoid timeout issues.

## Why Index Directly?

- **No Timeout Limits**: Direct indexing bypasses the 30-second `brain-trust4` timeout
- **Faster**: Indexes in the background without MCP protocol overhead
- **Persistent**: Index is saved to disk (`~/.cache/narsil-mcp`) for fast subsequent startups
- **Full Control**: See progress and verbose output during indexing

## Quick Start

### Basic Indexing

```bash
# From OI project root
cd "/Users/chad/Desktop/_CLIENTS/OI/product/Release Test /OI-v0.5.25-arm64"

# Index current directory (recommended)
./MCP-servers/OI-narsil-mcp/target/release/narsil-mcp --repos . --reindex --persist --verbose
```

**What this does:**
- `--repos .` - Index the current directory (OI project root)
- `--reindex` - Force full re-index (clears old index)
- `--persist` - Save index to `~/.cache/narsil-mcp` for fast startup
- `--verbose` - Show detailed progress during indexing

### Indexing Multiple Repositories

```bash
# Index multiple repositories
./MCP-servers/OI-narsil-mcp/target/release/narsil-mcp \
  --repos . \
  --repos ~/other-project \
  --repos ~/another-project \
  --reindex \
  --persist \
  --verbose
```

## Indexing Options

### Recommended Flags

```bash
./MCP-servers/OI-narsil-mcp/target/release/narsil-mcp \
  --repos . \
  --reindex \
  --persist \
  --git \
  --call-graph \
  --verbose
```

**Flag Descriptions:**
- `--repos <path>` - Repository path(s) to index (required)
- `--reindex` - Force full re-index (recommended for first run)
- `--persist` - Save index to disk (recommended for faster startup)
- `--git` - Enable git integration (blame, history, contributors)
- `--call-graph` - Enable call graph analysis (slower but more powerful)
- `--watch` - Auto-reindex on file changes (for development)
- `--verbose` - Show detailed progress

### Performance Options

**For Large Codebases:**
```bash
# Index with call graph (slower initial index, better analysis)
./MCP-servers/OI-narsil-mcp/target/release/narsil-mcp \
  --repos . \
  --reindex \
  --persist \
  --call-graph \
  --git \
  --verbose
```

**For Fast Indexing (Skip Advanced Features):**
```bash
# Basic indexing (faster, fewer features)
./MCP-servers/OI-narsil-mcp/target/release/narsil-mcp \
  --repos . \
  --reindex \
  --persist \
  --verbose
```

## Indexing Process

### What Gets Indexed

- **10,498 files** (typical for OI project)
- **388,443 symbols** (functions, classes, structs, etc.)
- **16 languages** supported (Rust, Python, JavaScript, TypeScript, Go, C/C++, Java, C#, Bash, Ruby, Kotlin, PHP, Swift, Verilog)

### Expected Duration

- **Small repos** (< 1,000 files): 10-30 seconds
- **Medium repos** (1,000-10,000 files): 2-5 minutes
- **Large repos** (> 10,000 files): 5-15 minutes

### Index Location

The index is saved to:
```
~/.cache/narsil-mcp/
```

This allows fast startup on subsequent runs (no re-indexing needed).

## Verifying Index Status

### Check via OI/BT4

```bash
# List indexed repositories
./brain-trust4 call OI-narsil-mcp list_repos '{}'

# Get index status
./brain-trust4 call OI-narsil-mcp get_index_status '{}'
```

### Check via Direct Command

```bash
# Run narsil-mcp and check status (will show indexed repos)
./MCP-servers/OI-narsil-mcp/target/release/narsil-mcp --repos . --verbose
```

## Troubleshooting

### Index Not Found

**Problem:** `list_repos` shows "No repositories indexed yet"

**Solution:**
1. Run direct indexing command (see Quick Start above)
2. Wait for indexing to complete (check verbose output)
3. Verify index exists: `ls -la ~/.cache/narsil-mcp/`

### Indexing Times Out via OI/BT4

**Problem:** `reindex` tool times out after 30 seconds

**Solution:** Use direct indexing command (bypasses timeout)

### Index is Stale

**Problem:** Code changes not reflected in search results

**Solution:**
```bash
# Force re-index
./MCP-servers/OI-narsil-mcp/target/release/narsil-mcp --repos . --reindex --persist --verbose
```

### Large Repository Performance

**Problem:** Indexing takes too long or uses too much memory

**Solution:**
1. Index specific subdirectories instead of entire repo
2. Use `--persist` to save progress
3. Skip `--call-graph` for faster indexing (can add later)

## Integration with OI/BT4

### Configuration

After indexing, ensure `config.json` includes:

```json
{
  "OI-narsil-mcp": {
    "args": [
      "--repos",
      ".",
      "--persist"
    ],
    "command": "../MCP-servers/OI-narsil-mcp/target/release/narsil-mcp"
  }
}
```

**Note:** The `--reindex` flag is only needed for initial indexing. Remove it from `config.json` after first run to avoid re-indexing on every startup.

### Auto-Indexing on Startup

With `--persist` flag, narsil-mcp will:
1. Load existing index from `~/.cache/narsil-mcp/`
2. Only re-index if files have changed (incremental updates)
3. Start much faster on subsequent runs

## Advanced Usage

### Index Specific Directories

```bash
# Index only src directory
./MCP-servers/OI-narsil-mcp/target/release/narsil-mcp \
  --repos ./src \
  --reindex \
  --persist \
  --verbose

# Index only MCP-servers
./MCP-servers/OI-narsil-mcp/target/release/narsil-mcp \
  --repos ./MCP-servers \
  --reindex \
  --persist \
  --verbose
```

### Watch Mode (Development)

```bash
# Auto-reindex on file changes
./MCP-servers/OI-narsil-mcp/target/release/narsil-mcp \
  --repos . \
  --persist \
  --watch \
  --verbose
```

**Note:** Watch mode keeps the process running. Press `Ctrl+C` to stop.

### Neural Semantic Search (Optional)

Requires API key (Voyage AI or OpenAI):

```bash
# Set API key
export VOYAGE_API_KEY="your-key-here"
# or
export OPENAI_API_KEY="your-key-here"

# Index with neural embeddings
./MCP-servers/OI-narsil-mcp/target/release/narsil-mcp \
  --repos . \
  --reindex \
  --persist \
  --neural \
  --neural-backend api \
  --neural-model voyage-code-2 \
  --verbose
```

## Post-Installation Checklist

After building OI-narsil-mcp:

- [ ] Run initial indexing: `./MCP-servers/OI-narsil-mcp/target/release/narsil-mcp --repos . --reindex --persist --verbose`
- [ ] Wait for indexing to complete (check verbose output)
- [ ] Verify index status: `./brain-trust4 call OI-narsil-mcp list_repos '{}'`
- [ ] Update `config.json` to include `--persist` flag (remove `--reindex` after first run)
- [ ] Test a search: `./brain-trust4 call OI-narsil-mcp search_code '{"query": "database connection"}'`

## Example Output

```
2025-12-31T23:33:16.182955Z  INFO narsil_mcp: Starting narsil-mcp v1.1.4
2025-12-31T23:33:16.184154Z  INFO narsil_mcp: Repos to index: ["."]
2025-12-31T23:33:16.184374Z  INFO narsil_mcp: Features: call_graph=false, git=false, watch=false, persist=true, lsp=false, streaming=false, remote=false, neural=false
2025-12-31T23:33:16.219636Z  INFO narsil_mcp::index: Engine created (initialization deferred for 1 repos)
2025-12-31T23:33:16.219861Z  INFO narsil_mcp: Re-indexing all repositories...
2025-12-31T23:33:16.219913Z  INFO narsil_mcp::index: Indexing repository: "."
...
2025-12-31T23:39:11.762854Z  INFO narsil_mcp::index: Indexed 10498 files, 388443 symbols in unknown
```

## Related Documentation

- [README.md](./README.md) - Full narsil-mcp documentation
- [docs/INSTALL.md](./docs/INSTALL.md) - Installation instructions
- [docs/configuration.md](./docs/configuration.md) - Configuration options
- [OI-NARSIL-MCP-WORKFLOWS.md](./OI-NARSIL-MCP-WORKFLOWS.md) - Usage workflows

