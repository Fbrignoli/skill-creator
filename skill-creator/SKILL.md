---
name: skill-creator
description: Create or update effective skills through a pilot-driven workflow that runs one step at a time, stopping for the user's validation before each next step, with concrete examples, real dry runs, user correction, and reusable resources. Use when users want to extend an AI agent with specialized knowledge, workflows, or tool integrations.
---

# Skill Creator

This skill provides guidance for creating effective skills.

## The one rule that overrides everything: one step at a time

Never announce, list, or preview more than one step ahead. The natural failure mode is to lay out the whole plan at once ("here are the 6 steps we'll do") and then run them back to back. That is exactly what breaks this skill. Build the skill the way you teach a child: show ONE thing, do it, get it validated, and only then reveal the next.

- Each of your messages contains exactly ONE step: its announcement, its content, and its request for validation. Then you stop and wait.
- Never run several steps in a row. Never produce the full skill in one pass.
- The step lists further down are your private menu for choosing the NEXT step. Never paste them into the chat, never present them as a plan. Pick a single step from them, silently, and reveal only that one.
- Wait for the user's explicit validation before starting the next step. No automatic chaining, ever. If you are about to do more than one step, stop.

## About Skills

Skills are modular, self-contained folders that extend the agent's capabilities by providing
specialized knowledge, workflows, and tools. Think of them as "onboarding guides" for specific
domains or tasks—they transform the agent from a general-purpose agent into a specialized agent
equipped with procedural knowledge that no model can fully possess.

### What Skills Provide

1. Specialized workflows - Multi-step procedures for specific domains
2. Tool integrations - Instructions for working with specific file formats or APIs
3. Domain expertise - Company-specific knowledge, schemas, business logic
4. Bundled resources - Scripts, references, and assets for complex and repetitive tasks

## Core Principles

### Concise is Key

The context window is a public good. Skills share the context window with everything else the agent needs: system prompt, conversation history, other Skills' metadata, and the actual user request.

**Default assumption: the agent is already very smart.** Only add context the agent doesn't already have. Challenge each piece of information: "Does the agent really need this explanation?" and "Does this paragraph justify its token cost?"

Prefer concise examples over verbose explanations.

### Set Appropriate Degrees of Freedom

Match the level of specificity to the task's fragility and variability:

**High freedom (text-based instructions)**: Use when multiple approaches are valid, decisions depend on context, or heuristics guide the approach.

**Medium freedom (pseudocode or scripts with parameters)**: Use when a preferred pattern exists, some variation is acceptable, or configuration affects behavior.

**Low freedom (specific scripts, few parameters)**: Use when operations are fragile and error-prone, consistency is critical, or a specific sequence must be followed.

Think of the agent as exploring a path: a narrow bridge with cliffs needs specific guardrails (low freedom), while an open field allows many routes (high freedom).

### Build Through a Real Pilot

Do not write a complete skill from an abstract description in one pass. Select one concrete pilot task or artifact and build the procedure one step at a time. For every step:

1. Propose the action and its purpose
2. Execute it on the pilot with real tools when possible
3. Show the actual output
4. Collect the user's correction
5. Update the draft before continuing

A successful dry run makes the step eligible for validation. A failed dry run starts another correction loop. Do not treat structural validation as proof that the skill works.

Skills do not improve automatically. Convert observed failures and user feedback into explicit changes to instructions, inputs, boundaries, scripts, or references, then test again.

### Protect Validation Integrity

You may use subagents during iteration to validate whether a skill works on realistic tasks or whether a suspected problem is real. This is most useful when you want an independent pass on the skill's behavior, outputs, or failure modes after a revision.  Only do this when it is possible to start new subagents.

When using subagents for validation, treat that as an evaluation surface. The goal is to learn whether the skill generalizes, not whether another agent can reconstruct the answer from leaked context.

Prefer raw artifacts such as example prompts, outputs, diffs, logs, or traces. Give the minimum task-local context needed to perform the validation. Avoid passing the intended answer, suspected bug, intended fix, or your prior conclusions unless the validation explicitly requires them.

### Anatomy of a Skill

Every skill consists of a required SKILL.md file and optional bundled resources:

```
skill-name/
├── SKILL.md (required)
│   ├── YAML frontmatter metadata (required)
│   │   ├── name: (required)
│   │   └── description: (required)
│   └── Markdown instructions (required)
├── agents/ (recommended)
│   └── openai.yaml - UI metadata for skill lists and chips
└── Bundled Resources (optional)
    ├── scripts/          - Executable code (Python/Bash/etc.)
    ├── references/       - Documentation intended to be loaded into context as needed
    └── assets/           - Files used in output (templates, icons, fonts, etc.)
```

