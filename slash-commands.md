# Mastering Slash Commands in Claude Code

Slash commands are powerful shortcuts that enhance your productivity when working with Claude Code. This comprehensive guide covers built-in slash commands, custom command creation, and advanced usage patterns to help you streamline your workflows.

## Introduction to Slash Commands

Slash commands are special directives triggered by typing a forward slash (`/`) in the Claude Code interface. They provide quick access to frequently used functions and custom workflows without requiring lengthy natural language instructions.

## Built-in Slash Commands

Claude Code comes with several built-in commands that provide immediate access to core functionality.

### Basic Commands

| Command | Description | Example Usage |
|---------|-------------|--------------|
| `/help` | Shows available commands and basic help | `/help` |
| `/clear` | Clears the conversation context | `/clear` |
| `/permissions` | Manages tool permissions | `/permissions` |
| `/mcp` | Lists connected MCP servers | `/mcp` |
| `/exit` | Exits Claude Code | `/exit` |

### File and Directory Commands

| Command | Description | Example Usage |
|---------|-------------|--------------|
| `/ls` | Lists files in the current directory | `/ls` |
| `/cd` | Changes the current directory | `/cd src/components` |
| `/find` | Searches for files matching a pattern | `/find "*.js"` |
| `/cat` | Displays the contents of a file | `/cat README.md` |

### Git Commands

| Command | Description | Example Usage |
|---------|-------------|--------------|
| `/git-status` | Shows the git status | `/git-status` |
| `/git-diff` | Shows git differences | `/git-diff` |
| `/git-log` | Shows git commit history | `/git-log` |
| `/git-branch` | Lists git branches | `/git-branch` |

### Utility Commands

| Command | Description | Example Usage |
|---------|-------------|--------------|
| `/init` | Initializes Claude Code in the current directory | `/init` |
| `/think` | Activates extended thinking mode | `/think` |
| `/version` | Shows Claude Code version | `/version` |
| `/memory` | Shows memory usage and management | `/memory` |

## Creating Custom Slash Commands

One of the most powerful features of Claude Code is the ability to create custom slash commands tailored to your specific workflows.

### Custom Command Directory Structure

Custom slash commands are stored in Markdown files within specific directories:

- **Project-specific commands**: `.claude/commands/` in your project directory
- **Global commands**: `~/.claude/commands/` for commands available in all projects

### Basic Custom Command Structure

Create a Markdown file in the appropriate commands directory:

```markdown
# File: .claude/commands/analyze-code.md

Analyze the code in $ARGUMENTS and provide:
1. A high-level overview of what the code does
2. Potential bugs or issues
3. Suggestions for improvement
4. Code quality assessment
```

You can then use this command with:

```
> /project:analyze-code src/components/Button.js
```

### Parameter Handling

The special `$ARGUMENTS` keyword in your command template is replaced with any text following the command:

```markdown
# File: .claude/commands/search-docs.md

Search the documentation for $ARGUMENTS and provide relevant information, 
examples, and usage guidelines.
```

Usage:
```
> /project:search-docs authentication methods
```

### Advanced Parameter Handling

For more complex commands, you can use positional parameters:

```markdown
# File: .claude/commands/compare-files.md

Compare the files $1 and $2, highlighting:
1. Structural differences
2. Functional differences
3. Potential improvements for both

First file: $1
Second file: $2
```

Usage:
```
> /project:compare-files src/v1/api.js src/v2/api.js
```

### Command Categories

Organize commands by creating subdirectories:

```
.claude/commands/
├── git/
│   ├── create-pr.md
│   └── review-pr.md
├── react/
│   ├── create-component.md
│   └── refactor-component.md
└── docs/
    ├── generate-readme.md
    └── update-docs.md
```

These commands are accessible via:
```
> /project:git:create-pr
> /project:react:create-component
> /project:docs:generate-readme
```

## Command Namespaces

Claude Code provides different namespaces for slash commands:

| Namespace | Prefix | Source |
|-----------|--------|--------|
| Built-in | `/` | Claude Code's built-in commands |
| Project | `/project:` | Commands in the current project's `.claude/commands/` |
| User | `/user:` | Commands in your `~/.claude/commands/` |
| MCP | `/mcp:` | Commands provided by connected MCP servers |

## Advanced Custom Command Techniques

### Command Templates with Variable Sections

Create flexible commands with conditional sections:

