# Managing Tool Permissions in Claude Code

Tool permissions are a critical aspect of Claude Code that balances security with productivity. This comprehensive guide explores how to effectively manage tool permissions, allowing Claude to take actions on your behalf while maintaining appropriate security boundaries.

## Understanding the Permission Model

Claude Code uses a permission-based model that requires explicit approval before Claude can execute certain actions like:

- Writing or modifying files
- Running shell commands
- Interacting with external services via APIs or MCP servers
- Accessing specific resources

This model puts you in control of what Claude can do in your environment while still allowing it to be highly capable.

## Types of Permissions

### 1. Tool Categories

Claude Code permissions are organized by tool categories:

- **Edit**: Permissions to create, modify, or delete files
- **Bash**: Permissions to execute shell commands
- **MCP**: Permissions to use Model-Computer-Programmer server tools
- **WebFetchTool**: Permissions to fetch web content
- **Custom Tools**: Any custom tools you've added to your environment

### 2. Permission Granularity

Permissions can be granted at different levels of granularity:

- **Category-wide**: Allow all tools in a category (e.g., all Edit operations)
- **Pattern-based**: Allow tools matching a pattern (e.g., `Bash(git *)` for all git commands)
- **Specific**: Allow a single specific tool (e.g., `Bash(npm install)`)

## Managing Permissions in Interactive Sessions

### Permission Prompts

When Claude attempts to use a tool that requires permission, you'll see a prompt like:

```
Claude wants to use: Edit(file: "app.js")
> Allow this time
> Always allow
> Deny
```

Your options are:

- **Allow this time**: Grant permission for this specific instance only
- **Always allow**: Grant permission for all future instances of this specific tool
- **Deny**: Deny permission for this specific instance

### Using the /permissions Command

You can view and manage permissions at any time with the `/permissions` command:

```
> /permissions
```

This opens the permissions management interface where you can:

- View currently allowed tools
- Add new allowed tools
- Remove existing allowed tools

To add a new allowed tool:

```
> /permissions add Edit
```

To remove an allowed tool:

```
> /permissions remove Bash(rm *)
```

To clear all permissions:

```
> /permissions clear
```

## Pre-approving Tools

### Via Settings Files

You can pre-approve tools by editing your settings files:

1. **Project-specific settings**: `.claude/settings.json` in your project directory
2. **Global settings**: `~/.claude.json` for settings that apply to all projects

Example settings.json:

```json
{
  "allowedTools": [
    "Edit",
    "Bash(git *)",
    "Bash(npm *)",
    "mcp__puppeteer__puppeteer_navigate",
    "mcp__puppeteer__puppeteer_screenshot"
  ]
}
```

### Via Command Line Arguments

You can pre-approve tools when starting Claude Code:

```bash
# Allow all Edit operations and git commands
claude --allowedTools "Edit,Bash(git *)"
```

### For Specific Commands

You can approve tools for a single command in headless mode:

```bash
# Allow npm commands for this specific invocation
claude -p "Update package dependencies" --allowedTools "Bash(npm *)"
```

## Permission Patterns and Syntax

### Basic Syntax

The basic syntax for tool permissions is:

```
ToolType(parameters)
```

Examples:
- `Edit` - All file editing operations
- `Bash(ls)` - The specific `ls` command
- `mcp__puppeteer__puppeteer_screenshot` - The screenshot tool from the Puppeteer MCP

### Using Wildcards

You can use wildcards to match multiple tools:

```
Bash(git *)      # All git commands
Edit(*.js)       # Edit operations on JavaScript files
mcp__puppeteer__* # All Puppeteer MCP tools
```

### Common Permission Patterns

Here are some useful permission patterns:

#### Development Workflow

```json
{
  "allowedTools": [
    "Edit",
    "Bash(git *)",
    "Bash(npm *)",
    "Bash(node *)"
  ]
}
```

#### Data Science Workflow

