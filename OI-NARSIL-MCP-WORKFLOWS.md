# OI-narsil-mcp Workflows for OI (Brain Trust 4)

This document demonstrates practical workflows using OI-narsil-mcp through the OI natural language interface.

## 🚀 Quick Start

**First-time setup:** The repository needs to be indexed. This happens automatically when you first use narsil tools, or you can trigger it manually:

```bash
# Check index status
./oi "narsil status"

# The server will automatically index when you first use it
# Or trigger reindex manually (if needed)
./brain-trust4 call OI-narsil-mcp reindex '{"repo": "."}'
```

## 📋 Workflow 1: Understanding a Codebase

**Goal:** Get a comprehensive understanding of an unfamiliar codebase.

### Step 1: Get Project Structure
```bash
./oi "narsil structure"
# Returns: Directory tree, file types, key files
```

### Step 2: Find Entry Points
```bash
./oi "narsil symbols main"
./oi "narsil search if __name__"
./oi "narsil symbols route"
```

### Step 3: Understand a Feature
```bash
./oi "narsil search authentication"
./oi "narsil symbols auth"
./oi "narsil file src/api/auth.py"
```

### Step 4: Map Dependencies
```bash
./oi "narsil dependencies"
./oi "narsil import graph"
```

### Step 5: Trace Data Flow
```bash
./oi "narsil data flow src/api/handler.rs"
./oi "narsil call graph"
```

**Complete Workflow Example:**
```bash
# Parallel execution for speed
./oi "narsil structure narsil symbols main narsil search authentication"
```

---

## 🔒 Workflow 2: Security Audit

**Goal:** Comprehensive security analysis before deployment.

### Step 1: Quick Security Overview
```bash
./oi "narsil security"
# Returns: Security summary with vulnerability counts
```

### Step 2: OWASP Top 10 Scan
```bash
./oi "narsil owasp"
# Returns: OWASP Top 10 2021 vulnerabilities
```

### Step 3: CWE Top 25 Scan
```bash
./oi "narsil cwe"
# Returns: CWE Top 25 Most Dangerous Weaknesses
```

### Step 4: Injection Vulnerabilities
```bash
./oi "narsil injection"
# Returns: SQL injection, XSS, command injection, path traversal
```

### Step 5: Dependency Vulnerabilities
```bash
./oi "narsil dependencies"
# Returns: Known CVEs in dependencies
```

### Step 6: License Compliance
```bash
./oi "narsil licenses"
# Returns: License compatibility analysis
```

### Step 7: Generate SBOM
```bash
./oi "narsil sbom"
# Returns: Software Bill of Materials (CycloneDX/SPDX)
```

**Complete Security Audit:**
```bash
# Parallel security checks
./oi "narsil security narsil owasp narsil cwe narsil dependencies"
```

---

## 🔍 Workflow 3: Code Search & Discovery

**Goal:** Find code patterns, symbols, and implementations.

### Semantic Search
```bash
./oi "narsil semantic authentication"
./oi "narsil semantic rate limiting"
```

### Hybrid Search (BM25 + TF-IDF)
```bash
./oi "narsil hybrid middleware"
./oi "narsil hybrid error handling"
```

### Symbol Discovery
```bash
./oi "narsil symbols"
./oi "narsil symbols function"
./oi "narsil symbols class"
```

### Find Usages
```bash
./oi "narsil usages DatabaseConnection"
./oi "narsil references AuthHandler"
```

### Go to Definition
```bash
./oi "narsil definition processOrder"
./oi "narsil definition src/api/handler.rs:45"
```

**Complete Search Workflow:**
```bash
# Parallel search
./oi "narsil search middleware narsil symbols handler narsil usages Database"
```

---

## 🐛 Workflow 4: Code Review & Quality

**Goal:** Review code changes and assess quality.

### Find Dead Code
```bash
./oi "narsil dead code"
./oi "narsil dead code src/services/"
```

### Check Type Errors
```bash
./oi "narsil type errors"
./oi "narsil type errors src/api/handler.ts"
```

### Find Circular Imports
```bash
./oi "narsil circular"
# Returns: Circular import dependencies
```

### Infer Types
```bash
./oi "narsil infer types src/utils/helper.py"
```

### Control Flow Analysis
```bash
./oi "narsil control flow src/handlers/auth.rs"
```

### Data Flow Analysis
```bash
./oi "narsil data flow src/handlers/auth.rs"
```

**Complete Quality Check:**
```bash
# Parallel quality checks
./oi "narsil dead code narsil circular narsil type errors"
```

---

## 📊 Workflow 5: Code Analysis & Metrics

**Goal:** Understand code complexity and relationships.

### Get Call Graph
```bash
./oi "narsil call graph"
# Returns: Function call relationships
```

