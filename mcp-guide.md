# Complete Guide to MCP Servers with Claude Code

Model-Computer-Programmer (MCP) servers extend Claude Code's capabilities by providing specialized tools and interfaces. This guide will help you understand, set up, and effectively use MCP servers with Claude Code.

## What are MCP Servers?

MCP servers act as extensions to Claude Code, providing additional tools and capabilities beyond what's available in the base environment. They allow Claude to:

- Control web browsers and automate web interactions
- Interact with databases and APIs
- Manipulate images and multimedia
- Work with mobile simulators
- Access specialized development tools
- Visualize data and results

Think of MCP servers as plugins that give Claude Code new superpowers for specific tasks.

## Core MCP Concepts

- **Server**: A program that provides tools to Claude Code
- **Tool**: A specific function provided by an MCP server (e.g., navigate a webpage)
- **Client**: Claude Code itself, which connects to MCP servers
- **Permission**: Authorization for Claude to use specific MCP tools

## Popular MCP Servers

### 1. Puppeteer MCP

The Puppeteer MCP server allows Claude to control a headless Chrome browser, enabling web automation, testing, and screenshot capabilities.

**Key capabilities:**
- Navigate to URLs
- Fill out forms
- Click elements
- Take screenshots
- Extract data from web pages
- Run JavaScript in the browser context

### 2. iOS Simulator MCP

The iOS Simulator MCP connects Claude to the iOS Simulator, allowing interaction with iOS applications.

**Key capabilities:**
- Launch iOS apps
- Tap, swipe, and interact with UI elements
- Take screenshots
- Read app state and content
- Automate iOS app testing

### 3. Database MCP

Database MCP servers allow Claude to interact with various database systems.

**Key capabilities:**
- Execute SQL queries
- Modify database schema
- Analyze data
- Visualize query results
- Manage database connections

### 4. Custom MCPs

You can create custom MCP servers for specialized tasks specific to your workflow or domain.

## Setting Up MCP Servers

### Installation Methods

#### 1. Using npm (for JavaScript-based MCPs)

```bash
# Install Puppeteer MCP globally
npm install -g @anthropic/mcp-puppeteer

# Start the server
mcp-puppeteer
```

#### 2. Using pip (for Python-based MCPs)

```bash
# Install a Python-based MCP
pip install anthropic-mcp-database

# Start the server
mcp-database
```

#### 3. Using Docker

```bash
# Pull and run a Docker image for an MCP server
docker pull anthropic/mcp-puppeteer
docker run -p 3000:3000 anthropic/mcp-puppeteer
```

### Configuration Options

Most MCP servers accept configuration options either via command line arguments or configuration files:

```bash
# Example: Running Puppeteer MCP with custom options
mcp-puppeteer --port 3001 --headless false
```

### Persistent Configuration

For project-specific MCP configuration, create an `.mcp.json` file in your project root:

```json
{
  "servers": [
    {
      "type": "puppeteer",
      "url": "http://localhost:3000",
      "name": "Web Automation"
    },
    {
      "type": "database",
      "url": "http://localhost:3001",
      "name": "SQL Database"
    }
  ]
}
```

For global configuration, create a `~/.claude/mcp.json` file with similar content.

## Connecting Claude Code to MCP Servers

### Automatic Discovery

Claude Code automatically discovers running MCP servers on startup:

```bash
# Start Claude Code
claude

# You should see a message like:
# ✓ Found 1 MCP server • /mcp
```

### Manual Connection

You can specify MCP servers to connect to using command-line arguments:

```bash
# Connect to specific MCP servers
claude --mcp http://localhost:3000,http://localhost:3001
```

### Checking Connected Servers

Once in Claude Code, you can check connected servers:

```
> /mcp

Available MCP servers:
- Puppeteer (http://localhost:3000)
- Database (http://localhost:3001)
```

## Managing MCP Permissions

By default, Claude Code asks for permission before using MCP tools. You can manage these permissions in several ways:

### 1. During Sessions

When Claude requests to use an MCP tool, you can select "Always allow" to permit future uses.

### 2. Using the /permissions Command

```
> /permissions
```

This opens the permissions management interface where you can add or remove allowed MCP tools.

### 3. Editing Settings Files

Edit `.claude/settings.json` in your project directory or `~/.claude.json` globally to pre-approve MCP tools:

