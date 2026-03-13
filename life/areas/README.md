# Knowledge Graph — Layer 1 Memory

This is the structured long-term memory store. Facts about the world, organized by entity.

## Structure

```
life/areas/
├── people/         # Everyone the user interacts with
│   └── <name>/
│       ├── summary.md     # Living doc, rewritten weekly — hot/warm/cold decay
│       └── items.json     # Atomic facts with timestamps and status
├── companies/      # Organizations, clients, employers
│   └── <name>/
│       ├── summary.md
│       └── items.json
├── projects/       # Active and past projects
│   └── <name>/
│       ├── summary.md
│       └── items.json
├── resources/      # Reference material, tools, systems
└── archive/        # Completed or inactive entities
```

## Atomic Fact Schema (items.json)

Each fact in items.json follows this structure:
```json
{
  "id": "unique-id",
  "fact": "The fact as a plain sentence.",
  "timestamp": "ISO-8601",
  "status": "active | superseded | historical",
  "supersededBy": "id-of-newer-fact or null",
  "accessCount": 0
}
```

## Decay Tiers (summary.md)

- **Hot** (<7 days): prominently featured at the top
- **Warm** (8–30 days): included but lower priority
- **Cold** (>30 days): omitted from summary, still in items.json
- High `accessCount` resists decay regardless of age

## Rules

- Never delete facts — mark as `superseded` with a reference to the newer fact
- summary.md is rewritten weekly (Sunday cron) from items.json
- Fact extraction runs every 30 minutes via heartbeat sub-agent
