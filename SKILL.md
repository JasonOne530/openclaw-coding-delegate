---
name: coding-delegate
description: Automatically delegate coding tasks to local OpenCode CLI to save main session tokens. Use when the user request involves writing, modifying, debugging, or reviewing code, or technical programming discussions. Skip delegation only when explicitly told to handle directly or when the task requires OpenClaw-specific knowledge/tools.
---

# Coding Delegate

## Overview

This skill routes programming-related tasks to the local OpenCode CLI, saving token usage in the main session. OpenCode handles code generation, debugging, refactoring, and other coding tasks efficiently.

## Detection: Is This a Coding Task?

Delegate to OpenCode when the request involves:

**Code Creation/Modification**
- Writing new functions, classes, or modules
- Adding features or implementing algorithms
- Refactoring or optimizing existing code
- Converting code between languages/frameworks

**Debugging & Troubleshooting**
- Fixing bugs or errors
- Debugging code issues
- Performance profiling and optimization
- Writing or improving tests

**Code Review & Analysis**
- Reviewing pull requests or code changes
- Analyzing code quality or security
- Explaining how code works
- Generating documentation from code

**Technical Configuration**
- Writing config files (webpack, eslint, docker-compose, etc.)
- Setting up build tools or CI/CD
- Writing infrastructure-as-code (Terraform, CloudFormation, etc.)

**Programming Concepts**
- Explaining APIs, libraries, or frameworks
- Discussing algorithms or data structures
- Comparing programming approaches or patterns

**Do NOT delegate when:**
- User explicitly asks to handle directly ("don't use opencode", "you handle it")
- Task involves OpenClaw-specific tools (feishu, sessions, gateway commands)
- Task is non-technical (general questions, chat, creative writing)
- Task requires access to workspace context that opencode doesn't have

## Delegation Pattern

Use this command pattern to delegate:

```bash
bash pty:true workdir:<project-dir> command:"opencode run '<user-request>'"
```

**For one-shot quick tasks:**
```bash
# Create temp repo for scratch work
SCRATCH=$(mktemp -d) && cd $SCRATCH && git init && \
bash pty:true workdir:$SCRATCH command:"opencode run 'Write a Python function to parse JSON'"

# In existing project
bash pty:true workdir:~/Projects/myproject command:"opencode run 'Add input validation to the login function'"
```

**For longer tasks (background mode):**
```bash
bash pty:true workdir:~/project background:true command:"opencode run 'Refactor the user auth module'"
# Returns sessionId for tracking

# Monitor progress
process action:log sessionId:<id>

# Check if done
process action:poll sessionId:<id>
```

## Workdir Selection

Choose `workdir` based on task context:

| Task Type | Workdir | Notes |
|-----------|---------|-------|
| Scratch/learning | `$(mktemp -d && git init)` | Temp folder, fresh git repo |
| Existing project | `~/path/to/project` | Project directory |
| OpenClaw itself | `/Users/bot/.openclaw/workspace` | Only if explicitly requested |
| User home | `~` | Rare, avoid unless needed |

**⚠️ Never** use workdir that contains sensitive configs or credentials unless required.

## Example Delegations

| User Request | OpenCode Command |
|---------------|-------------------|
| "写一个 Python 脚本来重命名文件夹里的所有图片" | `opencode run 'Write a Python script to rename all images in a folder'` |
| "Fix the memory leak in the cache module" | `opencode run 'Fix the memory leak in the cache module'` |
| "写一个 Dockerfile 来运行 Node.js 应用" | `opencode run 'Write a Dockerfile to run a Node.js application'` |
| "帮我写一个 Rust 的二分查找算法" | `opencode run 'Implement binary search algorithm in Rust'` |

## Handling OpenCode Responses

After OpenCode completes:

1. **Summarize key results** - What changed? What was built?
2. **Share code snippets** - Paste relevant code if small enough
3. **Provide guidance** - Next steps, testing, deployment
4. **Save to workspace** - If code is useful, save to appropriate location

If OpenCode fails or needs clarification:
- Share the error message
- Ask for clarification or context
- Retry with improved prompt if needed

## User Communication Style

**When delegating:** Briefly inform user ("交给 OpenCode 处理...") and wait for completion.

**When done:** Share results concisely. Don't echo OpenCode's full output unless it's small and valuable.

**If user asks follow-up:** Re-delegate if it's still coding-related. Build on previous context by referencing what OpenCode already did.
