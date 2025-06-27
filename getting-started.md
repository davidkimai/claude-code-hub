# Getting Started with Claude Code

This guide will help you quickly install Claude Code and begin using it effectively in your development workflow.

## What is Claude Code?

Claude Code is Anthropic's command line tool for agentic coding. It allows Claude to directly interact with your local development environment - reading files, running commands, making changes to your codebase, and more. Claude Code provides a seamless interface between Claude's natural language understanding and your programming tasks.

## Prerequisites

- **Claude Pro or Claude Team subscription**: Claude Code requires access to Claude's models
- **Operating System**: macOS, Linux, or Windows (via WSL)
- **Node.js**: Version 18 or higher
- **Git**: For repository interaction

## Installation

### Method 1: Installation Script (Recommended)

```bash
# Run the installation script
curl -fsSL https://claude.ai/code/install | bash

# Verify installation
claude --version
```

### Method 2: Manual Installation

```bash
# Download the appropriate binary for your system
# (Example for macOS)
curl -L https://claude.ai/code/download/macos -o claude

# Make the binary executable
chmod +x claude

# Move to a location in your PATH
sudo mv claude /usr/local/bin/
```

## Initial Setup

1. **Navigate to your project directory**:
   ```bash
   cd your-project
   ```

2. **Initialize Claude Code** (optional but recommended):
   ```bash
   claude init
   ```
   This creates a default CLAUDE.md file with helpful context for your project.

3. **Start Claude Code**:
   ```bash
   claude
   ```

## Basic Commands

Once Claude Code is running, you can interact with it using natural language:

```
> help me understand this codebase
```

Claude will scan your project files and provide an overview of the structure and functionality.

```
> analyze app.js and explain what it does
```

Claude will read the specified file and explain its purpose and functionality.

```
> add a feature to log all API calls
```

Claude will suggest and implement changes to add the requested feature.

## CLAUDE.md Configuration

One of the most powerful features of Claude Code is the ability to customize context through CLAUDE.md files. These files provide Claude with persistent information about your project, preferences, and workflow.

Create or edit your CLAUDE.md file:

```markdown
# Project Overview
This is a React application that manages a task list with user authentication.

# Bash Commands
- npm run dev: Start development server
- npm run test: Run test suite
- npm run build: Build for production

# Code Style
- Use functional components with hooks
- Follow ESLint configuration
- Use TypeScript interfaces for all data structures

# Important Files
- src/App.tsx: Main application component
- src/api/tasks.ts: API calls for task management
- src/components/TaskList.tsx: Component for displaying tasks
```

## Managing Permissions

By default, Claude Code asks for permission before executing commands that modify your system. You can customize permissions in several ways:

### During a Session

When Claude requests permission, you can select "Always allow" to permit similar actions in the future.

### Using the /permissions Command

```
> /permissions
```

This opens the permissions management interface where you can add or remove allowed tools.

### Editing Settings Files

Edit `.claude/settings.json` in your project directory or `~/.claude.json` globally.

## Common Workflows

### Exploring a New Codebase

```
> explain the architecture of this project
> what are the main components and how do they interact?
> where is the database connection configured?
```

### Implementing a New Feature

```
> I need to add user authentication to this application
> please create a plan for implementing this feature
> now implement the user login component
```

### Debugging an Issue

```
> help me understand why this test is failing
> analyze the error logs and suggest fixes
> refactor this function to fix the memory leak
```

## Next Steps

After getting familiar with the basics, explore these more advanced capabilities:

- **[Custom Slash Commands](../advanced-techniques/slash-commands.md)**: Create your own slash commands for repeated workflows
- **[Headless Mode](../advanced-techniques/headless.md)**: Use Claude Code in automation scripts
- **[MCP Integration](../advanced-techniques/mcp.md)**: Extend Claude Code with Model-Computer-Programmer servers
- **[Multi-Claude Workflows](../advanced-techniques/multi-claude.md)**: Use multiple Claude Code instances for complex tasks

## Troubleshooting

### Common Issues

**Claude seems to have incomplete context:**
- Try running `/clear` to reset the context window
- Ensure your CLAUDE.md file has relevant information
- Explicitly mention files you want Claude to analyze

**Claude is asking for permission too frequently:**
- Use the `/permissions` command to manage allowed tools
- Edit your settings.json file to pre-approve common tools

**Claude isn't making the changes I expect:**
- Be more specific in your instructions
- Ask Claude to explain its plan before implementing changes
- Use Extended Thinking with the "think" keyword for complex tasks

### Getting Help

If you encounter issues not covered here:

- Check the [Troubleshooting Guide](troubleshooting.md) for more detailed solutions
- Visit the [Claude Code GitHub Discussions](https://github.com/anthropics/claude-code-hub/discussions) to ask questions
- Check [Anthropic's Support Center](https://support.anthropic.com) for official guidance

## Example Session

Here's an example of a typical Claude Code session:

```
$ claude

✓ Found 1 MCP server • /mcp
✓ Loaded project + user memory • /memory

> help me understand the authentication system in this codebase

I'll help you understand the authentication system. Let me explore the codebase to find relevant files.

Call(Search(pattern: "auth|login|user.*password|session|token"))...
[Results show multiple files related to authentication]

Based on my search, the authentication system appears to be implemented across several files:

1. `src/auth/AuthProvider.js` - Central authentication context provider
2. `src/api/auth.js` - API calls for login, logout, registration
3. `src/components/LoginForm.js` - UI component for login
4. `src/utils/tokenStorage.js` - JWT token management

The system uses JWT (JSON Web Tokens) for authentication with the following workflow:
...
```

## Tips for Success

- **Be specific** in your instructions to Claude
- **Start with a plan** before implementing complex changes
- **Use images and screenshots** when working on visual components
- **Provide feedback** to help Claude course-correct
- **Document common commands** in your CLAUDE.md file

Remember that Claude Code is still in research preview, and its capabilities continue to evolve. Experiment with different approaches to find what works best for your workflow.

Happy coding with Claude!