```json
{
  "allowedTools": [
    "mcp__puppeteer__puppeteer_navigate",
    "mcp__puppeteer__puppeteer_screenshot"
  ]
}
```

### 4. Command Line Flag

```bash
# Start Claude with pre-approved MCP tools
claude --allowedTools "mcp__puppeteer__puppeteer_navigate,mcp__puppeteer__puppeteer_screenshot"
```

## Using MCP Servers Effectively

### Puppeteer MCP Examples

#### Web Automation

```
> Use Puppeteer to go to github.com and take a screenshot of the homepage

I'll use Puppeteer to navigate to GitHub and take a screenshot of the homepage.

Call(mcp__puppeteer__puppeteer_navigate(url: "https://github.com"))...
✓ Navigated to https://github.com

Call(mcp__puppeteer__puppeteer_screenshot(selector: "body"))...
✓ Screenshot taken

[Screenshot of GitHub homepage appears]

I've successfully navigated to GitHub and taken a screenshot of the homepage. Is there anything specific you'd like me to do on this page?
```

#### Web Scraping

```
> Use Puppeteer to find all repository names on my GitHub profile page

First, I'll navigate to your GitHub profile page and extract the repository names.

Call(mcp__puppeteer__puppeteer_navigate(url: "https://github.com/yourusername"))...
✓ Navigated to https://github.com/yourusername

Call(mcp__puppeteer__puppeteer_evaluate(script: `
  const repoElements = document.querySelectorAll('[itemprop="name codeRepository"]');
  return Array.from(repoElements).map(el => el.textContent.trim());
`))...
✓ Evaluated script

Results:
- claude-code-examples
- web-automation-tools
- data-visualization-library
- react-component-library
- python-utilities

I've found 5 repositories on your GitHub profile. Would you like me to get more details about any of these?
```

#### Form Automation

```
> Use Puppeteer to fill out and submit the contact form at example.com/contact

I'll navigate to the contact form and fill it out for you.

Call(mcp__puppeteer__puppeteer_navigate(url: "https://example.com/contact"))...
✓ Navigated to https://example.com/contact

Call(mcp__puppeteer__puppeteer_screenshot(selector: "form"))...
✓ Screenshot taken

[Screenshot of contact form appears]

Call(mcp__puppeteer__puppeteer_type(selector: "input[name='name']", text: "John Doe"))...
✓ Typed text

Call(mcp__puppeteer__puppeteer_type(selector: "input[name='email']", text: "john@example.com"))...
✓ Typed text

Call(mcp__puppeteer__puppeteer_type(selector: "textarea[name='message']", text: "This is a test message sent via Claude Code using Puppeteer MCP."))...
✓ Typed text

Call(mcp__puppeteer__puppeteer_click(selector: "button[type='submit']"))...
✓ Clicked element

Call(mcp__puppeteer__puppeteer_waitForNavigation(timeout: 5000))...
✓ Navigation complete

Call(mcp__puppeteer__puppeteer_screenshot(selector: "body"))...
✓ Screenshot taken

[Screenshot of submission confirmation page appears]

I've successfully filled out and submitted the contact form. The confirmation page is shown in the screenshot above.
```

### Database MCP Examples

#### Querying Data

```
> Use the Database MCP to query all users from our MySQL database

I'll connect to the MySQL database and retrieve all users.

Call(mcp__database__connect(type: "mysql", config: { host: "localhost", user: "root", password: "password", database: "myapp" }))...
✓ Connected to database

Call(mcp__database__query(sql: "SELECT * FROM users LIMIT 10"))...
✓ Query executed

Results:
| id | name        | email               | created_at          |
|----|-------------|---------------------|---------------------|
| 1  | Alice Smith | alice@example.com   | 2023-01-15 14:30:00 |
| 2  | Bob Johnson | bob@example.com     | 2023-01-16 09:45:00 |
| 3  | Carol White | carol@example.com   | 2023-01-16 16:20:00 |
| 4  | Dave Brown  | dave@example.com    | 2023-01-17 11:10:00 |
| 5  | Eve Davis   | eve@example.com     | 2023-01-18 08:55:00 |

I've retrieved the first 10 users from the database. Would you like me to modify this query to filter the results or retrieve more information?
```

#### Schema Analysis