### Get Import Graph
```bash
./oi "narsil import graph"
# Returns: Module dependency graph
```

### Find Similar Code
```bash
./oi "narsil similar"
./oi "narsil duplicate"
```

### Get Metrics
```bash
./oi "narsil metrics"
# Returns: Performance metrics, indexing stats
```

### Get File Excerpt
```bash
./oi "narsil excerpt src/api/handler.rs:45"
# Returns: Code excerpt with context
```

---

## 🎯 Common Use Cases

### "I need to understand this codebase"
```bash
./oi "narsil structure narsil symbols main narsil search entry point"
```

### "Find all security vulnerabilities"
```bash
./oi "narsil security narsil owasp narsil cwe narsil injection"
```

### "Where is this function used?"
```bash
./oi "narsil usages functionName"
./oi "narsil references ClassName"
```

### "What dependencies have vulnerabilities?"
```bash
./oi "narsil dependencies"
```

### "Show me the project structure"
```bash
./oi "narsil structure"
```

### "Find dead code"
```bash
./oi "narsil dead code"
```

### "Check for type errors"
```bash
./oi "narsil type errors"
```

### "Generate SBOM for compliance"
```bash
./oi "narsil sbom"
```

---

## 🔧 Advanced: Direct Tool Calls

For AI agents or when you need explicit parameters:

### Search Code
```bash
./brain-trust4 call OI-narsil-mcp search_code '{"query": "authentication", "repo": ".", "max_results": 10}'
```

### Security Scan
```bash
./brain-trust4 call OI-narsil-mcp scan_security '{"repo": ".", "severity_threshold": "medium"}'
```

### Find Symbols
```bash
./brain-trust4 call OI-narsil-mcp find_symbols '{"repo": ".", "pattern": "main*", "symbol_type": "function"}'
```

### Get Project Structure
```bash
./brain-trust4 call OI-narsil-mcp get_project_structure '{"repo": ".", "max_depth": 4}'
```

### Check Dependencies
```bash
./brain-trust4 call OI-narsil-mcp check_dependencies '{"repo": "."}'
```

---

## 📚 Available Intents

### Search & Discovery
- `narsil search` - Semantic and keyword search
- `narsil semantic` - BM25-ranked semantic search
- `narsil hybrid` - Hybrid search (BM25 + TF-IDF)

### Symbols & Definitions
- `narsil symbols` - Find data structures and functions
- `narsil definition` - Go to definition
- `narsil usages` - Find all usages of a symbol
- `narsil references` - Find all references

### Security
- `narsil security` - Full security scan
- `narsil owasp` - OWASP Top 10 scan
- `narsil cwe` - CWE Top 25 scan
- `narsil injection` - Injection vulnerability detection

### Dependencies & SBOM
- `narsil dependencies` - Check for vulnerable dependencies
- `narsil sbom` - Generate Software Bill of Materials
- `narsil licenses` - License compliance analysis

### Code Analysis
- `narsil dead code` - Find unreachable code
- `narsil circular` - Detect circular imports
- `narsil type errors` - Find type errors
- `narsil infer types` - Infer types in Python/JS/TS

### Graphs & Flow
- `narsil call graph` - Function call relationships
- `narsil import graph` - Module dependencies
- `narsil data flow` - Data flow analysis
- `narsil control flow` - Control flow graph

### Status & Metrics
- `narsil status` - Index status
- `narsil metrics` - Performance metrics
- `narsil repos` - List indexed repositories

---

## 💡 Tips

1. **Use Parallel Execution:** OI can run multiple narsil commands in parallel for 3-5x speedup
   ```bash
   ./oi "narsil security narsil dependencies narsil structure"
   ```

2. **Start Broad, Then Narrow:** Begin with structure, then drill into specific areas

3. **Combine with Other Tools:** Use narsil alongside other OI servers
   ```bash
   ./oi "narsil security cpu memory disk"
   ```

4. **Direct Calls for AI Agents:** Use `brain-trust4 call` for explicit parameters when needed

5. **Index First:** The repository needs to be indexed before search works. This happens automatically on first use.

---

## 🔗 Related Documentation

- [narsil-mcp GitHub](https://github.com/OI-OS/OI-narsil-mcp)
- [Understanding Codebase Workflow](docs/playbooks/workflows/understand-codebase.md)
- [Security Audit Workflow](docs/playbooks/workflows/security-audit.md)
- [Code Review Workflow](docs/playbooks/workflows/code-review.md)

---

## 📝 Notes

- All intents use the `narsil` prefix for easy recall
- Repository parameter defaults to current directory (`.`)
- Most tools support optional file/directory filtering
- Security scans can be filtered by severity and ruleset
- Search tools support natural language queries

