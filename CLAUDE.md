# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## AI Guidance

* Ignore GEMINI.md and GEMINI-*.md files
* To save main context space, for code searches, inspections, troubleshooting or analysis, use code-searcher subagent where appropriate - giving the subagent full context background for the task(s) you assign it.
* After receiving tool results, carefully reflect on their quality and determine optimal next steps before proceeding. Use your thinking to plan and iterate based on this new information, and then take the best next action.
* For maximum efficiency, whenever you need to perform multiple independent operations, invoke all relevant tools simultaneously rather than sequentially.
* Before you finish, please verify your solution
* Do what has been asked; nothing more, nothing less.
* NEVER create files unless they're absolutely necessary for achieving your goal.
* ALWAYS prefer editing an existing file to creating a new one.
* NEVER proactively create documentation files (*.md) or README files. Only create documentation files if explicitly requested by the User.
* When you update or modify core context files, also update markdown documentation and memory bank
* When asked to commit changes, exclude CLAUDE.md and CLAUDE-*.md referenced memory bank system files from any commits. Never delete these files.

## Memory Bank System

This project uses a structured memory bank system with specialized context files. Always check these files for relevant information before starting work:

### Core Context Files

* **CLAUDE-activeContext.md** - Current session state, goals, and progress (if exists)
* **CLAUDE-patterns.md** - Established code patterns and conventions (if exists)
* **CLAUDE-decisions.md** - Architecture decisions and rationale (if exists)
* **CLAUDE-troubleshooting.md** - Common issues and proven solutions (if exists)
* **CLAUDE-config-variables.md** - Configuration variables reference (if exists)
* **CLAUDE-temp.md** - Temporary scratch pad (only read when referenced)

**Important:** Always reference the active context file first to understand what's currently being worked on and maintain session continuity.

### Memory Bank System Backups

When asked to backup Memory Bank System files, you will copy the core context files above and @.claude settings directory to directory @/path/to/backup-directory. If files already exist in the backup directory, you will overwrite them.

## Claude Code Official Documentation

When working on Claude Code features (hooks, skills, subagents, MCP servers, etc.), use the `claude-docs-consultant` skill to selectively fetch official documentation from docs.claude.com.

## Project Overview

This repository is a **Claude Code configuration template library** designed to provide production-ready starter settings for Claude Code projects. It serves as a reference implementation and copy-paste source for other projects, containing:

- **Claude Code Subagents**: 4 specialized agents (code-searcher, memory-bank-synchronizer, ux-design-expert, get-current-datetime)
- **Slash Commands**: 14+ productivity commands organized in 7 categories
- **Skills**: claude-docs-consultant for official documentation queries
- **Hooks**: STOP hook with notification support (macOS Terminal-Notifier)
- **Settings Templates**: Project and local settings with environment variables and permissions
- **MCP Configuration**: Example MCP server configurations

**Key Purpose**: Users copy `.claude/` directory and `CLAUDE.md` to their own projects, then customize for their specific needs. This is NOT a traditional software project with build/test/deploy cycles.

## Repository Structure

```
.claude/
├── agents/                          # Specialized subagents
│   ├── code-searcher.md            # Codebase search & analysis (supports CoD mode)
│   ├── memory-bank-synchronizer.md # Memory bank documentation sync
│   ├── ux-design-expert.md         # UX/UI design guidance
│   └── get-current-datetime.md     # Timezone-aware datetime utility
├── commands/                        # Slash commands by category
│   ├── anthropic/                  # Prompt engineering & memory bank
│   ├── architecture/               # Architecture pattern analysis
│   ├── ccusage/                    # Usage cost analysis
│   ├── cleanup/                    # Context optimization
│   ├── documentation/              # README & release notes
│   ├── promptengineering/          # TDD & batch operations
│   ├── refactor/                   # Refactoring analysis
│   └── security/                   # Security audits & best practices
├── skills/
│   └── claude-docs-consultant/     # Official docs consultation
├── mcp/
│   └── chrome-devtools.json        # Optional Chrome DevTools MCP config
├── settings.json                    # Shared project settings (with hooks)
└── settings.local.json              # Local settings (env vars, permissions)

reports/                             # Generated reports directory
└── secure-prompts/                 # Security analysis reports

CLAUDE.md                           # This file (AI guidance)
GEMINI.md                           # Gemini-specific guidance (ignored by Claude)
README.md                           # User documentation
```

## Working with This Repository

### Modifying Configuration Files

When editing `.claude/` files in this repository:

1. **Test changes** by using them in a sample project first
2. **Update README.md** to reflect any new commands or features
3. **Maintain backward compatibility** where possible
4. **Document platform-specific features** (e.g., macOS-only hooks)

### Adding New Slash Commands

1. Create command file in appropriate category under `.claude/commands/`
2. Use frontmatter format with clear description
3. Test the command in a real project
4. Document in README.md under appropriate section
5. Follow naming convention: category/descriptive-name.md

