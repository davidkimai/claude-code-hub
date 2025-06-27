# Headless Mode with Claude Code: Automation and CI/CD Integration

Headless mode is a powerful feature of Claude Code that enables you to use Claude's capabilities in non-interactive contexts like automation scripts, CI/CD pipelines, and batch processing. This guide covers everything you need to know about running Claude Code in headless mode for maximum productivity and integration.

## What is Headless Mode?

Headless mode allows you to run Claude Code with a single prompt and capture its output without any interactive input. This is ideal for:

- Automating repetitive tasks
- Integrating Claude into CI/CD pipelines
- Processing files in batch mode
- Creating scheduled tasks
- Building custom tools around Claude's capabilities

## Basic Usage

To use Claude Code in headless mode, use the `-p` or `--prompt` flag followed by your prompt:

```bash
claude -p "Analyze the performance of app.js and suggest improvements"
```

This runs Claude with the specified prompt and outputs the results to the terminal.

## Output Formats

### Standard Output

By default, Claude Code outputs text to the terminal, including both Claude's responses and any tool calls it makes.

### JSON Output

For programmatic processing, you can request JSON output:

```bash
claude -p "Analyze app.js" --json
```

This formats Claude's responses as JSON objects, making them easier to parse in scripts.

### Streaming JSON

For real-time processing of Claude's responses, use streaming JSON:

```bash
claude -p "Analyze app.js" --output-format stream-json
```

This outputs each response chunk as a separate JSON object, enabling progressive processing.

### Output to File

You can direct Claude's output to a file:

```bash
claude -p "Document the API in api.js" --output api-documentation.md
```

This saves Claude's response to the specified file instead of printing it to the terminal.

## Managing Permissions

### Pre-approving Tools

In headless mode, you'll typically want to pre-approve the tools Claude can use:

```bash
claude -p "Fix linting issues in src/" --allowedTools "Edit,Bash(npm run lint:fix)"
```

You can specify multiple tools by separating them with commas.

### Skipping Permission Checks

For trusted environments, you can skip permission checks entirely:

```bash
claude -p "Generate documentation for the API" --dangerously-skip-permissions
```

> **⚠️ Warning**: Only use `--dangerously-skip-permissions` in controlled environments, like containers without internet access or with limited filesystem access. This can pose security risks in uncontrolled environments.

## Environment and Context Configuration

### Setting Working Directory

Specify the working directory:

```bash
claude -p "Analyze code" --dir /path/to/project
```

### Using Custom CLAUDE.md

Provide a specific CLAUDE.md file:

```bash
claude -p "Fix bugs" --claude-md /path/to/CLAUDE.md
```

### Setting Environment Variables

Claude Code inherits environment variables from the shell, allowing you to configure its behavior:

```bash
CLAUDE_TOKEN=your_token claude -p "Generate documentation"
```

## Security Considerations

When using headless mode, especially with `--dangerously-skip-permissions`, consider these security measures:

### Running in Containers

Use Docker containers to isolate Claude Code sessions:

```bash
# Create a Docker container for Claude Code
docker run -it --rm \
  -v $(pwd):/app \
  --workdir /app \
  claude-code-image \
  claude -p "Analyze code" --dangerously-skip-permissions
```

### Limiting File Access

Restrict file access to specific directories:

```bash
# Mount only necessary directories
docker run -it --rm \
  -v $(pwd)/src:/app/src:ro \  # Read-only source
  -v $(pwd)/docs:/app/docs:rw \ # Writable docs
  --workdir /app \
  claude-code-image \
  claude -p "Generate documentation for code in src/" --output docs/api.md --dangerously-skip-permissions
```

### Network Isolation

Prevent network access to limit potential risks:

```bash
# Run without network access
docker run -it --rm \
  --network none \
  -v $(pwd):/app \
  --workdir /app \
  claude-code-image \
  claude -p "Refactor code" --dangerously-skip-permissions
```

## Practical Automation Examples

### Example 1: Automated Code Reviews