#### SKILL.md (required)

Every SKILL.md consists of:

- **Frontmatter** (YAML): Contains `name` and `description` fields. These are the only fields that the agent reads to determine when the skill gets used, thus it is very important to be clear and comprehensive in describing what the skill is, and when it should be used.
- **Body** (Markdown): Instructions and guidance for using the skill. Only loaded AFTER the skill triggers (if at all).

#### Agents metadata (recommended)

- UI-facing metadata for skill lists and chips
- Read references/openai_yaml.md before generating values and follow its descriptions and constraints
- Create: human-facing `display_name`, `short_description`, and `default_prompt` by reading the skill
- Generate deterministically by passing the values as `--interface key=value` to `scripts/generate_openai_yaml.py` or `scripts/init_skill.py`
- On updates: validate `agents/openai.yaml` still matches SKILL.md; regenerate if stale
- Only include other optional interface fields (icons, brand color) if explicitly provided
- See references/openai_yaml.md for field definitions and examples

#### Bundled Resources (optional)

##### Scripts (`scripts/`)

Executable code (Python/Bash/etc.) for tasks that require deterministic reliability or are repeatedly rewritten.

- **When to include**: When the same code is being rewritten repeatedly or deterministic reliability is needed
- **Example**: `scripts/rotate_pdf.py` for PDF rotation tasks
- **Benefits**: Token efficient, deterministic, may be executed without loading into context
- **Note**: Scripts may still need to be read by the agent for patching or environment-specific adjustments

##### References (`references/`)

Documentation and reference material intended to be loaded as needed into context to inform the agent's process and thinking.

- **When to include**: For documentation that the agent should reference while working
- **Examples**: `references/finance.md` for financial schemas, `references/mnda.md` for company NDA template, `references/policies.md` for company policies, `references/api_docs.md` for API specifications
- **Use cases**: Database schemas, API documentation, domain knowledge, company policies, detailed workflow guides
- **Benefits**: Keeps SKILL.md lean, loaded only when the agent determines it's needed
- **Best practice**: If files are large (>10k words), include grep search patterns in SKILL.md
- **Avoid duplication**: Information should live in either SKILL.md or references files, not both. Prefer references files for detailed information unless it's truly core to the skill—this keeps SKILL.md lean while making information discoverable without hogging the context window. Keep only essential procedural instructions and workflow guidance in SKILL.md; move detailed reference material, schemas, and examples to references files.

##### Assets (`assets/`)

Files not intended to be loaded into context, but rather used within the output the agent produces.

- **When to include**: When the skill needs files that will be used in the final output
- **Examples**: `assets/logo.png` for brand assets, `assets/slides.pptx` for PowerPoint templates, `assets/frontend-template/` for HTML/React boilerplate, `assets/font.ttf` for typography
- **Use cases**: Templates, images, icons, boilerplate code, fonts, sample documents that get copied or modified
- **Benefits**: Separates output resources from documentation, enables the agent to use files without loading them into context

#### What to Not Include in a Skill

A skill should only contain essential files that directly support its functionality. Do NOT create extraneous documentation or auxiliary files, including:

- README.md
- INSTALLATION_GUIDE.md
- QUICK_REFERENCE.md
- CHANGELOG.md
- etc.

The skill should only contain the information needed for an AI agent to do the job at hand. It should not contain auxiliary context about the process that went into creating it, setup and testing procedures, user-facing documentation, etc. Creating additional documentation files just adds clutter and confusion.

### Progressive Disclosure Design Principle

Skills use a three-level loading system to manage context efficiently:

1. **Metadata (name + description)** - Always in context (~100 words)
2. **SKILL.md body** - When skill triggers (<5k words)
3. **Bundled resources** - As needed by the agent (Unlimited because scripts can be executed without reading into context window)

#### Progressive Disclosure Patterns

Keep SKILL.md body to the essentials and under 500 lines to minimize context bloat. Split content into separate files when approaching this limit. When splitting out content into other files, it is very important to reference them from SKILL.md and describe clearly when to read them, to ensure the reader of the skill knows they exist and when to use them.

**Key principle:** When a skill supports multiple variations, frameworks, or options, keep only the core workflow and selection guidance in SKILL.md. Move variant-specific details (patterns, examples, configuration) into separate reference files.

**Pattern 1: High-level guide with references**