```json
{
  "allowedTools": [
    "Edit",
    "Bash(python *)",
    "Bash(pip *)",
    "Bash(jupyter *)"
  ]
}
```

#### DevOps Workflow

```json
{
  "allowedTools": [
    "Edit",
    "Bash(docker *)",
    "Bash(kubectl *)",
    "Bash(terraform *)"
  ]
}
```

## The --dangerously-skip-permissions Flag

For trusted environments, you can use the `--dangerously-skip-permissions` flag to bypass all permission checks:

```bash
claude --dangerously-skip-permissions
```

Or in headless mode:

```bash
claude -p "Fix all linting issues" --dangerously-skip-permissions
```

> **⚠️ Warning**: Only use this flag in controlled environments like containers without internet access or with limited filesystem access. This bypasses all security checks and could potentially allow Claude to execute any command.

### Safety Measures When Skipping Permissions

If you need to use `--dangerously-skip-permissions`, consider these safety measures:

1. **Run in a container**: Use Docker to isolate Claude Code
2. **Limit filesystem access**: Mount only necessary directories
3. **Disable network access**: Run with `--network none` in Docker
4. **Use read-only mounts**: Mount source code as read-only when possible
5. **Review commands first**: Use in two-step processes where you review before executing

Example Docker command with safety measures:

```bash
docker run -it --rm \
  --network none \
  -v $(pwd)/src:/app/src:ro \
  -v $(pwd)/docs:/app/docs:rw \
  --workdir /app \
  claude-code-image \
  claude -p "Generate documentation for src/" --dangerously-skip-permissions
```

## Permission Management Strategies

### 1. Progressive Permission Granting

Start with minimal permissions and add more as needed:

1. Begin with no pre-approved tools
2. Allow tools one by one as Claude requests them
3. Notice patterns in your workflow
4. Update your settings to pre-approve common tools

### 2. Role-Based Permissions

Create different permission sets for different roles or tasks:

```bash
# For development tasks
claude --allowedTools "Edit,Bash(npm *),Bash(git *)"

# For documentation tasks
claude --allowedTools "Edit(*.md),Bash(git *)"

# For deployment tasks
claude --allowedTools "Bash(docker *),Bash(kubectl *)"
```

### 3. Project-Specific Permissions

Customize permissions for each project by creating project-specific `.claude/settings.json` files.

Example for a React project:
```json
{
  "allowedTools": [
    "Edit",
    "Bash(npm *)",
    "Bash(git *)",
    "Bash(jest *)"
  ]
}
```

Example for a Python project:
```json
{
  "allowedTools": [
    "Edit",
    "Bash(python *)",
    "Bash(pip *)",
    "Bash(pytest *)"
  ]
}
```

## Auto-Accept Mode

For a smoother experience during focused sessions, you can toggle auto-accept mode:

- Press `Shift+Tab` to enable auto-accept mode
- Press `Shift+Tab` again to disable it

When auto-accept mode is enabled, Claude will automatically proceed with tool usage without asking for permission each time.

> **Note**: Auto-accept mode only applies to the current session and does not permanently allow tools.

## Viewing Permission History

To understand what tools Claude has used in your session:

```
> What tools have you used in this session?
```

Claude will provide a summary of the tools it has used and when.

## Permission Troubleshooting

### Common Permission Issues

#### Claude Keeps Asking for the Same Permission

**Problem**: Claude repeatedly asks for permission for the same tool.

**Solutions**:
- Use "Always allow" instead of "Allow this time"
- Add the tool to your settings file
- Start Claude with pre-approved tools via command line

#### Claude Can't Perform Expected Actions

**Problem**: Claude says it can't perform actions you expect it to be able to do.

**Solutions**:
- Check that you've granted the necessary permissions
- Verify that the tool pattern matches what Claude is trying to use
- Consider using broader patterns (e.g., `Bash(git *)` instead of `Bash(git commit)`)

