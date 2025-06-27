# CLAUDE.md: The Practical Guide - Setup, Examples, and Best Practices

CLAUDE.md files are one of the most powerful features of Claude Code, serving as persistent memory and context for your coding sessions. This practical guide will help you create, optimize, and maintain effective CLAUDE.md files to maximize your productivity with Claude Code.

## What is a CLAUDE.md File?

A CLAUDE.md file is a special Markdown document that Claude automatically pulls into context when starting a conversation. Think of it as persistent memory that helps Claude understand your project, preferences, and working style.

When you run Claude Code in a directory containing a CLAUDE.md file, Claude reads this file and uses it to guide its responses and actions. This allows you to establish consistent context across sessions without having to repeat the same information each time.

## Why CLAUDE.md Files Matter

- **Consistent Context**: Maintains understanding across different sessions
- **Reduced Token Usage**: Pre-loads common information without consuming conversation tokens
- **Improved Accuracy**: Helps Claude better understand your specific project and preferences
- **Workflow Optimization**: Encodes your development practices and project conventions
- **Team Standardization**: Shares knowledge and conventions across a development team

## CLAUDE.md File Locations

Claude Code looks for CLAUDE.md files in several locations, in order of precedence:

1. **Project-specific**: `./CLAUDE.md` or `./CLAUDE.local.md` in your current directory
2. **Parent directories**: CLAUDE.md files in any parent of your current directory
3. **Child directories**: CLAUDE.md files in relevant subdirectories (loaded on demand)
4. **Global**: `~/.claude/CLAUDE.md` (applies to all Claude Code sessions)

> **Pro Tip**: Use `.gitignore` for `CLAUDE.local.md` files containing personal preferences, while committing shared `CLAUDE.md` files to benefit your entire team.

## Setting Up Your First CLAUDE.md File

### Method 1: Automatic Generation

The simplest way to create your first CLAUDE.md file is to let Claude generate one for you:

```bash
# Initialize Claude Code in your project directory
cd your-project
claude init
```

This will create a basic CLAUDE.md file tailored to your project based on what Claude can observe about your codebase.

### Method 2: Manual Creation

Alternatively, you can create a CLAUDE.md file manually:

```bash
# Create a new CLAUDE.md file
touch CLAUDE.md

# Open in your preferred editor
code CLAUDE.md  # for VS Code
```

Then add your content following the structure recommendations below.

## Essential CLAUDE.md Structure

An effective CLAUDE.md file typically includes these key sections:

### 1. Project Overview

Start with a concise description of your project:

```markdown
# Project Overview
This is a React-based e-commerce application with a Node.js backend and MongoDB database. It features user authentication, product catalog, shopping cart, and payment processing.

Main technologies:
- Frontend: React 18, TypeScript, Redux Toolkit
- Backend: Node.js, Express, MongoDB
- Testing: Jest, React Testing Library
- Deployment: Docker, AWS
```

### 2. Common Commands

Document frequently used commands:

```markdown
# Bash Commands
- `npm run dev`: Start development server on port 3000
- `npm run build`: Build production version
- `npm run test`: Run test suite
- `npm run lint`: Run ESLint
- `docker-compose up`: Start development environment with MongoDB
```

### 3. Code Style and Guidelines

Define your coding conventions:

```markdown
# Code Style
- Use functional components with hooks instead of class components
- Follow Airbnb ESLint config rules
- Use TypeScript interfaces for all data structures
- Write Jest tests for all new components
- Document public APIs with JSDoc comments

**IMPORTANT**: Always use ES modules (import/export) syntax, not CommonJS (require).
```

### 4. Important Files and Architecture

Highlight key files and architectural patterns:

```markdown
# Important Files
- `src/App.tsx`: Main application component
- `src/api/index.ts`: API client setup
- `src/components/`: Reusable UI components
- `src/pages/`: Page components
- `src/utils/`: Utility functions
- `src/hooks/`: Custom React hooks

# Architecture
- We use a feature-based folder structure
- State management via Redux with Redux Toolkit
- API calls centralized in api/ directory
- Authentication via JWT stored in HttpOnly cookies
```

### 5. Development Workflow

Explain your workflow preferences:

```markdown
# Development Workflow
- Create a feature branch from `main`
- Write tests before implementation (TDD approach)
- Submit PRs with concise descriptions
- Require at least one review before merging
- Squash commits when merging to main
```

### 6. Common Issues and Solutions

Document recurring problems:

```markdown
# Common Issues
- If `npm run dev` fails, check if port 3000 is already in use
- MongoDB connection issues usually mean the container isn't running
- TypeScript errors often require running `npm install` to update types
```