```bash
#!/bin/bash
# automated-review.sh
# Usage: ./automated-review.sh <pull-request-number>

PR_NUMBER=$1

# Get the list of files changed in the PR
FILES=$(gh pr view $PR_NUMBER --json files --jq '.files[].path' | tr '\n' ' ')

# Run Claude to review the changes
claude -p "Review the following files for code quality, performance issues, and potential bugs: $FILES" \
  --allowedTools "Bash(git diff)" \
  --output review-output.md

# Post the review as a comment on the PR
gh pr comment $PR_NUMBER -F review-output.md
```

### Example 2: Documentation Generator

```bash
#!/bin/bash
# generate-docs.sh
# Usage: ./generate-docs.sh <source-directory>

SRC_DIR=$1
OUTPUT_DIR="./docs"

# Create output directory if it doesn't exist
mkdir -p $OUTPUT_DIR

# Find all source files
FILES=$(find $SRC_DIR -type f -name "*.js" -o -name "*.ts")

# Generate documentation for each file
for FILE in $FILES; do
  FILENAME=$(basename $FILE)
  OUTPUT_FILE="$OUTPUT_DIR/${FILENAME%.*}.md"
  
  echo "Generating documentation for $FILE..."
  
  claude -p "Generate markdown documentation for this file: $FILE. Include a description of the file's purpose, function documentation, usage examples, and important notes." \
    --allowedTools "Edit" \
    --output $OUTPUT_FILE
done

echo "Documentation generation complete. Results in $OUTPUT_DIR"
```

### Example 3: Automated Bug Fixing

```bash
#!/bin/bash
# fix-failing-tests.sh
# Usage: ./fix-failing-tests.sh

# Run tests to identify failures
npm test > test-results.txt || true

# Extract failing test information
FAILING_TESTS=$(grep -A 5 "FAIL" test-results.txt)

if [ -z "$FAILING_TESTS" ]; then
  echo "No failing tests found."
  exit 0
fi

# Use Claude to fix the failing tests
claude -p "The following tests are failing. Analyze the failures, identify the root cause, and fix the issues: $FAILING_TESTS" \
  --allowedTools "Edit,Bash(npm test)" \
  --dangerously-skip-permissions

# Run tests again to verify fixes
npm test
```

### Example 4: Batch Code Refactoring

```bash
#!/bin/bash
# batch-refactor.sh
# Usage: ./batch-refactor.sh <directory> "<refactoring-instruction>"

DIR=$1
INSTRUCTION=$2

# Find all JavaScript files in the directory
FILES=$(find $DIR -type f -name "*.js")

# Process each file
for FILE in $FILES; do
  echo "Refactoring $FILE..."
  
  # Create a backup of the original file
  cp "$FILE" "${FILE}.bak"
  
  # Run Claude to refactor the file
  claude -p "Refactor the code in $FILE according to these instructions: $INSTRUCTION" \
    --allowedTools "Edit" \
    --dangerously-skip-permissions
  
  # Check if the file was modified
  if diff -q "$FILE" "${FILE}.bak" > /dev/null; then
    echo "No changes made to $FILE"
    rm "${FILE}.bak"
  else
    echo "Successfully refactored $FILE"
  fi
done
```

## CI/CD Integration

### GitHub Actions Integration

```yaml
# .github/workflows/claude-code-review.yml
name: Claude Code Review

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  code-review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      
      - name: Install Claude Code
        run: curl -fsSL https://claude.ai/code/install | bash
      
      - name: Get changed files
        id: changed-files
        run: |
          echo "files=$(git diff --name-only ${{ github.event.pull_request.base.sha }} ${{ github.event.pull_request.head.sha }} | tr '\n' ' ')" >> $GITHUB_OUTPUT
      
      - name: Run Claude Code Review
        run: |
          claude -p "Review the following files for code quality, security issues, and performance problems. Provide actionable feedback: ${{ steps.changed-files.outputs.files }}" \
            --allowedTools "Bash(git diff)" \
            --output review.md
      
      - name: Comment on PR
        uses: actions/github-script@v6
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          script: |
            const fs = require('fs');
            const review = fs.readFileSync('review.md', 'utf8');
            
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: review
            });
```

### Jenkins Pipeline Integration