```
> Use Database MCP to analyze our PostgreSQL database schema

I'll connect to the PostgreSQL database and analyze its schema.

Call(mcp__database__connect(type: "postgres", config: { host: "localhost", user: "postgres", password: "password", database: "myapp" }))...
✓ Connected to database

Call(mcp__database__query(sql: "SELECT table_name FROM information_schema.tables WHERE table_schema = 'public'"))...
✓ Query executed

Tables in database:
- users
- products
- orders
- order_items
- categories
- reviews

Let me examine the schema of each table:

Call(mcp__database__query(sql: "SELECT column_name, data_type, is_nullable FROM information_schema.columns WHERE table_name = 'users'"))...
✓ Query executed

Users table:
| column_name    | data_type                | is_nullable |
|----------------|--------------------------|-------------|
| id             | integer                  | NO          |
| name           | character varying(255)   | NO          |
| email          | character varying(255)   | NO          |
| password_hash  | character varying(255)   | NO          |
| created_at     | timestamp with time zone | NO          |
| updated_at     | timestamp with time zone | NO          |
| last_login_at  | timestamp with time zone | YES         |
| is_admin       | boolean                  | NO          |

[Continues with schema for other tables...]

Based on my analysis, this appears to be an e-commerce database with users, products, orders, and reviews. The schema follows good practices with proper foreign key relationships between tables.
```

### iOS Simulator MCP Examples

#### App Testing

```
> Use iOS Simulator to launch our app and test the login flow

I'll launch the app in the iOS Simulator and test the login flow.

Call(mcp__ios_simulator__launch_app(bundle_id: "com.example.myapp"))...
✓ App launched

Call(mcp__ios_simulator__screenshot())...
✓ Screenshot taken

[Screenshot of app home screen appears]

I can see the app has launched and is showing the welcome screen. Now I'll tap the login button and enter credentials.

Call(mcp__ios_simulator__tap(x: 200, y: 500))...
✓ Tapped screen at coordinates

Call(mcp__ios_simulator__screenshot())...
✓ Screenshot taken

[Screenshot of login screen appears]

Now I'll enter the test credentials.

Call(mcp__ios_simulator__tap(selector: "email_field"))...
✓ Tapped element

Call(mcp__ios_simulator__type(text: "test@example.com"))...
✓ Typed text

Call(mcp__ios_simulator__tap(selector: "password_field"))...
✓ Tapped element

Call(mcp__ios_simulator__type(text: "password123"))...
✓ Typed text

Call(mcp__ios_simulator__tap(selector: "login_button"))...
✓ Tapped element

Call(mcp__ios_simulator__wait(seconds: 2))...
✓ Waited for 2 seconds

Call(mcp__ios_simulator__screenshot())...
✓ Screenshot taken

[Screenshot of logged-in dashboard appears]

I've successfully tested the login flow. The app accepted the credentials and navigated to the user dashboard as expected.
```

## Creating Custom MCP Servers

You can create custom MCP servers for specialized tasks. Here's a basic template for creating one:

### Basic Structure

A simple MCP server using Node.js:

```javascript
// simple-mcp.js
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
const port = 3000;

app.use(bodyParser.json());

// Register available tools
const tools = {
  'custom_tool': async (params) => {
    // Tool implementation
    const { input } = params;
    return { result: `Processed: ${input}` };
  },
  'another_tool': async (params) => {
    // Another tool implementation
    return { result: 'Done' };
  }
};

// MCP endpoint for tool execution
app.post('/api/v1/mcp', async (req, res) => {
  const { tool, params } = req.body;
  
  if (!tools[tool]) {
    return res.status(404).json({ error: `Tool '${tool}' not found` });
  }
  
  try {
    const result = await tools[tool](params);
    res.json({ success: true, result });
  } catch (error) {
    res.status(500).json({ success: false, error: error.message });
  }
});

// MCP discovery endpoint
app.get('/api/v1/mcp', (req, res) => {
  res.json({
    name: 'Custom MCP',
    version: '1.0.0',
    tools: Object.keys(tools).map(tool => ({
      name: tool,
      description: `Custom tool: ${tool}`
    }))
  });
});

app.listen(port, () => {
  console.log(`Custom MCP server running on port ${port}`);
});
```

### Running Your Custom MCP

