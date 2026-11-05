# Frontend MR Reviewer Agent

### Do we really need MCP to create a custom agent?

While exploring custom agents, I wanted to find out whether an agent could perform a real code review without depending on MCP integrations.

The answer is yes!

I created my first **MCP-independent Frontend MR Reviewer Agent**. It does not depend on GitLab MCP, GitHub MCP, or any other MCP server.

Instead, it uses the local Git repository, the project's frontend coding standards, and the story requirements provided by the user.

**Note:** The agent still requires access to the local workspace and appropriate Git/terminal tools to execute Git commands.

## What does our agent need from the user?

Before starting the review, the agent needs three things:

1. **MR source branch** – The branch containing the changes to be reviewed.
2. **MR target branch** – The branch into which the changes are intended to be merged.
3. **Story / requirement** – The business context describing what the changes are expected to achieve.

The review is based on three main sources:

- **Git repository:** The source of truth for code changes.
- **Story / requirement:** Provides the business context for the review.
- **`frontend-rules.md`:** Defines our frontend engineering standards.

The `MR-Reviews/` directory is the only location where the agent writes review artifacts. It does not modify application source code.

The design also follows an important prompt-engineering principle: explicitly defining constraints, responsibilities, and expected output rather than relying on implicit behaviour.

## A short brief on how it works

Once the agent receives all three inputs, it follows a safety-first workflow.

1. It checks the current Git branch and working-tree status using `git status --short --branch`.
2. If the working tree is dirty, it stops and asks the user to preserve their changes before proceeding. It does not reset, stash, commit, or overwrite them.
3. If the working tree is clean, it asks the user for permission before switching to the target branch.
4. It fetches the latest remote references using `git fetch origin --prune`.
5. It verifies that the source and target branch references exist.
6. It identifies the MR changes using a three-dot Git diff.
7. It reviews the changes against the story, existing codebase patterns, and our custom frontend rules.

The best part of this agent is that it does not perform a generic code review. It follows the custom frontend standards defined in `frontend-rules.md`.

We can add, remove, or update frontend rules independently without having to rewrite the entire review workflow.

## Complete workflow

```text
                         User
                           |
                           |
                    "Review my MR"
                           |
                           v
                    Frontend Agent
                           |
             +-------------+-------------+
             |             |             |
             v             v             v
         Source         Target          Story /
         Branch         Branch       Requirement
             |             |             |
             +-------------+-------------+
                           |
                           v
                  Check Git status
              git status --short --branch
                           |
                    +------+------+
                    |             |
                  Dirty          Clean
                    |             |
                    v             v
                  STOP       Ask permission
                    |        to switch to
                    |        target branch
                    |             |
                    v             v
              Ask user to    git fetch origin
              preserve       --prune
              changes              |
                                   v
                         Verify source and
                         target references
                                   |
                                   v
                         Compare branch changes
                  git diff origin/target...origin/source
                                   |
                                   v
                              Review
                                   |
                +----------------+----------------+
                |                |                |
                v                v                v
              Story             Diff         Existing code
                                   |
                                   v
                           frontend-rules.md
                                   |
                           +-------+-------+
                           |               |
                           v               v
                        Findings       No findings
                           |               |
                           v               v
                      MR-Reviews/     "No changes are
                      <branch>-       needing it's good
                      review.md       to be approved."
```

## How to use it

Using the agent is quite simple. Integrate the following files into your project structure.

```text
your-project/
│
├── .github/
│   │
│   ├── agents/
│   │   │
│   │   └── frontend-mr-reviewer.md
│   │
│   ├── prompts/
│   │   │
│   │   └── reviewer.prompt.md
│   │
│   └── frontend-rules.md
│
├── MR-Reviews/
│   │
│   └── <generated-review-files>.md
│
├── src/
│   ├── components/
│   ├── services/
│   ├── hooks/
│   └── ...
│
└── ...
```

### How do the three files relate to each other?

Each file has a separate responsibility. Together, they define how the agent behaves, what it reviews, and which engineering standards it follows.

```text
       +--------------------------------------+
       |       frontend-mr-reviewer.md        |
       |                                      |
       |            CUSTOM AGENT              |
       |                                      |
       |  - Who am I?                         |
       |  - Git safety                        |
       |  - Branch workflow                   |
       |  - Review workflow                   |
       |  - Output behaviour                  |
       |  - No source-code modifications      |
       +------------------+-------------------+
                          |
                          | Uses review criteria
                          | and references rules
                          v
       +--------------------------------------+
       |          reviewer.prompt.md         |
       |                                      |
       |              REVIEW TASK             |
       |                                      |
       |  - What to review                    |
       |  - Review priorities                 |
       |  - Review scope                      |
       |  - Finding quality                   |
       +------------------+-------------------+
                          |
                          | Review against
                          v
       +--------------------------------------+
       |           frontend-rules.md         |
       |                                      |
       |          FRONTEND STANDARDS          |
       |                                      |
       |  - React                             |
       |  - Naming conventions                |
       |  - JavaScript / TypeScript           |
       |  - JSX                               |
       |  - Documentation                     |
       |  - Rule enforcement                  |
       +--------------------------------------+
```

**Important:** The custom agent must explicitly reference or incorporate the review criteria and frontend rules. Simply placing these files in the project does not automatically connect them.

### The overall flow

The Git repository provides the code changes and existing implementation. The agent uses these alongside the story requirements and frontend standards to perform the review.

The final review findings are saved locally.

```text
             +---------------------------------------+
             |              Git Repository           |
             |                                       |
             |  Target branch ----------------+      |
             |                                |      |
             |  Source / MR branch -----------+----->|
             |                                |      |
             |  Existing source code ---------+      |
             |                                       |
             +-------------------+-------------------+
                                 |
                                 v
                     +-----------------------+
                     |  Frontend MR Reviewer |
                     +-----------+-----------+
                                 |
                                 v
                     +-----------------------+
                     |      MR-Reviews/     |
                     |                       |
                     |  feature-ABC-review  |
                     |         .md          |
                     +-----------------------+
```

## Final thoughts

This is the first version (V1) of my Frontend MR Reviewer Agent.

__The idea is simple:__ build an agent that understands our team's coding standards, reviews changes in the context of actual business requirements, and produces actionable findings without modifying the application code or depending on external MCP integrations.

The goal is not to replace developer judgement. It is to make our code-review process more consistent, structured, and aligned with our team's engineering standards.