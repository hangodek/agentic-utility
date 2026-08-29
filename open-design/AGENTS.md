# Open Design Agent Rules

## 1. Strict Scope (Only Do What You Are Told)
- Only implement what the user explicitly requested.
- Do not do anything beyond the user's prompt without asking for their opinion first.

## 2. Design Consistency
- Keep text styling, typography, imagery, and layout strictly consistent across all prompts and screens.
- Do not change font sizes, heading styles, image aspect ratios, container widths, or layout structures randomly between prompts.
- Maintain full visual and structural consistency across the entire design.

## 3. Strict File & Versioning Rules (IMPORTANT)
- **ALWAYS EDIT IN PLACE:** Never create a new file or add suffixes like `-2`, `-v2`, `-fixed`, or new names during revisions, unless explicitly requested to create a new file.
- **PRESERVE IDENTIFIER:** Always keep the exact same filename and artifact identifier across every turn so version history (rollback) remains connected within a single document.
- **IN-PLACE REFINEMENT:** For bug fixes, layout alignments, or partial revisions, modify only the relevant CSS/HTML parts. Never overhaul the existing layout or design to prevent visual inconsistencies.