```groovy
// Jenkinsfile
pipeline {
    agent {
        docker {
            image 'node:16'
        }
    }
    
    stages {
        stage('Install Claude Code') {
            steps {
                sh 'curl -fsSL https://claude.ai/code/install | bash'
            }
        }
        
        stage('Analyze Code') {
            steps {
                sh 'claude -p "Analyze the codebase for performance issues and technical debt. Create a report with prioritized recommendations." --allowedTools "Bash(find . -type f -name \'*.js\')" --output code-analysis.md'
            }
        }
        
        stage('Generate Documentation') {
            steps {
                sh 'claude -p "Generate API documentation for all endpoints in the src/api directory." --allowedTools "Edit" --output api-docs.md'
            }
        }
        
        stage('Archive Results') {
            steps {
                archiveArtifacts artifacts: '*.md', allowEmptyArchive: true
            }
        }
    }
}
```

### CircleCI Integration

```yaml
# .circleci/config.yml
version: 2.1
jobs:
  claude-tasks:
    docker:
      - image: cimg/node:18.0.0
    steps:
      - checkout
      - run:
          name: Install Claude Code
          command: curl -fsSL https://claude.ai/code/install | bash
      - run:
          name: Generate Documentation
          command: |
            mkdir -p docs
            claude -p "Generate comprehensive documentation for the project. Include architecture overview, API documentation, and usage examples." \
              --allowedTools "Edit,Bash(find . -type f -name '*.js')" \
              --output docs/documentation.md
      - store_artifacts:
          path: docs
          destination: documentation

workflows:
  version: 2
  build-and-document:
    jobs:
      - claude-tasks
```

## Advanced Headless Workflows

### Chaining Multiple Claude Instances

Create a pipeline of Claude instances where the output of one feeds into the next:

```bash
#!/bin/bash
# code-pipeline.sh

# Step 1: Analyze requirements
claude -p "Analyze requirements in requirements.txt and create a detailed implementation plan" \
  --allowedTools "Edit" \
  --output implementation-plan.md

# Step 2: Generate code based on the implementation plan
claude -p "Generate code based on the implementation plan in implementation-plan.md" \
  --allowedTools "Edit" \
  --output generated-code.md

# Step 3: Create tests for the generated code
claude -p "Create tests for the code in generated-code.md" \
  --allowedTools "Edit" \
  --output tests.md

# Step 4: Implement the code and tests
claude -p "Implement the code from generated-code.md and tests from tests.md in the src/ and tests/ directories" \
  --allowedTools "Edit,Bash(mkdir -p src tests)" \
  --dangerously-skip-permissions
```

### Task Distribution

Distribute tasks across multiple Claude instances running in parallel:

```bash
#!/bin/bash
# parallel-tasks.sh

# Find all JavaScript files
FILES=$(find src -name "*.js")

# Create a temporary directory for output
mkdir -p tmp_output

# Process files in parallel
for FILE in $FILES; do
  # Extract the filename without path and extension
  FILENAME=$(basename "$FILE" .js)
  
  # Process the file in the background
  claude -p "Analyze and optimize the code in $FILE" \
    --allowedTools "Edit" \
    --output "tmp_output/$FILENAME.md" &
  
  # Limit the number of parallel processes
  if [[ $(jobs -r | wc -l) -ge 4 ]]; then
    wait -n
  fi
done

# Wait for all background processes to complete
wait

# Combine the results
cat tmp_output/*.md > optimization-report.md

# Clean up
rm -rf tmp_output
```

### Recursive Problem Solving

Implement a recursive approach to complex problems:

```bash
#!/bin/bash
# recursive-solver.sh

MAX_ITERATIONS=5
PROBLEM_FILE="problem.txt"
SOLUTION_FILE="solution.md"

# Initial attempt
claude -p "Solve the problem described in $PROBLEM_FILE" \
  --allowedTools "Edit" \
  --output $SOLUTION_FILE

# Check if the solution passes verification
for ((i=1; i<=$MAX_ITERATIONS; i++)); do
  # Verify the solution
  VERIFICATION=$(claude -p "Verify if the solution in $SOLUTION_FILE correctly addresses the problem in $PROBLEM_FILE. If not, explain what's missing or incorrect." \
    --allowedTools "Edit" \
    --output verification.md)
  
  # Check if verification indicates success
  if grep -q "The solution is correct" verification.md; then
    echo "Solution verified after $i iterations."
    break
  fi
  
  # If we've reached max iterations, exit
  if [[ $i -eq $MAX_ITERATIONS ]]; then
    echo "Failed to find a verified solution after $MAX_ITERATIONS iterations."
    exit 1
  fi
  
  echo "Iteration $i: Solution needs improvement. Refining..."
  
  # Refine the solution based on verification
  claude -p "Refine the solution in $SOLUTION_FILE based on the verification feedback in verification.md" \
    --allowedTools "Edit" \
    --output $SOLUTION_FILE
done
```

## Handling Output

### Parsing JSON Output

Python example for parsing Claude's JSON output:

```python
#!/usr/bin/env python3
# parse_claude_output.py

import json
import subprocess
import sys

def run_claude(prompt):
    """Run Claude Code in headless mode and parse the JSON output."""
    result = subprocess.run(
        ['claude', '-p', prompt, '--json'],
        capture_output=True,
        text=True
    )
    
    if result.returncode != 0:
        print(f"Error running Claude: {result.stderr}", file=sys.stderr)
        sys.exit(1)
    
    try:
        output = json.loads(result.stdout)
        return output
    except json.JSONDecodeError as e:
        print(f"Error parsing JSON: {e}", file=sys.stderr)
        print(f"Raw output: {result.stdout}", file=sys.stderr)
        sys.exit(1)

def main():
    if len(sys.argv) < 2:
        print("Usage: python parse_claude_output.py 'Your prompt here'", file=sys.stderr)
        sys.exit(1)
    
    prompt = sys.argv[1]
    output = run_claude(prompt)
    
    # Process the output as needed
    if 'content' in output:
        print(f"Claude's response: {output['content']}")
    
    if 'tools' in output:
        print(f"Tools used: {', '.join(tool['name'] for tool in output['tools'])}")

if __name__ == "__main__":
    main()
```

### Processing Streaming Output

Node.js example for processing streaming JSON output:

```javascript
// process_streaming.js

const { spawn } = require('child_process');
const readline = require('readline');

function runClaudeStreaming(prompt) {
  const claude = spawn('claude', [
    '-p', prompt,
    '--output-format', 'stream-json'
  ]);
  
  const rl = readline.createInterface({
    input: claude.stdout,
    terminal: false
  });
  
  rl.on('line', (line) => {
    try {
      const chunk = JSON.parse(line);
      processChunk(chunk);
    } catch (error) {
      console.error('Error parsing JSON:', error);
      console.error('Raw line:', line);
    }
  });
  
  claude.stderr.on('data', (data) => {
    console.error(`Claude error: ${data}`);
  });
  
  claude.on('close', (code) => {
    console.log(`Claude process exited with code ${code}`);
  });
}

function processChunk(chunk) {
  // Process each chunk of Claude's response
  // This could be updating a UI, saving to a database, etc.
  if (chunk.type === 'content') {
    console.log(`Content: ${chunk.content}`);
  } else if (chunk.type === 'tool_call') {
    console.log(`Tool call: ${chunk.tool.name}`);
  }
}

// Example usage
runClaudeStreaming('Analyze the performance of app.js');
```

## Real-World Use Cases

### 1. Automated Documentation Maintenance

Keep documentation in sync with code changes:

```bash
#!/bin/bash
# update-docs.sh

# Get list of files that have changed since last commit
CHANGED_FILES=$(git diff --name-only HEAD)

# Filter for source code files
CODE_FILES=$(echo "$CHANGED_FILES" | grep -E '\.(js|ts|py|rb|java|go)$')

if [ -z "$CODE_FILES" ]; then
  echo "No source code files have changed."
  exit 0
fi

# Update documentation for each changed file
for FILE in $CODE_FILES; do
  DOC_FILE="docs/$(basename $FILE .${FILE##*.}).md"
  
  # Check if documentation exists
  if [ -f "$DOC_FILE" ]; then
    echo "Updating documentation for $FILE..."
    
    claude -p "Update the documentation in $DOC_FILE to reflect the current state of $FILE. Preserve the existing structure but update any outdated information." \
      --allowedTools "Edit" \
      --dangerously-skip-permissions
  else
    echo "Creating documentation for $FILE..."
    
    mkdir -p $(dirname "$DOC_FILE")
    
    claude -p "Create comprehensive documentation for $FILE. Include purpose, usage examples, and API details." \
      --allowedTools "Edit" \
      --output "$DOC_FILE"
  fi
done
```

