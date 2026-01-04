# Usage Guide - AI Architecture Prompts

A practical guide to applying Eskil Steenberg's black box architecture principles using AI tools for maximum development velocity.

## 🎯 Quick Start Workflow

### 1. Choose Your Scenario

- **New Project** → Start with Claude Prompt (planning)
- **Refactoring Existing Code** → Start with Claude Code Prompt
- **Debugging/Testing Issues** → Use Cursor Prompt
- **Architecture Review** → Use Claude Prompt

### 2. Prepare Your Context (Recommended)

Extract relevant code context using AI context tools:

```bash
# For JavaScript/TypeScript projects
npx repomix --include "src/**/*.{js,ts,jsx,tsx}" --output context.xml

# For Python projects
git clone https://github.com/jimmc414/onefilellm.git
cd onefilellm
python onefilellm.py ./your-project/src/ --output context.xml

# For specific folders only
npx repomix --include "src/components/**" --output component-context.xml
```

### 3. Apply the Appropriate Prompt

Copy the relevant prompt from this repo and paste it into your AI tool, then include your context and specific question.

## 📋 Detailed Usage Scenarios

### Scenario 1: Planning a New Project

**Tool**: Claude (Web/Desktop)  
**Prompt**: `claude-prompt.md`

**Example Session**:

```
[Paste Claude Prompt]

I'm building a real-time chat application that needs to support:
- Multiple message types (text, images, files)
- User presence indicators
- Message history persistence
- Mobile and web clients

Can you help me architect this using black box principles?
```

**What You'll Get**:

- Module breakdown (MessageStore, PresenceManager, ClientAdapter, etc.)
- Interface specifications between components
- Risk assessment and mitigation strategies
- Implementation roadmap with priorities
- Team organization recommendations

### Scenario 2: Refactoring Complex Code

**Tool**: Claude Code  
**Prompt**: `claude-code-prompt.md`

**Example Session**:

```
[Paste Claude Code Prompt]

[Attach context.xml from your project]

This React component has grown to 500+ lines with complex state management,
DOM manipulation, and API calls all mixed together. How can I break this
into replaceable black box modules?
```

**What You'll Get**:

- Identification of black box boundaries
- Specific refactoring steps
- New interface designs
- Code examples for each module
- Testing strategy for the refactored code

### Scenario 3: Debugging and Testing Strategy

**Tool**: Cursor  
**Prompt**: `cursor-prompt.md`

**Example Session**:

```
[Paste Cursor Prompt]

[Include relevant code files]

I have integration issues between my payment module and user authentication.
The tests pass individually but fail when used together. How should I debug
this using black box principles?
```

**What You'll Get**:

- Debugging methodology for module boundaries
- Test isolation strategies
- Interface validation approaches
- Mock/stub recommendations
- Root cause analysis guidance

## 🔧 Advanced Workflows

### The Context + Prompt Combination

**Most Powerful Approach**: Combine AI context extraction with architecture prompts

1. **Extract Focused Context**

   ```bash
   # Focus on specific problem area
   npx repomix --include "src/auth/**" --include "src/payments/**" --output modules-context.xml
   ```

2. **Apply Architecture Analysis**

   ```
   [Claude Code Prompt]

   [Attach modules-context.xml]

   These two modules are tightly coupled and causing issues.
   How can I create clean black box interfaces between them?
   ```

3. **Implement Incrementally**
   - Use the AI's suggestions to refactor one module at a time
   - Maintain existing interfaces during transition
   - Test each change in isolation

### Multi-Stage Architecture Evolution

**Stage 1: Strategic Planning** (Claude Prompt)

- Define overall system architecture
- Identify major module boundaries
- Plan implementation phases

**Stage 2: Implementation** (Claude Code Prompt)

- Build individual modules
- Create clean interfaces
- Implement black box boundaries

**Stage 3: Testing & Debugging** (Cursor Prompt)

- Validate module interfaces
- Debug integration issues
- Optimize performance

### The "Folder Focus" Method

Perfect for large codebases:

1. **Identify Problem Folder**

   ```bash
   # Extract context for specific folder
   npx repomix --include "src/components/editor/**" --output editor-context.xml
   ```

