# Natural Writing Guidelines

These guidelines apply to all notes in this repository. They shape tone, structure, and clarity without adding fluff.

## Writing Rhythm and Flow
- Vary sentence length dramatically. Short, punchy lines. Then longer, explanatory ones with technical depth.
- Use natural transitions: "Here's where it gets interesting", "Let's look at the configuration", "This is the crucial part".
- Mix paragraph lengths. Single-sentence paragraphs for emphasis; multi-sentence paragraphs for detail.
- Avoid mechanical patterns. Don’t start consecutive sections the same way.

## Language Variety
- Banned phrases: "dive into", "best practices", "leverage", "utilize" (use "use"), "in order to" (use "to"), "actionable insights", "paradigm", "synergy", "bandwidth" (unless literal network bandwidth), "low-hanging fruit", "move the needle", "circle back".
- Balance precision and clarity: e.g., "The service is temporarily unavailable (HTTP 503)"; "Set up token‑based security (JWT)".

## Creating Natural Flow
- Conversational connectors: "Actually, there's more to this...", "The interesting part is...", "Here's what you need to know...", "One thing to watch out for...".
- Practical context:
  - Acknowledge complexity: "This gets complicated when..."
  - Alternatives: "Another approach would be..."
  - Warnings: "Careful with this setting..."
  - Real‑world: "In production environments..."

## Content Balance
- Do include:
  - Specific tool names and version requirements
  - Configuration examples with real values
  - Common error messages and fixes
  - Performance considerations when relevant
  - Security implications of configurations
  - Practical examples and use cases
- Don’t include:
  - Basic syntax tutorials unless essential
  - Generic tool descriptions available elsewhere
  - Excessive boilerplate explanations
  - Redundant concept definitions
  - Overly abstract theory without application

## Dynamic Tone
- Configurations: precise but accessible. Example: "Set the timeout to 30 seconds for standard loads".
- Troubleshooting: direct and helpful. Example: "If you see this error, check your permissions first".
- Architecture: clear but not oversimplified. Example: "The load balancer distributes requests based on current CPU usage".
- Warnings: explicit consequences. Example: "This will delete all data — make sure you have backups".

## How to Apply in These Notes
- Keep headings stable if they are already referenced elsewhere; prefer rewording prose first. When changing a heading (like "Best Practices"), update in‑file links and any index references.
- Replace banned phrases with:
  - "best practices" → "recommended patterns" or "practical guidance"
  - "leverage" → "use"
  - "utilize" → "use"
  - "in order to" → "to"
  - "synergy" → "integration" or the specific effect
  - "dive into" → "explore" or "look at"
- Add brief context where helpful: a one‑line "Why it matters" after key concepts.
- Include version notes when relevant (e.g., Java 17+, JUnit 5.10, Spring Boot 3.x). Be explicit if behavior differs between versions.
- For errors, document: the message text, likely cause, and a minimal fix example.

## Example Rewrites
- Before: "We’ll dive into stream best practices."
  - After: "We’ll explore practical stream patterns. Here’s where it gets interesting: short‑circuit operations change performance characteristics."
- Before: "Leverage dependency injection to move the needle."
  - After: "Use dependency injection to decouple construction from behavior. The interesting part is how it simplifies testing."

---

Questions about scope or bulk edits? Keep anchors stable first, then iterate file‑by‑file.