### 2. Code Quality Monitoring

Regularly analyze code quality and track changes over time:

```bash
#!/bin/bash
# monitor-code-quality.sh

# Create a timestamp for this run
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
REPORT_DIR="quality_reports"
REPORT_FILE="$REPORT_DIR/quality_report_$TIMESTAMP.md"

# Create report directory if it doesn't exist
mkdir -p $REPORT_DIR

# Run Claude to analyze code quality
claude -p "Perform a comprehensive code quality analysis of the entire codebase. Focus on:
1. Code complexity
2. Maintainability
3. Test coverage
4. Potential bugs
5. Performance issues
6. Security vulnerabilities

Compare with previous reports in the $REPORT_DIR directory if available to highlight improvements or regressions." \
  --allowedTools "Edit,Bash(find . -type f -name '*.js')" \
  --output $REPORT_FILE

# If this is not the first report, generate a comparison
PREVIOUS_REPORT=$(find $REPORT_DIR -name "quality_report_*.md" -not -name "quality_report_$TIMESTAMP.md" | sort -r | head -n1)

if [ -n "$PREVIOUS_REPORT" ]; then
  COMPARISON_FILE="$REPORT_DIR/comparison_$TIMESTAMP.md"
  
  claude -p "Compare the current code quality report ($REPORT_FILE) with the previous report ($PREVIOUS_REPORT). Highlight improvements, regressions, and provide trend analysis." \
    --allowedTools "Edit" \
    --output $COMPARISON_FILE
  
  echo "Comparison generated: $COMPARISON_FILE"
fi

echo "Quality report generated: $REPORT_FILE"
```

### 3. Localization Automation

Automate translation and localization of content:

```bash
#!/bin/bash
# localize-content.sh

# Configuration
SOURCE_LANG="en"
TARGET_LANGS=("es" "fr" "de" "ja" "zh")
CONTENT_DIR="content/$SOURCE_LANG"
TRANSLATIONS_BASE="content"

# Find all content files
CONTENT_FILES=$(find $CONTENT_DIR -type f -name "*.md")

# Process each content file
for FILE in $CONTENT_FILES; do
  # Get the relative path from the source language directory
  REL_PATH=${FILE#$CONTENT_DIR/}
  
  echo "Processing $REL_PATH..."
  
  # Translate to each target language
  for LANG in "${TARGET_LANGS[@]}"; do
    # Create target directory if it doesn't exist
    TARGET_DIR="$TRANSLATIONS_BASE/$LANG/$(dirname $REL_PATH)"
    mkdir -p "$TARGET_DIR"
    
    TARGET_FILE="$TRANSLATIONS_BASE/$LANG/$REL_PATH"
    
    echo "  Translating to $LANG..."
    
    claude -p "Translate the content in $FILE from $SOURCE_LANG to $LANG. Maintain the markdown formatting, links, and overall structure. Ensure the translation is natural and idiomatic in $LANG." \
      --allowedTools "Edit" \
      --output "$TARGET_FILE"
  done
done

echo "Localization complete."
```

## Performance Optimization

### Batching Similar Tasks

Group similar tasks to leverage context across runs:

```bash
#!/bin/bash
# batch-similar-tasks.sh

# Instead of running separate Claude instances for each file
# Group similar files and process them together

# Find all JavaScript files
JS_FILES=$(find src -name "*.js" | tr '\n' ' ')

# Process all JS files in one go
claude -p "Analyze the following JavaScript files for performance issues and suggest improvements: $JS_FILES" \
  --allowedTools "Edit" \
  --output js-performance-report.md

# Find all CSS files
CSS_FILES=$(find src -name "*.css" | tr '\n' ' ')

# Process all CSS files in one go
claude -p "Review the following CSS files for best practices, optimize for performance, and suggest improvements: $CSS_FILES" \
  --allowedTools "Edit" \
  --output css-optimization-report.md
```