## CLAUDE.md Templates for Different Project Types

### React Application Template

```markdown
# React Project: [Project Name]

## Project Overview
[Brief description of the project]

## Commands
- `npm start`: Run development server
- `npm test`: Run test suite
- `npm run build`: Build for production
- `npm run lint`: Run ESLint
- `npm run format`: Run Prettier

## Component Structure
- Use functional components with hooks
- Follow the Container/Presentation pattern
- Keep components small and focused
- Extract reusable logic into custom hooks

## State Management
- Use Context API for global state
- Use useState for component-local state
- Consider Redux only for complex state requirements

## File Organization
- Group by feature, not by type
- Keep related files close together
- Use index.js files for clean imports

## Testing
- Test components with React Testing Library
- Aim for behavioral tests, not implementation details
- Mock external dependencies

## Styling
- Use CSS Modules or styled-components
- Follow our design system guidelines
- Support responsive design for all components
```

### Node.js Backend Template

```markdown
# Node.js API Project: [Project Name]

## Project Overview
[Brief description of the project]

## Commands
- `npm run dev`: Start development server with hot reload
- `npm start`: Start production server
- `npm test`: Run test suite
- `npm run migrate`: Run database migrations

## Project Structure
- `src/controllers/`: Route handlers
- `src/models/`: Database models
- `src/middlewares/`: Express middlewares
- `src/services/`: Business logic
- `src/utils/`: Helper functions
- `src/routes/`: API route definitions

## Coding Standards
- Use async/await for asynchronous operations
- Handle errors with try/catch blocks
- Validate all request inputs
- Log meaningful information for debugging

## Database Practices
- Use an ORM (Sequelize/Prisma/Mongoose)
- Write migrations for schema changes
- Keep indices on frequently queried fields
- Use transactions for related operations

## API Design
- Follow RESTful principles
- Version your API endpoints
- Use proper HTTP status codes
- Document with Swagger/OpenAPI
```

### Python Data Science Template

```markdown
# Python Data Science Project: [Project Name]

## Environment
- `conda activate projectenv`: Activate project environment
- `python -m jupyter notebook`: Start Jupyter notebook
- `python run_analysis.py`: Run main analysis script
- `pytest`: Run test suite

## Project Structure
- `data/`: Raw and processed datasets
- `notebooks/`: Jupyter notebooks
- `src/`: Python modules and packages
- `tests/`: Test files
- `results/`: Output figures and data

## Best Practices
- Use virtual environments
- Keep raw data immutable
- Document data preprocessing steps
- Version control large files with DVC
- Follow PEP 8 style guidelines

## Analysis Workflow
- Data acquisition → Cleaning → Exploration → Modeling → Evaluation → Visualization
- Document assumptions and decisions
- Make analyses reproducible
- Save intermediate results

## Visualization Standards
- Label axes clearly
- Include units of measurement
- Use colorblind-friendly palettes
- Add descriptive titles and captions
```

## Advanced CLAUDE.md Optimization Techniques

### Emphasize Critical Instructions

Use emphasis to highlight critical instructions that Claude should follow:

```markdown
**IMPORTANT**: Always use TypeScript interfaces for new data structures.

**CRITICAL**: Never modify the database schema directly. Use migrations.
```

### Provide Context-Specific Instructions

Add instructions for specific types of tasks:

```markdown
# When Implementing New Features
- Create a new branch from main
- Write tests first (TDD approach)
- Update documentation alongside code
- Consider accessibility requirements
- Add appropriate error handling

# When Fixing Bugs
- Verify the bug with a failing test
- Look for similar patterns elsewhere in the codebase
- Consider root causes, not just symptoms
- Add regression tests
- Document the fix in the commit message
```

### Include Visual Aids

Add ASCII diagrams to explain architecture or workflows:

```markdown
# Architecture Diagram
```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   React UI  │────▶│  API Layer  │────▶│   Database  │
└─────────────┘     └─────────────┘     └─────────────┘
       ▲                   ▲                   ▲
       │                   │                   │
       └───────────────────┴───────────────────┘
                  Two-way data flow
```

### Domain-Specific Knowledge

Include specialized knowledge relevant to your project:

```markdown
# Domain Knowledge
- User roles: Admin, Manager, Customer
- Order statuses: Pending, Processing, Shipped, Delivered, Canceled
- Payment methods: Credit Card, PayPal, Store Credit
```

### Custom Instructions for Claude

Add specific instructions for how Claude should work with your project:

```markdown
# Instructions for Claude
- Please suggest test cases when implementing new features
- Always check for potential security issues in API endpoints
- When editing CSS, maintain consistency with our design system
- Always consider mobile responsiveness when modifying UI components
```

## Real-World CLAUDE.md Examples

### E-Commerce Application Example

```markdown
# E-Commerce Platform

