# coding-delegate

OpenClaw skill that automatically delegates coding tasks to local OpenCode CLI to save main session token consumption.

## Description

This skill detects programming-related tasks and routes them to the local OpenCode CLI instead of handling them directly in the main session. This helps reduce token usage while maintaining high-quality coding assistance.

## When It Delegates

The skill automatically delegates to OpenCode when the request involves:

- **Code Creation/Modification**: Writing functions, classes, features, refactoring
- **Debugging & Troubleshooting**: Fixing bugs, performance profiling, writing tests
- **Code Review & Analysis**: Reviewing PRs, analyzing code quality, generating docs
- **Technical Configuration**: Writing config files, setting up build tools, CI/CD
- **Programming Concepts**: Explaining APIs, algorithms, comparing approaches

## When It Doesn't Delegate

- User explicitly requests direct handling
- Task involves OpenClaw-specific tools (feishu, sessions, gateway commands)
- Task is non-technical (general questions, chat, creative writing)
- Task requires OpenClaw workspace context that opencode doesn't have

## Installation

Install via ClawHub:

```bash
clawhub install coding-delegate
```

Or manually copy to your OpenClaw skills directory.

## Usage

Once installed, the skill activates automatically when coding-related tasks are detected. No manual invocation required.

## Requirements

- OpenCode CLI installed and available in PATH
- OpenClaw with skill system support

## License

MIT