```markdown
# PDF Processing

## Quick start

Extract text with pdfplumber:
[code example]

## Advanced features

- **Form filling**: See [FORMS.md](FORMS.md) for complete guide
- **API reference**: See [REFERENCE.md](REFERENCE.md) for all methods
- **Examples**: See [EXAMPLES.md](EXAMPLES.md) for common patterns
```

The agent loads FORMS.md, REFERENCE.md, or EXAMPLES.md only when needed.

**Pattern 2: Domain-specific organization**

For Skills with multiple domains, organize content by domain to avoid loading irrelevant context:

```
bigquery-skill/
├── SKILL.md (overview and navigation)
└── reference/
    ├── finance.md (revenue, billing metrics)
    ├── sales.md (opportunities, pipeline)
    ├── product.md (API usage, features)
    └── marketing.md (campaigns, attribution)
```

When a user asks about sales metrics, the agent only reads sales.md.

Similarly, for skills supporting multiple frameworks or variants, organize by variant:

```
cloud-deploy/
├── SKILL.md (workflow + provider selection)
└── references/
    ├── aws.md (AWS deployment patterns)
    ├── gcp.md (GCP deployment patterns)
    └── azure.md (Azure deployment patterns)
```

When the user chooses AWS, the agent only reads aws.md.

**Pattern 3: Conditional details**

Show basic content, link to advanced content:

```markdown
# DOCX Processing

## Creating documents

Use docx-js for new documents. See [DOCX-JS.md](DOCX-JS.md).

## Editing documents

For simple edits, modify the XML directly.

**For tracked changes**: See [REDLINING.md](REDLINING.md)
**For OOXML details**: See [OOXML.md](OOXML.md)
```

The agent reads REDLINING.md or OOXML.md only when the user needs those features.

**Important guidelines:**

- **Avoid deeply nested references** - Keep references one level deep from SKILL.md. All reference files should link directly from SKILL.md.
- **Structure longer reference files** - For files longer than 100 lines, include a table of contents at the top so the agent can see the full scope when previewing.

## Skill Creation Process

Skill creation involves these steps:

1. Frame the need and choose a real pilot
2. Plan the procedure and reusable contents
3. Initialize a minimal draft
4. Build and dry-run one procedure step at a time
5. Finalize the skill from observed corrections
6. Validate end to end and obtain approval
7. Install the approved skill and iterate from real usage

Follow these steps in order. Do not skip the pilot, dry-run loop, or final approval.

### Skill Naming

- Use lowercase letters, digits, and hyphens only; normalize user-provided titles to hyphen-case (e.g., "Plan Mode" -> `plan-mode`).
- When generating names, generate a name under 64 characters (letters, digits, hyphens).
- Prefer short, verb-led phrases that describe the action.
- Namespace by tool when it improves clarity or triggering (e.g., `gh-address-comments`, `linear-address-issue`).
- Name the skill folder exactly after the skill name.

### Step 1: Frame the Need and Choose a Pilot

Start from the user's need, not from a generic skill template. Establish these elements before writing the procedure:

- Objective in one sentence
- Concrete phrases or situations that should trigger the skill
- Expected output or observable action
- One real pilot task, file, URL, project, or artifact
- Target installation location
- Important exclusions, unsafe actions, and failure conditions

Ask only for critical missing information. Propose reasonable defaults for minor details and let the user correct them. Do not replace the pilot with a purely hypothetical example unless no real artifact can exist.

Restate the objective, pilot, expected result, and boundaries. Resolve material misunderstandings before continuing.

### Step 2: Plan the Procedure and Reusable Contents

Walk through the pilot from scratch and identify:

1. The smallest ordered procedure that can produce the expected result
2. Inputs each step requires
3. Decisions that need heuristics versus deterministic rules
4. Reusable `scripts/`, `references/`, or `assets/`
5. Likely mistakes, exclusions, and observable failure signals

Keep the procedure to seven steps or fewer. If it needs more, propose splitting the skill. Include a script when deterministic reliability is needed or the same code would otherwise be rewritten. Include references for detailed knowledge and assets for files copied into outputs.

Treat this plan as a hypothesis to test, not as the final procedure.

### Step 3: Initialize a Minimal Draft

For a new skill, ask where it should ultimately be installed. If the user does not specify a location, use the skills directory of the tool in use (for example `~/.claude/skills`, `~/.codex/skills`, or `~/.gemini/skills`; see the repository INSTALL.md for each tool).

Initialize the skill in a staging location so an incomplete draft does not replace or pollute the installed skill:

```bash
scripts/init_skill.py <skill-name> --path <staging-parent> [--resources scripts,references,assets] [--examples]
```