### Optimizing Prompts

Create efficient prompts to reduce processing time:

```bash
# Less efficient prompt
claude -p "Look at the code in this repository and fix any bugs you find." \
  --dangerously-skip-permissions

# More efficient prompt
claude -p "Analyze app.js for potential bugs, focusing specifically on:
1. Null or undefined reference errors
2. Incorrect error handling
3. Race conditions in asynchronous code
4. Memory leaks in event listeners
5. Improper API usage

For each issue found, provide:
1. The exact location (line number)
2. A description of the problem
3. The recommended fix" \
  --allowedTools "Edit" \
  --output bug-fixes.md
```

### Resource Management

For resource-intensive workflows, manage Claude instances carefully:

```bash
#!/bin/bash
# resource-managed-workflow.sh

# Configuration
MAX_CONCURRENT=3
TASKS_FILE="tasks.txt"
RESULTS_DIR="results"

# Create results directory
mkdir -p $RESULTS_DIR

# Read tasks from file
TASKS=($(cat $TASKS_FILE))

# Process tasks with limited concurrency
for ((i=0; i<${#TASKS[@]}; i++)); do
  TASK="${TASKS[$i]}"
  TASK_ID=$(printf "%03d" $i)
  OUTPUT_FILE="$RESULTS_DIR/result_$TASK_ID.md"
  
  echo "Starting task $TASK_ID: $TASK"
  
  # Run Claude in the background
  claude -p "$TASK" --output "$OUTPUT_FILE" &
  
  # Limit concurrent processes
  if [[ $(jobs -r | wc -l) -ge $MAX_CONCURRENT || $i -eq $((${#TASKS[@]} - 1)) ]]; then
    echo "Waiting for current tasks to complete..."
    wait
  fi
done

echo "All tasks completed. Results in $RESULTS_DIR/"
```

## Troubleshooting

### Common Issues and Solutions

#### Permissions Errors

**Problem**: Claude can't access required files or execute commands.

**Solution**:
- Check file permissions
- Use `--allowedTools` to pre-approve necessary tools
- Consider using `--dangerously-skip-permissions` in controlled environments

#### Memory or Context Limitations

**Problem**: Claude struggles with large files or complex tasks.

**Solution**:
- Break down tasks into smaller chunks
- Use more specific prompts to focus Claude's attention
- Create summaries or abstractions of large codebases

#### Inconsistent Results

**Problem**: Claude produces different results for similar tasks.

**Solution**:
- Use more structured prompts with explicit instructions
- Provide examples of desired output format
- Set a consistent environment with CLAUDE.md files

#### Authentication Issues

**Problem**: Authentication fails in CI/CD environments.

**Solution**:
- Securely store and provide authentication tokens as environment variables
- Use service accounts for CI/CD pipelines
- Verify token permissions and expiration

### Debugging Techniques

#### Verbose Output

Enable verbose output to see what's happening:

```bash
claude -p "Debug the issue in app.js" --verbose
```

#### Logging Claude's Actions

Create a log of Claude's actions:

```bash
claude -p "Fix the bugs in app.js" 2>&1 | tee claude-debug.log
```

#### Step-by-Step Execution

Break complex tasks into explicit steps:

```bash
#!/bin/bash
# step-by-step.sh

echo "Step 1: Analyzing code..."
claude -p "Analyze app.js and identify potential issues. Only identify, don't fix yet." \
  --output analysis.md

echo "Step 2: Planning fixes..."
claude -p "Based on the analysis in analysis.md, create a plan to fix the identified issues." \
  --output fix-plan.md

echo "Step 3: Implementing fixes..."
claude -p "Implement the fixes according to the plan in fix-plan.md." \
  --allowedTools "Edit" \
  --dangerously-skip-permissions

echo "Step 4: Verifying fixes..."
claude -p "Verify that the implemented fixes resolved the issues identified in analysis.md." \
  --allowedTools "Bash(npm test)" \
  --output verification.md
```

