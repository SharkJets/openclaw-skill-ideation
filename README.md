# OpenClaw Ideation Skill

Transform raw brain dumps into structured implementation artifacts through a confidence-gated workflow.

Inspired by [nicknisi/claude-plugins/ideation](https://github.com/nicknisi/claude-plugins/tree/main/plugins/ideation), adapted for OpenClaw.

## What It Does

Turn messy, stream-of-consciousness ideas into:
- **Contracts** — Problem statements, goals, success criteria, scope boundaries
- **PRDs** (optional) — Phased requirements documents
- **Implementation Specs** — Detailed technical specs ready for execution

## Installation

### Option 1: Clone + symlink (recommended)

```bash
mkdir -p ~/.openclaw/skills ~/.openclaw/skills-src
git clone https://github.com/SharkJets/openclaw-skill-ideation.git ~/.openclaw/skills-src/openclaw-skill-ideation
ln -s ~/.openclaw/skills-src/openclaw-skill-ideation/ideation ~/.openclaw/skills/ideation
```

### Option 2: degit (no git history)

```bash
mkdir -p ~/.openclaw/skills
npx degit SharkJets/openclaw-skill-ideation/ideation ~/.openclaw/skills/ideation
```

### Option 3: Manual clone

```bash
mkdir -p ~/.openclaw/skills
git clone https://github.com/SharkJets/openclaw-skill-ideation.git /tmp/oc-ideation
cp -r /tmp/oc-ideation/ideation ~/.openclaw/skills/
rm -rf /tmp/oc-ideation
```

### Option 4: Workspace-local

Clone or copy the `ideation/` folder into your project's `skills/` directory for per-project use.

### After Installation

OpenClaw snapshots skills when a session starts. After installing, **start a new session** for the skill to be available. A gateway restart alone isn't enough — you need a fresh session.

Alternatively, enable the skills watcher for hot-reload in `~/.openclaw/openclaw.json`:

```json
{
  "skills": {
    "load": {
      "watch": true
    }
  }
}
```

## Usage

Just dump your messy ideas:

```
I want to build a thing that lets users save bookmarks but like, 
not just URLs, also notes and stuff. Maybe tags? Folders feel 
old school. Should work offline I think. Oh and it needs to 
sync across devices somehow...
```

The skill will:

1. **Intake** — Accept your mess without judgment
2. **Explore** — Understand your existing codebase (if applicable)
3. **Score** — Rate clarity across 5 dimensions (0-100)
4. **Clarify** — Ask targeted questions until confidence ≥ 95
5. **Contract** — Generate `contract.md` for your approval
6. **Spec** — Create implementation specs with validation commands

## Confidence Scoring

Each dimension is scored 0-20 points:

| Dimension | Question |
|-----------|----------|
| Problem Clarity | Do I understand what we're solving and why? |
| Goal Definition | Are goals specific and measurable? |
| Success Criteria | Can I write tests for "done"? |
| Scope Boundaries | Do I know what's in/out? |
| Consistency | Are there contradictions? |

**Thresholds:**
- < 70: Ask 5+ questions
- 70-84: Ask 3-5 questions  
- 85-94: Ask 1-2 questions
- ≥ 95: Generate contract

## Output Structure

```
./docs/ideation/{project-name}/
├── contract.md              # Problem, goals, scope (always)
├── prd-phase-1.md           # Requirements (if PRDs chosen)
├── prd-phase-2.md
├── spec-phase-1.md          # Implementation specs (always)
├── spec-phase-2.md
└── spec-template-{name}.md  # Shared template for repeatable phases
```

## Files

```
ideation/
├── SKILL.md                    # Main skill definition
└── references/
    ├── confidence-rubric.md    # Detailed scoring criteria
    ├── contract-template.md    # Contract output template
    ├── prd-template.md         # PRD output template
    └── spec-template.md        # Spec output template
```

## Credits

Based on the ideation plugin by [Nick Nisi](https://github.com/nicknisi) from [claude-plugins](https://github.com/nicknisi/claude-plugins).

## License

MIT