2. **Analyze with Architecture Prompt**

   ```
   [Claude Code Prompt]
   [Attach editor-context.xml]

   This folder contains our text editor components. They're becoming
   hard to maintain and test. How can I restructure this using
   black box principles?
   ```

3. **Implement Suggested Changes**
   - Refactor one component at a time
   - Maintain backward compatibility
   - Test interfaces thoroughly

## 🎨 Prompt Customization Tips

### Adapting for Your Domain

Add domain-specific context to any prompt:

```markdown
## Additional Context

You are working on a [HEALTHCARE/FINTECH/GAMING] application where:

- [Specific regulatory requirements]
- [Performance constraints]
- [Security considerations]
- [Integration requirements]

Apply black box principles while considering these domain constraints.
```

### Technology-Specific Adaptations

**For React Projects**:

```markdown
## React-Specific Considerations

- Components should be black boxes with clear prop interfaces
- State management should be isolated within modules
- Side effects should be contained and testable
- Context should not leak implementation details
```

**For Backend APIs**:

```markdown
## API-Specific Considerations

- Each endpoint should represent a clear business capability
- Database interactions should be hidden behind service interfaces
- External API dependencies should be wrapped and mockable
- Error handling should be consistent across all modules
```

## 🚨 Common Pitfalls & Solutions

### Pitfall 1: Over-Engineering

**Problem**: Creating too many tiny modules  
**Solution**: Use the "one person rule" - each module should be maintainable by one developer

### Pitfall 2: Leaky Abstractions

**Problem**: Interfaces that expose implementation details  
**Solution**: Test if you could completely rewrite the implementation using only the interface

### Pitfall 3: Context Overload

**Problem**: Including too much code context, confusing the AI  
**Solution**: Focus on specific folders or files relevant to your question

### Pitfall 4: Ignoring Dependencies

**Problem**: Not considering how modules connect  
**Solution**: Always ask about interface design and integration points

## 📊 Measuring Success

### Before/After Metrics

Track these improvements:

- **Lines of code per module** - should decrease
- **Interface complexity** - fewer public methods/functions
- **Test isolation** - can modules be tested independently?
- **Replacement difficulty** - how hard is it to rewrite a module?
- **Developer onboarding** - how quickly can new devs contribute?

### Architecture Quality Checklist

After applying the prompts, verify:

- [ ] Each module has a single, clear responsibility
- [ ] Interfaces are documented and obvious to use
- [ ] Implementation details are completely hidden
- [ ] Modules can be tested without knowing internals
- [ ] External dependencies are wrapped, not used directly
- [ ] Any module could be rewritten using only its interface

## 🔄 Iteration Strategy

### Continuous Architecture Improvement

1. **Weekly Architecture Reviews**

   - Pick one module that's causing friction
   - Apply the appropriate prompt
   - Implement one suggested improvement

2. **New Feature Integration**

   - Before building new features, use Claude Prompt for planning
   - Ensure new code follows black box principles
   - Update existing interfaces if needed

3. **Bug-Driven Refactoring**
   - When bugs occur, use Cursor Prompt to understand root cause
   - Often bugs indicate poor module boundaries
   - Fix the architecture, not just the symptom

## 💡 Pro Tips

### Combining with Other Tools

- **Use with GitHub Copilot**: Let Copilot generate code, then use these prompts to structure it properly
- **Integration with IDEs**: Keep prompt snippets in your IDE for quick access
- **Documentation**: Use the architectural insights to improve your README and API docs

### Team Collaboration

- **Share Architecture Decisions**: Use Claude Prompt outputs as design documents
- **Code Review Focus**: Review interfaces and module boundaries, not just implementation
- **Onboarding**: New team members can use these prompts to understand the codebase architecture

### Long-term Maintenance

- **Architecture Debt**: Regularly review and refactor using these prompts
- **Technology Migration**: Black box architecture makes it easier to migrate technologies
- **Scaling Preparation**: Well-architected modules handle growth better

Remember: The goal isn't perfect architecture immediately, but architecture that stays maintainable as your project grows. Use these prompts iteratively to continuously improve your codebase structure.