```markdown
# File: .claude/commands/review-code.md

Review the code in $ARGUMENTS with $DEPTH depth.

{{if $DEPTH == "deep"}}
Perform a comprehensive review including:
1. Algorithmic complexity analysis
2. Memory usage assessment
3. Security vulnerability scanning
4. Performance optimization opportunities
5. Thorough code quality evaluation
{{else}}
Perform a standard review including:
1. Code correctness
2. Basic style and formatting
3. Simple optimization opportunities
{{endif}}

Provide feedback in a clear, actionable format.
```

Usage:
```
> /project:review-code src/algorithm.js deep
```

### Environment-Aware Commands

Create commands that adapt to the current environment:

```markdown
# File: .claude/commands/setup-env.md

{{if $PROJECT_TYPE == "react"}}
Setup a React development environment with:
1. ESLint configuration for React
2. Jest and React Testing Library
3. Prettier configuration
4. Husky pre-commit hooks
{{elif $PROJECT_TYPE == "node"}}
Setup a Node.js development environment with:
1. ESLint configuration for Node.js
2. Mocha and Chai testing
3. Prettier configuration
4. Husky pre-commit hooks
{{else}}
Determine the project type and suggest an appropriate development environment setup.
{{endif}}
```

### Interactive Commands

Create commands that involve multiple steps and user interaction:

```markdown
# File: .claude/commands/create-feature.md

I'll help you create a new feature. Please answer these questions:

1. What's the feature name?
2. What problem does it solve?
3. Which existing components will it interact with?
4. Do you have any specific requirements or constraints?

Based on your answers, I'll:
1. Create a feature implementation plan
2. Design the necessary components
3. Implement the core functionality
4. Write tests to verify the feature works correctly
5. Document the feature for other developers

Let's proceed step by step.
```

### Tool-Integrated Commands

Create commands that leverage specific tools:

```markdown
# File: .claude/commands/analyze-performance.md

Analyze the performance of $ARGUMENTS using Lighthouse.

First, I'll run Lighthouse to get performance metrics:
1. Run Lighthouse audit on the specified URL or file
2. Analyze the results to identify performance bottlenecks
3. Suggest specific improvements to address each issue
4. Prioritize recommendations by impact and implementation effort

Let's begin the performance analysis.
```

## Creating Command Libraries

Organize related commands into libraries for specific domains or workflows:

### Example: React Component Library

```
.claude/commands/react/
├── create-component.md
├── refactor-component.md
├── add-tests.md
├── optimize-rendering.md
└── extract-hooks.md
```

### Example: Documentation Library

```
.claude/commands/docs/
├── generate-readme.md
├── create-api-docs.md
├── update-changelog.md
├── document-component.md
└── create-user-guide.md
```

### Example: Git Workflow Library

```
.claude/commands/git/
├── create-feature-branch.md
├── prepare-commit.md
├── create-pr.md
├── review-pr.md
└── merge-strategy.md
```

## Sharing Custom Commands

Share custom commands with your team by checking them into version control:

```bash
# Add all project commands to git
git add .claude/commands/

# Commit with a descriptive message
git commit -m "Add custom Claude Code commands for project workflows"

# Push to share with the team
git push
```

## Practical Command Examples

Here are some practical custom commands you can implement right away:

### Feature Implementation Command

```markdown
# File: .claude/commands/implement-feature.md

Implement the feature described in $ARGUMENTS.

Follow these steps:
1. Understand the requirements and break them down
2. Plan the implementation, identifying affected components
3. Write tests that define the expected behavior
4. Implement the feature with clean, maintainable code
5. Verify all tests pass
6. Document the changes for other developers

Start by exploring the codebase to understand the context, then proceed with implementation.
```

### Code Review Command

```markdown
# File: .claude/commands/review-pr.md

Review the Pull Request $ARGUMENTS with thorough attention to:

1. Code correctness and functionality
2. Test coverage and quality
3. Performance implications
4. Security considerations
5. Code style and consistency
6. Documentation completeness

Provide specific, actionable feedback with code examples where appropriate.
Use the GitHub CLI to fetch PR details if available.
```

### Project Starter Command

```markdown
# File: .claude/commands/starter-project.md

Create a starter project for $ARGUMENTS with the following components:

1. Project structure following best practices
2. Configuration files (linting, formatting, etc.)
3. Build system setup
4. Testing framework configuration
5. Documentation templates
6. Basic CI/CD workflow

Generate all necessary files and provide instructions for getting started.
```

### Debugging Command

```markdown
# File: .claude/commands/debug-issue.md

Help debug the issue described in $ARGUMENTS by:

1. Understanding the problem and expected behavior
2. Exploring potential causes
3. Devising a systematic debugging approach
4. Testing hypotheses to isolate the issue
5. Implementing and verifying a fix
6. Suggesting preventative measures for the future

Start by gathering more information about the issue, then proceed with debugging.
```