```bash
# Install dependencies
npm install express body-parser

# Run the server
node simple-mcp.js
```

### Using Your Custom MCP with Claude Code

Once your custom MCP server is running, you can connect Claude Code to it:

```bash
# Start Claude Code with your custom MCP
claude --mcp http://localhost:3000
```

Then, in your Claude Code session:

```
> Use the custom tool with input "Hello, MCP!"

I'll use the custom tool from our MCP server.

Call(mcp__Custom MCP__custom_tool(input: "Hello, MCP!"))...
✓ Tool executed

Result: Processed: Hello, MCP!

The custom tool processed your input successfully.
```

## Advanced MCP Usage Patterns

### 1. Multi-MCP Workflows

Combine multiple MCP servers for complex workflows:

```
> Use Puppeteer to scrape product data from our website, then use the Database MCP to store it

I'll scrape product data and store it in the database.

[Puppeteer commands to navigate and extract data]

[Database commands to store the extracted data]

I've successfully scraped 15 products from the website and stored them in the database.
```

### 2. MCP with Custom Slash Commands

Create custom slash commands that leverage MCP capabilities:

```markdown
# In .claude/commands/take-screenshot.md

Use Puppeteer to navigate to $ARGUMENTS and take a screenshot of the entire page.
```

Then use it in Claude Code:

```
> /project:take-screenshot https://example.com

Navigating to https://example.com and taking a screenshot...

[Puppeteer commands execute]

[Screenshot appears]

Screenshot captured successfully.
```

### 3. Headless Mode with MCPs

Use Claude Code in headless mode with MCPs for automation:

```bash
# Run a headless Claude Code session that uses Puppeteer MCP
claude -p "Navigate to our company website, take screenshots of all product pages, and save them to the 'screenshots' directory" --allowedTools "mcp__puppeteer__*" --mcp http://localhost:3000
```

### 4. MCP in CI/CD Pipelines

Integrate Claude Code with MCPs in your CI/CD workflows:

```yaml
# GitHub Actions example
jobs:
  visual-testing:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Start Puppeteer MCP
        run: |
          npm install -g @anthropic/mcp-puppeteer
          mcp-puppeteer &
      - name: Run Visual Tests with Claude Code
        run: |
          claude -p "Compare the staging site with the production site and report any visual differences" --dangerously-skip-permissions --mcp http://localhost:3000
```

## Troubleshooting MCP Issues

### Common Problems and Solutions

#### MCP Server Not Found

**Problem**: Claude Code can't find your MCP server.

**Solutions**:
- Verify the MCP server is running
- Check the port number
- Ensure no firewall is blocking the connection
- Try specifying the MCP URL explicitly with `--mcp`

#### Permission Denied

**Problem**: Claude Code requests permission but you want to pre-approve it.

**Solutions**:
- Use the `/permissions` command to manage permissions
- Edit your settings.json file
- Use the `--allowedTools` flag
- Use the `--dangerously-skip-permissions` flag (in controlled environments only)

#### Tool Execution Failed

**Problem**: An MCP tool fails to execute properly.

**Solutions**:
- Check the MCP server logs for errors
- Verify the parameters passed to the tool
- Ensure the tool is properly implemented
- Try simplifying the request

### Debugging Techniques

#### 1. Enable MCP Debug Mode

```bash
# Start MCP server with debug logging
mcp-puppeteer --debug

# Start Claude Code with MCP debugging
claude --mcp-debug
```

#### 2. Examine Network Traffic

Use tools like Wireshark or the browser's Network tab to inspect the communication between Claude Code and the MCP server.

#### 3. Step-by-Step Verification

Break complex MCP operations into smaller steps and verify each step individually.

## Security Considerations

### Best Practices

1. **Run in Isolated Environments**: Use containers or virtual machines to isolate MCP servers
2. **Limit Tool Permissions**: Only allow the specific tools needed for your task
3. **Avoid `--dangerously-skip-permissions`**: Use more granular permission controls instead
4. **Monitor MCP Activity**: Log and review MCP server activity
5. **Keep MCPs Updated**: Regularly update MCP servers to get security fixes
6. **Restrict Network Access**: Limit which hosts MCP servers can connect to
7. **Use Authentication**: Implement authentication for your MCP servers when possible

### Safe Use of Headless Mode