## Core Concepts
- Products have variants (size, color, etc.)
- Inventory is tracked at the variant level
- Orders go through states: Created → Paid → Fulfilled → Delivered
- Users can have roles: Customer, Vendor, Admin

## Key Files
- `src/modules/cart/CartProvider.tsx`: Shopping cart context provider
- `src/modules/checkout/CheckoutFlow.tsx`: Checkout workflow
- `src/api/orders.ts`: Order management API
- `src/hooks/useInventory.ts`: Inventory management hook

## Development Notes
- API endpoints are mocked in dev mode using MSW
- Run `npm run seed` to populate database with test data
- Payment processing uses Stripe in test mode
```

### Internal Dashboard Example

```markdown
# Internal Analytics Dashboard

## Project Purpose
This tool aggregates data from multiple internal systems to provide real-time insights for the customer support team.

## Authentication
- Uses company SSO for authentication
- Role-based access control
- Session timeouts after 2 hours of inactivity

## Data Sources
- CRM: customer information (Salesforce)
- Support tickets: Zendesk API
- Product usage: Internal data warehouse (BigQuery)
- Billing information: Stripe API

## Refresh Patterns
- CRM data: daily sync
- Support tickets: real-time via webhooks
- Usage data: hourly batch updates
- Billing data: daily sync

## Known Issues
- Zendesk API rate limits can cause temporary data gaps
- Charts may not render correctly in Internet Explorer
```

## Dynamic CLAUDE.md Management

### Adding Content During Sessions

Press the `#` key during a Claude Code session to add information to CLAUDE.md:

```
# Please remember that we use Prettier for code formatting with a 100-character line length limit
```

Claude will automatically incorporate this into the relevant CLAUDE.md file.

### Updating CLAUDE.md Content

You can update your CLAUDE.md file directly or use Claude to help you improve it:

```
> Help me improve my CLAUDE.md file to better describe our project architecture
```

Claude will analyze your existing CLAUDE.md file and suggest improvements.

### Multi-Level CLAUDE.md Strategy

For complex projects, consider a hierarchical approach:

1. **Root CLAUDE.md**: Project overview, architecture, global conventions
2. **Service-specific CLAUDE.md files**: In each microservice or module directory
3. **Feature-specific CLAUDE.md files**: In specialized feature directories

For example:

```
/project/CLAUDE.md           # Global project information
/project/frontend/CLAUDE.md  # Frontend-specific information
/project/backend/CLAUDE.md   # Backend-specific information
/project/docs/CLAUDE.md      # Documentation guidelines
```

## Best Practices for CLAUDE.md Files

### 1. Keep It Focused

Focus on information that will help Claude assist you more effectively:
- Project-specific context
- Workflow preferences
- Important file locations
- Common commands and patterns

Avoid general information that Claude already knows, like basic language syntax or standard libraries.

### 2. Be Specific and Clear

Use clear, specific language and examples:

```markdown
# Good
When creating new React components:
- Place them in `src/components/{feature-name}/`
- Use the `.tsx` extension
- Follow the naming pattern `{ComponentName}.tsx`
- Create a matching test file `{ComponentName}.test.tsx`

# Not as helpful
Put components in the right folder and use TypeScript.
```

### 3. Prioritize Important Information

Place the most critical information at the beginning of your CLAUDE.md file:

```markdown
# MOST IMPORTANT GUIDELINES
1. Never commit directly to main
2. Always write tests for new features
3. Follow the established coding style

[Detailed project information follows...]
```

### 4. Use Markdown Effectively

Take advantage of Markdown formatting to organize information:
- Use headers for different sections
- Use lists for steps or related items
- Use code blocks for commands and code examples
- Use emphasis for important points
- Use tables for structured data

### 5. Keep It Up to Date

Regularly update your CLAUDE.md file as your project evolves:
- Remove outdated information
- Add new patterns and conventions
- Update command examples
- Reflect architectural changes

### 6. Collaborate on CLAUDE.md Files

For team projects:
- Review CLAUDE.md changes during code reviews
- Document major updates in commit messages
- Consider CLAUDE.md changes as part of your project documentation

## Troubleshooting CLAUDE.md Issues

### Claude Isn't Using Information from CLAUDE.md

If Claude doesn't seem to be using information from your CLAUDE.md file:

1. **Check file location**: Ensure the CLAUDE.md file is in the correct directory
2. **Verify file name**: The file should be named exactly `CLAUDE.md` (case-sensitive)
3. **Check file format**: Ensure it's a plain text file with Markdown formatting
4. **Restart Claude Code**: Sometimes a fresh session helps Claude pick up changes
5. **Be explicit**: You can always directly refer Claude to information in the CLAUDE.md file

### Claude Is Using Outdated Information

If Claude is using outdated information:

1. **Check for multiple CLAUDE.md files**: There might be conflicting files in different locations
2. **Clear context**: Use the `/clear` command to reset the conversation context
3. **Update file**: Make sure you've saved your changes to the CLAUDE.md file
4. **Restart Claude Code**: Start a fresh session to pick up the latest changes

### CLAUDE.md File Is Too Large

If your CLAUDE.md file has grown too large:

1. **Focus on essentials**: Remove non-critical information
2. **Split into multiple files**: Use the hierarchical approach with separate CLAUDE.md files
3. **Use links**: Reference external documentation for detailed information
4. **Prioritize**: Keep the most important information at the top

## Advanced Usage Patterns

### Role-Specific CLAUDE.md Files

Create CLAUDE.md files tailored to different roles:

```bash
# For development work
cp CLAUDE.md CLAUDE.dev.md
# Edit for development focus

# For documentation work
cp CLAUDE.md CLAUDE.docs.md
# Edit for documentation focus

# Use the appropriate file
claude --claude-md CLAUDE.dev.md
# or
claude --claude-md CLAUDE.docs.md
```

### Temporary Context Injection

Create temporary CLAUDE.md files for specific tasks:

```bash
# Create a task-specific CLAUDE.md
cat CLAUDE.md > CLAUDE.temp.md
echo "# Current Task: Refactoring authentication system" >> CLAUDE.temp.md
echo "Focus on improving security and performance" >> CLAUDE.temp.md

# Use the temporary file
claude --claude-md CLAUDE.temp.md
```

### Context Layering with Multiple Files

Use multiple CLAUDE.md files simultaneously:

```bash
# Use multiple context files
claude --claude-md CLAUDE.md,CLAUDE.architecture.md,CLAUDE.current-sprint.md
```

## Integrating CLAUDE.md with Development Workflows

### Version Control Integration

Commit your CLAUDE.md files to version control:

```bash
# Add to git
git add CLAUDE.md

# Commit with meaningful message
git commit -m "Update CLAUDE.md with new database schema info"
```

### Automated Updates

Set up automation to keep CLAUDE.md files updated:

```bash
# Example script to update commands in CLAUDE.md
#!/bin/bash
# update-claude-md.sh

# Extract commands from package.json
COMMANDS=$(jq -r '.scripts | to_entries | map("- `npm run \(.key)`: \(.value)") | .[]' package.json)

# Update the Commands section in CLAUDE.md
sed -i '/^# Commands/,/^# /c\# Commands\n'"$COMMANDS"'\n\n# ' CLAUDE.md
```

### CI/CD Pipeline Integration

Include CLAUDE.md validation in your CI/CD pipeline:

```yaml
# GitHub Actions example
jobs:
  validate-claude-md:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Validate CLAUDE.md
        run: |
          # Check for required sections
          grep -q "# Project Overview" CLAUDE.md || exit 1
          grep -q "# Commands" CLAUDE.md || exit 1
          # Add more validation as needed
```

## Conclusion

CLAUDE.md files are a powerful feature that can significantly enhance your experience with Claude Code. By creating well-structured, informative CLAUDE.md files, you provide Claude with the context it needs to assist you more effectively.

Start with a basic CLAUDE.md file and iteratively improve it as you work with Claude Code. Pay attention to what information is most helpful and focus on providing that context. Over time, your CLAUDE.md files will evolve to become an invaluable part of your development workflow.

Remember that the goal of a CLAUDE.md file is to help Claude understand your project and preferences, so it can provide more relevant and helpful assistance. With well-crafted CLAUDE.md files, you'll get more accurate, consistent, and context-aware help from Claude Code.

## Additional Resources

- [Claude Code Documentation](https://docs.anthropic.com/claude/docs/claude-code)
- [Claude Code Best Practices](https://docs.anthropic.com/claude/docs/claude-code-best-practices)
- [CLAUDE.md Templates Repository](https://github.com/anthropics/claude-code-hub/templates/claude-md)
- [Community CLAUDE.md Examples](https://github.com/anthropics/claude-code-hub/community/contributions/claude-md)

---

Want to contribute your own CLAUDE.md templates or examples? Submit them to the [Claude Code Hub](https://github.com/anthropics/claude-code-hub) repository!