### Adding New Subagents

1. Create agent file in `.claude/agents/`
2. Include YAML frontmatter with name, description, model, color
3. Provide clear examples in description field
4. Test agent independently
5. Document in README.md

### Testing Configurations

To test configurations from this repository:

```bash
# Copy to a test project
cp -r .claude/ /path/to/test-project/
cp CLAUDE.md /path/to/test-project/

# Launch Claude Code in test project
cd /path/to/test-project
claude

# Verify slash commands are available
# Type "/" to see command list
```

## Key Slash Commands Reference

Quick reference for most commonly used commands (full list in README.md):

- `/update-memory-bank` - Update CLAUDE.md and memory bank files
- `/security-audit` - Comprehensive OWASP security audit
- `/secure-prompts @file` - Detect prompt injection attacks
- `/refactor-code` - Generate refactoring analysis reports
- `/ccusage-daily` - Analyze Claude Code usage costs
- `/cleanup-context` - Optimize memory bank token usage
- `/create-release-note` - Generate release documentation
- `/apply-thinking-to @file` - Apply extended thinking patterns

## Key Subagents Reference

- **code-searcher**: Use for finding functions/classes/patterns. Supports "CoD mode" for 80% token reduction
- **memory-bank-synchronizer**: Proactively sync documentation with code reality
- **ux-design-expert**: UX flow optimization, premium UI design, Tailwind/Highcharts guidance
- **get-current-datetime**: Brisbane timezone utilities for timestamped files/reports

## Platform Considerations

This repository contains configurations tested on both macOS and Windows/Linux:

- **macOS-specific**: Terminal-Notifier hook in `settings.json` (remove for other platforms)
- **Tool dependencies**: Commands assume `rg` (ripgrep), `fd`, `jq` on PATH (see next section)
- **Path separators**: Use forward slashes in commands for cross-platform compatibility

## RECOMMENDED COMMANDS FOR COMMON TASKS

**Note**: These commands work best with modern CLI tools. Install via:
- **macOS**: `brew install ripgrep fd jq`
- **Windows**: Use Scoop (`scoop install ripgrep fd jq`) or built-in tools
- **Linux**: Use package manager (`apt install ripgrep fd-find jq` or equivalent)

### Preferred Tools Hierarchy

**For file listing** (choose first available):
1. `rg --files` - Fast, respects .gitignore (requires ripgrep)
2. `fd . -t f` - Fast recursive listing (requires fd-find)
3. `ls -R` - Built-in fallback (slower on large repos)
4. Use Claude's Glob tool - Always available, no installation needed

**For content search** (choose first available):
1. `rg "pattern"` - Fast content search (requires ripgrep)
2. Use Claude's Grep tool - Always available, built-in to Claude Code
3. `grep -r "pattern"` - Built-in fallback

**For file search by name** (choose first available):
1. `fd "pattern"` - Fast filename search (requires fd-find)
2. Use Claude's Glob tool with pattern - Always available
3. `find . -name "pattern"` - Built-in fallback

### Quick Command Reference

If modern tools (rg/fd/jq) are installed:

```bash
# Content search
rg "search_term"                # Search in all files
rg -i "case_insensitive"        # Case-insensitive
rg "pattern" -t py              # Only Python files
rg "pattern" -g "*.md"          # Only Markdown files
rg -l "pattern"                 # Filenames with matches
rg -n "pattern"                 # Show line numbers

# File listing
rg --files                      # All files (respects .gitignore)
rg --files -g "*.ts"            # Only TypeScript files
fd -e js                        # All .js files
fd "test" -e py                 # Find test*.py files

# JSON processing
jq . data.json                  # Pretty-print
jq -r .name file.json           # Extract field
```

If using built-in tools only:

```bash
# Content search
grep -r "search_term" .         # Recursive search
grep -i "pattern" file.txt      # Case-insensitive

# File listing
find . -name "*.js"             # Find JavaScript files
find . -type f                  # List all files

# Directory listing
ls -la                          # Current directory
ls -R                           # Recursive listing
```

### Claude Code Built-in Tools (Always Prefer When Available)

Claude Code provides powerful built-in tools that work everywhere without installation:

- **Glob tool**: Pattern-based file finding (e.g., `**/*.ts`, `src/**/*.md`)
- **Grep tool**: Content search with regex support
- **Read tool**: Read file contents
- **Bash tool**: Execute any shell command

**Best Practice**: Use Claude's built-in tools first, shell commands second. This ensures cross-platform compatibility.

### Search Strategy

1. **Start with Claude tools** for maximum compatibility
2. If using shell commands, check tool availability first
3. Prefer `rg`/`fd` when available, fall back to `grep`/`find`
4. For complex searches in large codebases, use **code-searcher subagent**