When using `--dangerously-skip-permissions` in headless mode:

- Run in a container without internet access
- Limit filesystem access to specific directories
- Restrict the MCP tools that can be used
- Review the prompt carefully before execution
- Monitor the output and logs

## Custom MCP Development

### Creating Specialized MCPs

#### Example: Data Visualization MCP

```javascript
// visualization-mcp.js
const express = require('express');
const { createCanvas } = require('canvas');
const fs = require('fs');
const app = express();
const port = 3002;

app.use(express.json());

const tools = {
  'create_bar_chart': async (params) => {
    const { data, labels, title, outputPath } = params;
    
    // Create canvas and draw chart
    const canvas = createCanvas(800, 600);
    const ctx = canvas.getContext('2d');
    
    // Drawing logic here...
    
    // Save to file
    const buffer = canvas.toBuffer('image/png');
    fs.writeFileSync(outputPath, buffer);
    
    return { 
      success: true, 
      message: `Chart saved to ${outputPath}` 
    };
  }
};

// MCP endpoints
app.post('/api/v1/mcp', async (req, res) => {
  // Tool execution logic
});

app.get('/api/v1/mcp', (req, res) => {
  // MCP discovery endpoint
});

app.listen(port, () => {
  console.log(`Visualization MCP server running on port ${port}`);
});
```

### MCP Protocol Specification

For developers creating custom MCPs, here's the protocol specification:

#### Discovery Endpoint

`GET /api/v1/mcp`

Response:
```json
{
  "name": "MCP Name",
  "version": "1.0.0",
  "tools": [
    {
      "name": "tool_name",
      "description": "Tool description",
      "parameters": {
        "param1": {
          "type": "string",
          "description": "Parameter description"
        }
      }
    }
  ]
}
```

#### Execution Endpoint

`POST /api/v1/mcp`

Request:
```json
{
  "tool": "tool_name",
  "params": {
    "param1": "value1"
  }
}
```

Response (Success):
```json
{
  "success": true,
  "result": {
    // Tool-specific result data
  }
}
```

Response (Error):
```json
{
  "success": false,
  "error": "Error message"
}
```

## MCP Server Examples Repository

To help you get started with MCPs, we've created a repository with ready-to-use MCP server examples:

- [GitHub: Claude Code MCP Examples](https://github.com/anthropics/claude-code-mcp-examples)

This repository contains:
- Puppeteer MCP with enhanced capabilities
- Database MCP for various database systems
- Visualization MCP for data plotting
- Custom MCP templates
- Example workflows combining multiple MCPs

## Future of MCPs

The MCP ecosystem is still evolving, with new capabilities being developed. Some areas of active development include:

- **AI Model Interaction**: MCPs that allow Claude to use other AI models
- **Hardware Integration**: MCPs for interacting with IoT devices and hardware
- **Collaborative Tools**: MCPs for real-time collaboration with human teams
- **Extended Visualization**: Enhanced visualization and data exploration capabilities
- **Domain-Specific Tools**: MCPs tailored for specific industries or use cases

## Community MCPs

The Claude Code community has developed several useful MCP servers:

- **GraphQL MCP**: For interacting with GraphQL APIs
- **Redis MCP**: For working with Redis databases
- **AWS MCP**: For managing AWS resources
- **Terraform MCP**: For infrastructure as code
- **PDF Processing MCP**: For creating and manipulating PDF files

Check the [Claude Code Hub Community Contributions](../community/contributions/README.md) for more information on these community-developed MCPs.

## Conclusion

MCP servers significantly extend Claude Code's capabilities, allowing it to interact with a wide range of tools and services. By understanding how to set up, configure, and use MCPs effectively, you can unlock powerful workflows and automation possibilities.

Whether you're using the built-in MCPs, creating custom ones, or leveraging community contributions, MCPs are a key part of the Claude Code ecosystem that enable more complex and sophisticated agentic coding scenarios.

---

## Additional Resources

- [Official MCP Documentation](https://docs.anthropic.com/claude/docs/mcp)
- [MCP Development Guide](https://docs.anthropic.com/claude/docs/developing-mcps)
- [Claude Code Best Practices](https://docs.anthropic.com/claude/docs/claude-code-best-practices)
- [Community MCP Directory](https://github.com/anthropics/claude-code-hub/community/contributions/mcps)