## Best Practices

### 1. Write Clear, Specific Prompts

Create detailed prompts with explicit instructions:

```bash
# Vague prompt
claude -p "Improve the code"

# Better prompt
claude -p "Refactor the code in utils.js to:
1. Use modern JavaScript features (ES6+)
2. Improve error handling with try/catch blocks
3. Replace callback patterns with async/await
4. Add JSDoc comments for all functions
5. Optimize performance of data processing functions"
```

### 2. Use Appropriate Permission Controls

Match permission levels to the environment and task:

- Development: Interactive permissions
- Trusted environments: Pre-approved tools
- CI/CD pipelines: Containerized with `--dangerously-skip-permissions`

### 3. Leverage CLAUDE.md for Context

Create specialized CLAUDE.md files for different automation scenarios:

```markdown
# CLAUDE.md for Documentation Generation

## Documentation Standards
- Use standard markdown format
- Include examples for all functions
- Document parameters and return values
- Note any side effects or exceptions
- Follow JSDoc conventions for code blocks

## Project Structure
[Project-specific details here]

## Special Instructions for Claude
- Focus on clarity and completeness
- Include diagrams for complex concepts
- Ensure links between related documentation
```

### 4. Implement Error Handling

Add robust error handling to automation scripts:

```bash
#!/bin/bash
# robust-automation.sh

# Exit on any error
set -e

# Function to handle errors
handle_error() {
  echo "Error occurred at line $1"
  # Clean up any temporary files or resources
  rm -f temp_*
  exit 1
}

# Set up error handling
trap 'handle_error $LINENO' ERR

# Run Claude with timeout and retry logic
run_claude_with_retry() {
  local prompt="$1"
  local output="$2"
  local max_attempts=3
  local attempt=1
  
  while [ $attempt -le $max_attempts ]; do
    echo "Attempt $attempt of $max_attempts..."
    
    if timeout 300s claude -p "$prompt" --output "$output"; then
      echo "Claude completed successfully"
      return 0
    else
      echo "Claude failed or timed out"
      
      if [ $attempt -eq $max_attempts ]; then
        echo "Maximum attempts reached, giving up"
        return 1
      fi
      
      echo "Retrying in 5 seconds..."
      sleep 5
      attempt=$((attempt + 1))
    fi
  done
}

# Main process
echo "Starting automated task..."

if ! run_claude_with_retry "Analyze the code in src/" "analysis.md"; then
  echo "Claude analysis failed after multiple attempts"
  exit 1
fi

echo "Task completed successfully"
```

### 5. Monitor and Log Results

Implement logging to track Claude's performance:

```bash
#!/bin/bash
# logging-example.sh

LOG_DIR="logs"
mkdir -p $LOG_DIR

# Create a timestamped log file
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
LOG_FILE="$LOG_DIR/claude_run_$TIMESTAMP.log"

# Log function
log() {
  echo "[$(date +"%Y-%m-%d %H:%M:%S")] $1" | tee -a "$LOG_FILE"
}

log "Starting Claude headless run"

# Run Claude and capture result code
log "Running Claude with prompt: 'Analyze app.js'"
claude -p "Analyze app.js" --output "analysis_$TIMESTAMP.md" 2>&1 | tee -a "$LOG_FILE"
RESULT=$?

if [ $RESULT -eq 0 ]; then
  log "Claude completed successfully"
else
  log "Claude failed with exit code $RESULT"
fi

log "Run completed"
```

## Conclusion

Headless mode is a powerful feature of Claude Code that opens up a wide range of automation possibilities. By leveraging headless mode, you can integrate Claude's capabilities into your development workflows, CI/CD pipelines, and automated systems.

The examples and techniques in this guide provide a starting point for your own headless Claude Code implementations. As you gain experience, you'll likely discover new ways to leverage Claude's capabilities in your specific context.

Remember to always consider security implications when using headless mode, especially with permission-skipping options. By following the best practices outlined in this guide, you can safely and effectively incorporate Claude Code into your automation workflows.