#### Permissions Not Persisting Between Sessions

**Problem**: Permissions granted with "Always allow" don't persist to new sessions.

**Solutions**:
- Check that you're using the same working directory
- Add permissions to your settings file
- Use command line arguments to pre-approve tools

## Best Practices

### Security-Focused Approach

1. **Least Privilege**: Only grant permissions necessary for the current task
2. **Audit Regularly**: Review and clean up your allowed tools list periodically
3. **Avoid Overly Broad Patterns**: Use specific patterns when possible
4. **Sensitive Commands**: Be cautious with destructive commands (e.g., `rm -rf`)
5. **Network Access**: Be careful with tools that access external networks

### Productivity-Focused Approach

1. **Pre-approve Common Tools**: Add frequently used tools to your settings file
2. **Use Auto-Accept Mode**: Enable for focused work sessions
3. **Create Task-Specific Settings**: Maintain different settings for different types of work
4. **Document Permissions**: Add permission requirements to your CLAUDE.md file

## Example Permission Setups

### Web Development Setup

```json
{
  "allowedTools": [
    "Edit",
    "Bash(npm *)",
    "Bash(yarn *)",
    "Bash(node *)",
    "Bash(git *)",
    "Bash(jest *)",
    "Bash(eslint *)",
    "mcp__puppeteer__*"
  ]
}
```

### Data Science Setup

```json
{
  "allowedTools": [
    "Edit",
    "Bash(python *)",
    "Bash(pip *)",
    "Bash(conda *)",
    "Bash(jupyter *)",
    "Bash(pandas *)",
    "Bash(git *)",
    "Bash(pytest *)"
  ]
}
```

### DevOps Setup

```json
{
  "allowedTools": [
    "Edit",
    "Bash(git *)",
    "Bash(docker *)",
    "Bash(docker-compose *)",
    "Bash(kubectl *)",
    "Bash(terraform *)",
    "Bash(aws *)",
    "Bash(gcloud *)"
  ]
}
```

## Team Permission Management

For teams working with Claude Code, consider these approaches:

### Shared Settings in Version Control

Add a `.claude/settings.json` file to your repository with team-approved tools:

```json
{
  "allowedTools": [
    "Edit",
    "Bash(npm *)",
    "Bash(git *)",
    "Bash(jest *)"
  ]
}
```

### Personal Overrides

Team members can create personal overrides with `.claude/settings.local.json` (which should be in `.gitignore`):

```json
{
  "allowedTools": [
    "Bash(some-personal-tool *)"
  ]
}
```

### Permission Documentation

Document your team's permission policies in CLAUDE.md:

```markdown
# Tool Permissions

Claude has pre-approved permissions for:
- All file editing operations
- Git commands
- NPM commands
- Jest testing commands

For security reasons, please do not grant permissions for:
- Production deployment commands
- Database manipulation outside of local development
- Commands that modify system settings
```

## Conclusion

Effective permission management is key to using Claude Code productively while maintaining security. By understanding the permission model and implementing appropriate strategies, you can create a streamlined workflow where Claude can assist you effectively without unnecessary interruptions or security risks.

Remember that the best permission strategy depends on your specific needs, environment, and security requirements. Start with a conservative approach and adjust as you become more comfortable with Claude Code's capabilities.

## Additional Resources

- [Claude Code Documentation](https://docs.anthropic.com/claude/docs/claude-code)
- [Claude Code Best Practices](https://docs.anthropic.com/claude/docs/claude-code-best-practices)
- [Security Considerations](https://docs.anthropic.com/claude/docs/security-considerations)
- [Docker Integration Guide](../advanced-techniques/docker-integration.md)
- [Team Workflows](../best-practices/team-workflows.md)

---

Want to contribute your own permission management strategies to the Claude Code community? Submit them to the [Claude Code Hub](https://github.com/anthropics/claude-code-hub) repository!