Pass generated `display_name`, `short_description`, and `default_prompt` with `--interface key=value`. Read `references/openai_yaml.md` before generating these values.

For an existing skill, make changes in a staging copy when overwriting the installed version would expose an incomplete workflow. Preserve the current installed version until approval.

Keep the first draft minimal. Include only:

- Required `name` and `description` frontmatter
- A visible draft note naming the pilot
- Objective and required inputs
- Boundaries and failure modes
- An initially empty or skeletal procedure
- Expected output contract

Do not install or overwrite the active skill at this stage. Delete unused example placeholders and resource directories.

### Step 4: Build and Dry-Run One Step at a Time

For each proposed procedure step, complete the full loop before moving to the next step:

1. **Propose** — State the action, purpose, inputs, and success signal.
2. **Execute** — Run the step on the real pilot with the actual tools and source material. Do not merely describe what would happen when execution is possible.
3. **Show** — Present the concrete output, artifact, diff, log, or decision produced by the step.
4. **Correct** — Ask the user what is missing, wrong, unsafe, or unnecessarily rigid.
5. **Integrate** — Update that procedure step in the draft before continuing.

Propagate every correction to the appropriate part of the skill:

- Missing information becomes a required input
- A recurring error becomes a guardrail or failure mode
- An invalid context becomes a boundary or exclusion
- Repeated code becomes a tested script
- Detailed domain knowledge becomes a reference
- A stable output becomes part of the output contract

If the dry run fails, revise the current step and run it again. Do not advance merely because the written instruction looks plausible. If the user explicitly delegates judgment, apply the agreed acceptance criteria and record material assumptions.

If the same step is corrected three times for the same reason, stop and ask the user to restate the intended behavior in their own words. Stay within the pilot's scope; do not add speculative features.

### Step 5: Finalize the Skill from Observed Corrections

Once every procedure step succeeds on the pilot:

- Write all instructions in imperative or infinitive form
- Keep only non-obvious procedural and domain knowledge
- Make `description` state what the skill does and the concrete triggers or contexts for using it
- Add routing exclusions to `description` when they affect whether the skill should trigger
- Keep operational prohibitions, unsafe actions, failure signals, and recovery behavior explicit in the body
- Turn the successful pilot output into the canonical output contract or concise example
- Convert user corrections into guardrails and pitfalls
- Test every added script with representative inputs
- Remove the draft note, TODOs, unused placeholders, and duplicate documentation

The YAML frontmatter may contain only `name` and `description`.

### Step 6: Validate End to End and Obtain Approval

Run the complete draft on the pilot from a clean starting point, using only the skill and raw pilot inputs. Compare the actual result with the expected output established in Step 1.

Then run structural validation:

```bash
scripts/quick_validate.py <path/to/skill-folder>
```

Structural validation checks frontmatter and naming; it does not replace the behavioral dry run. Show the user the final skill and the relevant pilot output, diff, or test result.

- If the result is accepted, obtain explicit approval to install or overwrite the skill.
- If the result is rejected, return to the failing step, integrate the correction, and repeat the dry run.

Do not call the skill complete while either validation is failing.

### Step 7: Install and Iterate from Real Usage

After approval:

1. Install the staged skill at the agreed destination or apply the approved diff to the existing skill
2. Generate or refresh `agents/openai.yaml` when its interface metadata is stale
3. Verify the final folder, resource paths, and discovery location
4. Run `quick_validate.py` once more against the installed copy

For later improvements, reproduce the real failure with a concrete case, edit the smallest relevant instruction or resource, and repeat the same dry-run and approval loop. Never claim that a skill self-improves without observed evidence and an explicit file update.

## Forward-Testing Beyond the Pilot

Use forward-testing for complex or high-impact skills after the pilot succeeds. Test one or two adjacent realistic tasks to check whether the procedure generalizes without overfitting.

When fresh agents are available and their use is permitted, give them only the skill, a realistic request, and raw artifacts. Do not leak the expected answer, suspected bug, or intended fix. Review their outputs and clean up test artifacts between runs.

Ask for approval before a forward test that may take significant time, require additional permissions, or modify live systems. If a forward test fails, return to the relevant procedure step and repeat the correction loop.

## Completion Gate

A skill is complete only when all of these are true:

- A real pilot has been executed end to end
- Every procedure step has produced an observable successful result
- User corrections have been integrated into the draft
- Positive triggers and important exclusions are explicit
- Scripts and structural validation pass
- The user has approved the final behavior
- Only the approved version has been installed
