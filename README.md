# lar-ai-phpstorm-promts

- [Article Link](https://laraveldaily.com/post/my-cursor-rules-for-laravel)
- [Claude templates](#claude-templates)
- [Ultimate Claude Code Cheat Sheet](#ultimate-claude-code-sheet)

## Files

- .junie/guidelines.md file in PhpStorm Junie AI
- CLAUDE.md file in Claude Code


## General code instructions
 
- Don't generate code comments above the methods or code blocks if they are obvious. Don't add docblock comments when defining variables, unless instructed to, like `/** @var \App\Models\User $currentUser */`. Generate comments only for something that needs extra explanation for the reasons why that code was written.
 
---
 
## PHP instructions
 
- In PHP, use `match` operator over `switch` whenever possible
- Use PHP 8 constructor property promotion. Don't create an empty Constructor method if it doesn't have any parameters.
- Using Services in Controllers: if Service class is used only in ONE method of Controller, inject it directly into that method with type-hinting. If Service class is used in MULTIPLE methods of Controller, initialize it in Constructor.
- Use return types in functions whenever possible, adding the full path to classname to the top in `use` section
- Generate Enums always in the folder `app/Enums`, not in the main `app/` folder, unless instructed differently.
 
---
 
## Laravel instructions
 
- For DB pivot tables, use correct alphabetical order, like "project_role" instead of "role_project"
- I am using Laravel Herd locally, so always assume that the main URL of the project is `http://[folder_name].test`
- **Eloquent Observers** should be registered in Eloquent Models with PHP Attributes, and not in AppServiceProvider. Example: `#[ObservedBy([UserObserver::class])]` with `use Illuminate\Database\Eloquent\Attributes\ObservedBy;` on top
- When generating Controllers, put validation in Form Request classes and use `$request->validated()` instead of listing inputs one by one.
- Aim for "slim" Controllers and put larger logic pieces in Service classes
- Use Laravel helpers instead of `use` section classes whenever possible. Examples: use `auth()->id()` instead of `Auth::id()` and adding `Auth` in the `use` section. Another example: use `redirect()->route()` instead of `Redirect::route()`.
- Don't use `whereKey()` or `whereKeyNot()`, use specific fields like `id`. Example: instead of `->whereKeyNot($currentUser->getKey())`, use `->where('id', '!=', $currentUser->id)`.
- Don't add `::query()` when running Eloquent `create()` statements. Example: instead of `User::query()->create()`, use `User::create()`.
- In Livewire projects, don't use Livewire Volt. Only Livewire class components.
- When creating pivot tables in migrations, if you use `timestamps()`, then in Eloquent Models, add `withTimestamps()` to the `BelongsToMany` relationships.
 
---
 
## Use Laravel 11+ skeleton structure
 
- **Service Providers**: there are no other service providers except AppServiceProvider. Don't create new service providers unless absolutely necessary. Use Laravel 11+ new features, instead. Or, if you really need to create a new service provider, register it in `bootstrap/providers.php` and not `config/app.php` like it used to be before Laravel 11.
- **Event Listeners**: since Laravel 11, Listeners auto-listen for the events if they are type-hinted correctly.
- **Console Scheduler**: scheduled commands should be in `routes/console.php` and not `app/Console/Kernel.php` which doesn't exist since Laravel 11.
- **Middleware**: whenever possible, use Middleware by class name in the routes. But if you do need to register Middleware alias, it should be registered in `bootstrap/app.php` and not `app/Http/Kernel.php` which doesn't exist since Laravel 11.
- **Tailwind**: in new Blade pages, use Tailwind and not Bootstrap, unless instructed otherwise in the prompt. Tailwind is already pre-configured since Laravel 11, with Vite.
- **Faker**: in Factories, use `fake()` helper instead of `$this->faker`.
- **Policies**: Laravel automatically auto-discovers Policies, no need to register them in the Service Providers.
 
---
 
## Testing instructions
 
Use Pest and not PHPUnit. Run tests with `php artisan test`.
 
Every test method should be structured with Arrange-Act-Assert.
 
In the Arrange phase, use Laravel factories but add meaningful column values and variable names if they help to understand failed tests better.
Bad example: `$user1 = User::factory()->create();`
Better example: `$adminUser = User::factory()->create(['email' => 'admin@admin.com'])`;
 
In the Assert phase, perform these assertions when applicable:
- HTTP status code returned from Act: `assertStatus()`
- Structure/data returned from Act (Blade or JSON): functions like `assertViewHas()`, `assertSee()`, `assertDontSee()` or `assertJsonContains()`
- Or, redirect assertions like `assertRedirect()` and `assertSessionHas()` in case of Flash session values passed
- DB changes if any create/update/delete operation was performed: functions like `assertDatabaseHas()`, `assertDatabaseMissing()`, `expect($variable)->toBe()` and similar.



# Claude Templates

## Top Claude Commands

1. `/analyze-issue`

**What It Does**: Fetches a GitHub issue, extracts requirements, and generates a complete implementation spec with tasks, test cases, and edge cases.

**Time Saved**: Planning phase drops from 90 minutes to 15 minutes (83% reduction)

**Why It Matters**: Most bugs come from misunderstood requirements. This command forces comprehensive planning before a single line of code is written.

**Copy-Paste Template**:

```
---
description: Generate implementation spec from GitHub issue
argument-hint: <issue-number>
---
# Analyze Issue #$ARGUMENTS

1. Fetch issue: `gh issue view $ARGUMENTS`
2. **Requirements Analysis**
   - Extract user story and acceptance criteria
   - List functional requirements
   - Note non-functional requirements (performance, security)
3. **Technical Specification**
   - Files to modify/create
   - API contracts (request/response schemas)
   - Database schema changes
   - External dependencies
4. **Implementation Plan**
   - Break into 5-7 sub-tasks with complexity estimates (1-5 scale)
   - Identify risks and implementation order
5. **Test Strategy**
   - Unit tests, integration tests, E2E scenarios
   - Edge cases to cover
6. **Definition of Done**
   - Functionality checklist, test coverage requirements
   - Documentation updates, performance benchmarks
Create `specs/issue-$ARGUMENTS-spec.md` with complete analysis.
```

**Real Impact**: Last sprint, this command caught 12 edge cases in planning that would’ve become production bugs. One of those edge cases involved payment processing — catching it early saved us a potential $xK in refunds and customer trust issues.


2. `/feature-scaffold`

**What It Does:** Generates complete feature folder structure with boilerplate components, tests, types, and documentation following your team’s conventions.

**Time Saved**: Setup time drops from 35 minutes to 2 minutes (94% reduction)

**Why It Matters**: Consistency is everything. When every feature follows the same structure, code reviews are faster, onboarding is smoother, and bugs hide less effectively.

**Copy-Paste Template**:

```
---
description: Generate feature scaffold with tests, types, and docs
argument-hint: <feature-name>
---
# Scaffold Feature: $ARGUMENTS

1. Create feature directory: `src/features/$ARGUMENTS/`
2. **Generate Core Files:**

$ARGUMENTS.tsx 
# Main component $ARGUMENTS.test.tsx 
# Unit tests $ARGUMENTS.types.ts 
# TypeScript interfaces $ARGUMENTS.styles.ts 
# Styled components index.ts 
# Barrel export

3. **Component Template:**
- Props interface with JSDoc
- Error boundary wrapper
- Loading and error states
- Accessibility attributes

4. **Test Template:**
- Render test
- User interaction tests
- Error state tests
- Accessibility tests (axe-core)
5. **Documentation:**
- Create `$ARGUMENTS/README.md` with:
  - Feature overview
  - Props documentation
  - Usage examples
  - Known limitations
6. **Git Integration:**
- Stage all files: `git add src/features/$ARGUMENTS/`
- Create feature branch: `git checkout -b feature/$ARGUMENTS`
```

**Team Impact:** Since implementing this command, our code review time dropped 41%. Reviewers spend less time checking structure and more time evaluating logic. New team members contribute production code on day 3 instead of week 3.

3. `/session-start`

**What It Does**: Initializes a development session with task tracking, auto-commits at milestones, and generates handoff documentation for asynchronous teams.

**Time Saved**: Eliminates 20 minutes of daily status updates and context-switching overhead

**Why It Matters**: Remote teams across timezones need perfect handoffs. This command creates a detailed paper trail that makes async collaboration actually work.

**Copy-Paste Template**:

```
---
description: Start tracked development session with auto-documentation
argument-hint: <task-description>
---
# Start Session: $ARGUMENTS

1. **Session Initialization:**
   - Create session log: `.sessions/session-$(date +%Y%m%d-%H%M%S).md`
   - Log start time and task description
2. **Context Capture:**
   - Current branch: `git branch --show-current`
   - Recent commits: `git log -3 --oneline`
   - Open files in editor workspace
   - Relevant documentation links
3. **Task Breakdown:**
   - Break $ARGUMENTS into 3-5 concrete subtasks
   - Estimate each subtask (S/M/L complexity)
   - Identify dependencies and blockers
4. **Checkpoint System:**
   - Auto-commit every 30 minutes with: `git add -A && git commit -m "checkpoint: [progress-description]"`
   - Log decisions and discoveries in session file
5. **Handoff Template:**
   ```markdown
   ## Progress Summary
   [Completed tasks]
   
   ## Current State
   [What's working, what's blocked]
   
   ## Next Steps
   1. [Immediate priority]
   2. [Secondary task]
   3. [Future consideration]
   
   ## Questions for Team
   - [Specific question 1]
   - [Specific question 2]

**Remote Team Win:** Our distributed team (Berlin, SF, Tokyo) ships features without timezone delays. When Berlin signs off, SF picks up with zero onboarding time. Before this command, handoffs took 30-45 minutes. Now? 5 minutes to read the session log and continue.
---
## **Command 4: `/security-scan` - Proactive Vulnerability Detection**
**What It Does:** Runs comprehensive security audit on recent changes, checking for common vulnerabilities, exposed secrets, and security misconfigurations.
**Time Saved:** Security review drops from 60 minutes to 8 minutes (87% reduction)
**Why It Matters:** According to Apiiro's 2024 research, AI-generated code introduces 322% more privilege escalation paths and 153% more design flaws than human-written code. You need automated guardrails.
**Copy-Paste Template:**
```markdown
---
description: Comprehensive security audit of recent changes
---
# Security Scan
1. **Secret Detection:**
   ```bash
   git diff --cached | grep -E '(api_key|password|secret|token|aws_access)' || echo "✓ No secrets detected"
```

4. Work thinking

**Context Window Death Spiral**: Your agent starts with perfect understanding — project structure, dependencies, architectural decisions. Five minutes later, it’s suggesting npm packages you explicitly excluded. Ten minutes in, it’s rewriting interfaces it just created. This isn’t minor degradation, it’s catastrophic for complex tasks.

I tracked token distribution across failed sessions. The pattern is consistent: implementation details consume 73% of context within the first 2,000 tokens, pushing architectural requirements below the attention threshold. Once architecture drops below 30% context influence, implementation drift becomes inevitable.

**Permission Interrupt Cascade**: Every file modification triggers a permission request. Approve. Continue. Another permission. Approve. Another. By the fifth interruption, your agent has lost its implementation strategy and you’ve lost your flow state. The context fragmentation compounds — each restart loads slightly different context, creating subtle inconsistencies that cascade into major architectural violations.

**Agent Collision Syndrome**: Running multiple agents without coordination creates a special kind of chaos. Agent A refactors your database schema while Agent B writes queries for the old structure. Agent C updates the API based on Agent B’s assumptions. Without wave-based generation patterns to manage context limits, agents operate in isolation, creating incompatible implementations at exponential rates.


# Ultimate Claude Code Sheet

## Core Session Commands

These are your bread and butter for managing conversations:

/help — Your first stop when stuck. Displays usage help and command reference.

/clear — Wipes the conversation history clean. Use this when you want a fresh start or when context gets too cluttered.

/exit — Gracefully exits the REPL session. (Or just use Ctrl+D)

/status — Shows version info and connectivity status. Great for troubleshooting.


## Context & Memory Management

Here’s where Claude Code really shines. Managing context effectively is the difference between a helpful AI and a confused one:

/context — Visualizes your current context usage as a colored grid. Think of this as your context "fuel gauge"—when it fills up, responses slow down and quality degrades.

/compact [instructions] — This is your context compression tool. It summarizes the conversation history while preserving important details. You can add optional instructions like /compact focus on authentication logic to keep specific topics intact.

/memory — Opens your CLAUDE.md memory files for editing. This is where you store project conventions, coding standards, and preferences that persist across sessions.

/init — Initializes a new project with a CLAUDE.md guide. Run this in every new repository.

Pro tip: Use the # prefix for quick memory adds. Type # Use 2-space indentation for JavaScript files and it's instantly added to CLAUDE.md.

