## Integrating with External Tools

Custom slash commands can be integrated with external tools via bash commands or MCP servers:

### Example: Lighthouse Integration

```markdown
# File: .claude/commands/lighthouse-audit.md

Run a Lighthouse audit on $ARGUMENTS and analyze the results.

I'll use the Lighthouse CLI to perform the audit and then analyze the output to provide actionable recommendations.
```

### Example: Database Query Command

```markdown
# File: .claude/commands/db-query.md

Execute the SQL query $ARGUMENTS on our database and analyze the results.

I'll connect to the database using the Database MCP, execute the query, and provide a detailed analysis of the results.
```

## Command Management

### Listing Available Commands

To see all available commands:

```
> /help
```

This shows built-in commands and custom commands from all namespaces.

### Updating Commands

To update a custom command, simply edit its Markdown file:

```bash
# Edit a project command
nano .claude/commands/my-command.md

# Edit a user command
nano ~/.claude/commands/my-command.md
```

### Removing Commands

To remove a custom command, delete its Markdown file:

```bash
# Remove a project command
rm .claude/commands/my-command.md

# Remove a user command
rm ~/.claude/commands/my-command.md
```

## Best Practices for Slash Commands

### Command Naming

- Use clear, descriptive names
- Follow a consistent naming convention
- Use kebab-case for multi-word commands (e.g., `analyze-code`)
- Group related commands with consistent prefixes

### Command Structure

- Start with a clear purpose statement
- Include detailed instructions for Claude
- Use numbered lists for step-by-step processes
- Keep commands focused on a single task or workflow
- Include examples where helpful

### Parameter Usage

- Use `$ARGUMENTS` for simple commands with a single input
- Use positional parameters (`$1`, `$2`, etc.) for commands with multiple inputs
- Document parameter expectations at the beginning of the command
- Provide default values or fallbacks when possible

### Documentation

- Include a brief description at the top of each command
- Document expected parameters and their formats
- Include examples of how to use the command
- Note any prerequisites or dependencies

## Advanced Command Patterns

### Workflow Automation Commands

Create commands that automate multi-step workflows:

```markdown
# File: .claude/commands/onboard-feature.md

Onboard the feature described in $ARGUMENTS through the entire development lifecycle:

1. Requirements analysis and breakdown
2. Architecture and design
3. Test case development
4. Implementation
5. Code review preparation
6. Documentation generation
7. Release planning

I'll guide you through each step, ensuring all aspects are properly addressed.
```

### Team Coordination Commands

Create commands that facilitate team coordination:

```markdown
# File: .claude/commands/sprint-planning.md

Facilitate sprint planning for the tasks in $ARGUMENTS:

1. Break down tasks into manageable subtasks
2. Estimate effort for each subtask
3. Identify dependencies between tasks
4. Suggest task assignments based on team member skills
5. Create a sprint timeline with milestones
6. Generate a summary for team communication

Let's organize these tasks for an efficient sprint.
```

### Learning and Exploration Commands

Create commands that help you learn and explore:

```markdown
# File: .claude/commands/explain-pattern.md

Explain the $ARGUMENTS design pattern with:

1. Core concept and purpose
2. Key components and their relationships
3. Implementation examples in our codebase
4. Advantages and disadvantages
5. Alternative patterns for comparison
6. Best practices for implementation

Include diagrams and code examples to illustrate the pattern.
```

## Conclusion

Slash commands in Claude Code are a powerful feature that can significantly enhance your productivity. By mastering built-in commands and creating custom commands tailored to your workflows, you can streamline your interactions with Claude Code and accomplish complex tasks more efficiently.

Start with the built-in commands, experiment with simple custom commands, and gradually build up a library of commands that support your specific development processes. As you become more comfortable with slash commands, you'll discover new ways to leverage them for even greater productivity.

## Additional Resources

- [Claude Code Documentation](https://docs.anthropic.com/claude/docs/claude-code)
- [Custom Command Examples Repository](https://github.com/anthropics/claude-code-commands)
- [Community Command Libraries](https://github.com/anthropics/claude-code-hub/community/contributions/commands)
- [MCP Server Documentation](https://docs.anthropic.com/claude/docs/mcp)

---

Want to contribute your own slash commands to the Claude Code community? Submit them to the [Claude Code Hub](https://github.com/anthropics/claude-code-hub) repository!
