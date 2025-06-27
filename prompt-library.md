# Claude Code Prompt Library: Effective Prompting Patterns

This library contains carefully crafted prompt templates for Claude Code, organized by purpose and complexity. Each prompt includes explanations, variables to customize, and best practices to maximize effectiveness.

## Table of Contents

- [Project Understanding](#project-understanding)
- [Code Development](#code-development)
- [Refactoring](#refactoring)
- [Debugging](#debugging)
- [Testing](#testing)
- [Documentation](#documentation)
- [Git & GitHub Operations](#git--github-operations)
- [Learning & Exploration](#learning--exploration)

## How to Use This Library

1. Find a prompt that matches your need
2. Replace variables (indicated with `{curly_braces}`)
3. Adjust the prompt based on your specific context
4. Use the prompt in your Claude Code session

> **Pro Tip**: For complex tasks, combine an "Explore & Plan" prompt with an "Implement" prompt for better results.

---

## Project Understanding

### Codebase Overview

```
I need to understand this codebase. Please:

1. Scan key directories and files to understand the project structure
2. Identify the main components/modules and their relationships
3. Explain the architecture and design patterns used
4. Highlight important files I should know about
5. Summarize the tech stack and dependencies

Focus on giving me a high-level mental model rather than excessive detail.
```

### Technology-Specific Analysis

```
Analyze this {technology_type} project and help me understand:

1. How the {specific_aspect} is implemented
2. Key patterns and conventions used
3. How data flows through the system
4. Any unusual or non-standard approaches
5. Potential areas for improvement

Example files to look at: {example_file_paths}
```

### Feature Understanding

```
Help me understand how the {feature_name} feature works in this codebase. Specifically:

1. Which files implement this feature
2. The entry points and control flow
3. How data is processed
4. External systems or APIs it interacts with
5. Any known limitations or edge cases

Look through relevant directories like {directory_paths} to find the implementation.
```

### Mental Model Building

```
I'm new to this codebase and want to build a mental model of how it works. Think through this step by step:

1. First, identify the core abstractions and data structures
2. Map out how these connect to form the overall system
3. Explain the main execution paths and control flows
4. Identify where key business logic lives
5. Describe how the pieces fit together conceptually

Consider creating a diagram or visualization if that would help explain the architecture.
```

---

## Code Development

### Feature Implementation (Exploration & Planning)

```
I need to implement a new feature: {feature_description}

Before writing any code, please:
1. Think through the requirements and clarify any ambiguities
2. Explore the existing codebase to understand relevant components
3. Create a detailed implementation plan
4. Identify potential challenges or edge cases
5. Suggest a testing strategy

Do not write any implementation code yet - just focus on planning.
```

### Feature Implementation (Development)

```
Based on our plan, please implement {feature_name}.

Requirements:
- {requirement_1}
- {requirement_2}
- {requirement_3}

Technical guidelines:
- Follow the existing code style and patterns
- Add appropriate error handling
- Include tests to verify functionality
- Update any relevant documentation

Start by implementing the core functionality, then we can refine as needed.
```

### Algorithm Development

```
Help me develop an efficient algorithm for {problem_description}.

Requirements:
- Input: {input_description}
- Output: {output_description}
- Constraints: {constraints}
- Performance targets: {performance_goals}

First think through different approaches, analyzing tradeoffs.
Then implement the most suitable solution, explaining your reasoning.
```

### Component Creation

```
Create a new {framework} component for {purpose}.

The component should:
- {requirement_1}
- {requirement_2}
- {requirement_3}

It should integrate with our existing system by:
- {integration_point_1}
- {integration_point_2}

Look at similar components like {example_component} for style and patterns to follow.
Include appropriate tests and documentation.
```

### API Integration

```
Help me integrate with the {api_name} API. We need to:

1. Set up the client/connection
2. Implement these endpoints:
   - {endpoint_1} for {purpose_1}
   - {endpoint_2} for {purpose_2}
3. Handle authentication using {auth_method}
4. Properly manage errors and edge cases
5. Create tests to verify the integration works

Please explore the API documentation at {docs_url} if needed.
```

---

## Refactoring

### Code Quality Improvement

```
Review the code in {file_path} and suggest improvements for:

1. Readability and maintainability
2. Performance optimization
3. Error handling
4. Following best practices
5. Removing duplicated logic

After suggesting improvements, refactor the code to implement them.
Maintain the same functionality while making the code cleaner and more efficient.
```

### Technical Debt Reduction

```
Help me address technical debt in {component_or_file}.

Specifically, look for:
- Repeated or duplicated code
- Overly complex methods/functions
- Poor error handling
- Outdated patterns or approaches
- Performance bottlenecks
- Missing tests

Refactor the code to improve these issues while maintaining existing functionality.
```

### Architecture Refactoring

```
I want to refactor our architecture to address {problem_description}.

Current architecture:
- {current_component_1} handles {responsibility_1}
- {current_component_2} handles {responsibility_2}

Issues:
- {issue_1}
- {issue_2}

Please help me:
1. Design an improved architecture
2. Plan the refactoring steps
3. Implement the changes incrementally
4. Ensure tests pass at each step
5. Update documentation to reflect the new structure
```

### Code Modernization

```
Modernize the code in {file_path} to use current {language} features and best practices.

Specifically:
- Update from {old_pattern} to {new_pattern}
- Replace {old_library} with {new_library}
- Use modern syntax like {syntax_example}
- Improve type safety
- Update deprecated API calls

Keep the same functionality while making the code more maintainable and efficient.
```

---

## Debugging

### Error Analysis

```
I'm encountering the following error:

```
{error_message}
```

It happens when {steps_to_reproduce}.

Please help me:
1. Understand what's causing this error
2. Find the root issue in the code
3. Develop a fix
4. Verify the solution works
5. Suggest ways to prevent similar issues

Relevant files: {file_paths}
```

### Performance Debugging

```
I'm experiencing performance issues with {feature_or_component}.

Symptoms:
- {symptom_1}
- {symptom_2}

Metrics:
- Current performance: {current_metrics}
- Target performance: {target_metrics}

Please help me:
1. Identify potential bottlenecks
2. Profile the code to locate specific issues
3. Suggest and implement optimizations
4. Measure the impact of changes
5. Recommend further improvements if needed
```

### Test Failure Analysis

```
The following test is failing:

```
{test_output}
```

Please help me:
1. Understand why the test is failing
2. Identify if the issue is in the test or the implementation
3. Propose a fix
4. Implement and verify the solution

Relevant files:
- Test file: {test_file_path}
- Implementation: {implementation_file_path}
```

### Logic Bug Investigation

```
I think there's a logic bug in {feature_description}.

Expected behavior:
- {expected_behavior}

Actual behavior:
- {actual_behavior}

Please help me:
1. Trace through the code execution
2. Identify where the actual behavior diverges from expected
3. Explain the root cause
4. Implement a fix
5. Add tests to prevent regression
```

---

## Testing

### Test Suite Creation

```
Create a comprehensive test suite for {component_or_feature}.

The tests should cover:
1. Unit tests for individual functions/methods
2. Integration tests for component interactions
3. Edge cases and error conditions
4. Performance considerations (if applicable)

Use {testing_framework} and follow our existing testing patterns.
Focus on testing behavior rather than implementation details.
```

### Test-Driven Development

```
I want to implement {feature_name} using test-driven development.

Requirements:
- {requirement_1}
- {requirement_2}
- {requirement_3}

Please:
1. Write failing tests that define the expected behavior
2. Create the minimum implementation to make tests pass
3. Refactor for clarity and efficiency
4. Repeat until all requirements are covered

Let's focus on one test at a time and build up incrementally.
```

### Test Coverage Improvement

```
Our test coverage for {component} is currently {current_coverage}%.
I want to improve it to at least {target_coverage}%.

Please:
1. Analyze current test coverage to identify gaps
2. Prioritize untested or under-tested areas
3. Write additional tests focusing on:
   - Critical paths
   - Edge cases
   - Error conditions
   - Business logic
4. Ensure tests are meaningful and not just for coverage
```

### Mocking and Stubbing

```
Help me write tests for {component} that requires mocking external dependencies.

The component interacts with:
- {dependency_1} through {interface_1}
- {dependency_2} through {interface_2}

Please:
1. Set up appropriate mocks/stubs for these dependencies
2. Write tests that verify the component's behavior in isolation
3. Include tests for error handling and edge cases
4. Ensure mocks don't make tests brittle or implementation-specific
```

---

## Documentation

### Code Documentation

```
Please add comprehensive documentation to {file_path}.

Include:
1. File-level documentation describing purpose and responsibility
2. Function/method documentation with:
   - Purpose
   - Parameters
   - Return values
   - Exceptions/errors
   - Examples (if helpful)
3. Complex logic explanation
4. Important design decisions or constraints

Follow our documentation style using {documentation_format}.
```

### README Creation

```
Create a comprehensive README.md for this project with:

1. Project overview and purpose
2. Installation instructions
3. Usage examples
4. API documentation
5. Configuration options
6. Development setup
7. Testing instructions
8. Contribution guidelines
9. License information

Make it clear and user-friendly, with code examples where appropriate.
```

### Architecture Documentation

```
Create architecture documentation for this project covering:

1. High-level system overview (components and their relationships)
2. Data flow through the system
3. Key design patterns and architectural decisions
4. External dependencies and integrations
5. Deployment architecture
6. Performance and scaling considerations
7. Security model

Include diagrams where helpful using ASCII art or describe them clearly.
```

### API Documentation

```
Generate comprehensive API documentation for {api_or_service}.

For each endpoint, include:
1. URL path and method
2. Request parameters and body schema
3. Response format and status codes
4. Authentication requirements
5. Rate limiting information
6. Example requests and responses
7. Error handling
8. Usage notes or limitations

Format the documentation in {format} style.
```

---

## Git & GitHub Operations

### Pull Request Creation

```
Create a pull request for the changes we've made.

PR details:
- Title: {pr_title}
- Base branch: {base_branch}
- Description should include:
  - Purpose of the changes
  - How they address the issue
  - Testing performed
  - Any notes for reviewers

Before creating the PR:
1. Ensure all tests pass
2. Lint and format the code
3. Create a clear commit message
4. Check for any unintended changes
```

### Code Review

```
Please review the code changes in {pr_or_commit} for:

1. Functionality - does it work as intended?
2. Code quality and style
3. Potential bugs or edge cases
4. Performance considerations
5. Security implications
6. Test coverage

Provide specific feedback with suggestions for improvement.
Distinguish between critical issues and minor recommendations.
```

### Git History Management

```
Help me clean up the git history for {branch_name}.

I need to:
1. Combine related commits
2. Write clear commit messages
3. Remove any temporary or debug changes
4. Ensure the branch is ready for review

Please suggest and execute the appropriate git commands.
```

### Merge Conflict Resolution

```
I have merge conflicts when trying to merge {source_branch} into {target_branch}.

Conflicting files:
- {file_1}
- {file_2}

Please help me:
1. Understand the nature of each conflict
2. Resolve the conflicts preserving the correct functionality
3. Ensure tests pass after resolution
4. Commit the resolved changes
```

---

## Learning & Exploration

### Concept Explanation

```
Explain {concept} as it's used in this codebase.

Specifically:
1. Provide a clear definition
2. Show examples from our code
3. Explain why and how we're using it
4. Compare with alternative approaches
5. Highlight best practices or potential issues

Assume I'm familiar with programming but not necessarily with this specific concept.
```

### Technology Comparison

```
Compare {technology_1} and {technology_2} for use in our project.

Please analyze:
1. Feature comparison relevant to our needs
2. Performance characteristics
3. Integration with our existing stack
4. Learning curve and team familiarity
5. Community support and maturity
6. Long-term maintenance considerations

Make a recommendation based on our specific requirements: {requirements}
```

### Pattern Implementation

```
Show me how to implement the {design_pattern} pattern in our codebase.

Specifically:
1. Explain the pattern and when it's useful
2. Identify where in our code it would be applicable
3. Implement a concrete example
4. Compare with our current approach
5. Discuss any tradeoffs or considerations

Use {language} and follow our project's conventions.
```

### Technology Exploration

```
I want to explore using {technology} in our project.

Please help me:
1. Understand the core concepts and benefits
2. Create a simple proof of concept
3. Evaluate how it would integrate with our existing system
4. Identify any challenges or limitations
5. Suggest a path forward if we decide to adopt it

Our current tech stack includes: {current_technologies}
```

## Advanced Combined Prompts

### Full Feature Development Lifecycle

```
I need to implement {feature_name} from start to finish.

Requirements:
- {requirement_1}
- {requirement_2}
- {requirement_3}

Please guide me through the complete process:

1. Exploration phase:
   - Understand the requirements
   - Research technical approaches
   - Identify affected components

2. Planning phase:
   - Design the solution architecture
   - Create implementation plan
   - Identify potential challenges

3. Development phase:
   - Write tests first (TDD approach)
   - Implement the core functionality
   - Refine and optimize

4. Quality assurance:
   - Ensure comprehensive test coverage
   - Review code quality
   - Verify requirements are met

5. Documentation:
   - Update relevant documentation
   - Add code comments
   - Prepare for code review

6. Finalization:
   - Create pull request
   - Address feedback
   - Prepare for deployment

Let's focus on one phase at a time and make sure each is complete before moving on.
```

### Codebase Audit and Improvement

```
Conduct a comprehensive audit of {component_or_directory} and develop an improvement plan.

Please analyze:

1. Code quality and maintainability:
   - Complexity metrics
   - Adherence to best practices
   - Readability and documentation
   - Code duplication

2. Architecture and design:
   - Component boundaries
   - Separation of concerns
   - Design patterns usage
   - Extensibility

3. Performance:
   - Algorithmic efficiency
   - Resource usage
   - Bottlenecks
   - Optimization opportunities

4. Testing and reliability:
   - Test coverage
   - Test quality
   - Error handling
   - Edge cases

5. Security:
   - Input validation
   - Authentication/authorization
   - Data protection
   - Common vulnerabilities

After the audit, create:
- A prioritized list of issues
- Recommended improvements
- An implementation plan for addressing critical issues
```

### Technical Spike Investigation

```
Conduct a technical spike on {technology_or_approach} to determine if it's suitable for {our_use_case}.

Please:

1. Research phase:
   - Understand key concepts and capabilities
   - Identify relevant features for our needs
   - Research known limitations or issues

2. Proof of concept:
   - Create a minimal working example
   - Implement core functionality we need
   - Test under conditions similar to our use case

3. Integration assessment:
   - Evaluate compatibility with our tech stack
   - Identify integration points and approaches
   - Assess migration effort (if applicable)

4. Evaluation:
   - Measure performance under expected load
   - Assess developer experience and learning curve
   - Consider long-term maintenance and support

5. Recommendation:
   - Provide a clear go/no-go recommendation
   - Suggest alternatives if not recommended
   - Outline implementation strategy if recommended

Document your findings and provide code examples that demonstrate key capabilities.
```

## Tips for Effective Prompting

1. **Be specific**: Clear, detailed prompts yield better results
2. **Plan first**: Ask Claude to plan before implementing complex tasks
3. **Provide context**: Reference specific files or code sections
4. **Use step-by-step instructions**: Break complex tasks into steps
5. **Request thinking**: Use the word "think" to trigger extended reasoning
6. **Iterate**: Build on Claude's responses with follow-up prompts
7. **Course-correct**: Provide feedback to guide Claude if needed
8. **Include examples**: Show Claude examples of what you want
9. **Specify format**: Request specific output formats for consistency
10. **Use visuals**: Include screenshots or diagrams for visual tasks

## Adapting Prompts for Different Experience Levels

### For Beginners

- Add more explanation requests
- Ask for step-by-step breakdowns
- Request learning resources
- Have Claude explain terminology

Example adaptation:
```
Help me understand how the authentication system works in this codebase. 
Please explain in simple terms with:
- Basic concepts I should know first
- Step-by-step explanation of the flow
- Explanation of any technical terms
- Code examples with detailed comments
```

### For Advanced Users

- Focus on efficiency and depth
- Request comparative analysis
- Ask for performance considerations
- Explore edge cases and optimizations

Example adaptation:
```
Analyze our authentication system for:
- Performance bottlenecks under high load
- Security vulnerabilities
- Scaling limitations
- Potential optimizations
Compare our implementation with industry best practices and suggest improvements.
```

---

This prompt library is a living document. Feel free to contribute your own effective prompts or suggest improvements to existing ones!
