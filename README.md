#  Claude Code Hub

<div align="center">

  <h3>A comprehensive resource for mastering agentic coding with Claude Code</h3>
  

</div>


##  Overview

Welcome to the Claude Code Hub! This repository serves as an early resource for working with Claude Code, Anthropic's command-line tool for agentic coding. Whether you're a beginner just getting started or an advanced user looking to optimize your workflow, you'll find valuable resources, guides, and examples here.

Claude Code is a powerful tool that allows Claude to write, modify, and interact with code in your local environment. This hub provides everything you need to leverage Claude Code effectively in your development workflow.

>  **Research Preview**: Claude Code is currently in research preview. This hub is maintained by the community with guidance from Anthropic.

##  What's Inside

- **[ Documentation](#documentation)**: Comprehensive guides and tutorials
- **[ Prompt Library](#prompt-library)**: Curated, categorized prompts for different use cases
- **[ Tools & Utilities](#tools--utilities)**: Helper scripts and extensions
- **[ MCP Integrations](#mcp-integrations)**: Model-Computer-Programmer server examples
- **[ Community Resources](#community-resources)**: User contributions and showcases

##  Quick Start

### Prerequisites

- Claude Pro or Claude Team subscription
- macOS, Linux, or Windows (via WSL)
- Node.js 18+

### Installation

```bash
# Install Claude Code
curl -fsSL https://claude.ai/code/install | bash

# Verify installation
claude --version
```

### First Steps

1. Navigate to your project:
   ```bash
   cd your-project
   ```

2. Initialize Claude Code:
   ```bash
   claude init
   ```

3. Start Claude Code:
   ```bash
   claude
   ```

4. Try a basic command:
   ```
   > help me understand this codebase
   ```

For more detailed instructions, check out our [Getting Started Guide](docs/getting-started/README.md).

##  Documentation

Our documentation is organized by experience level and use case:

### Getting Started
- [Installation Guide](docs/getting-started/installation.md)
- [Quick Start Tutorial](docs/getting-started/quick-start.md)
- [Understanding the Interface](docs/getting-started/interface.md)
- [Command Reference](docs/getting-started/commands.md)

### Best Practices
- [Customizing Your Environment](docs/best-practices/environment.md)
- [Optimizing CLAUDE.md Files](docs/best-practices/claude-md.md)
- [Managing Tool Permissions](docs/best-practices/permissions.md)
- [Effective Prompting Strategies](docs/best-practices/prompting.md)
- [Common Workflow Patterns](docs/best-practices/workflows.md)

### Advanced Techniques
- [Headless Mode for Automation](docs/advanced-techniques/headless.md)
- [Multi-Claude Workflows](docs/advanced-techniques/multi-claude.md)
- [Git Worktrees](docs/advanced-techniques/git-worktrees.md)
- [Customizing Slash Commands](docs/advanced-techniques/slash-commands.md)
- [MCP Integration](docs/advanced-techniques/mcp.md)

##  Prompt Library

Our prompt library contains carefully crafted prompts for various coding tasks:

- [Development Prompts](prompts/development/README.md)
- [Refactoring Prompts](prompts/refactoring/README.md)
- [Debugging Prompts](prompts/debugging/README.md)
- [Documentation Prompts](prompts/documentation/README.md)
- [Testing Prompts](prompts/testing/README.md)
- [Automation Prompts](prompts/automation/README.md)

Each prompt includes:
- Description of purpose
- The prompt template
- Example usage
- Expected outcome
- Tips for customization

##  Tools & Utilities

- [VS Code Extension](tools/extensions/vscode/README.md)
- [Shell Integrations](tools/shell/README.md)
- [Custom Bash Scripts](tools/scripts/README.md)
- [Docker Configurations](tools/docker/README.md)
- [CI/CD Integration Examples](tools/ci-cd/README.md)

##  MCP Integrations

Model-Computer-Programmer (MCP) servers extend Claude Code's capabilities:

- [Puppeteer MCP for Web Automation](mcp/servers/puppeteer/README.md)
- [iOS Simulator MCP](mcp/servers/ios-simulator/README.md)
- [Database MCP](mcp/servers/database/README.md)
- [Creating Custom MCPs](mcp/custom/README.md)

##  Community Resources

- [Showcase: Success Stories](community/showcase/README.md)
- [User Contributions](community/contributions/README.md)
- [Discussions](https://github.com/anthropics/claude-code-hub/discussions)

### Upvoting System

Our repository uses GitHub reactions for upvoting community contributions. The most popular resources will be featured in our curated collections.

##  Contributing

We welcome contributions from the community! Check out our [Contribution Guidelines](CONTRIBUTING.md) to get started.

Ways to contribute:
- Add new prompts to the library
- Improve documentation
- Share your custom tools or MCP servers
- Report bugs or suggest enhancements

##  Stay Updated

- [Star this repository](https://github.com/anthropics/claude-code-hub) to show your support
- [Watch this repository](https://github.com/anthropics/claude-code-hub/subscription) for updates
- Follow [@AnthropicAI](https://twitter.com/AnthropicAI) on Twitter

##  License

This repository is licensed under the [MIT License](LICENSE).

##  Acknowledgements

Special thanks to the Anthropic team and the Claude Code user community for their valuable insights and contributions that have helped shape these resources.

